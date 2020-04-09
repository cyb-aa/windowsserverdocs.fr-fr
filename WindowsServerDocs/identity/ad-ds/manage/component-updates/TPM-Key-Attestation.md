---
ms.assetid: 16a344a9-f9a6-4ae2-9bea-c79a0075fd04
title: Attestation de clé TPM
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: de5a38ff6f811046d06c52a1ca4598f9650b3cfe
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80823012"
---
# <a name="tpm-key-attestation"></a>Attestation de clé TPM

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

**Auteur**: Justin Turner, ingénieur du support technique senior avec le groupe Windows  
  
> [!NOTE]  
> Ce contenu est écrit par un ingénieur du support client Microsoft et est destiné aux administrateurs expérimentés et aux architectes système qui recherchent des explications techniques plus approfondies des fonctionnalités et des solutions Windows Server 2012 R2 que n'en proposent généralement les rubriques de TechNet. Toutefois, il n'a pas subi les mêmes passes de correction. De ce fait, une partie du langage peut sembler moins finalisée que le contenu de TechNet.  
  
## <a name="overview"></a>Overview  
Bien que la prise en charge des clés protégées par le module de plateforme sécurisée ait existé depuis Windows 8, il n’existait aucun mécanisme permettant au chiffrement d’attester que la clé privée du demandeur de certificat est réellement protégée par une Module de plateforme sécurisée (TPM) (TPM). Cette mise à jour permet à une autorité de certification d’effectuer cette attestation et de refléter cette attestation dans le certificat émis.  
  
