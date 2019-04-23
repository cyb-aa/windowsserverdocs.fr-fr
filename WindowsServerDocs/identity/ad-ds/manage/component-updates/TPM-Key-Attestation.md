---
ms.assetid: 16a344a9-f9a6-4ae2-9bea-c79a0075fd04
title: Attestation de clé TPM
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 3cfc047fe1a66617abbda1de5f2c5842dcbdeb12
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862880"
---
# <a name="tpm-key-attestation"></a>Attestation de clé TPM

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

**Auteur**: Justin Turner, ingénieur Support résolution Senior auprès du groupe Windows  
  
> [!NOTE]  
> Ce contenu est écrit par un ingénieur du support client Microsoft et est destiné aux administrateurs expérimentés et aux architectes système qui recherchent des explications techniques plus approfondies des fonctionnalités et des solutions Windows Server 2012 R2 que n'en proposent généralement les rubriques de TechNet. Toutefois, il n'a pas subi les mêmes passes de correction. De ce fait, une partie du langage peut sembler moins finalisée que le contenu de TechNet.  
  
## <a name="overview"></a>Vue d'ensemble  
Lors de la prise en charge pour les clés protégées par le module de plateforme sécurisée existe depuis Windows 8, il n’y avait aucun mécanisme pour les autorités de certification d’attester par chiffrement que la clé privée de demandeur de certificat est réellement protégée par un Module de plateforme sécurisée (TPM). Cette mise à jour permet à une autorité de certification pour effectuer cette attestation et afin de refléter cette attestation dans le certificat émis.  
  
