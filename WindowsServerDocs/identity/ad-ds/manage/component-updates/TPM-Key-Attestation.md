---
ms.assetid: 16a344a9-f9a6-4ae2-9bea-c79a0075fd04
title: "Attestation de clé TPM"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 9d08efe825e10d526763a1654c7e090427719c51
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="tpm-key-attestation"></a>Attestation de clé TPM

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

**Auteur**: Justin Turner, ingénieur Support résolution Senior avec le groupe de Windows  
  
> [!NOTE]  
> Ce contenu est écrit par un ingénieur du support client Microsoft et est destiné aux administrateurs expérimentés et architectes système qui recherchent des explications techniques plus approfondies des fonctionnalités et des solutions Windows Server2012R2 que proposent généralement les rubriques sur TechNet. Toutefois, il n’a pas subi les mêmes passes, afin de la partie du langage peut sembler que moins finalisée que ce qui se trouvent généralement sur TechNet.  
  
## <a name="overview"></a>Vue d’ensemble  
Lors de la prise en charge pour les clés protégées par le module de plateforme sécurisée existe depuis Windows8, ont été aucun mécanisme pour les autorités de certification d’attester par chiffrement que la clé privée du demandeur du certificat est réellement protégée par un Module de plateforme sécurisée (TPM). Cette mise à jour permet à une autorité de certification pour effectuer cette attestation et afin de refléter cette attestation dans le certificat émis.  
  