> [!NOTE]  
> Cet article suppose que le lecteur est familiarisé avec le concept de modèle de certificat (pour référence, consultez [modèles de certificats](https://technet.microsoft.com/library/cc730705.aspx)). Il suppose également que le lecteur est familiarisé avec la configuration des autorités de certification d’entreprise pour émettre des certificats basés sur des modèles de certificats (pour référence, consultez [liste de vérification : configurer des autorités de certification pour émettre et gérer des certificats](https://technet.microsoft.com/library/cc771533.aspx)).  
  
### <a name="terminology"></a>Terminology  
  
|Terme|Définition|  
|--------|--------------|  
|EK|Clé de type EK. Il s’agit d’une clé asymétrique contenue dans le module de plateforme sécurisée (injectée au moment de la fabrication). Le EK est unique pour chaque module de plateforme sécurisée et peut l’identifier. Impossible de modifier ou de supprimer EK.|  
|EKpub|Fait référence à la clé publique de EK.|  
|EKPriv|Fait référence à la clé privée de EK.|  
|EKCert|Certificat EK. Un certificat émis par un fabricant de module de plateforme sécurisée pour EKPub. Toutes les plateforme sécurisée n’ont pas de EKCert.|  
|TPM|Module de plateforme sécurisée (TPM). Un module de plateforme sécurisée (TPM) est conçu pour fournir des fonctions de sécurité basées sur le matériel. Une puce TPM est un processeur de chiffrement sécurisé conçu pour effectuer des opérations de chiffrement. La puce comprend plusieurs mécanismes de sécurité physique qui la protègent contre la falsification, et les logiciels malveillants ne peuvent pas falsifier les fonctions de sécurité du TPM.|  
  
### <a name="background"></a>Arrière-plan  
À partir de Windows 8, un Module de plateforme sécurisée (TPM) (TPM) peut être utilisé pour sécuriser la clé privée d’un certificat. Le fournisseur de stockage de clés (KSP) du fournisseur de chiffrement de plate-forme Microsoft Active cette fonctionnalité. Il y a deux problèmes avec l’implémentation :  

-   Il n’existait aucune garantie qu’une clé soit effectivement protégée par un module de plateforme sécurisée (une personne peut facilement usurper un KSP de logiciel en tant que KSP TPM avec des informations d’identification d’administrateur local).

-   Il n’était pas possible de limiter la liste des plateforme sécurisée autorisés à protéger les certificats émis par l’entreprise (dans le cas où l’administrateur de l’infrastructure à clé publique souhaite contrôler les types d’appareils qui peuvent être utilisés pour obtenir des certificats dans l’environnement).  

### <a name="tpm-key-attestation"></a>Attestation de clé TPM  
L’attestation de clé du module de plateforme sécurisée est la capacité de l’entité qui demande un certificat à prouver par chiffrement à une autorité de certification que la clé RSA de la demande de certificat est protégée par un module de plateforme sécurisée (« a ») ou « le » (« The ») approuvé par l’autorité de certification. Le modèle d’approbation TPM est abordé plus en détail dans la section [vue d’ensemble du déploiement](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_DeploymentOverview) , plus loin dans cette rubrique.  
  
### <a name="why-is-tpm-key-attestation-important"></a>Pourquoi l’attestation de clé du module de plateforme sécurisée est-elle importante ?  
Un certificat d’utilisateur avec une clé avec attestation TPM fournit une garantie de sécurité accrue, sauvegardée par une non-exportabilité, une détection et une isolation des clés fournies par le module de plateforme sécurisée.  
  
Avec l’attestation de clé TPM, un nouveau paradigme de gestion est désormais possible : un administrateur peut définir l’ensemble d’appareils que les utilisateurs peuvent utiliser pour accéder aux ressources de l’entreprise (par exemple, le VPN ou le point d’accès sans fil) **et garantir qu'** aucun autre appareil ne peut être utilisé pour y accéder. Ce nouveau paradigme de contrôle d’accès est **fort** , car il est lié à une identité d’utilisateur *liée au matériel* , ce qui est plus fort que les informations d’identification logicielles.
  
### <a name="how-does-tpm-key-attestation-work"></a>Comment fonctionne l’attestation de clé TPM ?  
En général, l’attestation de clé du module de plateforme sécurisée est basée sur les piliers suivants :  
  
1.  Chaque module de plateforme sécurisée est fourni avec une clé asymétrique unique, appelée clé de type *EK* (EK), gravée par le fabricant. Nous faisons référence à la partie publique de cette clé en tant que *EKPub* et à la clé privée associée en tant que *EKPriv*. Certaines puces TPM disposent également d’un certificat EK émis par le fabricant du EKPub. Nous faisons référence à ce certificat en tant que *EKCert*.  
  
2.  Une autorité de certification établit l’approbation dans le module de plateforme sécurisée via EKPub ou EKCert.  
  
3.  Un utilisateur prouve à l’autorité de certification que la clé RSA pour laquelle le certificat est demandé est liée par chiffrement à EKPub et que l’utilisateur possède la EKpriv.  
  
4.  L’autorité de certification émet un certificat avec un OID de stratégie d’émission spécial pour indiquer que la clé est maintenant sanctionnée comme protégée par un module de plateforme sécurisée (TPM).  
  
## <a name="deployment-overview"></a><a name="BKMK_DeploymentOverview"></a>Présentation du déploiement  
Dans ce déploiement, il est supposé qu’une autorité de certification Windows Server 2012 R2 Enterprise est configurée. En outre, les clients (Windows 8.1) sont configurés pour s’inscrire auprès de cette autorité de certification d’entreprise à l’aide de modèles de certificats. 

Le déploiement de l’attestation de clé TPM s’exécute en trois étapes :  
  
1.  **Planifier le modèle d’approbation TPM :** La première étape consiste à choisir le modèle d’approbation TPM à utiliser. Trois méthodes sont prises en charge pour cela :  
  
    -   **Approbation basée sur les informations d’identification de l’utilisateur :** L’autorité de certification d’entreprise approuve le EKPub fourni par l’utilisateur dans le cadre de la demande de certificat et aucune validation n’est effectuée à l’exception des informations d’identification de domaine de l’utilisateur.  
  
    -   **Approbation basée sur EKCert :** L’autorité de certification d’entreprise valide la chaîne EKCert fournie dans le cadre de la demande de certificat par une liste gérée par l’administrateur de *chaînes de certificats EK acceptables*. Les chaînes acceptables sont définies par fabricant et sont exprimées via deux magasins de certificats personnalisés sur l’autorité de certification émettrice (un magasin pour le intermédiaire et un pour les certificats d’autorité de certification racine). Ce mode d’approbation signifie que **tous les** plateforme sécurisée d’un fabricant donné sont approuvés. Notez que dans ce mode, plateforme sécurisée en cours d’utilisation dans l’environnement doit contenir EKCerts.
  
    -   **Approbation basée sur EKPub :** L’autorité de certification d’entreprise valide que les EKPub fournies dans le cadre de la demande de certificat s’affichent dans une liste gérée par l’administrateur des EKPubs autorisées. Cette liste est exprimée sous la forme d’un répertoire de fichiers dans lequel le nom de chaque fichier de ce répertoire correspond au hachage SHA-2 du EKPub autorisé. Cette option offre le niveau d’assurance le plus élevé, mais nécessite un effort administratif plus important, car chaque appareil est identifié individuellement. Dans ce modèle d’approbation, seuls les appareils dont le EKPub TPM a été ajouté à la liste autorisée de EKPubs sont autorisés à s’inscrire pour obtenir un certificat d’attestation TPM.  
  
    Selon la méthode utilisée, l’autorité de certification appliquera un autre OID de stratégie d’émission au certificat émis. Pour plus d’informations sur les OID de stratégie d’émission, voir le tableau des OID de stratégie d’émission dans la section [configurer un modèle de certificat](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_ConfigCertTemplate) dans cette rubrique.  
  
    Notez qu’il est possible de choisir une combinaison de modèles d’approbation TPM. Dans ce cas, l’autorité de certification accepte l’une des méthodes d’attestation et les OID de stratégie d’émission reflètent toutes les méthodes d’attestation qui ont été correctement exécutées.  
  
2.  **Configurez le modèle de certificat :** La configuration du modèle de certificat est décrite dans la section [Détails du déploiement](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_DeploymentDetails) de cette rubrique. Cet article ne traite pas de la façon dont ce modèle de certificat est attribué à l’autorité de certification d’entreprise ou de l’attribution de l’accès à un groupe d’utilisateurs. Pour plus d’informations, consultez [liste de vérification : configurer des autorités de certification pour émettre et gérer des certificats](https://technet.microsoft.com/library/cc771533.aspx).  
  
3.  **Configurer l’autorité de certification pour le modèle d’approbation TPM**  
  
    1.  **Approbation basée sur les informations d’identification de l’utilisateur :** Aucune configuration spécifique n’est requise.  
  
    2.  **Approbation basée sur EKCert :** L’administrateur doit obtenir les certificats de chaîne EKCert auprès des fabricants de module de plateforme sécurisée et les importer dans deux nouveaux magasins de certificats, créés par l’administrateur, sur l’autorité de certification qui effectue l’attestation de clé TPM. Pour plus d’informations, voir la section Configuration de l' [autorité de certification](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig) dans cette rubrique.  
  
    3.  **Approbation basée sur EKPub :** L’administrateur doit obtenir les EKPub pour chaque appareil qui aura besoin de certificats sanctionnés par un module de plateforme sécurisée et les ajoutera à la liste des EKPubs autorisées. Pour plus d’informations, voir la section Configuration de l' [autorité de certification](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig) dans cette rubrique.  
  
    > [!NOTE]  
    > -   Cette fonctionnalité nécessite Windows 8.1/Windows Server 2012 R2.  
    > -   L’attestation de clé TPM pour les KSP de carte à puce tierces n’est pas prise en charge. Le KSP du fournisseur de chiffrement de la plate-forme Microsoft doit être utilisé.  
    > -   L’attestation de clé du module de plateforme sécurisée fonctionne uniquement pour les clés RSA.  
    > -   L’attestation de clé du module de plateforme sécurisée n’est pas prise en charge pour une autorité de certification autonome.  
    > -   L’attestation de clé du module de plateforme sécurisée ne prend pas en charge [le traitement des certificats non persistants](https://technet.microsoft.com/library/ff934598).  
  
## <a name="deployment-details"></a><a name="BKMK_DeploymentDetails"></a>Détails du déploiement  
  
### <a name="configure-a-certificate-template"></a><a name="BKMK_ConfigCertTemplate"></a>Configurer un modèle de certificat  
Pour configurer le modèle de certificat pour l’attestation de clé TPM, effectuez les étapes de configuration suivantes :  
  
1.  Onglet **compatibilité**  
  
    Dans la section **paramètres de compatibilité** :  
  
    -   Vérifiez que **Windows Server 2012 R2** est sélectionné pour l' **autorité de certification**.  
  
    -   Vérifiez **Windows 8.1/Windows Server 2012 R2** est sélectionné pour le **destinataire du certificat**.  
  
    ![Attestation de clé TPM](media/TPM-Key-Attestation/GTR_ADDS_CompatibilityTab.gif)  
  
2.  Onglet **chiffrement**  
  
    Assurez-vous que **fournisseur de stockage de clés** est sélectionné pour la catégorie de **fournisseur** et que **RSA** est sélectionné pour le nom de l' **algorithme**. Vérifiez que les **requêtes doivent utiliser l’un des fournisseurs suivants** est sélectionné et que l’option **fournisseur de chiffrement de plateforme Microsoft** est sélectionnée sous **fournisseurs**.  
  
    ![Attestation de clé TPM](media/TPM-Key-Attestation/GTR_ADDS_CryptoTab.gif)  
  
3.  Onglet **attestation de clé**  
  
    Il s’agit d’un nouvel onglet pour Windows Server 2012 R2 :  
  
    ![Attestation de clé TPM](media/TPM-Key-Attestation/GTR_ADDS_ConfigCertTemplate.gif)  
  
    Choisissez un mode d’attestation parmi les trois options possibles.  
  
    ![Attestation de clé TPM](media/TPM-Key-Attestation/GTR_ADDS_KeyModes.gif)  
  
    -   **Aucun :** Implique que l’attestation de clé ne doit pas être utilisée  
  
    -   **Obligatoire, si le client est en capacité :** Permet aux utilisateurs sur un appareil qui ne prend pas en charge l’attestation de clé TPM de continuer à s’inscrire pour ce certificat. Les utilisateurs qui peuvent effectuer l’attestation seront distingués par un OID de stratégie d’émission spécial. Certains appareils peuvent ne pas être en mesure d’effectuer une attestation en raison d’un ancien module de plateforme sécurisée qui ne prend pas en charge l’attestation de clé, ou l’appareil n’a pas de module de plateforme sécurisée.
  
    -   **Obligatoire :** Le client *doit* effectuer l’attestation de clé TPM. sinon, la demande de certificat échoue.  
  
    Choisissez ensuite le modèle d’approbation TPM. Trois options s’imposent :  
  
    ![Attestation de clé TPM](media/TPM-Key-Attestation/GTR_ADDS_KeyTypeToEnforce.gif)  
  
    -   **Informations d’identification de l’utilisateur :** Autorisez un utilisateur à authentifier ses données pour un module de plateforme sécurisée valide en spécifiant leurs informations d’identification de domaine.  
  
    -   **Approbation du certificat :** Le EKCert de l’appareil doit valider les certificats de l’autorité de certification intermédiaire du module de plateforme sécurisée gérés par l’administrateur vers un certificat d’autorité de certification racine géré par l’administrateur. Si vous choisissez cette option, vous devez configurer des magasins de certificats EKCA et EKRoot sur l’autorité de certification émettrice, comme décrit dans la section Configuration de l' [autorité de certification](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig) de cette rubrique.  
  
    -   **Clé de type EK :** Le EKPub de l’appareil doit figurer dans la liste gérée par l’administrateur de l’infrastructure à clé publique. Cette option offre le niveau d’assurance le plus élevé, mais nécessite un effort administratif plus important. Si vous choisissez cette option, vous devez configurer une liste EKPub sur l’autorité de certification émettrice comme décrit dans la section Configuration de l' [autorité de certification](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig) de cette rubrique.  
  
    Enfin, choisissez la stratégie d’émission à afficher dans le certificat émis. Par défaut, chaque type de contrainte est associé à un identificateur d’objet (OID) qui sera inséré dans le certificat s’il passe ce type d’application, comme décrit dans le tableau suivant. Notez qu’il est possible de choisir une combinaison de méthodes de mise en application. Dans ce cas, l’autorité de certification acceptera l’une des méthodes d’attestation et l’OID de la stratégie d’émission reflétera toutes les méthodes d’attestation qui ont réussi.  
  
    **OID de stratégie d’émission**  
  
    |OID|Type d’attestation de clé|Description|Niveau d’assurance|  
    |-------|------------------------|---------------|-------------------|  
    |1.3.6.1.4.1.311.21.30|EK|« EK vérifié » : pour la liste gérée par l’administrateur de EK|Élevée|  
    |1.3.6.1.4.1.311.21.31|Endosser le certificat|« Certificat EK vérifié » : lorsque la chaîne de certificats EK est validée|Moyenne|  
    |1.3.6.1.4.1.311.21.32|Informations d’identification utilisateur|« EK Trusted on use » : pour les EK avec attestation utilisateur|Basse|  
  
    Les OID sont insérés dans le certificat émis si l’option **inclure les stratégies d’émission** est sélectionnée (configuration par défaut).  
  
    ![Attestation de clé TPM](media/TPM-Key-Attestation/GTR_ADDS_IssuancePolicies.gif)  
  
    > [!TIP]  
    > L’utilisation potentielle de l’OID dans le certificat consiste à limiter l’accès aux réseaux VPN ou sans fil à certains appareils. Par exemple, votre stratégie d’accès peut autoriser la connexion (ou l’accès à un autre réseau local virtuel) si l’OID 1.3.6.1.4.1.311.21.30 est présent dans le certificat. Cela vous permet de limiter l’accès aux appareils dont le TPM EK est présent dans la liste EKPUB.  
  
### <a name="ca-configuration"></a><a name="BKMK_CAConfig"></a>Configuration de l’autorité de certification  
  
1.  **Configurer des magasins de certificats EKCA et EKROOT sur une autorité de certification émettrice**  
  
    Si vous avez choisi **endosser le certificat** pour les paramètres du modèle, effectuez les étapes de configuration suivantes :  
  
    1.  Utilisez Windows PowerShell pour créer deux nouveaux magasins de certificats sur le serveur de l’autorité de certification (CA) qui effectuera l’attestation de clé TPM.  
  
    2.  Procurez-vous le ou les certificats d’autorité de certification racine et intermédiaire du ou des fabricants que vous souhaitez autoriser dans votre environnement d’entreprise. Ces certificats doivent être importés dans les magasins de certificats créés précédemment (EKCA et EKROOT), le cas échéant.  
  
    Le script Windows PowerShell suivant effectue ces deux étapes. Dans l’exemple suivant, le fabricant du module de plateforme sécurisée Fabrikam a fourni un certificat racine *FabrikamRoot. cer* et un certificat d’autorité de certification émettrice *Fabrikamca. cer*.  
  
    ```powershell  
    PS C:>\cd cert:  
    PS Cert:\>cd .\\LocalMachine  
    PS Cert:\LocalMachine> new-item EKROOT  
    PS Cert:\ LocalMachine> new-item EKCA  
    PS Cert:\EKCA\copy FabrikamCa.cer .\EKCA  
    PS Cert:\EKROOT\copy FabrikamRoot.cer .\EKROOT  
    ```  
  
2.  **Configurer la liste EKPUB en cas d’utilisation d’un type d’attestation EK**  
  
    Si vous avez choisi la clé de type **EK** dans les paramètres du modèle, les étapes de configuration suivantes sont la création et la configuration d’un dossier sur l’autorité de certification émettrice, contenant des fichiers de 0 octet, chacun nommé pour le hachage SHA-2 d’une EK autorisée. Ce dossier sert de « liste verte » de périphériques autorisés à obtenir des certificats attestés par clé TPM. Étant donné que vous devez ajouter manuellement les EKPUB pour chaque appareil qui requiert un certificat sanctionné, il fournit à l’entreprise une garantie des appareils autorisés à obtenir des certificats attestés de clé TPM. La configuration d’une autorité de certification pour ce mode requiert deux étapes :  
  
    1.  **Créez l’entrée de Registre EndorsementKeyListDirectories :** Utilisez l’outil en ligne de commande certutil pour configurer les emplacements de dossier où les EKpubs approuvés sont définis comme décrit dans le tableau suivant.  
  
        |Opération|Syntaxe de la commande|  
        |-------------|------------------|  
        |Ajouter des emplacements de dossier|certutil. exe-setreg CA\EndorsementKeyListDirectories + «<folder>»|  
        |Supprimer les emplacements de dossier|certutil. exe-setreg CA\EndorsementKeyListDirectories-«<folder>»|  
  
        La commande EndorsementKeyListDirectories dans certutil est un paramètre du Registre, comme décrit dans le tableau suivant.  
  
        |Nom de valeur|Type|Données|  
        |--------------|--------|--------|  
        |EndorsementKeyListDirectories|REG_MULTI_SZ|< chemin d’accès LOCAL ou UNC à la ou les listes d’autorisation EKPUB ><p>Exemple :<p>*\\\blueCA.contoso.com\ekpub*<p>*\\\bluecluster1.contoso.com\ekpub*<p>D:\ekpub|  
  
        HKLM\SYSTEM\CurrentControlSet\Services\CertSvc\Configuration\\<CA Sanitized Name>  
  
        *EndorsementKeyListDirectories* contient une liste de chemins de système de fichiers UNC ou locaux, chacun pointant vers un dossier auquel l’autorité de certification a accès en lecture. Chaque dossier peut contenir zéro, une ou plusieurs entrées de liste verte, où chaque entrée est un fichier avec un nom qui est le hachage SHA-2 d’un EKpub approuvé, sans extension de fichier. 
        La création ou la modification de cette configuration de clé de Registre nécessite un redémarrage de l’autorité de certification, tout comme les paramètres de configuration du registre de l’autorité de certification existants. Toutefois, les modifications apportées au paramètre de configuration prennent effet immédiatement et ne nécessitent pas le redémarrage de l’autorité de certification.  
  
        > [!IMPORTANT]  
        > Sécurisez les dossiers de la liste contre tout accès non autorisé en configurant des autorisations afin que seuls les administrateurs autorisés disposent d’un accès en lecture et en écriture. Le compte d’ordinateur de l’autorité de certification requiert un accès en lecture seule.  
  
    2.  **Renseignez la liste EKPUB :** Utilisez l’applet de commande Windows PowerShell suivante pour obtenir le hachage de clé publique du module TPM EK à l’aide de Windows PowerShell sur chaque appareil, puis envoyez ce hachage de clé publique à l’autorité de certification et stockez-la dans le dossier EKPubList.  
  
        ```powershell  
        PS C:>\$a=Get-TpmEndorsementKeyInfo -hashalgorithm sha256  
        PS C:>$b=new-item $a.PublicKeyHash -ItemType file  
        ```  
  
## <a name="troubleshooting"></a>Résolution des problèmes  
  
### <a name="key-attestation-fields-are-unavailable-on-a-certificate-template"></a>Les champs d’attestation de clé ne sont pas disponibles sur un modèle de certificat  
Les champs d’attestation de clé ne sont pas disponibles si les paramètres de modèle ne satisfont pas à la configuration requise pour l’attestation. Les raisons les plus courantes sont les suivantes :  
  
1.  Les paramètres de compatibilité ne sont pas configurés correctement. Assurez-vous qu’ils sont configurés comme suit :  
  
    1.  **Autorité de certification**: **Windows Server 2012 R2**  
  
    2.  **Destinataire du certificat**: **Windows 8.1/Windows Server 2012 R2**  
  
2.  Les paramètres de chiffrement ne sont pas configurés correctement. Assurez-vous qu’ils sont configurés comme suit :  
  
    1.  **Catégorie de fournisseur**: **fournisseur de stockage de clés**  
  
    2.  **Nom de l’algorithme**: **RSA**  
  
    3.  **Fournisseurs**: **fournisseur de chiffrement de plateforme Microsoft**  
  
3.  Les paramètres de traitement de la demande ne sont pas configurés correctement. Assurez-vous qu’ils sont configurés comme suit :  
  
    1.  L’option **autoriser l’exportation de la clé privée** ne doit pas être sélectionnée.  
  
    2.  L’option **Archiver la clé privée de chiffrement du sujet** ne doit pas être sélectionnée.  
  
### <a name="verification-of-tpm-device-for-attestation"></a>Vérification du périphérique TPM pour l’attestation  
Utilisez l’applet de commande Windows PowerShell, **Confirm-CAEndorsementKeyInfo**, pour vérifier qu’un appareil TPM spécifique est approuvé pour l’attestation par autorité de certification. Il existe deux options : une pour vérifier le EKCert et l’autre pour vérifier un EKPub. L’applet de commande est exécutée localement sur une autorité de certification ou sur les autorités de certification distantes à l’aide de la communication à distance Windows PowerShell.  
  
1.  Pour vérifier l’approbation sur un EKPub, effectuez les deux étapes suivantes :  
  
    1.  **Extrayez le EKPub de l’ordinateur client :** Les EKPub peuvent être extraits à partir d’un ordinateur client via la **TpmEndorsementKeyInfo**. À partir d’une invite de commandes avec élévation de privilèges, exécutez la commande suivante :  
  
        ```  
        PS C:>\$a=Get-TpmEndorsementKeyInfo -hashalgorithm sha256  
        ```  
  
    2.  **Vérifier l’approbation sur un EKCert sur un ordinateur de l’autorité de certification :** Copiez la chaîne extraite (le hachage SHA-2 du EKPub) sur le serveur (par exemple, par e-mail) et transmettez-la à l’applet de commande Confirm-CAEndorsementKeyInfo. Notez que ce paramètre doit être de 64 caractères.  
  
        ```  
        Confirm-CAEndorsementKeyInfo [-PublicKeyHash] <string>  
        ```  
  
2.  Pour vérifier l’approbation sur un EKCert, effectuez les deux étapes suivantes :  
  
    1.  **Extrayez le EKCert de l’ordinateur client :** Les EKCert peuvent être extraits à partir d’un ordinateur client via la **TpmEndorsementKeyInfo**. À partir d’une invite de commandes avec élévation de privilèges, exécutez la commande suivante :  
  
        ```  
        PS C:>\$a=Get-TpmEndorsementKeyInfo
        PS C:>\$a.manufacturerCertificates|Export-Certificate -filepath c:\myEkcert.cer
        ```  
  
    2.  **Vérifier l’approbation sur un EKCert sur un ordinateur de l’autorité de certification :** Copiez le EKCert extrait (EkCert. cer) vers l’autorité de certification (par exemple, par e-mail ou Xcopy). Par exemple, si vous copiez le fichier de certificat dans le dossier « c:\diagnose » sur le serveur de l’autorité de certification, exécutez la commande suivante pour terminer la vérification :  
  
        ```  
        PS C:>new-object System.Security.Cryptography.X509Certificates.X509Certificate2 "c:\diagnose\myEKcert.cer" | Confirm-CAEndorsementKeyInfo  
        ```  
  
## <a name="see-also"></a>Voir aussi  
[Présentation de la technologie Module de plateforme sécurisée (TPM)](https://technet.microsoft.com/library/jj131725.aspx)  
[Ressource externe : Module de plateforme sécurisée (TPM)](http://www.cs.unh.edu/~it666/reading_list/Hardware/tpm_fundamentals.pdf)  
