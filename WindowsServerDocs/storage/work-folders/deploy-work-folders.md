---
ms.assetid: d2429185-9720-4a04-ad94-e89a9350cdba
title: Déploiement de Dossiers de travail
ms.prod: windows-server-threshold
ms.technology: storage-work-folders
ms.topic: article
author: JasonGerend
manager: dongill
ms.author: jgerend
ms.date: 6/24/2017
description: Comment déployer Dossiers de travail, y compris l’installation du rôle de serveur, la création des partages de synchronisation et la création d’enregistrements DNS.
ms.openlocfilehash: 1f7a0aa0b7e08a1dd444cd6b488a1ced6ee3d9d7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812540"
---
# <a name="deploying-work-folders"></a>Déploiement de Dossiers de travail

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows 10, Windows 8.1, Windows 7

Cette rubrique présente les étapes requises pour déployer Dossiers de travail. Elle part du principe que vous avez déjà lu [Planification d’un déploiement de Dossiers de travail](plan-work-folders.md).  
  
 Pour déployer Dossiers de travail, un processus qui peut impliquer plusieurs serveurs et technologies, procédez comme suit.  
  
> [!TIP]
>  Le déploiement de Dossiers de travail le plus simple est un serveur de fichiers unique (souvent appelé serveur de synchronisation) sans prise en charge de la synchronisation via Internet. Ce déploiement peut s'avérer utile pour un laboratoire de test ou en tant que solution de synchronisation pour les ordinateurs clients appartenant à un domaine. Pour créer un déploiement simple, voici la procédure minimale à suivre : 
>  -   Étape 1 : obtenir des certificats SSL  
>  -   Étape 2 : créer des enregistrements DNS 
>  -   Étape 3 : installer Dossiers de travail sur des serveurs de fichiers  
>  -   Étape 4 : lier le certificat SSL aux serveurs de synchronisation
>  -   Étape 5 : créer des groupes de sécurité pour Dossiers de travail  
>  -   Étape 7 : créer des partages de synchronisation pour les données utilisateur  
  
## <a name="step-1-obtain-ssl-certificates"></a>Étape 1 : obtenir des certificats SSL  
 Dossiers de travail utilise le protocole HTTPS pour synchroniser en toute sécurité des fichiers entre les clients Dossiers de travail et le serveur Dossiers de travail. Les critères requis pour les certificats SSL utilisés par Dossiers de travail sont les suivants :  
  
-   Le certificat doit être délivré par une autorité de certification approuvée. Pour la plupart des implémentations de Dossiers de travail, une autorité de certification publiquement approuvée est conseillée dans la mesure où les certificats seront utilisés par des appareils basés sur Internet et n’appartenant pas à un domaine.  
  
-   Le certificat doit être valide.  
  
-   La clé privée du certificat doit pouvoir être exportée (car vous devrez installer le certificat sur plusieurs serveurs).  
  
-   Le nom du sujet du certificat doit contenir l’URL de Dossiers de travail publique utilisée pour la détection du service Dossiers de travail depuis Internet (elle doit se présenter au format `workfolders.`*<nom_domaine>*).  
  