> [!NOTE]  
> Cet article suppose que le lecteur est familiarisé avec le concept de modèle de certificat (pour référence, consultez [modèles de certificats](https://technet.microsoft.com/library/cc730705.aspx)). Il suppose également que le lecteur est familiarisé avec la manière de configurer les autorités de certification d’entreprise pour émettre des certificats basés sur des modèles de certificat (pour référence, consultez [liste de vérification : Configurer les autorités de certification pour émettre et gérer des certificats](https://technet.microsoft.com/library/cc771533.aspx)).  
  
### <a name="terminology"></a>Terminology  
  
|Terme|Définition|  
|--------|--------------|  
|EK|Paire de clés. Il s’agit d’une clé asymétrique contenue dans le module de plateforme sécurisée (injectée au moment de la fabrication). Le EK est unique pour chaque module TPM et pouvez l’identifier. Le EK ne peut pas être modifié ou supprimé.|  
|EKpub|Fait référence à une clé publique de la paire.|  
|EKPriv|Fait référence à la clé privée de la paire.|  
|EKCert|Certificat EK. Un certificat TPM émis par le fabricant pour EKPub. Pas tous les modules TPM ont EKCert.|  
|TPM|Module de plateforme sécurisée. Un module de plateforme sécurisée est conçu pour fournir des fonctions liées à la sécurité basée sur le matériel. Une puce TPM est un processeur de chiffrement sécurisé conçu pour effectuer des opérations de chiffrement. La puce comprend plusieurs mécanismes de sécurité physique qui la protègent contre la falsification, et les logiciels malveillants ne peuvent pas falsifier les fonctions de sécurité du TPM.|  
  
### <a name="background"></a>Arrière-plan  
À compter de Windows 8, un Module de plateforme sécurisée (TPM) peut être utilisé pour sécuriser la clé privée d’un certificat. Le fournisseur de stockage de clés (KSP) de Microsoft Platform Crypto fournisseur Active cette fonctionnalité. Il existait deux problèmes avec l’implémentation :  

-   Il n’a aucune garantie qu’une clé est réellement protégée par un module de plateforme sécurisée (un utilisateur peut facilement usurper l’identité un KSP de logiciel en tant qu’un TPM KSP avec informations d’identification d’administrateur local).

-   Il n’était pas possible de limiter la liste des modules de plateforme sécurisée qui sont autorisés à protéger l’entreprise a émis les certificats (dans le cas où l’administrateur d’infrastructure à clé publique souhaite contrôler les types d’appareils qui peuvent être utilisées pour obtenir des certificats dans l’environnement).  

### <a name="tpm-key-attestation"></a>Attestation de clé TPM  
Attestation de clé TPM est la capacité de l’entité qui demande un certificat de prouver par chiffrement pour une autorité de certification que la clé RSA dans la demande de certificat est protégée par « a » ou « the » TPM qui approuve l’autorité de certification. Le modèle d’approbation de module de plateforme sécurisée est abordé plus dans le [vue d’ensemble du déploiement](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_DeploymentOverview) section plus loin dans cette rubrique.  
  
### <a name="why-is-tpm-key-attestation-important"></a>Pourquoi les attestation de clé TPM est-elle importante ?  
Un certificat de l’utilisateur avec une clé attesté par le module de plateforme sécurisée fournit la garantie de sécurité plus élevé, sauvegardé par non-exportabilité anti-hameçonnage, isolation et de clés fournies par le module de plateforme sécurisée.  
  
Avec l’attestation de clé TPM, un nouveau paradigme de gestion est désormais possible : Un administrateur peut définir l’ensemble des périphériques les utilisateurs peuvent utiliser pour accéder aux ressources d’entreprise (par exemple, VPN ou point d’accès sans fil) et avoir **fort** garantit qu’aucun autre périphérique ne peut être utilisé pour y accéder. Ce nouveau paradigme de contrôle d’accès est **fort** , car il est lié à un *liées au matériel* identité de l’utilisateur, qui est plus puissante que les informations d’identification basé sur logiciel.
  
### <a name="how-does-tpm-key-attestation-work"></a>Comment fonctionne l’attestation de clé TPM ?  
En règle générale, l’attestation de clé TPM est basée sur les piliers suivants :  
  
1.  Chaque module TPM est livré avec une clé asymétrique unique, appelée le *ek* EK (Endorsement Key), gravé par le fabricant. Nous faisons référence à la partie publique de cette clé comme *EKPub* et la clé privée associée en tant que *EKPriv*. Certains processeurs TPM ont également un certificat EK qui est émis par le fabricant pour le EKPub. Nous faisons référence à ce certificat en tant que *EKCert*.  
  
2.  Une autorité de certification établit la confiance dans le module de plateforme sécurisée via EKPub ou EKCert.  
  
3.  Un utilisateur se révèle l’autorité de certification que la clé RSA pour laquelle le certificat est demandé est cryptographiquement liée à la EKPub et que l’utilisateur possède le EKpriv.  
  
4.  L’autorité de certification émet un certificat avec une stratégie d’émission spéciale OID pour indiquer que la clé est désormais attestée devant être protégée par un module de plateforme sécurisée.  
  
## <a name="BKMK_DeploymentOverview"></a>Vue d’ensemble du déploiement  
Dans ce déploiement, il est supposé qu’une autorité de certification d’entreprise de Windows Server 2012 R2 est configurée. En outre, les clients (Windows 8.1) est configurés pour vous inscrire sur cette autorité de certification d’entreprise à l’aide pour les modèles de certificats. 

Il existe trois étapes de déploiement de l’attestation de clé TPM :  
  
1.  **Planifiez le modèle d’approbation TPM :** La première étape consiste à décider quel modèle d’approbation de module de plateforme sécurisée à utiliser. Il existe 3 méthodes prises en charge pour y parvenir :  
  
    -   **Approbation en fonction des informations d’identification de l’utilisateur :** L’autorité de certification d’entreprise approuve le EKPub fourni par l’utilisateur dans le cadre de la demande de certificat et aucune validation n’est effectuée autres que des informations d’identification de domaine de l’utilisateur.  
  
    -   **Approbation selon EKCert :** L’autorité de certification valide la chaîne EKCert qui est fournie dans le cadre de la demande de certificat par rapport à une liste gérée par l’administrateur de *les chaînes de certificat EK acceptables*. Les chaînes acceptables sont définis par fabricant et sont exprimées par deux magasins de certificats personnalisé sur l’autorité de certification émettrice (un magasin pour intermédiaires) et l’autre pour les certificats d’autorité de certification racine. Ce mode d’approbation signifie que **tous les** modules de plateforme sécurisée à partir d’un fabricant donné sont approuvés. Notez que dans ce mode, les modules de plateforme sécurisée en cours d’utilisation dans l’environnement doivent contenir EKCerts.
  
    -   **Approbation selon EKPub :** L’autorité de certification valide le fait que le EKPub fourni comme partie de la demande de certificat s’affiche dans une liste gérée par l’administrateur d’autorisé EKPubs. Cette liste est exprimée en tant que répertoire de fichiers où le nom de chaque fichier dans ce répertoire est le hachage SHA-2 de la EKPub autorisée. Cette option offre le plus haut niveau d’assurance, mais nécessite plus d’efforts d’administration, car chaque appareil est identifiée individuellement. Dans ce modèle d’approbation, seuls les appareils qui ont connu EKPub leur module de plateforme sécurisée ajouté à la liste autorisée de EKPubs sont autorisés à s’inscrire pour un certificat attesté par le module de plateforme sécurisée.  
  
    Selon la méthode utilisée, l’autorité de certification s’applique une stratégie d’émission différentes OID avec le certificat émis. Pour plus d’informations sur les OID de la stratégie d’émission, consultez le tableau de l’OID de stratégie d’émission dans le [configurer un modèle de certificat](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_ConfigCertTemplate) dans cette rubrique.  
  
    Notez qu’il est possible de choisir une combinaison de modèles d’approbation de module de plateforme sécurisée. Dans ce cas, l’autorité de certification accepte toutes les méthodes d’attestation et la stratégie d’émission Qu'oid reflète toutes les méthodes de l’attestation réussiront.  
  
2.  **Configurer le modèle de certificat :** Configuration du modèle de certificat est décrit dans la [détails du déploiement](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_DeploymentDetails) dans cette rubrique. Cet article ne couvre la manière dont ce modèle de certificat est attribué à l’autorité de certification d’entreprise ni comment inscrire des accès est accordé à un groupe d’utilisateurs. Pour plus d'informations, consultez [Liste de vérification : Configurer les autorités de certification pour émettre et gérer des certificats](https://technet.microsoft.com/library/cc771533.aspx).  
  
3.  **Configurer l’autorité de certification pour le modèle d’approbation de module de plateforme sécurisée**  
  
    1.  **Approbation en fonction des informations d’identification de l’utilisateur :** Aucune configuration spécifique est requise.  
  
    2.  **Approbation selon EKCert :** L’administrateur doit obtenir les certificats de la chaîne EKCert provenant de fabricants TPM et importez-les dans deux magasins de nouveaux certificats, créés par l’administrateur, sur l’autorité de certification qui effectuent l’attestation de clé TPM. Pour plus d’informations, consultez le [configuration de l’autorité de certification](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig) dans cette rubrique.  
  
    3.  **Approbation selon EKPub :** L’administrateur doit obtenir le EKPub pour chaque appareil qui sera attesté par le module de plateforme sécurisée les certificats nécessaires et les ajouter à la liste des EKPubs autorisées. Pour plus d’informations, consultez le [configuration de l’autorité de certification](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig) dans cette rubrique.  
  
    > [!NOTE]  
    > -   Cette fonctionnalité nécessite Windows 8.1 / Windows Server 2012 R2.  
    > -   L’attestation de clé TPM KSP de carte à puce tiers n’est pas pris en charge. Microsoft Platform Crypto fournisseur KSP doit être utilisé.  
    > -   Attestation de clé TPM fonctionne uniquement pour les clés RSA.  
    > -   Attestation de clé TPM n’est pas pris en charge pour une autorité de certification autonome.  
    > -   Attestation de clé TPM ne prend pas en charge [traitement de certificat non persistant](https://technet.microsoft.com/library/ff934598).  
  
## <a name="BKMK_DeploymentDetails"></a>Détails du déploiement  
  
### <a name="BKMK_ConfigCertTemplate"></a>Configurer un modèle de certificat  
Pour configurer le modèle de certificat pour l’attestation de clé TPM, effectuez les étapes de configuration suivantes :  
  
1.  **Compatibilité** onglet  
  
    Dans le **les paramètres de compatibilité** section :  
  
    -   Vérifiez **Windows Server 2012 R2** est sélectionné pour le **autorité de Certification**.  
  
    -   Vérifiez **Windows 8.1 / Windows Server 2012 R2** est sélectionné pour le **destinataire du certificat**.  
  
    ![Attestation de clé TPM](media/TPM-Key-Attestation/GTR_ADDS_CompatibilityTab.gif)  
  
2.  **Chiffrement** onglet  
  
    Vérifiez **Key Storage Provider** est sélectionné pour le **catégorie de fournisseur** et **RSA** est sélectionné pour le **nom de l’algorithme**. Vérifiez **requêtes doivent utiliser un des fournisseurs suivants** est sélectionné et le **fournisseur de chiffrement de plateforme Microsoft** option est sélectionnée sous **fournisseurs**.  
  
    ![Attestation de clé TPM](media/TPM-Key-Attestation/GTR_ADDS_CryptoTab.gif)  
  
3.  **Attestation de clé** onglet  
  
    Il s’agit d’un nouvel onglet pour Windows Server 2012 R2 :  
  
    ![Attestation de clé TPM](media/TPM-Key-Attestation/GTR_ADDS_ConfigCertTemplate.gif)  
  
    Choisir un mode d’attestation parmi les trois options possibles.  
  
    ![Attestation de clé TPM](media/TPM-Key-Attestation/GTR_ADDS_KeyModes.gif)  
  
    -   **None :** Implique que l’attestation de clé ne doit pas être utilisée  
  
    -   **Requis si le client est compatible avec :** Permet aux utilisateurs sur un appareil qui ne prend pas en charge l’attestation de clé TPM pour continuer à inscrire pour ce certificat. Les utilisateurs autorisés à effectuer d’attestation seront unique avec une stratégie d’émission spéciale OID. Certains appareils n’est peut-être pas en mesure d’effectuer l’attestation en raison d’un ancien module de plateforme sécurisée ne prenant pas en charge l’attestation de clé ou de l’appareil n’a ne pas un module de plateforme sécurisée du tout.
  
    -   **Obligatoire :** Client *doit* effectuer l’attestation de clé TPM, sinon la demande de certificat échoue.  
  
    Puis choisissez le modèle d’approbation de module de plateforme sécurisée. À nouveau trois options sont disponibles :  
  
    ![Attestation de clé TPM](media/TPM-Key-Attestation/GTR_ADDS_KeyTypeToEnforce.gif)  
  
    -   **Informations d’identification de l’utilisateur :** Autoriser un utilisateur d’authentification garantir un TPM valid en spécifiant leurs informations d’identification de domaine.  
  
    -   **Certificat d’approbation :** Le EKCert de l’appareil doit valider via gérée par l’administrateur GPC autorité de certification certificats intermédiaires pour un certificat d’autorité de certification racine gérée par l’administrateur. Si vous choisissez cette option, vous devez configurer EKCA et EKRoot magasins sur l’autorité de certification émettrice du certificat comme décrit dans la [configuration de l’autorité de certification](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig) dans cette rubrique.  
  
    -   **Paire de clés :** Le EKPub de l’appareil doit apparaître dans la liste gérée par l’administrateur d’infrastructure à clé publique. Cette option offre le plus haut niveau d’assurance, mais nécessite plus d’efforts d’administration. Si vous choisissez cette option, vous devez configurer une liste EKPub sur l’autorité de certification émettrice comme décrit dans la [configuration de l’autorité de certification](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig) dans cette rubrique.  
  
    Enfin, indiquez la stratégie d’émission à afficher dans le certificat émis. Par défaut, chaque type d’application a un identificateur d’objet associé (OID) qui sera inséré dans le certificat si elle passe ce type d’application, comme décrit dans le tableau suivant. Notez qu’il est possible de choisir une combinaison de méthodes d’application. Dans ce cas, l’autorité de certification accepte une des méthodes d’attestation et la stratégie d’émission OID reflète toutes les méthodes de l’attestation a réussi.  
  
    **OID de stratégie d’émission**  
  
    |OID|Type de l’attestation de clé|Description|Niveau d’assurance|  
    |-------|------------------------|---------------|-------------------|  
    |1.3.6.1.4.1.311.21.30|EK|« EK vérifié » :   Pour la liste gérée par l’administrateur de EK|Élevée|  
    |1.3.6.1.4.1.311.21.31|Certificat d’approbation|« Certificat EK vérifié » : Lorsque la chaîne de certificat EK est validée|Moyen|  
    |1.3.6.1.4.1.311.21.32|Informations d’identification utilisateur|« EK de confiance sur l’utilisation » : Pour EK attesté par l’utilisateur|Faible|  
  
    Les OID seront insérées dans le certificat émis si **incluent les stratégies d’émission** est sélectionné (la configuration par défaut).  
  
    ![Attestation de clé TPM](media/TPM-Key-Attestation/GTR_ADDS_IssuancePolicies.gif)  
  
    > [!TIP]  
    > Une utilisation potentielle d’avoir l’OID présent dans le certificat consiste à limiter l’accès VPN ou réseau à certains périphériques sans fil. Par exemple, votre stratégie d’accès peut autoriser connexion (ou accès à un réseau local virtuel différent) si OID 1.3.6.1.4.1.311.21.30 est présent dans le certificat. Cela vous permet de limiter l’accès aux appareils dont EK TPM n’est présent dans la liste EKPUB.  
  
### <a name="BKMK_CAConfig"></a>Configuration de l’autorité de certification  
  
1.  **Le programme d’installation EKCA et EKROOT des magasins de certificats sur une autorité de certification émettrice**  
  
    Si vous avez choisi **Endorsement certificat** pour les paramètres de modèle, effectuez les étapes de configuration suivantes :  
  
    1.  Utiliser Windows PowerShell pour créer deux nouveaux magasins de certificats sur le serveur d’autorité de certification qui effectue l’attestation de clé TPM.  
  
    2.  Obtenir les intermédiaires et racine ou les certificats d’autorité de certification à partir des fabricants que vous souhaitez autoriser dans votre environnement d’entreprise. Ces certificats doivent être importés dans les magasins de certificats créé précédemment (EKCA et EKROOT) comme il convient.  
  
    Le script Windows PowerShell suivant effectue ces deux étapes. Dans l’exemple suivant, le fabricant du module de plateforme sécurisée Fabrikam a fourni un certificat racine *FabrikamRoot.cer* et un certificat d’autorité de certification émettrice *Fabrikamca.cer*.  
  
    ```powershell  
    PS C:>\cd cert:  
    PS Cert:\>cd .\\LocalMachine  
    PS Cert:\LocalMachine> new-item EKROOT  
    PS Cert:\ LocalMachine> new-item EKCA  
    PS Cert:\EKCA\copy FabrikamCa.cer .\EKCA  
    PS Cert:\EKROOT\copy FabrikamRoot.cer .\EKROOT  
    ```  
  
2.  **Le programme d’installation de liste EKPUB si vous utilisez le type de l’attestation EK**  
  
    Si vous avez choisi **ek** dans les paramètres de modèle, les étapes de configuration suivantes pour créer et configurer un dossier sur l’autorité de certification émettrice, qui contient les fichiers de 0 octet, chacune nommée pour le hachage SHA-2 un EK autorisée. Ce dossier sert d’une « liste verte » de périphériques qui sont autorisés à obtenir des certificats attesté par la clé de module de plateforme sécurisée. Étant donné que vous devez ajouter manuellement le EKPUB pour chaque périphérique qui nécessite un certificat attestée, il fournit l’entreprise avec une garantie des appareils qui sont autorisés à obtenir des certificats attestées clés de module de plateforme sécurisée. Configuration d’une autorité de certification pour ce mode requiert deux étapes :  
  
    1.  **Créez l’entrée de Registre EndorsementKeyListDirectories :** Utilisez l’outil de ligne de commande Certutil pour configurer les emplacements de dossier où EKpubs approuvés sont définis comme décrit dans le tableau suivant.  
  
        |Opération|Syntaxe de commande|  
        |-------------|------------------|  
        |Ajouter des emplacements de dossier|certutil.exe -setreg CA\EndorsementKeyListDirectories +"<folder>"|  
        |Supprimer des emplacements de dossier|certutil.exe -setreg CA\EndorsementKeyListDirectories -"<folder>"|  
  
        Le EndorsementKeyListDirectories dans la commande certutil est un paramètre de Registre comme décrit dans le tableau suivant.  
  
        |Nom de valeur|Type|Données|  
        |--------------|--------|--------|  
        |EndorsementKeyListDirectories|REG_MULTI_SZ|< chemin d’accès LOCAL ou UNC à EKPUB autoriser ou les listes de ><br /><br />Exemple :<br /><br />*\\\blueCA.contoso.com\ekpub*<br /><br />*\\\bluecluster1.contoso.com\ekpub*<br /><br />D:\ekpub|  
  
        HKLM\SYSTEM\CurrentControlSet\Services\CertSvc\Configuration\\<CA Sanitized Name>  
  
        *EndorsementKeyListDirectories* contient une liste de chemins d’accès de système de fichiers local, ou UNC, chacun pointant vers un dossier de l’autorité de certification a un accès en lecture à. Chaque dossier peut contenir zéro ou plus autoriser les entrées de la liste, où chaque entrée est un fichier portant un nom qui est le SHA-2 hachage d’un EKpub approuvé, sans extension de fichier. 
        Création ou la modification de cette configuration de clé de Registre nécessite un redémarrage de l’autorité de certification, tout comme les paramètres de configuration de Registre existants autorité de certification. Toutefois, les modifications aux paramètres de configuration prendront effet immédiatement et ne nécessitent pas de redémarrer l’autorité de certification.  
  
        > [!IMPORTANT]  
        > Sécuriser les dossiers dans la liste contre la falsification et tout accès non autorisé en configurant les autorisations afin que seuls les administrateurs autorisés ai lu et l’accès en écriture. Le compte d’ordinateur de l’autorité de certification nécessite un accès en lecture seule.  
  
    2.  **Remplir la liste EKPUB :** Utilisez l’applet de commande Windows PowerShell suivante pour obtenir le hachage de clé publique de la EK TPM à l’aide de Windows PowerShell sur chaque appareil et ensuite envoyer ce hachage de clé publique à l’autorité de certification et les stocker sur le dossier EKPubList.  
  
        ```powershell  
        PS C:>\$a=Get-TpmEndorsementKeyInfo -hashalgorithm sha256  
        PS C:>$b=new-item $a.PublicKeyHash -ItemType file  
        ```  
  
## <a name="troubleshooting"></a>Résolution des problèmes  
  
### <a name="key-attestation-fields-are-unavailable-on-a-certificate-template"></a>Champs de l’attestation de clé ne sont pas disponibles sur un modèle de certificat  
Les champs de l’Attestation de clé ne sont pas disponibles si les paramètres du modèle ne répondent pas à la configuration requise pour l’attestation. Causes courantes sont :  
  
1.  Les paramètres de compatibilité ne sont pas configurés correctement. Assurez-vous qu’ils sont configurés comme suit :  
  
    1.  **Autorité de certification** : **Windows Server 2012 R2**  
  
    2.  **Destinataire du certificat**: **Windows 8.1 / Windows Server 2012 R2**  
  
2.  Les paramètres de chiffrement ne sont pas configurés correctement. Assurez-vous qu’ils sont configurés comme suit :  
  
    1.  **Catégorie de fournisseur**: **Fournisseur de stockage de clés**  
  
    2.  **Nom de l’algorithme**: **RSA**  
  
    3.  **Fournisseurs**: **Fournisseur de chiffrement de plateforme Microsoft**  
  
3.  Paramètres de gestion de la demande ne sont pas configurés correctement. Assurez-vous qu’ils sont configurés comme suit :  
  
    1.  Le **autoriser à exporter la clé privée** option ne doit pas être sélectionnée.  
  
    2.  Le **archiver la clé privée de chiffrement du sujet** option ne doit pas être sélectionnée.  
  
### <a name="verification-of-tpm-device-for-attestation"></a>Vérification de l’appareil de module de plateforme sécurisée pour l’attestation  
Utilisez l’applet de commande Windows PowerShell, **Confirm-CAEndorsementKeyInfo**, pour vérifier qu’un appareil TPM spécifique est approuvé pour l’attestation par les autorités de certification. Il existe deux options : une pour la vérification de la EKCert et l’autre pour la vérification d’un EKPub. L’applet de commande est soit exécutée localement sur une autorité de certification, ou sur des autorités de certification à distance à l’aide de communication à distance de Windows PowerShell.  
  
1.  Pour la vérification d’approbation sur un EKPub, effectuez les deux étapes suivantes :  
  
    1.  **Extraire le EKPub à partir de l’ordinateur client :** Le EKPub peut être extraites à partir d’un ordinateur client via **Get-TpmEndorsementKeyInfo**. À partir d’une invite de commandes avec élévation de privilèges, exécutez la commande suivante :  
  
        ```  
        PS C:>\$a=Get-TpmEndorsementKeyInfo -hashalgorithm sha256  
        ```  
  
    2.  **Vérifier la relation d’approbation sur un EKCert sur un ordinateur d’autorité de certification :** Copiez la chaîne extraite (le hachage SHA-2 de la EKPub) sur le serveur (par exemple, par courrier électronique) et passer à l’applet de commande Confirm-CAEndorsementKeyInfo. Notez que ce paramètre doit être de 64 caractères.  
  
        ```  
        Confirm-CAEndorsementKeyInfo [-PublicKeyHash] <string>  
        ```  
  
2.  Pour la vérification d’approbation sur un EKCert, effectuez les deux étapes suivantes :  
  
    1.  **Extraire le EKCert à partir de l’ordinateur client :** Le EKCert peut être extraites à partir d’un ordinateur client via **Get-TpmEndorsementKeyInfo**. À partir d’une invite de commandes avec élévation de privilèges, exécutez la commande suivante :  
  
        ```  
        PS C:>\$a=Get-TpmEndorsementKeyInfo
        PS C:>\$a.manufacturerCertificates|Export-Certificate -filepath c:\myEkcert.cer
        ```  
  
    2.  **Vérifier la relation d’approbation sur un EKCert sur un ordinateur d’autorité de certification :** Copiez l’extrait EKCert (EkCert.cer) à l’autorité de certification (par exemple, via la messagerie électronique ou xcopy). Par exemple, si vous copiez le fichier de certificat du dossier « c:\diagnose » sur le serveur d’autorité de certification, exécutez la commande suivante pour terminer la vérification :  
  
        ```  
        PS C:>new-object System.Security.Cryptography.X509Certificates.X509Certificate2 "c:\diagnose\myEKcert.cer" | Confirm-CAEndorsementKeyInfo  
        ```  
  
## <a name="see-also"></a>Voir aussi  
[Vue d’ensemble de la technologie Trusted Platform Module](https://technet.microsoft.com/library/jj131725.aspx)  
[Ressources externes : Module de plateforme sécurisée](http://www.cs.unh.edu/~it666/reading_list/Hardware/tpm_fundamentals.pdf)  