> [!NOTE]  
> Cet article part du principe que le lecteur est familiarisé avec le concept de modèle de certificat (pour référence, voir [modèles de certificats](https://technet.microsoft.com/library/cc730705.aspx)). Il suppose également que le lecteur connaît comment configurer les autorités de certification d’entreprise pour émettre des certificats basés sur des modèles de certificat (pour référence, voir [liste de vérification: configurer l’autorités de certification pour émettre et gérer des certificats](https://technet.microsoft.com/library/cc771533.aspx)).  
  
### <a name="terminology"></a>Terminologie  
  
|Terme|Définition|  
|--------|--------------|  
|EK|Paire de clés. Il s’agit d’une clé asymétrique contenue dans le module de plateforme sécurisée (injecté à la fabrication). Le EK est unique pour chaque module de plateforme sécurisée et peut l’identifier. Le EK ne peuvent pas être modifié ou supprimé.|  
|EKpub|Fait référence à la clé publique de la paire.|  
|EKPriv|Fait référence à la clé privée de la paire.|  
|EKCert|Certificat EK. Un certificat TPM émis par le fabricant pour EKPub. Pas de tous les modules TPM ont EKCert.|  
|MODULE DE PLATEFORME SÉCURISÉE|Module de plateforme sécurisée. Un module de plateforme sécurisée est conçu pour fournir des fonctions de sécurité en fonction du matériel. Une puce TPM est un processeur de chiffrement sécurisé conçu pour effectuer des opérations de chiffrement. La puce comprend plusieurs mécanismes de sécurité physique pour rendre inviolable, et des logiciels malveillants ne peut pas falsifier les fonctions de sécurité du TPM.|  
  
### <a name="background"></a>En arrière-plan  
À compter de Windows8, un Module de plateforme sécurisée (TPM) peut être utilisé pour sécuriser la clé privée d’un certificat. Le fournisseur de stockage de clés (KSP) de Microsoft plate-forme Crypto fournisseur Active cette fonctionnalité. Il existait deux problèmes avec l’implémentation:  

-   Il n’a aucune garantie qu’une clé est réellement protégée par un module de plateforme sécurisée (quelqu'un peut facilement usurper un logiciel KSP comme un KSP TPM avec les informations d’identification d’administrateur local).

-   Il n’était pas possible de limiter la liste des modules de plateforme sécurisée qui sont autorisés à protéger l’entreprise qui ont émis les certificats (dans le cas où l’administrateur d’infrastructure à clé publique souhaite contrôler les types d’appareils qui peuvent être utilisés pour obtenir des certificats dans l’environnement).  

### <a name="tpm-key-attestation"></a>Attestation de clé TPM  
Attestation de clé de module de plateforme sécurisée est la possibilité de l’entité demandant un certificat de prouver par chiffrement pour une autorité de certification que la clé RSA dans la demande de certificat est protégée par «a» ou «le» TPM qui l’approuve l’autorité de certification. Le modèle d’approbation de module de plateforme sécurisée est abordé plus dans le [vue d’ensemble du déploiement](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_DeploymentOverview) plus loin dans cette rubrique.  
  
### <a name="why-is-tpm-key-attestation-important"></a>Pourquoi les attestation de clé TPM est-elle importante?  
Un certificat d’utilisateur avec une clé attesté par le module de plateforme sécurisée offre l’assurance de sécurité plus élevé, sauvegardée par non-exportabilité, anti-martèlement et l’isolement des clés fournies par le module de plateforme sécurisée.  
  
Avec attestation de clé TPM, un nouveau modèle de gestion est désormais possible: un administrateur peut définir l’ensemble des périphériques que les utilisateurs peuvent utiliser pour accéder aux ressources d’entreprise (par exemple, VPN ou point d’accès sans fil) et ont **fort** garantit qu’aucun autre périphérique ne peut être utilisé pour y accéder. Ce nouveau paradigme de contrôle d’accès est **fort**, car il est lié à un *liée au matériel* identité de l’utilisateur, qui est plus puissante que les informations d’identification de logiciel.
  
### <a name="how-does-tpm-key-attestation-work"></a>Fonctionne de l’attestation de clé TPM  
En règle générale, attestation de clé de module de plateforme sécurisée est basée sur les piliers suivantes:  
  
1.  Chaque module de plateforme sécurisée est fourni avec une clé asymétrique unique, appelé le *paire de clés* EK (Endorsement Key), gravé par le fabricant. Nous faisons référence à la partie publique de cette clé comme *EKPub* et la clé privée associée en tant que *EKPriv*. Certaines puces de TPM ont également un certificat EK émis par le fabricant pour le EKPub. Nous faisons référence à ce certificat en tant que *EKCert*.  
  
2.  Une autorité de certification établit la confiance dans le module de plateforme sécurisée via EKPub ou EKCert.  
  
3.  Un utilisateur destiné à prouver à l’autorité de certification que la clé RSA pour lequel le certificat est demandé est par chiffrement liée à la EKPub et que l’utilisateur possède le EKpriv.  
  
4.  L’autorité de certification émet un certificat avec une stratégie d’émission spéciale OID pour indiquer que la clé est désormais attestée d’être protégée par un module de plateforme sécurisée.  
  
## <a name="BKMK_DeploymentOverview"></a>Vue d’ensemble du déploiement  
Dans ce déploiement, il est supposé qu’une autorité de certification d’entreprise Windows Server2012R2 est configurée. En outre, les clients (Windows8.1) est configurés pour s’inscrire sur cette autorité de certification d’entreprise à l’aide des modèles de certificat. 

Il existe trois étapes de déploiement d’attestation de clé de module de plateforme sécurisée:  
  
1.  **Planifier le modèle d’approbation de module de plateforme sécurisée:** la première étape consiste à déterminer le modèle d’approbation de module de plateforme sécurisée à utiliser. Il existe 3méthodes prises en charge pour ce faire:  
  
    -   **Approbation basée sur les informations d’identification de l’utilisateur:** l’autorité de certification d’entreprise approuve le EKPub fourni par l’utilisateur dans le cadre de la demande de certificat et aucune validation n’est effectuée autres que des informations d’identification de domaine de l’utilisateur.  
  
    -   **Approbation basée sur EKCert:** l’autorité de certification valide la chaîne EKCert qui est fournie dans le cadre de la demande de certificat par rapport à une liste gérée par l’administrateur de *les chaînes de certificat EK acceptables*. Les chaînes acceptables sont définis par fabricant et sont exprimées par deux magasins de certificats personnalisés sur l’autorité de certification émettrice (un Windows store pour intermédiaires) et l’autre pour les certificats d’autorité de certification racine. Ce mode d’approbation signifie que **tous les** TPM d’un fabricant donné sont approuvés. Notez que dans ce mode, les modules de plateforme sécurisée en cours d’utilisation dans l’environnement doivent contenir EKCerts.
  
    -   **Approbation basée sur EKPub:** l’autorité de certification valide que le EKPub fourni en partie de la demande de certificat apparaît dans une liste gérée par l’administrateur d’autorisé EKPubs. Cette liste est exprimée en tant que répertoire de fichiers où le nom de chaque fichier dans ce répertoire est le hachage SHA-2 le EKPub autorisée. Cette option offre le niveau d’assurance le plus élevé, mais nécessite plus d’efforts d’administration, dans la mesure où chaque périphérique est identifié individuellement. Dans ce modèle d’approbation, que les périphériques qui ont connu des EKPub de leur module de plateforme sécurisée ajouté à la liste autorisée de EKPubs sont autorisés à s’inscrire pour un certificat attesté par le module de plateforme sécurisée.  
  
    En fonction de la méthode utilisée, l’autorité de certification s’applique une stratégie d’émission différentes OID avec le certificat émis. Pour plus d’informations sur les OID de stratégie d’émission, consultez le tableau de l’OID de stratégie d’émission dans les [configurer un modèle de certificat](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_ConfigCertTemplate) dans cette rubrique.  
  
    Notez qu’il est possible de choisir une combinaison des modèles d’approbation de module de plateforme sécurisée. Dans ce cas, l’autorité de certification accepte toutes les méthodes d’attestation et la stratégie d’émission Qu'oid correspondent à toutes les méthodes d’attestation réussiront.  
  
2.  **Configurer le modèle de certificat:** la configuration du modèle de certificat est décrit dans le [détails du déploiement](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_DeploymentDetails) dans cette rubrique. Cet article ne couvre la façon dont ce modèle de certificat est attribué à l’autorité de certification d’entreprise ou comment inscrire accès est accordé à un groupe d’utilisateurs. Pour plus d’informations, voir [liste de vérification: configurer l’autorités de certification pour émettre et gérer des certificats](https://technet.microsoft.com/library/cc771533.aspx).  
  
3.  **Configurer l’autorité de certification pour le modèle d’approbation de module de plateforme sécurisée**  
  
    1.  **Approbation basée sur les informations d’identification de l’utilisateur:** aucune configuration spécifique n’est requise.  
  
    2.  **Approbation basée sur EKCert:** l’administrateur doit obtenir les certificats de la chaîne EKCert de fabricants TPM et les importer sur deux magasins de nouveaux certificats, créés par l’administrateur, sur l’autorité de certification qui effectuent l’attestation de clé de module de plateforme sécurisée. Pour plus d’informations, voir la [configuration de l’autorité de certification](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig) dans cette rubrique.  
  
    3.  **Approbation basée sur EKPub:** l’administrateur doit obtenir le EKPub pour chaque périphérique qui sera nécessitant attesté par le module de plateforme sécurisée les certificats et les ajouter à la liste des EKPubs autorisées. Pour plus d’informations, voir la [configuration de l’autorité de certification](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig) dans cette rubrique.  
  
    > [!NOTE]  
    > -   Cette fonctionnalité nécessite Windows8.1 / Windows Server2012R2.  
    > -   Attestation de clé TPM KSP de carte à puce tiers n’est pas pris en charge. Chiffrement fournisseur KSP de Microsoft plate-forme doit être utilisé.  
    > -   Attestation de clé TPM fonctionne uniquement pour les clés RSA.  
    > -   Attestation de clé TPM n’est pas pris en charge pour une autorité de certification autonome.  
    > -   Ne gère pas l’attestation de clé TPM [traitement des certificats non persistantes](https://technet.microsoft.com/library/ff934598).  
  
## <a name="BKMK_DeploymentDetails"></a>Détails du déploiement  
  
### <a name="BKMK_ConfigCertTemplate"></a>Configurer un modèle de certificat  
Pour configurer le modèle de certificat pour l’attestation de clé de module de plateforme sécurisée, effectuez les étapes de configuration suivantes:  
  
1.  **Compatibilité** onglet  
  
    Dans le **les paramètres de compatibilité** section:  
  
    -   Vérifiez **Windows Server2012R2** est activée pour le **autorité de Certification**.  
  
    -   Vérifiez **Windows8.1 / Windows Server2012R2** est activée pour le **destinataire du certificat**.  
  
    ![Attestation de clé de module de plateforme sécurisée](media/TPM-Key-Attestation/GTR_ADDS_CompatibilityTab.gif)  
  
2.  **Chiffrement** onglet  
  
    Vérifiez **fournisseur de stockage de clés** est activée pour le **catégorie de fournisseur** et **RSA** est activée pour le **nom de l’algorithme**. Vérifiez **requêtes doivent utiliser un des fournisseurs suivants** est sélectionné et le **fournisseur de chiffrement de plateforme Microsoft** option est sélectionnée sous **fournisseurs**.  
  
    ![Attestation de clé de module de plateforme sécurisée](media/TPM-Key-Attestation/GTR_ADDS_CryptoTab.gif)  
  
3.  **Attestation de clé** onglet  
  
    Il s’agit d’un nouvel onglet pour Windows Server2012R2:  
  
    ![Attestation de clé de module de plateforme sécurisée](media/TPM-Key-Attestation/GTR_ADDS_ConfigCertTemplate.gif)  
  
    Choisissez un mode d’attestation dans les trois options possibles.  
  
    ![Attestation de clé de module de plateforme sécurisée](media/TPM-Key-Attestation/GTR_ADDS_KeyModes.gif)  
  
    -   **None:** implique qu’attestation de clé ne doit pas être utilisée  
  
    -   **Obligatoire, si le client est compatible avec:** attestation pour continuer l’inscription pour que le certificat de clé permet aux utilisateurs sur un appareil qui ne prend pas en charge le module de plateforme sécurisée. Les utilisateurs autorisés à effectuer l’attestation seront unique avec une stratégie d’émission spéciale OID. Certains appareils n’est peut-être pas en mesure d’effectuer d’attestation en raison d’un module de plateforme sécurisée ancien qui ne prend pas en charge l’attestation de clé ou de l’appareil n’ayant ne pas un module de plateforme sécurisée du tout.
  
    -   **Requis:** Client *doit* effectuer l’attestation de clé TPM, sinon la demande de certificat échoue.  
  
    Puis choisissez le modèle d’approbation de module de plateforme sécurisée. Il existe une nouvelle fois trois options:  
  
    ![Attestation de clé de module de plateforme sécurisée](media/TPM-Key-Attestation/GTR_ADDS_KeyTypeToEnforce.gif)  
  
    -   **Informations d’identification utilisateur:** autoriser un utilisateur d’authentification garantir un module de plateforme sécurisée valide en spécifiant leurs informations d’identification de domaine.  
  
    -   **Certificat de paire:** le EKCert de l’appareil doit valider via gérée par l’administrateur TPM autorité de certification certificats intermédiaires pour un certificat d’autorité de certification racine gérés par l’administrateur. Si vous choisissez cette option, vous devez configurer EKCA et EKRoot magasins sur l’autorité de certification émettrice du certificat comme décrit dans le [configuration de l’autorité de certification](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig) dans cette rubrique.  
  
    -   **Paire de clés:** le EKPub de l’appareil doit apparaître dans la liste gérée par l’administrateur d’infrastructure à clé publique. Cette option offre le niveau d’assurance le plus élevé, mais nécessite plus d’efforts d’administration. Si vous choisissez cette option, vous devez configurer une liste EKPub sur l’autorité de certification émettrice comme décrit dans le [configuration de l’autorité de certification](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig) dans cette rubrique.  
  
    Enfin, décider quelle stratégie d’émission à afficher dans le certificat émis. Par défaut, chaque type d’application a un identificateur d’objet associé (OID) qui sera inséré dans le certificat si elle transmet ce type d’application, comme décrit dans le tableau suivant. Notez qu’il est possible de choisir une combinaison de méthodes d’application. Dans ce cas, l’autorité de certification accepte une des méthodes de l’attestation, et la stratégie d’émission OID correspondent à toutes les méthodes d’attestation a réussi.  
  
    **OID de stratégie d’émission**  
  
    |OID|Type d’attestation de clé|Description|Niveau d’assurance|  
    |-------|------------------------|---------------|-------------------|  
    |1.3.6.1.4.1.311.21.30|EK|«EK vérifié»: pour gérée par l’administrateur la liste des EK|Élevé|  
    |1.3.6.1.4.1.311.21.31|Certificat (Endorsement Key)|«EK certificat vérifié»: lors de la chaîne de certificat EK est validée.|Moyenne|  
    |1.3.6.1.4.1.311.21.32|Informations d’identification utilisateur|«EK approuvé sur l’utilisation»: pour EK attesté par l’utilisateur|Faible|  
  
    Les OID seront insérées dans le certificat délivré si **incluent les stratégies d’émission** est sélectionné (la configuration par défaut).  
  
    ![Attestation de clé de module de plateforme sécurisée](media/TPM-Key-Attestation/GTR_ADDS_IssuancePolicies.gif)  
  
    > [!TIP]  
    > Une utilisation potentielle d’avoir l’OID présent dans le certificat consiste à limiter l’accès VPN ou réseau à certains appareils sans fil. Par exemple, votre stratégie d’accès peut autoriser connexion (ou l’accès à un réseau local virtuel différent) si OID 1.3.6.1.4.1.311.21.30 est présent dans le certificat. Cela vous permet de limiter l’accès aux périphériques dont EK TPM est présent dans la liste EKPUB.  
  
### <a name="BKMK_CAConfig"></a>Configuration de l’autorité de certification  
  
1.  **Programme d’installation EKCA et EKROOT des magasins de certificats sur une autorité de certification émettrice**  
  
    Si vous avez choisi **certificat de paire** pour les paramètres du modèle, effectuez les étapes de configuration suivantes:  
  
    1.  Utiliser Windows PowerShell pour créer deux magasins de nouveaux certificats sur le serveur d’autorité de certification qui effectue l’attestation de clé de module de plateforme sécurisée.  
  
    2.  Obtenez intermédiaires et les certificats d’autorité de certification de fabricants que vous voulez autoriser dans votre environnement d’entreprise racine. Ces certificats doivent être importées dans les magasins de certificats créé précédemment (EKCA et EKROOT) appropriées.  
  
    Le script Windows PowerShell suivant effectue ces deux étapes. Dans l’exemple suivant, le fabricant du module de plateforme sécurisée Fabrikam a fourni un certificat racine *FabrikamRoot.cer* et un certificat d’autorité de certification émettrice *Fabrikamca.cer*.  
  
    ```powershell  
    PS C:>\cd cert:  
    PS Cert:\>cd .\\LocalMachine  
    PS Cert:\LocalMachine> new-item EKROOT  
    PS Cert:\ LocalMachine> new-item EKCA  
    PS Cert:\EKCA\copy FabrikamCa.cer .\EKCA  
    PS Cert:\EKROOT\copy FabrikamRoot.cer .\EKROOT  
    ```  
  
2.  **Programme d’installation de la liste EKPUB si vous utilisez le type d’attestation EK**  
  
    Si vous avez choisi **paire de clés** dans les paramètres du modèle, les étapes de configuration pour créer et configurer un dossier sur l’autorité de certification émettrice, contenant les fichiers de 0octet, chacun nommé pour le hachage SHA-2 une EK autorisée. Ce dossier sert d’une «liste verte» des périphériques qui sont autorisés à obtenir des certificats d’attestation de clé de module de plateforme sécurisée. Étant donné que vous devez ajouter manuellement le EKPUB pour chaque périphérique qui requiert un certificat attestée, il fournit l’entreprise avec une garantie des périphériques qui sont autorisés à obtenir des certificats attestées clés de module de plateforme sécurisée. La configuration d’une autorité de certification pour ce mode requiert deux étapes:  
  
    1.  **Créer l’entrée de Registre EndorsementKeyListDirectories:** utiliser l’outil de ligne de commande Certutil pour configurer les emplacements de dossier où EKpubs approuvés sont définis comme décrit dans le tableau suivant.  
  
        |Opération|Syntaxe de commande|  
        |-------------|------------------|  
        |Ajouter des emplacements de dossier|Certutil.exe - setreg CA\EndorsementKeyListDirectories + «<folder>»|  
        |Supprimer des emplacements de dossier|Certutil.exe - setreg CA\EndorsementKeyListDirectories-«<folder>»|  
  
        Le EndorsementKeyListDirectories dans la commande certutil est un paramètre de Registre, comme décrit dans le tableau suivant.  
  
        |Nom de la valeur|Type|Données|  
        |--------------|--------|--------|  
        |EndorsementKeyListDirectories|REG_MULTI_SZ|< chemin d’accès LOCAL ou UNC au EKPUB autoriser listes ><br /><br />Exemple:<br /><br />*\\\blueCA.contoso.com\ekpub*<br /><br />*\\\bluecluster1.contoso.com\ekpub*<br /><br />D:\ekpub|  
  
        HKLM\SYSTEM\CurrentControlSet\Services\CertSvc\Configuration\\<CA Sanitized Name>  
  
        *EndorsementKeyListDirectories* contient une liste des chemins d’accès du système de fichiers local, ou UNC chacun pointant vers un dossier de l’autorité de certification a accès en lecture. Chaque dossier peut contenir zéro ou plus autoriser les entrées de liste, où chaque entrée est un fichier dont le nom est le SHA-2 hachage d’un EKpub approuvé, sans extension de fichier. 
        Création ou la modification de cette configuration de clé de Registre nécessite un redémarrage de l’autorité de certification, tout comme les paramètres de configuration du Registre autorité de certification existantes. Toutefois, les modifications apportées au paramètre de configuration prennent effet immédiatement et ne nécessitent pas l’autorité de certification à redémarrer.  
  
        > [!IMPORTANT]  
        > Sécurisez les dossiers dans la liste contre la falsification et tout accès non autorisé en configurant des autorisations afin que seuls les administrateurs autorisés ont lire et écrire l’accès. Le compte d’ordinateur de l’autorité de certification requiert un accès en lecture seule.  
  
    2.  **Remplir la liste EKPUB:** utiliser l’applet de commande Windows PowerShell suivante pour obtenir le hachage de clé publique de l’EK TPM à l’aide de Windows PowerShell sur chaque appareil et envoyer ce public de la clé de hachage pour l’autorité de certification et la stocker dans le dossier EKPubList.  
  
        ```powershell  
        PS C:>\$a=Get-TpmEndorsementKeyInfo -hashalgorithm sha256  
        PS C:>$b=new-item $a.PublickKeyHash -ItemType file  
        ```  
  
## <a name="troubleshooting"></a>Résolution des problèmes  
  
### <a name="key-attestation-fields-are-unavailable-on-a-certificate-template"></a>Champs de l’attestation de clé ne sont pas disponibles sur un modèle de certificat  
Les champs de l’Attestation de clé ne sont pas disponibles si les paramètres du modèle ne répondent pas à la configuration requise pour l’attestation. Les raisons courantes sont:  
  
1.  Les paramètres de compatibilité ne sont pas configurés correctement. Assurez-vous qu’elles sont configurées comme suit:  
  
    1.  **Autorité de certification**: **Windows Server2012R2**  
  
    2.  **Destinataire du certificat**: **Windows8.1 / Windows Server2012R2**  
  
2.  Les paramètres de chiffrement ne sont pas configurés correctement. Assurez-vous qu’elles sont configurées comme suit:  
  
    1.  **Catégorie de fournisseur**: **fournisseur de stockage de clé**  
  
    2.  **Nom de l’algorithme**: **RSA**  
  
    3.  **Fournisseurs**: **fournisseur de chiffrement de plateforme Microsoft**  
  
3.  Les paramètres de gestion demande ne sont pas configurés correctement. Assurez-vous qu’elles sont configurées comme suit:  
  
    1.  Le **autoriser à exporter la clé privée** option ne doit pas être sélectionnée.  
  
    2.  Le **archiver la clé privée de chiffrement du sujet** option ne doit pas être sélectionnée.  
  
### <a name="verification-of-tpm-device-for-attestation"></a>Vérification de l’appareil de module de plateforme sécurisée pour l’attestation  
Utilisez l’applet de commande Windows PowerShell, **confirmer-CAEndorsementKeyInfo**, pour vérifier qu’un périphérique spécifique de module de plateforme sécurisée est approuvé pour l’attestation par les autorités de certification. Il existe deux options: une pour la vérification de la EKCert et l’autre pour vérifier une EKPub. L’applet de commande soit exécuté localement sur une autorité de certification, ou sur les autorités de certification à distance à l’aide de la communication à distance de Windows PowerShell.  
  
1.  Pour la vérification de l’approbation sur un EKPub, effectuez les deux étapes suivantes:  
  
    1.  **Extraire le EKPub à partir de l’ordinateur client:** le EKPub peuvent être extraites à partir d’un ordinateur client via **Get-TpmEndorsementKeyInfo**. À partir d’une invite de commandes avec élévation de privilèges, exécutez la commande suivante:  
  
        ```  
        PS C:>\$a=Get-TpmEndorsementKeyInfo -hashalgorithm sha256  
        ```  
  
    2.  **Vérifier l’approbation sur un EKCert sur un ordinateur d’autorité de certification:** copier la chaîne extraite (le hachage SHA-2 de la EKPub) sur le serveur (par exemple, par e-mail) et le passer à la cmdlet Confirm-CAEndorsementKeyInfo. Notez que ce paramètre doit être de 64caractères.  
  
        ```  
        Confirm-CAEndorsementKeyInfo [-PublicKeyHash] <string>  
        ```  
  
2.  Pour la vérification de l’approbation sur un EKCert, effectuez les deux étapes suivantes:  
  
    1.  **Extraire le EKCert à partir de l’ordinateur client:** le EKCert peuvent être extraites à partir d’un ordinateur client via **Get-TpmEndorsementKeyInfo**. À partir d’une invite de commandes avec élévation de privilèges, exécutez la commande suivante:  
  
        ```  
        PS C:>\$a= Get- TpmEndorsementKeyInfo  
        PS C:>\$a.manufacturerCertificates|Export-Certificate c:\myEkcert.cer  
        ```  
  
    2.  **Vérifier l’approbation sur un KCert sur un ordinateur d’autorité de certification:** copier les extraits (EkCert.cer) EKCert sur l’autorité de certification (par exemple, par courrier électronique ou xcopy). Par exemple, si vous copiez le fichier de certificat le dossier «c:\diagnose» sur le serveur d’autorité de certification, exécutez la commande suivante pour terminer la vérification en deux étapes:  
  
        ```  
        PS C:>new-object System.Security.Cryptography.X509Certificates.X509Certificate2 "c:\diagnose\myEKcert.cer" | Confirm-CAEndorsementKeyInfo  
        ```  
  
## <a name="see-also"></a>Voir aussi  
[Présentation de la technologie Trusted Platform Module](https://technet.microsoft.com/library/jj131725.aspx)  
[Des ressources externes: Module de plateforme sécurisée](http://www.cs.unh.edu/~it666/reading_list/Hardware/tpm_fundamentals.pdf)  
  