-   Les autres noms de sujet doivent être présents sur le certificat qui répertorie le nom de chaque serveur de synchronisation utilisé.

 Le blog Gestion des certificats de Dossiers de travail [blog](https://blogs.technet.microsoft.com/filecab/2013/08/09/work-folders-certificate-management/) fournit des informations supplémentaires sur l’utilisation de certificats avec Dossiers de travail.
  
## <a name="step-2-create-dns-records"></a>Étape 2 : créer des enregistrements DNS  
 Pour permettre aux utilisateurs d’effectuer la synchronisation sur Internet, vous devez créer un enregistrement d’hôte (A) dans le DNS public pour autoriser les clients Internet à résoudre votre URL de Dossiers de travail. Cet enregistrement DNS doit être résolu en interface externe du serveur proxy inverse.  
  
 Sur votre réseau interne, créez un enregistrement CNAME dans DNS nommé « dossiersTravail », qui résout les noms de domaines complets d’un serveur Dossiers de travail. Lorsque les clients de dossiers de travail utilisent la détection automatique, l’URL utilisée pour détecter le serveur dossiers de travail est https://workfolders.domain.com. Si vous prévoyez d’utiliser la détection automatique, l’enregistrement CNAME dossiersTravail doit exister dans DNS.  
  
## <a name="step-3-install-work-folders-on-file-servers"></a>Étape 3 : installer Dossiers de travail sur des serveurs de fichiers  
 Vous pouvez installer Dossiers de travail sur un serveur appartenant à un domaine en utilisant le Gestionnaire de serveur ou Windows PowerShell, en local ou à distance sur un réseau. Cela s’avère utile si vous configurez plusieurs serveurs de synchronisation sur votre réseau.  
  
Pour déployer le rôle dans le Gestionnaire de serveur, procédez comme suit :  
  
1.  Démarrez l’**Assistant Ajout de rôles et de fonctionnalités**.  
  
2.  Dans la page **Sélectionner le type d’installation**, choisissez **Installation basée sur un rôle ou une fonctionnalité**.  
  
3.  Dans la page **Sélectionner le serveur de destination**, sélectionnez le serveur sur lequel vous voulez installer Dossiers de travail.  
  
4.  Dans la page **Sélectionner des rôles de serveurs**, développez **Services de fichiers et de stockage**, **Services de fichiers et iSCSI**, puis sélectionnez **Dossiers de travail**.  
  
5.  Lorsque le système vous demande si vous voulez installer **IIS Hostable Web Core**, cliquez sur **OK** pour installer la version minimale des services Internet (IIS) requise par Dossiers de travail.  
  
6.  Cliquez sur **Suivant** jusqu’à ce que vous ayez terminé l’Assistant.

Pour déployer le rôle à l’aide de Windows PowerShell, utilisez l’applet de commande suivante :
  
```powershell  
Add-WindowsFeature FS-SyncShareService  
```

## <a name="step-4-binding-the-ssl-certificate-on-the-sync-servers"></a>Étape 4 : lier le certificat SSL aux serveurs de synchronisation
 La fonctionnalité Dossiers de travail installe IIS Hostable Web Core, qui est un composant IIS conçu pour activer les services Web sans nécessiter une installation complète des services Internet (IIS). Après l’installation d’IIS Hostable Web Core, vous devez lier le certificat SSL pour le serveur au site Web par défaut sur le serveur de fichiers. Toutefois, IIS Hostable Web Core n’installe pas la Console de gestion IIS.

 Il existe deux possibilités pour lier le certificat à l’interface Web par défaut. Vous devez dans les deux cas avoir installé la clé privée pour le certificat dans le magasin personnel de l’ordinateur.

- Utilisez la Console de gestion IIS sur un serveur sur lequel elle est installée. À partir de la Console, connectez-vous au serveur de fichiers à gérer, puis sélectionnez le site Web par défaut pour ce serveur. Le site Web par défaut apparaît désactivé, mais vous pouvez quand même modifier les liaisons pour le site et sélectionner le certificat pour le lier à ce site Web.

- Utilisez la commande netsh pour lier le certificat à l’interface https du site Web par défaut. La commande se présente comme suit :

    ```
    netsh http add sslcert ipport=<IP address>:443 certhash=<Cert thumbprint> appid={CE66697B-3AA0-49D1-BDBD-A25C8359FD5D} certstorename=MY
    ```

## <a name="step-5-create-security-groups-for-work-folders"></a>Étape 5 : créer des groupes de sécurité pour Dossiers de travail
 Avant de créer des partages de synchronisation, un membre du groupe Admins du domaine ou Administrateurs de l’entreprise doit créer quelques groupes de sécurité dans les services de domaine Active Directory (AD DS) pour Dossiers de travail (il peut également déléguer une partie du contrôle comme décrit à l’étape 6). Voici les groupes dont vous avez besoin :

- Un groupe par partage de synchronisation pour indiquer les utilisateurs qui sont autorisés à effectuer la synchronisation avec le partage de synchronisation

- Un groupe pour tous les administrateurs de Dossiers de travail afin qu’ils puissent modifier un attribut sur chaque objet utilisateur qui lie l’utilisateur au serveur de synchronisation approprié (si vous comptez utiliser plusieurs serveurs de synchronisation)

 Les groupes doivent suivre une convention d’affectation de noms standard et ne doivent être utilisés que pour Dossiers de travail afin d’éviter des conflits éventuels avec d’autres exigences de sécurité.

 Pour créer les groupes de sécurité appropriés, utilisez la procédure suivante plusieurs fois, une fois pour chaque partage de synchronisation et une fois pour créer éventuellement un groupe pour les administrateurs des serveurs de fichiers.

#### <a name="to-create-security-groups-for-work-folders"></a>Pour créer des groupes de sécurité pour Dossiers de travail

1. Ouvrez le Gestionnaire de serveur sur un ordinateur Windows Server 2012 R2 ou Windows Server 2016 sur lequel le Centre d'administration Active Directory est installé.

2.  Dans le menu **Outils** , cliquez sur **Centre d’administration Active Directory**. Le Centre d’administration Active Directory s’affiche.

3.  Cliquez avec le bouton droit sur le conteneur où vous voulez créer le groupe (par exemple, le conteneur Utilisateurs de l’unité d’organisation ou du domaine approprié), cliquez sur **Nouveau**, puis sur **Groupe**.

4.  Dans la fenêtre **Créer un groupe**, dans la section **Groupe**, indiquez les paramètres suivants :

    -   Dans **Nom du groupe**, tapez le nom du groupe de sécurité, par exemple : **Utilisateurs Partage de synchronisation RH**, ou **Administrateurs de Dossiers de travail**.  
  
    -   Dans **Étendue du groupe**, cliquez sur **Sécurité**, puis sur **Globale**.  
  
5.  Dans la section **Membres**, cliquez sur **Ajouter**. La boîte de dialogue Sélectionnez Utilisateurs, Contacts, Ordinateurs ou Groupes s’affiche.  
  
6.  Tapez les noms des utilisateurs ou des groupes auxquels vous accordez l’accès à un partage de synchronisation spécifique (si vous créez un groupe pour contrôler l’accès à un partage de synchronisation) ou tapez les noms des administrateurs de Dossiers de travail (si vous comptez configurer des comptes d’utilisateurs pour détecter automatiquement le serveur de synchronisation approprié), cliquez sur **OK**, puis une nouvelle fois sur **OK**.

Pour créer un groupe de sécurité à l’aide de Windows PowerShell, utilisez les applets de commande suivantes :

```powershell  
$GroupName = "Work Folders Administrators"  
$DC = "DC1.contoso.com"  
$ADGroupPath = "CN=Users,DC=contoso,DC=com"  
$Members = "CN=Maya Bender,CN=Users,DC=contoso,DC=com","CN=Irwin Hume,CN=Users,DC=contoso,DC=com"  
  
New-ADGroup -GroupCategory:"Security" -GroupScope:"Global" -Name:$GroupName -Path:$ADGroupPath -SamAccountName:$GroupName -Server:$DC  
Set-ADGroup -Add:@{'Member'=$Members} -Identity:$GroupName -Server:$DC
```

## <a name="step-6-optionally-delegate-user-attribute-control-to-work-folders-administrators"></a>Étape 6 : déléguer éventuellement le contrôle des attributs utilisateur aux administrateurs de Dossiers de travail  
 Si vous déployez plusieurs serveurs de synchronisation et voulez diriger automatiquement les utilisateurs vers le serveur de synchronisation approprié, vous devez mettre à jour un attribut sur chaque compte d’utilisateur dans les services de domaine Active Directory. Toutefois, cette opération exige normalement de demander à un membre du groupe Admins du domaine ou Administrateurs de l’entreprise de mettre à jour les attributs, ce qui peut devenir rapidement pénible si vous devez fréquemment ajouter des utilisateurs ou les déplacer entre les serveurs de synchronisation.  
  
 Pour cette raison, un membre du groupe Admins du domaine ou Administrateurs de l’entreprise peut vouloir déléguer la capacité à modifier la propriété msDS-SyncServerURL des objets utilisateur au groupe Administrateurs de Dossiers de travail que vous avez créé à l’étape 5, comme décrit dans la procédure suivante.  
  
#### <a name="delegate-the-ability-to-edit-the-msds-syncserverurl-property-on-user-objects-in-ad-ds"></a>Déléguer la possibilité de modifier la propriété msDS-SyncServerURL sur des objets utilisateur dans les services de domaine Active Directory  
  
1.  Ouvrez le Gestionnaire de serveur sur un ordinateur Windows Server 2012 R2 ou Windows Server 2016 sur lequel le composant Utilisateurs et ordinateurs Active Directory est installé.  
  
2.  Dans le menu **Outils**, cliquez sur **Utilisateurs et ordinateurs Active Directory**. Le composant Utilisateurs et ordinateurs Active Directory s’affiche.  
  
3.  Cliquez avec le bouton droit sur l’unité d’organisation sous laquelle figurent tous les objets utilisateur pour Dossiers de travail (si les utilisateurs sont stockés dans plusieurs unités d’organisation ou domaines, cliquez avec le bouton droit sur le conteneur commun à tous les utilisateurs), puis cliquez sur **Délégation de contrôle…**. L’Assistant Délégation de contrôle s’affiche.  
  
4.  Dans la page **Utilisateurs ou groupes**, cliquez sur **Ajouter...** Puis, spécifiez le groupe que vous avez créé pour les administrateurs de Dossiers de travail (par exemple, **Administrateurs de Dossiers de travail**).  
  
5.  Dans la page **Tâches à déléguer**, cliquez sur **Créer une tâche personnalisée à déléguer**.  
  
6.  Dans la page **Type d’objet Active Directory**, cliquez sur **Seulement des objets suivants dans le dossier**, puis activez la case à cocher **Objets utilisateur**.  
  
7.  Dans la page **Autorisations**, désactivez la case à cocher **Général**, activez la case à cocher **Spécifiques aux propriétés**, puis les cases à cocher **Read msDS-SyncServerUrl** et **Write msDS-SyncServerUrl**.

Pour déléguer la capacité à modifier la propriété msDS-SyncServerURL sur des objets utilisateur à l’aide de Windows PowerShell, utilisez l’exemple de script suivant qui emploie la commande DsAcls.
  
```powershell  
$GroupName = "Contoso\Work Folders Administrators"  
$ADGroupPath = "CN=Users,dc=contoso,dc=com"  
  
DsAcls $ADGroupPath /I:S /G ""$GroupName":RPWP;msDS-SyncServerUrl;user"  
```  
  
> [!NOTE]
>  L’exécution de l’opération de délégation peut prendre un peu de temps dans les domaines avec un grand nombre d’utilisateurs.  
  
## <a name="step-7-create-sync-shares-for-user-data"></a>Étape 7 : créer des partages de synchronisation pour les données utilisateur  
 À ce stade, vous êtes prêt à désigner un dossier sur le serveur de synchronisation pour stocker les fichiers des utilisateurs. Ce dossier est appelé partage de synchronisation et vous pouvez procéder comme suit pour en créer un.  
  
1.  Si vous ne disposez pas déjà d’un volume NTFS avec de l’espace libre pour le partage de synchronisation et les fichiers utilisateur qu’il contiendra, créez un volume et formatez-le avec le système de fichiers NTFS.  
  
2.  Dans le Gestionnaire de serveur, cliquez sur **Services de fichiers et de stockage**, puis sur **Dossiers de travail**.  
  
3.  Une liste de tous les partages de synchronisation existants est visible en haut du volet d’informations. Pour créer un partage de synchronisation, dans le menu **Tâches**, choisissez **Nouveau partage de synchronisation…**. L’Assistant Nouveau partage de synchronisation s’affiche.  
  
4.  Dans la page **Sélectionner le serveur et le chemin d’accès**, indiquez l’emplacement où stocker le partage de synchronisation. Si vous disposez déjà d’un partage de fichiers créé pour ces données utilisateur, vous pouvez choisir ce partage. Vous pouvez aussi créer un dossier.  
  
    > [!NOTE]
    >  Par défaut, les partages de synchronisation ne sont pas directement accessibles via un partage de fichiers (sauf si vous choisissez un partage de fichiers existant). Si vous voulez rendre un partage de synchronisation accessible via un partage de fichiers, utilisez la vignette **Partages** du Gestionnaire de serveur ou l’applet de commande [New-SmbShare](https://technet.microsoft.com/library/jj635722.aspx) pour créer un partage de fichiers, de préférence avec l’énumération basée sur l’accès activée.  
  
5.  Dans la page **Spécifier la structure des dossiers utilisateur**, choisissez une convention d’affectation de noms pour les dossiers utilisateur dans le partage de synchronisation. Deux options sont disponibles :  
  
    -   **alias utilisateur** crée des dossiers utilisateur sans nom de domaine. Si vous utilisez un partage de fichiers qui est déjà utilisé avec la redirection de dossiers ou une autre solution de données utilisateur, sélectionnez cette convention d’affectation de noms. Vous pouvez éventuellement activer la case à cocher **Synchroniser uniquement le sous-dossier suivant** pour synchroniser uniquement un sous-dossier spécifique, par exemple le dossier Documents.  
  
    -   **alias@domain utilisateur** crée des dossiers utilisateur avec un nom de domaine. Si vous n’utilisez pas un partage de fichiers qui est déjà utilisé avec la redirection de dossiers ou une autre solution de données utilisateur, sélectionnez cette convention d’affectation de noms pour éliminer les conflits entre noms de dossier lorsque plusieurs utilisateurs du partage ont des alias identiques (ce qui peut se produire si les utilisateurs appartiennent à des domaines différents).  
  
6.  Dans la page **Entrer le nom du partage de synchronisation**, fournissez un nom et une description pour le partage de synchronisation. Ces informations ne sont pas publiées sur le réseau, mais sont visibles dans le Gestionnaire de serveur et Windows PowerShell pour mieux différencier chacun des partages de synchronisation.  
  
7.  Dans la page **Accorder aux groupes l’accès à la synchronisation**, indiquez le groupe que vous avez créé et qui répertorie les utilisateurs autorisés à employer ce partage de synchronisation.  
  
    > [!IMPORTANT]
    >  Pour améliorer les performances et renforcer la sécurité, accordez l’accès à des groupes et non à des utilisateurs individuels et soyez le plus spécifique possible, en évitant des groupes génériques comme Utilisateurs authentifiés et Utilisateurs du domaine. En accordant l’accès à des groupes avec un grand nombre d’utilisateurs, vous augmentez le temps nécessaire à Dossiers de travail pour interroger les services de domaine Active Directory. Si vous disposez d’un grand nombre d’utilisateurs, créez plusieurs partages de synchronisation pour répartir la charge.  
  
8.  Dans la page **Spécifier la stratégie de périphériques**, indiquez s’il convient de demander des restrictions de sécurité sur les PC et appareils clients. Deux stratégies de périphériques peuvent être sélectionnées individuellement :  
  
    -   **Chiffrer Dossiers de travail** : demande le chiffrement de Dossiers de travail sur les PC et appareils clients  
  
    -   **Verrouiller automatiquement l’écran et exiger un mot de passe** : demande le verrouillage automatique des écrans des PC et appareils clients après 15 minutes, un mot de passe de six caractères ou plus pour déverrouiller l’écran et l’activation du mode de verrouillage d’un appareil après 10 tentatives échouées  
  
        > [!IMPORTANT]
        >  Pour appliquer les stratégies de mot de passe pour les PC Windows 7 et pour les personnes autres que les administrateurs sur les PC appartenant à un domaine, utilisez les stratégies de mot de passe de stratégie de groupe pour les domaines des ordinateurs et excluez ces domaines des stratégies de mot de passe de Dossiers de travail. Vous pouvez exclure les domaines en utilisant l’applet de commande [Set-Syncshare -PasswordAutoExcludeDomain](https://technet.microsoft.com/library/dn296649\(v=wps.630\).aspx) après la création du partage de synchronisation. Pour plus d'informations sur la définition des stratégies de mot de passe de stratégie de groupe, voir [Stratégie de mot de passe](https://technet.microsoft.com/library/hh994572(v=ws.11).aspx).  
  
9. Passez en revue vos sélections et terminez l’exécution de l’Assistant pour créer le partage de synchronisation.

Vous pouvez créer des partages de synchronisation avec Windows PowerShell en utilisant l’applet de commande [New-SyncShare](https://technet.microsoft.com/library/dn296635.aspx). Voici un exemple de cette méthode :  
  
```powershell  
New-SyncShare "HR Sync Share" K:\Share-1 –User "HR Sync Share Users"  
```  
  
L’exemple ci-dessus crée un partage de synchronisation nommé *Partage01* avec le chemin d’accès *K:\Partage-1* et un accès accordé au groupe nommé *Utilisateurs Partage de synchronisation RH*  
  
> [!TIP]
>  Après avoir créé des partages de synchronisation, vous pouvez utiliser la fonctionnalité Gestionnaire de ressources du serveur de fichiers pour gérer les données dans les partages. Par exemple, vous pouvez utiliser la vignette **Quota** de la page Dossiers de travail dans le Gestionnaire de serveur pour définir des quotas sur les dossiers utilisateur. Vous pouvez également utiliser [Gestion du filtrage de fichiers](https://technet.microsoft.com/library/cc732074.aspx) pour contrôler les types de fichiers qui seront synchronisés par Dossiers de travail, ou les scénarios décrits dans [Contrôle d’accès dynamique](https://technet.microsoft.com/windows-server-docs/identity/solution-guides/dynamic-access-control--scenario-overview) pour afficher des tâches de classification des fichiers plus élaborées.  
  
## <a name="step-8-optionally-specify-a-tech-support-email-address"></a>Étape 8 : Si vous le souhaitez spécifier une adresse de messagerie du support technique   
 Après l’installation de Dossiers de travail sur un serveur de fichiers, vous voudrez probablement spécifier une adresse de messagerie de contact administratif pour le serveur. Pour ajouter une adresse de messagerie, procédez comme suit :  
  
#### <a name="specifying-an-administrative-contact-email"></a>Spécification d'une adresse de messagerie administrative 
  
1.  Dans le Gestionnaire de serveur, cliquez sur **Services de fichiers et de stockage**, puis sur **Serveurs**.  
  
2.  Cliquez avec le bouton droit sur le serveur de synchronisation, puis cliquez sur **Paramètres de Dossiers de travail**. La fenêtre Paramètres de Dossiers de travail s’affiche.  
  
3.  Dans le volet de navigation, cliquez sur **Messagerie électronique de support**, puis tapez l’adresse ou les adresses de messagerie que les utilisateurs doivent employer lorsqu’ils demandent de l’aide sur le service Dossiers de travail par courrier électronique. Cliquez sur **OK** lorsque vous avez terminé.  
  
     Les utilisateurs de Dossiers de travail peuvent cliquer sur un lien dans l’élément Panneau de configuration de Dossiers de travail qui envoie un message électronique contenant des informations de diagnostic sur le PC client aux adresses que vous spécifiez ici.  
  
## <a name="step-9-optionally-set-up-server-automatic-discovery"></a>Étape 9 : configurer éventuellement la découverte automatique de serveurs  
 Si vous hébergez plusieurs serveurs de synchronisation dans votre environnement, vous devez configurer la découverte automatique de serveurs en remplissant la propriété **msDS-SyncServerURL** sur les comptes d’utilisateurs dans les services de domaine Active Directory.  
  
>[!NOTE]
>La propriété msDS-SyncServerURL dans Active Directory ne doit pas être définie pour les utilisateurs distants qui accèdent à Dossiers de travail par le biais d’une solution de proxy inverse comme le proxy d’application web ou le proxy d’application Azure AD. Si la propriété msDS-SyncServerURL est définie, le client Dossiers de travail va tenter d’accéder à une URL interne non accessible via la solution de proxy inverse. Lorsque vous utilisez le proxy d’application web ou le proxy d’application Azure AD, vous devez créer des applications de proxy uniques pour chaque serveur Dossiers de travail. Pour plus d’informations, consultez [Deploying Work Folders with AD FS et Proxy d’Application Web : Vue d’ensemble](deploy-work-folders-adfs-overview.md) ou [déploiement de dossiers de travail avec le Proxy d’Application Azure AD](https://blogs.technet.microsoft.com/filecab/2017/05/31/enable-remote-access-to-work-folders-using-azure-active-directory-application-proxy/).


 Avant cela, vous devez installer un contrôleur de domaine Windows Server 2012 R2 ou mettre à jour les schémas de la forêt et du domaine à l’aide des commandes `Adprep /forestprep` et `Adprep /domainprep`. Pour plus d’informations sur l’exécution de ces commandes en toute sécurité, voir [Exécution d’Adprep.exe](https://technet.microsoft.com/library/dd464018.aspx).  
  
 Il est possible que vous souhaitiez également créer un groupe de sécurité pour les administrateurs des serveurs de fichiers et leur affecter des autorisations déléguées pour modifier cet attribut utilisateur spécifique, comme décrit dans les étapes 5 et 6. Sans ces étapes, vous devriez demander à un membre du groupe Admins du domaine ou Administrateurs de l’entreprise de configurer la découverte automatique pour chaque utilisateur.  
  
#### <a name="to-specify-the-sync-server-for-users"></a>Pour spécifier le serveur de synchronisation pour les utilisateurs  
  
1.  Ouvrez le Gestionnaire de serveur sur un ordinateur sur lequel les outils d’administration Active Directory sont installés.  
  
2.  Dans le menu **Outils** , cliquez sur **Centre d’administration Active Directory**. Le Centre d’administration Active Directory s’affiche.  
  
3.  Accédez au conteneur **Utilisateurs** dans le domaine approprié, cliquez avec le bouton droit sur l’utilisateur que vous voulez affecter à un partage de synchronisation, puis cliquez sur **Propriétés**.  
  
4.  Dans le volet de navigation, cliquez sur **Extensions**.  
  
5.  Cliquez sur l’onglet **Éditeur d’attributs**, sélectionnez **msDS-SyncServerUrl** et cliquez sur **Modifier**. La boîte de dialogue Éditeur de chaînes à valeurs multiples s’affiche.  
  
6.  Dans la zone **Valeur à ajouter**, tapez l’URL du serveur de synchronisation avec lequel cet utilisateur doit se synchroniser, cliquez sur **Ajouter**, sur **OK**, puis une nouvelle fois sur **OK**.  
  
    > [!NOTE]
    >  L’URL du serveur de synchronisation est simplement `https://` ou `http://` (selon si vous souhaitez une connexion sécurisée) suivi du nom de domaine complet du serveur de synchronisation. Par exemple, **https://sync1.contoso.com**.

Pour remplir l’attribut pour plusieurs utilisateurs, utilisez Active Directory PowerShell. Voici un exemple qui remplit l’attribut pour tous les membres du groupe *Utilisateurs Partage de synchronisation RH*, décrit à l’étape 5.
  
```powershell  
$SyncServerURL = "https://sync1.contoso.com"  
$GroupName = "HR Sync Share Users"  
  
Get-ADGroupMember -Identity $GroupName |  
Set-ADUser –Add @{"msDS-SyncServerURL"=$SyncServerURL}  
  
```  
  
## <a name="step-10-optionally-configure-web-application-proxy-azure-ad-application-proxy-or-another-reverse-proxy"></a>Étape 10 : Si vous le souhaitez configurer le Proxy d’Application Web, le Proxy d’Application ou un autre proxy inverse  

Pour permettre aux utilisateurs distants d'accéder à leurs fichiers, vous devez publier le serveur Dossiers de travail via un proxy inverse, ce qui rend Dossiers de travail disponible en externe sur Internet. Vous pouvez utiliser le proxy d’application web, le proxy d’application Azure Active Directory ou une autre solution de proxy inverse.  
  
Pour configurer l'accès à Dossiers de travail à l'aide d'AD FS et du proxy d’application web, voir [Déploiement de Dossiers de travail avec AD FS et le proxy d'application web](deploy-work-folders-adfs-overview.md). Pour obtenir des informations générales sur le proxy d’application web, voir [Proxy d’application web dans Windows Server 2016](https://technet.microsoft.com/windows-server-docs/identity/web-application-proxy/web-application-proxy-windows-server). Pour plus d’informations sur la publication d’applications, telles que Dossiers de travail, sur Internet à l’aide du proxy d’application web, voir [Publication d’applications à l’aide de la pré-authentification AD FS](https://technet.microsoft.com/windows-server-docs/identity/web-application-proxy/publishing-applications-using-ad-fs-preauthentication).
 
Pour configurer l’accès à Dossiers de travail à l’aide du proxy d’application Azure Active Directory, voir [Activer l’accès à distance à Dossiers de travail à l’aide du proxy d’application Azure Active Directory](https://blogs.technet.microsoft.com/filecab/?p=7945) 
  
## <a name="step-11-optionally-use-group-policy-to-configure-domain-joined-pcs"></a>Étape 11 : utiliser éventuellement une stratégie de groupe pour configurer les PC appartenant à un domaine  

Si vous disposez d’un grand nombre de PC appartenant à un domaine sur lesquels vous voulez déployer Dossiers de travail, vous pouvez utiliser une stratégie de groupe pour effectuer les tâches de configuration des PC clients suivantes :  
  
-   Indiquer le serveur de synchronisation avec lequel les utilisateurs doivent effectuer la synchronisation  
  
-   Forcer la configuration automatique de Dossiers de travail, en utilisant les paramètres par défaut (passez en revue la discussion sur la stratégie de groupe dans [Conception d’une implémentation de Dossiers de travail](plan-work-folders.md) avant d’effectuer cette opération)  
  
 Pour contrôler ces paramètres, créez un objet de stratégie de groupe pour Dossiers de travail, puis configurez les paramètres de stratégie de groupe suivants selon les besoins :  
  
-   Paramètre de stratégie « Spécifier les paramètres de Dossiers de travail » dans Configuration utilisateur\Stratégies\Modèles d’administration\Composants Windows\Dossiers de travail  
  
-   Paramètre de stratégie « Forcer la configuration automatique pour tous les utilisateurs » dans Configuration ordinateur\Stratégies\Modèles d’administration\Composants Windows\Dossiers de travail  
  
> [!NOTE]
>  Ces paramètres de stratégie ne sont disponibles que lors de la modification de la stratégie de groupe à partir d'un ordinateur exécutant la gestion des stratégies de groupe sur Windows 8.1, Windows Server 2012 R2 ou versions ultérieures. Ce paramètre n’est pas disponible dans les versions de la gestion des stratégies de groupe de systèmes d’exploitation antérieurs. Ces paramètres de stratégie s’appliquent aux PC Windows 7 sur lesquels l'application [Dossiers de travail pour Windows 7](http://blogs.technet.com/b/filecab/archive/2014/04/24/work-folders-for-windows-7.aspx) a été installée.  
  
##  <a name="BKMK_LINKS"></a> Voir aussi  
 Pour plus d’informations connexes, voir les ressources suivantes.  
  
|Type de contenu|Références|  
|------------------|----------------|  
|**Présentation**|-   [Dossiers de travail](work-folders-overview.md)|  
|**Planification**|-   [Conception d’une implémentation de dossiers de travail](plan-work-folders.md)|
|**Déploiement**|-   [Déploiement de dossiers de travail avec AD FS et Proxy d’Application Web (WAP)](deploy-work-folders-adfs-overview.md)<br />-   [Utiliser des dossiers de déploiement de laboratoire de tests](http://blogs.technet.com/b/filecab/archive/2013/07/10/work-folders-test-lab-deployment.aspx) (billet de blog)<br />-   [Un nouvel attribut utilisateur pour l’Url du serveur dossiers de travail](http://blogs.technet.com/b/filecab/archive/2013/10/09/a-new-user-attribute-for-work-folders-server-url.aspx) (billet de blog)|  
|**Référence technique**|-   [Ouverture de session interactive : Seuil de verrouillage du compte ordinateur](https://technet.microsoft.com/library/jj966264(v=ws.11).aspx)<br />-   [Applets de commande de synchronisation partage](https://docs.microsoft.com/powershell/module/syncshare/?view=win10-ps)|
