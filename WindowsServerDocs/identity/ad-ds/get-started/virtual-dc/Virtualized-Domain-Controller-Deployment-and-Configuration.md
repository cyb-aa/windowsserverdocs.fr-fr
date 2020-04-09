---
ms.assetid: b146f47e-3081-4c8e-bf68-d0f993564db2
title: Déploiement et configuration des contrôleurs de domaine virtualisés
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 97d726f8bfbbe664dfdfd6b7000988f009174631
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80824692"
---
# <a name="virtualized-domain-controller-deployment-and-configuration"></a>Déploiement et configuration des contrôleurs de domaine virtualisés

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette rubrique traite des sujets suivants :  
  
-   [Considérations relatives à l’installation](../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Deployment-and-Configuration.md#BKMK_InstallConsiderations)  
  
    Cela inclut les conditions requises par la plateforme et d'autres contraintes importantes.  
  
-   [Clonage des contrôleurs de domaine virtualisés](../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Deployment-and-Configuration.md#BKMK_VDCCloning)  
  
    Description détaillée de l'ensemble du processus de clonage des contrôleurs de domaine virtualisés.  
  
-   [Restauration sécurisée d’un contrôleur de domaine virtualisé](../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Deployment-and-Configuration.md#BKMK_VDCSafeRestore)  
  
    Description détaillée des validations effectuées durant la restauration sécurisée des contrôleurs de domaine virtualisés.  
  
## <a name="installation-considerations"></a><a name="BKMK_InstallConsiderations"></a>Considérations relatives à l’installation  
Il n'y a aucune installation de rôle ou de composant spécifique pour les contrôleurs de domaine virtualisés. Tous les contrôleurs de domaine contiennent automatiquement les fonctionnalités relatives au clonage et à la restauration sécurisée. Vous ne pouvez pas supprimer ou désactiver ces fonctionnalités.  
  
L'utilisation de contrôleurs de domaine Windows Server 2012 demande un schéma AD DS Windows Server 2012 version 56 ou supérieure et un niveau fonctionnel de forêt correspondant à Windows Server 2003 version native ou supérieure.  
  
Les contrôleurs de domaine, qu'ils soient accessibles en écriture ou en lecture seule, prennent en charge tous les aspects d'un contrôleur de domaine virtualisé, comme le font les catalogues globaux et les rôles FSMO.  
  
> [!IMPORTANT]  
> Le détenteur d'un rôle FSMO d'émulateur de contrôleur de domaine principal doit être en ligne quand le clonage commence.  
  
### <a name="platform-requirements"></a><a name="BKMK_PlatformReqs"></a>Configuration requise pour la plateforme  
Le clonage des contrôleurs de domaine virtualisés demande les éléments suivants :  
  
-   rôle FSMO d'émulateur de contrôleur de domaine principal hébergé sur un contrôleur de domaine Windows Server 2012 ;  
  
-   émulateur de contrôleur de domaine principal disponible durant les opérations de clonage.  
  
Le clonage et la restauration sécurisée demandent les éléments suivants :  
  
-   invités virtualisés Windows Server 2012 ;  
  
-   plateforme hôte de virtualisation prenant en charge les ID de génération de machine virtuelle.  
  
Consultez les produits de virtualisation dans le tableau ci-dessous, puis vérifiez s'ils prennent en charge les contrôleurs de domaine virtualisés et l'ID de génération d'ordinateur virtuel.  
  
|||  
|-|-|  
|**Produit de virtualisation**|**Prend en charge les contrôleurs de domaine virtualisés et VMGID**|  
|**Serveur Microsoft Windows Server 2012 avec fonctionnalité Hyper-V**|Oui|  
|**Microsoft Windows Server 2012 Hyper-V Server**|Oui|  
|**Microsoft Windows 8 avec la fonctionnalité de client Hyper-V**|Oui|  
|**Windows Server 2008 R2 et Windows Server 2008**|Non|  
|**Solutions de virtualisation non-Microsoft**|Contactez un fournisseur|  
  
Bien que Microsoft prenne en charge Windows 7 Virtual PC, Virtual PC 2007, Virtual PC 2004 et Virtual Server 2005, ces produits ne peuvent pas exécuter d'invités 64 bits. En outre, ils ne prennent pas en charge les ID de génération de machine virtuelle.  
  
Pour obtenir de l'aide sur les produits tiers de virtualisation et leurs possibilités de prise en charge des contrôleurs de domaine virtualisés, contactez directement le fournisseur correspondant.  
  
Pour plus d’informations, consultez la politique de Microsoft en matière de support technique des [logiciels Microsoft qui exécutent des logiciels de virtualisation matérielle non Microsoft](https://support.microsoft.com/kb/897615).  
  
### <a name="critical-caveats"></a>Avertissements critiques  
Les contrôleurs de domaine virtuels ne prennent *pas* en charge la restauration sécurisée des éléments suivants :  
  
-   fichiers VHD et VHDX copiés manuellement en remplacement des fichiers VHD existants ;  
  
-   fichiers VHD et VHDX restaurés à l'aide d'un logiciel de sauvegarde de fichiers ou de sauvegarde de disque complète.  
  
> [!NOTE]  
> Les fichiers VHDX sont nouveaux dans Windows Server 2012 Hyper-V.  
  
Aucune de ces opérations n'est couverte par la sémantique des ID de génération de machine virtuelle. Ainsi, l'ID de génération de machine virtuelle reste inchangé. La restauration des contrôleurs de domaine à l'aide de ces méthodes peut entraîner une restauration USN et une mise en quarantaine du contrôleur de domaine, ou introduire des objets en attente et nécessiter le recours à des opérations de nettoyage à l'échelle de la forêt.  
  
> [!WARNING]  
> La restauration sécurisée des contrôleurs de domaine virtualisés ne remplace pas les sauvegardes de l'état du système, ni la Corbeille AD DS.  
>   
> Après la restauration d'une capture instantanée, les deltas des changements non répliqués précédemment en provenance de ce contrôleur de domaine et postérieurs à la capture instantanée sont définitivement perdus. La restauration sécurisée implémente une restauration automatique ne faisant pas autorité pour éviter *uniquement*le risque de mise en quarantaine du contrôleur de domaine.  
  
Pour plus d’informations sur les bulles USN et les objets en attente, voir l’article consacré à la [résolution des problèmes d’opérations Active Directory qui échouent avec l’erreur 8606 : « Des attributs insuffisants ont été donnés pour créer un objet. »](https://support.microsoft.com/kb/2028495).  
  
## <a name="virtualized-domain-controller-cloning"></a><a name="BKMK_VDCCloning"></a>Clonage des contrôleurs de domaine virtualisés  
Il existe un certain nombre de stades et d'étapes pour le clonage d'un contrôleur de domaine virtualisé, indépendamment de l'utilisation d'outils graphiques ou de Windows PowerShell. De manière générale, il existe trois stades :  
  
**Préparer l’environnement**  
  
-   Étape 1 : Vérifier que l’hyperviseur prend en charge les ID de génération de machine virtuelle et donc le clonage  
  
-   Étape 2 : Vérifiez que le rôle d’émulateur de contrôleur de domaine principal est hébergé par un contrôleur de domaine qui exécute Windows Server 2012 et qu’il est en ligne et accessible par le contrôleur de domaine cloné lors du clonage.  
  
**Préparer le contrôleur de domaine source**  
  
-   Étape 3 : Autoriser le contrôleur de domaine source pour le clonage  
  
-   Étape 4 : Supprimer les services ou programmes incompatibles, ou les ajouter au fichier CustomDCCloneAllowList.xml  
  
-   Étape 5 : Créer DCCloneConfig.xml  
  
-   Étape 6 : Mettre le contrôleur de domaine source hors connexion  
  
**Créer le contrôleur de domaine cloné**  
  
-   Étape 7 : Copier ou exporter la machine virtuelle source et ajouter le code XML s’il n’a pas déjà été copié  
  
-   Étape 8 : Créer une machine virtuelle à partir de la copie  
  
-   Étape 9 : Démarrer la nouvelle machine virtuelle pour commencer le clonage  
  
Il n'y a pas de différences de procédure entre l'utilisation d'outils graphiques tels que la console de gestion Hyper-V ou l'utilisation d'outils en ligne de commande tels que Windows PowerShell. Ainsi, les étapes ne sont présentées qu'une seule fois pour les deux interfaces. Cette rubrique fournit des exemples Windows PowerShell pour vous permettre d'explorer l'automatisation du processus de clonage de bout en bout. Ils ne sont nécessaires pour aucune étape. Il n'existe aucun outil de gestion graphique des contrôleurs de domaine virtualisés dans Windows Server 2012.  
  
À plusieurs stades de la procédure, vous pouvez choisir comment créer l'ordinateur cloné et comment ajouter les fichiers xml. Ces étapes sont décrites dans les détails fournis ci-dessous. En dehors de ce cadre, le processus est irréversible.  
  
Le schéma suivant illustre le processus de clonage d'un contrôleur de domaine virtualisé, où le domaine existe déjà.  
  
![Déploiement de DC virtualisé](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_CloningProcessFlow.png)  
  
### <a name="step-1---validate-the-hypervisor"></a>Étape 1 - Valider l'hyperviseur  
Vérifiez que le contrôleur de domaine source s'exécute sur un hyperviseur pris en charge en consultant la documentation du fournisseur. Les contrôleurs de domaine virtualisés sont indépendants de l'hyperviseur et ne demandent pas obligatoirement Hyper-V.  
  
Si l’hyperviseur est Microsoft Hyper-V, assurez-vous qu’il s’exécute sur Windows Server 2012. Vous pouvez effectuer cette vérification à l'aide de la Gestion des périphériques  
  
Ouvrez **Devmgmt.msc** , puis recherchez dans **Périphériques système** les périphériques et pilotes Microsoft Hyper-V installés. Le périphérique système spécifique nécessaire à un contrôleur de domaine virtualisé est le **compteur de génération Microsoft Hyper-V** (pilote : vmgencounter.sys).  
  
![Déploiement de DC virtualisé](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVVMGenIDCounter.png)  
  
### <a name="step-2---verify-the-pdce-fsmo-role"></a>Étape 2 - Vérifier le rôle FSMO d'émulateur de contrôleur de domaine principal  
Avant de tenter de cloner un contrôleur de domaine, vous devez vérifier que le contrôleur de domaine qui héberge le rôle FSMO d'émulateur de contrôleur de domaine principal exécute Windows Server 2012. L'émulateur de contrôleur de domaine principal (PDCE) est nécessaire pour plusieurs raisons :  
  
1.  L'émulateur de contrôleur de domaine principal crée le groupe spécial des **Contrôleurs de domaine clonables** et définit son autorisation à la racine du domaine pour permettre à un contrôleur de domaine de se cloner.  
  
2.  Le contrôleur de domaine de clonage contacte l'émulateur de contrôleur de domaine principal directement à l'aide du protocole RPC DRSUAPI, pour créer des objets ordinateur pour le contrôleur de domaine clone.  
  
    > [!NOTE]  
    > Windows Server 2012 étend le protocole distant du service de réplication d'annuaire (DRS), (UUID **E3514235-4B06-11D1-AB04-00C04FC2DCD2**), pour inclure une nouvelle méthode RPC **IDL_DRSAddCloneDC** (Opnum **28**). La méthode **IDL_DRSAddCloneDC** crée un objet contrôleur de domaine en copiant les attributs d'un objet contrôleur de domaine existant.  
    >   
    > Les états d'un contrôleur de domaine comprennent l'ordinateur, le serveur, les paramètres NTDS, les services FRS et DFSR, ainsi que les objets de connexion gérés pour chaque contrôleur de domaine. Durant la duplication d'un objet, cette méthode RPC remplace toutes les références au contrôleur de domaine d'origine par les objets correspondants du nouveau contrôleur de domaine. L'appelant doit avoir le droit de contrôle d'accès DS-Clone-Domain-Controller pour le contexte d'appellation du domaine.  
    >   
    > L'utilisation de cette nouvelle méthode nécessite toujours un accès direct au contrôleur de domaine de l'émulateur de contrôleur de domaine principal à partir de l'appelant.  
    >   
    > Dans la mesure où cette méthode RPC est nouvelle, votre logiciel d'analyse réseau nécessite des analyseurs mis à jour pour inclure les champs du nouvel Opnum 28 dans l'UUID E3514235-4B06-11D1-AB04-00C04FC2DCD2 existant. Sinon, vous ne pouvez pas analyser ce trafic.  
    >   
    > Pour plus d’informations, consultez [4.1.29 IDL_DRSAddCloneDC (Opnum 28)](https://msdn.microsoft.com/library/hh554213(v=prot.13).aspx).  
  
***Cela signifie également qu'avec des réseaux non routés complètement, le clonage d'un contrôleur de domaine virtualisé demande des segments réseau ayant accès à l'émulateur de contrôleur de domaine principal***. Il est possible de déplacer un contrôleur de domaine cloné vers un autre réseau après le clonage (de même qu'un contrôleur de domaine physique), du moment que vous veillez à mettre à jour les informations du site logique AD DS.  
  
> [!IMPORTANT]  
> Durant le clonage d'un domaine qui ne contient qu'un seul contrôleur de domaine, vous devez vous assurer que le contrôleur de domaine source est remis en ligne avant le démarrage des copies clones. Un domaine de production doit toujours contenir au moins deux contrôleurs de domaine.  
  
#### <a name="active-directory-users-and-computers-method"></a>Méthode basée sur Utilisateurs et ordinateurs Active Directory  
  
1.  À l'aide du composant logiciel enfichable Dsa.msc, cliquez avec le bouton droit sur le domaine, puis cliquez sur **Maître d'opérations**. Notez le nom du contrôleur de domaine sous l'onglet CDP, puis fermez la boîte de dialogue.  
  
2.  Cliquez avec le bouton droit sur l'objet ordinateur du contrôleur de domaine, cliquez sur **Propriétés**, puis vérifiez les informations relatives au système d'exploitation.  
  
#### <a name="windows-powershell-method"></a>Méthode basée sur Windows PowerShell  
Vous pouvez combiner les applets de commande suivantes du module Windows PowerShell pour Active Directory et retourner la version de l'émulateur de contrôleur de domaine principal :  
  
```  
Get-adddomaincontroller  
Get-adcomputer  
```  
  
Si le domaine n'est pas indiqué, ces applets de commande prennent en compte le domaine de l'ordinateur sur lequel elles sont exécutées.  
  
La commande suivante retourne les informations relatives à l'émulateur de contrôleur de domaine principal et au système d'exploitation :  
  
```  
get-adcomputer(Get-ADDomainController -Discover -Service "PrimaryDC").name -property * | format-list dnshostname,operatingsystem,operatingsystemversion  
```  
  
L'exemple ci-dessous montre comment spécifier le nom de domaine et filtrer les propriétés retournées avant le pipeline Windows PowerShell :  
  
![Déploiement de DC virtualisé](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PDCOSInfo.png)  
  
### <a name="step-3---authorize-a-source-dc"></a>Étape 3 - Autoriser un contrôleur de domaine source  
Le contrôleur de domaine source doit disposer du droit de contrôle d'accès **Autoriser un contrôleur de domaine à créer un clone de lui-même** dans l'en-tête de contexte d'appellation du domaine. Par défaut, le groupe **Contrôleurs de domaine clonables** dispose de cette autorisation et ne contient aucun membre. L'émulateur de contrôleur de domaine principal crée ce groupe quand le rôle FSMO est transféré vers un contrôleur de domaine Windows Server 2012.  
  
#### <a name="active-directory-administrative-center-method"></a>Méthode basée sur le Centre d'administration Active Directory  
  
1.  Démarrez Dsac.exe et accédez au contrôleur de domaine source, puis ouvrez sa page de détails.  
  
2.  Dans la section **Membre de**, ajoutez le groupe **Contrôleurs de domaine clonables** pour ce domaine.  
  
#### <a name="windows-powershell-method"></a>Méthode basée sur Windows PowerShell  
Vous pouvez combiner les applets de commande du module Windows PowerShell pour Active Directory **get-adcomputer** et **add-adgroupmember** pour ajouter un contrôleur de domaine au groupe **Contrôleurs de domaine clonables** :  
  
```  
Get-adcomputer <dc name> | %{add-adgroupmember "cloneable domain controllers" $_.samaccountname}  
```  
  
Par exemple, ceci permet d'ajouter le serveur DC1 au groupe, sans devoir préciser le nom unique du membre du groupe :  
  
![Déploiement de DC virtualisé](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_AddDcToGroup.png)  
  
#### <a name="rebuilding-default-permissions"></a>Recréation des autorisations par défaut  
Si vous supprimez cette autorisation de l'en-tête du domaine, le clonage échoue. Vous pouvez recréer l'autorisation à l'aide du Centre d'administration Active Directory ou de Windows PowerShell.  
  
##### <a name="active-directory-administrative-center-method"></a>Méthode basée sur le Centre d'administration Active Directory  
  
1.  Ouvrez le **Centre d'administration Active Directory**, cliquez avec le bouton droit sur l'en-tête du domaine, cliquez sur **Propriétés**, cliquez sur l'onglet **Extensions**, sur **Sécurité**, puis sur **Avancé**. Cliquez sur **Cet objet uniquement**.  
  
2.  Cliquez sur **Ajouter**, sous **Entrez le nom de l'objet à sélectionner**, tapez le nom de groupe **Contrôleurs de domaine clonables**.  
  
3.  Sous Autorisations, cliquez sur **Autoriser un contrôleur de domaine à créer un clone de lui-même**, puis sur **OK**.  
  
> [!NOTE]  
> Vous pouvez également supprimer l'autorisation par défaut et ajouter des contrôleurs de domaine individuels. Toutefois, cela risque d'entraîner des problèmes de maintenance, car les nouveaux administrateurs ne seront pas informés de cette personnalisation. Le changement du paramètre par défaut ne renforce pas la sécurité et est déconseillé.  
  
##### <a name="windows-powershell-method"></a>Méthode basée sur Windows PowerShell  
Utilisez les commandes suivantes dans une console Windows PowerShell avec privilèges d'administrateur élevés. Ces commandes détectent le nom de domaine et restaurent les autorisations par défaut :  
  
```  
import-module activedirectory  
cd ad:  
$domainNC = get-addomain  
$dcgroup = get-adgroup "Cloneable Domain Controllers"  
$sid1 = (get-adgroup $dcgroup).sid  
$acl = get-acl $domainNC  
$objectguid = new-object Guid 3e0f7e18-2c7a-4c10-ba82-4d926db99a3e  
$ace1 = new-object System.DirectoryServices.ActiveDirectoryAccessRule $sid1,"ExtendedRight","Allow",$objectguid  
$acl.AddAccessRule($ace1)  
set-acl -aclobject $acl $domainNC  
cd c:  
```  
  
Vous pouvez également exécuter l'exemple [FixVDCPermissions.ps1](../../../ad-ds/reference/virtual-dc/Virtualized-Domain-Controller-Technical-Reference-Appendix.md#BKMK_FixPDCPerms) dans une console Windows PowerShell, en la faisant démarrer avec des privilèges d'administrateur élevés sur un contrôleur de domaine du domaine affecté. Les autorisations sont définies automatiquement. L'exemple se trouve dans l'annexe de ce module.  
  
### <a name="step-4---remove-incompatible-applications-or-services-if-not-using-customdccloneallowlistxml"></a>Étape 4 - Supprimer les applications ou services incompatibles (si vous n'utilisez pas CustomDCCloneAllowList.xml)  
Les programmes ou services retournés précédemment par Get-ADDCCloningExcludedApplicationList ( *et non ajoutés au fichier CustomDCCloneAllowList.xml* ) doivent être retirés avant le clonage. La désinstallation de l'application ou du service est la méthode recommandée.  
  
> [!WARNING]  
> Tout programme ou service incompatible qui n'a pas été désinstallé ou ajouté au fichier CustomDCCloneAllowList.xml empêche le clonage.  
  
Utilisez l'applet de commande Get-AdComputerServiceAccount pour localiser les comptes de service administrés autonomes du domaine et déterminer si cet ordinateur utilise l'un d'entre eux. Si un compte de service administré est installé, utilisez l'applet de commande Uninstall-ADServiceAccount pour supprimer le compte de service installé localement. Une fois que vous avez fini de mettre hors connexion le contrôleur de domaine source à l'étape 6, vous pouvez rajouter le compte de service administré via Install-ADServiceAccount quand le serveur est de nouveau en ligne. Pour plus d’informations, consultez [Uninstall-ADServiceAccount](https://technet.microsoft.com/library/hh852310).  
  
> [!IMPORTANT]  
> Les comptes de service administrés autonomes (apparus avec Windows Server 2008 R2) ont été remplacés dans Windows Server 2012 par les comptes de service administrés de groupe. Les comptes de service administrés de groupe prennent en charge le clonage.  
  
### <a name="step-5---create-dccloneconfigxml"></a>Étape 5 - Créer DCCloneConfig.xml  
Le fichier DcCloneConfig.xml est indispensable pour le clonage des contrôleurs de domaine. Son contenu vous permet de spécifier des détails uniques comme le nouveau nom de l'ordinateur et son adresse IP.  
  
Le fichier CustomDCCloneAllowList.xml est facultatif, sauf si vous installez des applications ou des services Windows potentiellement incompatibles sur le contrôleur de domaine source. Les fichiers nécessitent une dénomination, une mise en forme et un placement précis. Sinon, le clonage échoue.  
  
C'est la raison pour laquelle vous devez toujours utiliser les applets de commande Windows PowerShell pour créer les fichiers XML et les placer à l'emplacement approprié.  
  
#### <a name="generating-with-new-addccloneconfigfile"></a>Génération avec New-ADDCCloneConfigFile  
Le module Windows PowerShell pour Active Directory contient une nouvelle applet de commande dans Windows Server 2012 :  
  
```  
New-ADDCCloneConfigFile  
```  
  
Vous devez exécuter l'applet de commande sur le contrôleur de domaine source que vous avez l'intention de cloner. L'applet de commande prend en charge plusieurs arguments. Quand elle est utilisée, elle teste toujours l'ordinateur et l'environnement où elle est exécutée, sauf si vous spécifiez l'argument -offline.  
  
||||  
|-|-|-|  
|**Directory**<p>**PolicySchedule**|**Arguments**|**Explicatif**|  
|**New-ADDCCloneConfigFile**|*<no argument specified>*|Crée un fichier DcCloneConfig.xml vide dans le répertoire de travail DSA (par défaut : %systemroot%\ntds)|  
||-CloneComputerName|Spécifie le nom d'ordinateur du contrôleur de domaine clone. Type de données chaîne.|  
||-Path|Spécifie le dossier où créer le fichier DcCloneConfig.xml. Si rien n’est spécifié, le répertoire de travail DSA est choisi (par défaut : %systemroot%\ntds). Type de données chaîne.|  
||-SiteName|Spécifie le nom du site logique Active Directory à joindre durant la création du compte d'ordinateur cloné. Type de données chaîne.|  
||-IPv4Address|Spécifie l'adresse IPv4 statique de l'ordinateur cloné. Type de données chaîne.|  
||-IPv4SubnetMask|Spécifie le masque de sous-réseau IPv4 statique de l'ordinateur cloné. Type de données chaîne.|  
||-IPv4DefaultGateway|Spécifie l'adresse de passerelle par défaut IPv4 statique de l'ordinateur cloné. Type de données chaîne.|  
||-IPv4DNSResolver|Spécifie les entrées DNS IPv4 statiques de l'ordinateur cloné dans une liste dont les valeurs sont séparées par des virgules. Type de données tableau. Quatre entrées au maximum peuvent être fournies.|  
||-PreferredWINSServer|Spécifie l'adresse IPv4 statique du serveur WINS principal. Type de données chaîne.|  
||-AlternateWINSServer|Spécifie l'adresse IPv4 statique du serveur WINS secondaire. Type de données chaîne.|  
||-IPv6DNSResolver|Spécifie les entrées DNS IPv6 statiques de l'ordinateur cloné dans une liste dont les valeurs sont séparées par des virgules. Il est impossible de définir des informations IPv6 statiques dans le clonage de contrôleur de domaine virtualisé. Type de données tableau.|  
||-Offline|N'effectue pas les tests de validation et remplace tout fichier dccloneconfig.xml existant. N'a pas de paramètres.|  
||*-Statique*|Obligatoire pour la spécification des arguments IP statiques IPv4SubnetMask, IPv4SubnetMask ou IPv4DefaultGateway. N'a pas de paramètres.|  
  
Tests effectués durant l'exécution en mode en ligne :  
  
-   L'émulateur de contrôleur de domaine principal est Windows Server 2012 (ou version supérieure)  
  
-   Le contrôleur de domaine source est membre du groupe Contrôleurs de domaine clonables  
  
-   Le contrôleur de domaine source ne comprend pas les applications ou services exclus  
  
-   Le contrôleur de domaine source ne contient pas déjà de fichier DcCloneConfig.xml dans le chemin d'accès spécifié  
  
![Déploiement de DC virtualisé](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSNewDCCloneConfig.png)  
  
### <a name="step-6---take-the-source-domain-controller-offline"></a>Étape 6 - Mettre le contrôleur de domaine source hors connexion  
Vous ne pouvez pas copier un contrôleur de domaine source en cours d'exécution. Il doit être arrêté de manière normale. Ne clonez pas un contrôleur de domaine arrêté à la suite d'une coupure d'alimentation.  
  
#### <a name="graphical-method"></a>Méthode graphique  
Utilisez le bouton d'arrêt du contrôleur de domaine en cours d'exécution, ou le bouton d'arrêt du Gestionnaire Hyper-V.  
  
![Déploiement de DC virtualisé](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_Shutdown.png)  
  
![Déploiement de DC virtualisé](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVShutdown.png)  
  
#### <a name="windows-powershell-method"></a>Méthode basée sur Windows PowerShell  
Vous pouvez arrêter une machine virtuelle à l'aide de l'une des applets de commande suivantes :  
  
```  
Stop-computer  
Stop-vm  
```  
  
Stop-computer est une applet de commande qui prend en charge l'arrêt des ordinateurs indépendamment de la virtualisation. Elle est similaire à l'ancien utilitaire Shutdown.exe. Stop-vm est une nouvelle applet de commande du module Windows PowerShell pour Hyper-V dans Windows Server 2012. Elle équivaut aux options d'alimentation du Gestionnaire Hyper-V. Ce dernier est utile dans les environnements lab où le contrôleur de domaine opère souvent sur un réseau virtualisé privé.  
  
![Déploiement de DC virtualisé](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_StopComputer2.png)  
  
![Déploiement de DC virtualisé](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_StopVM.png)  
  
### <a name="step-7---copy-disks"></a>Étape 7 - Copier les disques  
Un choix d'administration est nécessaire durant la phase de copie :  
  
-   Copier les disques manuellement sans Hyper-V  
  
-   Exporter la machine virtuelle avec Hyper-V  
  
-   Exporter les disques fusionnés avec Hyper-V  
  
Tous les disques d'une machine virtuelle doivent être copiés, pas seulement le lecteur système. Si le contrôleur de domaine source utilise des disques de différenciation et si vous envisagez de déplacer votre contrôleur de domaine cloné vers un autre hôte Hyper-V, vous devez procéder à une exportation.  
  
La copie manuelle de disques est recommandée si le contrôleur de domaine source ne dispose que d'*un* seul lecteur. L'exportation/importation est recommandée pour les ordinateurs virtuels qui comportent *plusieurs* lecteurs ou des personnalisations complexes du matériel virtualisé, par exemple plusieurs cartes d'interface réseau.  
  
Si vous copiez manuellement les fichiers, supprimez les captures instantanées avant d'effectuer la copie. Si vous exportez la machine virtuelle, supprimez les captures instantanées avant l'exportation, ou supprimez-les de la nouvelle machine virtuelle une fois l'importation effectuée.  
  
> [!WARNING]  
> Les captures instantanées sont des disques de différenciation qui peuvent restaurer un contrôleur de domaine à un état antérieur. Si vous clonez un contrôleur de domaine et restaurez ensuite sa capture instantanée antérieure au clonage, vous obtenez des contrôleurs de domaine dupliqués dans la forêt. Il n'y a aucune valeur dans les captures instantanées antérieures d'un contrôleur de domaine qui vient d'être cloné.  
  
#### <a name="manually-copying-disks"></a>Copie manuelle des disques  
  
##### <a name="hyper-v-manager-method"></a>Méthode basée sur le Gestionnaire Hyper-V  
Utilisez le composant logiciel enfichable Gestionnaire Hyper-V pour déterminer quels sont les disques associés au contrôleur de domaine source. Utilisez l'option Inspecter pour vérifier si le contrôleur de domaine utilise des disques de différenciation (ce qui vous oblige à copier également le disque parent).  
  
![Déploiement de DC virtualisé](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVInspect.png)  
  
Pour supprimer des captures instantanées, sélectionnez une machine virtuelle, puis supprimez la sous-arborescence des captures instantanées.  
  
![Déploiement de DC virtualisé](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVDeleteSnapshot.gif)  
  
Vous pouvez ensuite copier manuellement les fichiers VHD or VHDX via l'Explorateur Windows, Xcopy.exe ou Robocopy.exe. Aucune étape particulière n'est nécessaire. Il est recommandé de changer les noms de fichiers, même si vous les déplacez vers un autre dossier.  
  
> [!NOTE]  
> Si vous effectuez une copie entre des ordinateurs hôtes situés sur un réseau local (1 Gbit ou plus), l'option **Xcopy.exe /J** permet de copier les fichiers VHD/VHDX beaucoup plus rapidement que n'importe quel autre outil, au prix d'une plus grande utilisation de la bande passante.  
  
##### <a name="windows-powershell-method"></a>Méthode basée sur Windows PowerShell  
Pour identifier les disques à l'aide de Windows PowerShell, utilisez les modules Hyper-V :  
  
```  
Get-vmidecontroller  
Get-vmscsicontroller  
Get-vmfibrechannelhba  
Get-vmharddiskdrive  
```  
  
Par exemple, vous pouvez retourner tous les disques durs IDE d'une machine virtuelle nommée **DC2** à l'aide de l'exemple suivant :  
  
![Déploiement de DC virtualisé](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_ReturnIDE.png)  
  
Si le chemin d'accès du disque pointe vers un fichier AVHD ou AVHDX, il s'agit d'une capture instantanée. Pour supprimer les captures instantanées associées à un disque et fusionner les fichiers VHD ou VHDX réels, utilisez les applets de commande suivantes :  
  
```  
Get-VMSnapshot  
Remove-VMSnapshot  
```  
  
Par exemple, pour supprimer toutes les captures instantanées d'un ordinateur virtuel nommé DC2-SOURCECLONE :  
  
![Déploiement de DC virtualisé](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_DelSnapshots.png)  
  
Pour copier les fichiers à l'aide de Windows PowerShell, utilisez l'applet de commande suivante :  
  
```  
Copy-Item  
```  
  
Combinez les applets de commande d'ordinateur virtuel sous forme de pipelines pour faciliter l'automatisation. Le pipeline est un canal utilisé entre plusieurs applets de commande pour transmettre des données. Par exemple, pour copier le lecteur d'un contrôleur de domaine source hors connexion nommé DC2-SOURCECLONE vers un nouveau disque appelé c:\temp\copy.vhd sans être obligé de connaître le chemin d'accès exact de son lecteur système :  
  
```  
Get-VMIdeController dc2-sourceclone | Get-VMHardDiskDrive | select-Object {copy-item -path $_.path -destination c:\temp\copy.vhd}  
```  
  
![Déploiement de DC virtualisé](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSCopyDrive.png)  
  
> [!IMPORTANT]  
> Vous ne pouvez pas utiliser de « disques pass-through » (disques directs) avec le clonage, car ils ne reposent pas sur un fichier de disque virtuel mais sur un disque dur réel.  
  
> [!NOTE]  
> Pour plus d’informations sur d’autres opérations Windows PowerShell avec des pipelines, consultez [Définition et utilisation des pipelines dans Windows PowerShell](https://technet.microsoft.com/library/ee176927.aspx).  
  
#### <a name="exporting-the-vm"></a>Exportation de l'ordinateur virtuel  
En remplacement de la copie des disques, vous pouvez exporter la totalité d'un ordinateur virtuel Hyper-V sous forme de copie. L'exportation crée automatiquement un dossier nommé pour l'ordinateur virtuel et contenant l'ensemble des disques et informations de configuration.  
  
![Déploiement de DC virtualisé](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVExport.png)  
  
##### <a name="hyper-v-manager-method"></a>Méthode basée sur le Gestionnaire Hyper-V  
Pour exporter un ordinateur virtuel avec le Gestionnaire Hyper-V :  
  
1.  Cliquez avec le bouton droit sur le contrôleur de domaine source, puis cliquez sur **Exporter**.  
  
2.  Sélectionnez un dossier existant comme conteneur d'exportation.  
  
3.  Attendez que la colonne État cesse d'afficher **Exportation**.  
  
##### <a name="windows-powershell-method"></a>Méthode basée sur Windows PowerShell  
Pour exporter un ordinateur virtuel à l'aide du module Windows PowerShell pour Hyper-V, utilisez l'applet de commande suivante :  
  
```  
Export-vm  
```  
  
Par exemple, pour exporter un ordinateur virtuel nommé DC2-SOURCECLONE vers un dossier nommé C:\VM :  
  
![Déploiement de DC virtualisé](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSExport.png)  
  
> [!NOTE]  
> Windows Server 2012 Hyper-V prend en charge de nouvelles fonctionnalités d'exportation et d'importation qui dépassent le cadre de cette formation. Pour plus d'informations, voir TechNet.  
  
#### <a name="exporting-merged-disks-using-hyper-v"></a>Exportation de disques fusionnés, avec Hyper-V  
La dernière possibilité consiste à utiliser les options de fusion et de conversion de disques d’Hyper-V. Elles vous permettent de faire une copie d’une structure de disque existante (même en incluant des fichiers AVHD/AVHDX de capture instantanée) vers un seul nouveau disque. Comme pour le scénario de copie manuelle de disque, cette solution est principalement destinée aux machines virtuelles plus simples qui utilisent un seul lecteur, par exemple C :\\. Son seul avantage, contrairement à la copie manuelle, est de ne pas vous obliger à supprimer d'abord les captures instantanées. Cette opération est nécessairement plus lente que la simple suppression des captures instantanées et la copie de disques.  
  
##### <a name="hyper-v-manager-method"></a>Méthode basée sur le Gestionnaire Hyper-V  
Pour créer un disque fusionné avec le Gestionnaire Hyper-V :  
  
1.  Cliquez sur **modifier le disque**.  
  
2.  Recherchez le disque enfant au niveau le plus bas. Par exemple, si vous utilisez un disque de différenciation, le disque enfant est l'enfant situé au niveau le plus bas. Si l'ordinateur virtuel a une capture instantanée (ou plusieurs), la capture instantanée actuellement sélectionnée est le disque enfant au niveau le plus bas.  
  
3.  Sélectionnez l'option **Fusionner** pour créer un disque unique à partir de l'ensemble de la structure parent-enfant.  
  
4.  Sélectionnez un nouveau disque dur virtuel et indiquez un chemin d'accès. Cela permet de combiner les fichiers VHD/VHDX existants en une nouvelle unité portable unique, dont la restauration des captures instantanées antérieures ne présente aucun risque.  
  
##### <a name="windows-powershell-method"></a>Méthode basée sur Windows PowerShell  
Pour créer un disque fusionné à partir d'un ensemble complexe de parents via le module Windows PowerShell pour Hyper-V, utilisez l'applet de commande suivante :  
  
```  
Convert-vm  
```  
  
Par exemple, pour exporter la totalité de la chaîne des captures instantanées de disque d'un ordinateur virtuel (cette fois, sans inclure les disques de différenciation) et le disque parent vers un nouveau disque unique nommé DC4-CLONED.VHDX :  
  
![Déploiement de DC virtualisé](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSConvertVhd.png)  
  
#### <a name="adding-xml-to-the-offline-system-disk"></a><a name="BKMK_Offline"></a>Ajout de XML au disque système hors connexion  
Si vous avez copié le fichier Dccloneconfig.xml sur le contrôleur de domaine source en cours d'exécution, vous devez à présent copier le fichier dccloneconfig.xml mis à jour sur le disque système copié/exporté hors connexion. Selon les applications installées détectées avec l'applet de commande Get-ADDCCloningExcludedApplicationList utilisée plus tôt, vous pouvez également être amené à copier le fichier CustomDCCloneAllowList.xml sur le disque.  
  
Les emplacements suivants peuvent contenir le fichier DcCloneConfig.xml :  
  
1.  DSA Working Directory (répertoire de travail DSA)  
  
2.  %windir%\NTDS  
  
3.  Médias de lecture/écriture amovibles, en fonction de l’ordre de la lettre qui identifie le lecteur, sur la racine du lecteur.  
  
Ces chemins d'accès ne sont pas configurables. Après le début du clonage, le processus vérifie ces emplacements dans un ordre spécifique et utilise le premier fichier DcCloneConfig.xml trouvé, quel que soit le contenu de l'autre dossier.  
  
Les emplacements suivants peuvent contenir le fichier CustomDCCloneAllowList.xml :  
  
1.  HKey_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters  
  
    AllowListFolder (*REG_SZ*)  
  
2.  DSA Working Directory (répertoire de travail DSA)  
  
3.  %windir%\NTDS  
  
4.  Médias de lecture/écriture amovibles, en fonction de l’ordre de la lettre qui identifie le lecteur, sur la racine du lecteur.  
  
Vous pouvez exécuter New-ADDCCloneConfigFile avec l'argument **-offline** (également appelé mode hors connexion) pour créer le fichier DcCloneConfig.xml et le placer à l'emplacement approprié. Les exemples suivants montrent comment exécuter New-ADDCCloneConfigFile en mode hors connexion.  
  
Pour créer un contrôleur de domaine clone nommé CloneDC1 en mode hors connexion, dans un site nommé « REDMOND » avec une adresse IPv4 statique, tapez :  
  
```  
New-ADDCCloneConfigFile -Offline -CloneComputerName CloneDC1 -SiteName REDMOND -IPv4Address "10.0.0.2" -IPv4DNSResolver "10.0.0.1" -IPv4SubnetMask "255.255.0.0" -IPv4DefaultGateway "10.0.0.1" -Static -Path F:\Windows\NTDS  
```  
  
Pour créer un contrôleur de domaine clone appelé Clone2 en mode hors connexion avec des paramètres IPv4 et IPv6 statiques, tapez :  
  
```  
New-ADDCCloneConfigFile -Offline -IPv4Address "10.0.0.2" -IPv4DNSResolver "10.0.0.1" -IPv4SubnetMask "255.255.0.0" -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -CloneComputerName "Clone2" -PreferredWINSServer "10.0.0.1" -AlternateWINSServer "10.0.0.3" -Path F:\Windows\NTDS  
```  
  
Pour créer un contrôleur de domaine clone en mode hors connexion avec des paramètres IPv4 statiques et des paramètres IPv6 dynamiques et spécifier plusieurs serveurs DNS pour les paramètres de résolution DNS, tapez :  
  
```  
New-ADDCCloneConfigFile -Offline -IPv4Address "10.0.0.10" -IPv4SubnetMask "255.255.0.0" -IPv4DefaultGateway "10.0.0.1" -IPv4DNSResolver @( "10.0.0.1","10.0.0.2" ) -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -Path F:\Windows\NTDS   
```  
  
Pour créer un contrôleur de domaine clone appelé Clone1 en mode hors connexion avec des paramètres IPv4 dynamiques et des paramètres IPv6 statiques, tapez :  
  
```  
New-ADDCCloneConfigFile -Offline -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -CloneComputerName "Clone1" -PreferredWINSServer "10.0.0.1" -AlternateWINSServer "10.0.0.3" -SiteName "REDMOND" -Path F:\Windows\NTDS  
```  
  
Pour créer un contrôleur de domaine clone en mode hors connexion avec des paramètres IPv4 et IPv6 dynamiques, tapez :  
  
```  
New-ADDCCloneConfigFile -Offline -IPv4DNSResolver "10.0.0.1" -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -Path F:\Windows\NTDS  
  
```  
  
##### <a name="windows-explorer-method"></a>Méthode basée sur l'Explorateur Windows  
Windows Server 2012 offre désormais une option graphique pour le montage des fichiers VHD et VHDX. Cela demande l'installation de la fonctionnalité « Expérience utilisateur » de Windows Server 2012.  
  
1.  Cliquez sur le fichier VHD/VHDX récemment copié et qui contient le dossier correspondant à l'emplacement du lecteur système ou du répertoire de travail DSA du contrôleur de domaine source, puis cliquez sur **Monter** dans le menu **Outils d'image de disque**.  
  
2.  Dans le lecteur monté, copiez les fichiers XML à un emplacement valide. Vous pouvez être invité à fournir des autorisations d'accès au dossier.  
  
3.  Cliquez sur le lecteur monté, puis sur **Éjecter** dans le menu **Outils d'image de disque**.  
  
![Déploiement de DC virtualisé](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVClickMountedDrive.png)  
  
![Déploiement de DC virtualisé](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVDetailsMountedDrive.gif)  
  
![Déploiement de DC virtualisé](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVEjectMountedDrive.gif)  
  
##### <a name="windows-powershell-method"></a>Méthode basée sur Windows PowerShell  
Vous pouvez également monter le disque hors connexion et copier le fichier XML à l'aide des applets de commande Windows PowerShell :  
  
```  
mount-vhd  
get-disk  
get-partition  
get-volume  
Add-PartitionAccessPath  
Copy-Item  
```  
  
Cela vous permet d'avoir un contrôle complet sur le processus. Par exemple, vous pouvez monter le lecteur avec une lettre de lecteur spécifique, copier le fichier, puis démonter le lecteur.  
  
```  
mount-vhd <disk path> -passthru -nodriveletter | get-disk | get -partition | get-volume | get-partition | where {$_.partition number -eq 2} | Add-PartitionAccessPath -accesspath <drive letter>  
  
copy-item <xml file path><destination path>\dccloneconfig.xml  
  
dismount-vhd <disk path>  
```  
  
Par exemple :  
  
![Déploiement de DC virtualisé](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSMountVHD.png)  
  
Vous pouvez également utiliser la nouvelle applet de commande **Mount-DiskImage** pour monter un fichier VHD (ou ISO).  
  
### <a name="step-8---create-the-new-virtual-machine"></a>Étape 8 - Créer l'ordinateur virtuel  
La dernière étape de configuration avant le démarrage du processus de clonage est la création d'un ordinateur virtuel qui utilise les disques du contrôleur de domaine source copié. Selon le choix effectué durant la phase de copie de disques, vous avez deux possibilités :  
  
1.  Associer un nouvel ordinateur virtuel au disque copié  
  
2.  Importer l'ordinateur virtuel exporté  
  
#### <a name="associating-a-new-vm-with-copied-disks"></a>Association d'un nouvel ordinateur virtuel à des disques copiés  
Si vous avez copié le disque système manuellement, vous devez créer un ordinateur virtuel à l'aide du disque copié. L'hyperviseur définit automatiquement l'ID de génération d'ordinateur virtuel quand un ordinateur virtuel est créé. Aucun changement de configuration n'est nécessaire sur l'ordinateur virtuel ou l'hôte Hyper-V.  
  
![Déploiement de DC virtualisé](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVConnectVHD.gif)  
  
##### <a name="hyper-v-manager-method"></a>Méthode basée sur le Gestionnaire Hyper-V  
  
1.  Créez un ordinateur virtuel.  
  
2.  Spécifiez le nom de l'ordinateur virtuel, sa quantité de mémoire et son réseau.  
  
3.  Dans la page Connecter un disque dur virtuel, spécifiez le disque système copié.  
  
4.  Terminez l'Assistant pour créer l'ordinateur virtuel.  
  
S'il existe plusieurs disques, cartes réseau ou autres personnalisations, configurez-les avant de démarrer le contrôleur de domaine. La méthode « Exportation/Importation » pour copier les disques est recommandée dans le cas des ordinateurs virtuels complexes.  
  
##### <a name="windows-powershell-method"></a>Méthode basée sur Windows PowerShell  
Vous pouvez utiliser le module Windows PowerShell pour Hyper-V pour automatiser la création d'ordinateurs virtuels dans Windows Server 2012, à l'aide de l'applet de commande suivante :  
  
```  
New-VM  
```  
  
Par exemple, l'ordinateur virtuel DC4-CLONEDFROMDC2 est créé avec 1 Go de RAM. Il démarre à partir du fichier c:\vm\dc4-systemdrive-clonedfromdc2.vhd et utilise le réseau virtuel 10.0 :  
  
![Déploiement de DC virtualisé](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSNewVM.png)  
  
#### <a name="import-vm"></a>Importer l'ordinateur virtuel  
Si vous avez déjà exporté votre ordinateur virtuel, vous devez maintenant le réimporter sous forme de copie. Le code XML exporté permet de recréer l'ordinateur avec tous les paramètres antérieurs relatifs aux lecteurs, aux réseaux et à la mémoire.  
  
Si vous avez l'intention de créer des copies supplémentaires du même ordinateur virtuel exporté, effectuez autant de copies que nécessaire de ce dernier. Utilisez ensuite l'option Importer pour chaque copie.  
  
> [!IMPORTANT]  
> Il est important d'utiliser l'option **Copier** , car l'exportation conserve toutes les informations de la source. L'importation **par déplacement** ou l'importation **sur place** du serveur entraîne une collision des informations, si l'opération est effectuée sur le même serveur hôte Hyper-V.  
  
##### <a name="hyper-v-manager-method"></a>Méthode basée sur le Gestionnaire Hyper-V  
Pour importer à l'aide du composant logiciel enfichable Gestionnaire Hyper-V :  
  
1.  Cliquez sur **Importer un ordinateur virtuel**.  
  
2.  Dans la page **Localiser le dossier** , sélectionnez le fichier de définition d'ordinateur virtuel exporté à l'aide du bouton Parcourir.  
  
3.  Dans la page **Sélectionner l'ordinateur virtuel**, cliquez sur l'ordinateur source.  
  
4.  Dans la page **Choisir le type d'importation** , cliquez sur **Copier l'ordinateur virtuel (créer un ID unique)** , puis sur **Terminer**.  
  
5.  Renommez l'ordinateur virtuel importé si vous effectuez l'importation sur le même hôte Hyper-V. Il aura le même nom que le contrôleur de domaine source exporté.  
  
![Déploiement de DC virtualisé](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVImportLocateFolder.png)  
  
![Déploiement de DC virtualisé](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVImportSelectVM.png)  
  
![Déploiement de DC virtualisé](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVImportChooseType.gif)  
  
N'oubliez pas d'enlever toutes les captures instantanées importées, à l'aide du composant logiciel enfichable Gestionnaire Hyper-V :  
  
![Déploiement de DC virtualisé](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVImportDelSnap.gif)  
  
> [!WARNING]  
> La suppression des captures instantanées importées est très importante. En effet, si celles-ci sont appliquées, elles restaurent le contrôleur de domaine cloné à l'état d'un contrôleur de domaine antérieur (et éventuellement dynamique), ce qui risque d'entraîner un échec de la réplication, une duplication des informations IP et d'autres problèmes.  
  
##### <a name="windows-powershell-method"></a>Méthode basée sur Windows PowerShell  
Vous pouvez utiliser le module Windows PowerShell pour Hyper-V pour automatiser l'importation d'ordinateurs virtuels dans Windows Server 2012, à l'aide des applets de commande suivantes :  
  
```  
Import-VM  
Rename-VM  
```  
  
Par exemple, l'ordinateur virtuel VM DC2-CLONED est importé à l'aide de son fichier XML déterminé automatiquement, puis renommé immédiatement en nouvel ordinateur virtuel DC5-CLONEDFROMDC2 :  
  
![Déploiement de DC virtualisé](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSImportVM.png)  
  
N'oubliez pas d'enlever toutes les captures instantanées importées, à l'aide des applets de commande suivantes :  
  
```  
Get-VMSnapshot  
Remove-VMSnapshot  
```  
  
Par exemple :  
  
![Déploiement de DC virtualisé](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSGetVMSnap.png)  
  
> [!WARNING]
> Assurez-vous que, durant l'importation de l'ordinateur, des adresses MAC statiques n'ont pas été affectées au contrôleur de domaine source. Si un ordinateur source avec une adresse MAC statique est cloné, les ordinateurs copiés n'enverront ou ne recevront pas correctement le trafic réseau. Définissez une nouvelle adresse MAC statique ou dynamique unique, le cas échéant. Vous pouvez déterminer si un ordinateur virtuel utilise des adresses MAC statiques avec la commande suivante :  
> 
> **Obtient-VM-VMName**   
>  ***test-machine virtuelle* | Récupération-VMNetworkAdapter | \\FL***  
  
### <a name="step-9---clone-the-new-virtual-machine"></a>Étape 9 - Cloner le nouvel ordinateur virtuel  
Éventuellement, avant de commencer le clonage, redémarrez le contrôleur de domaine source clone hors connexion. Assurez-vous que l'émulateur de contrôleur de domaine principal est en ligne.  
  
Pour commencer le clonage, démarrez simplement le nouvel ordinateur virtuel. Le processus démarre automatiquement et le contrôleur de domaine redémarre automatiquement après la fin du clonage.  
  
> [!IMPORTANT]  
> Il est déconseillé de garder les contrôleurs de domaine hors tension pendant une période prolongée. Si le clone se joint au même site que son contrôleur de domaine source, la création de la topologie de réplication intrasite et intersite initiale risque de prendre plus de temps, si le contrôleur de domaine source est hors connexion.  
  
Si vous utilisez Windows PowerShell pour démarrer un ordinateur virtuel, la nouvelle applet de commande du module Hyper-V est :  
  
```  
Start-VM  
```  
  
Par exemple :  
  
![Déploiement de DC virtualisé](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSStartVM.png)  
  
Une fois que l'ordinateur a redémarré après la fin du clonage, il est désormais un contrôleur de domaine. Vous pouvez vous connecter normalement pour vérifier son bon fonctionnement. Si des erreurs se produisent, le serveur est configuré pour démarrer en mode de restauration des services d'annuaire à des fins de diagnostic.  
  
## <a name="virtualization-safeguards"></a><a name="BKMK_VDCSafeRestore"></a>Protections de la virtualisation  
Contrairement au clonage de contrôleur de domaine virtualisé, les dispositifs de protection en matière de virtualisation de Windows Server 2012 ne comportent pas d'étapes de configuration. La fonctionnalité est opérationnelle sans intervention, à condition de respecter certaines conditions simples :  
  
-   L'hyperviseur prend en charge les ID de génération d'ordinateur virtuel.  
  
-   Il existe un contrôleur de domaine partenaire valide dont les changements peuvent faire l'objet d'une réplication ne faisant pas autorité par un contrôleur de domaine restauré.  
  
### <a name="validate-the-hypervisor"></a>Valider l'hyperviseur  
Vérifiez que le contrôleur de domaine source s'exécute sur un hyperviseur pris en charge en consultant la documentation du fournisseur. Les contrôleurs de domaine virtualisés sont indépendants de l'hyperviseur et ne demandent pas obligatoirement Hyper-V.  
  
Consultez la section précédente [Conditions requises par la plateforme](../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Deployment-and-Configuration.md#BKMK_PlatformReqs) pour connaître les modalités de prise en charge des ID de génération d'ordinateur virtuel.  
  
Si vous effectuez la migration d'ordinateurs virtuels d'un hyperviseur source vers un hyperviseur cible distinct, les dispositifs de protection en matière de virtualisation peuvent éventuellement se déclencher, selon que les hyperviseurs prennent en charge ou non les ID de génération d'ordinateur virtuel, comme expliqué dans le tableau suivant.  
  
|Hyperviseur source|Hyperviseur cible|Résultat|  
|---------------------|---------------------|----------|  
|Prend en charge les ID de génération d'ordinateur virtuel|Ne prend pas en charge les ID de génération d'ordinateur virtuel|Dispositifs de protection non déclenchés (si un fichier DCCloneConfigFile.xml est présent, le contrôleur de domaine démarre en mode DSRM)|  
|Ne prend pas en charge les ID de génération d'ordinateur virtuel|Prend en charge les ID de génération d'ordinateur virtuel|Dispositifs de protection déclenchés|  
|Prend en charge les ID de génération d'ordinateur virtuel|Prend en charge les ID de génération d'ordinateur virtuel|Dispositifs de protection déclenchés, car la définition d'ordinateur virtuel n'a pas changé, ce qui signifie que l'ID de génération d'ordinateur virtuel reste le même|  
  
### <a name="validate-the-replication-topology"></a>Valider la topologie de réplication  
Les dispositifs de protection en matière de virtualisation démarrent une réplication entrante ne faisant pas autorité pour le delta de la réplication Active Directory, ainsi qu'une resynchronisation ne faisant pas autorité de l'ensemble du contenu de SYSVOL. Cela permet de garantir que le contrôleur de domaine est restauré avec toutes ses fonctionnalités à partir d'une capture instantanée, et qu'il est cohérent avec le reste de l'environnement.  
  
Cette nouvelle fonctionnalité s'accompagne de plusieurs conditions requises et restrictions :  
  
-   Un contrôleur de domaine restauré doit être capable de contacter un contrôleur de domaine accessible en écriture  
  
-   Tous les contrôleurs de domaine d'un domaine ne doivent pas être restaurés simultanément  
  
-   Tout changement provenant d'un contrôleur de domaine restauré et qui n'a pas encore été répliqué en sortie depuis l'exécution de la capture instantanée est définitivement perdu  
  
Bien que la section de résolution des problèmes couvre ces scénarios, les détails ci-dessous vous permettent de vérifier que vous ne créez pas de topologie qui puisse causer des problèmes.  
  
#### <a name="writable-domain-controller-availability"></a>Disponibilité du contrôleur de domaine accessible en écriture  
Si un contrôleur de domaine est restauré, il doit disposer d'une connectivité vers un contrôleur de domaine accessible en écriture. Un contrôleur de domaine en lecture seule ne peut pas envoyer le delta des mises à jour. La topologie est déjà probablement correcte, car un contrôleur de domaine accessible en écriture a toujours besoin d'un partenaire accessible en écriture. Toutefois, si tous les contrôleurs de domaine accessibles en écriture sont en cours de restauration simultanément, aucun d'eux ne pourra trouver une source valide. Il en va de même si les contrôleurs de domaine accessibles en écriture sont hors connexion pour raison de maintenance, ou s'ils ne sont pas joignables par le réseau.  
  
#### <a name="simultaneous-restore"></a>Restauration simultanée  
Ne restaurez pas tous les contrôleurs de domaine en même temps dans un seul domaine. Si toutes les captures instantanées sont restaurées en même temps, la réplication Active Directory fonctionne normalement mais la réplication SYSVOL s'arrête. L'architecture de restauration des services FRS et DFSR nécessite que leur instance de réplica soit définie en mode de synchronisation ne faisant pas autorité. Si tous les contrôleurs de domaine sont restaurés en même temps, et si chaque contrôleur de domaine se marque comme ne faisant pas autorité pour SYSVOL, ils essaieront tous de se synchroniser aux stratégies de groupe et aux scripts d'un partenaire faisant autorité. À ce stade, cependant, aucun des partenaires ne fait autorité.  
  
> [!IMPORTANT]  
> Si tous les contrôleurs de domaine sont restaurés en même temps, utilisez les articles suivants pour définir un seul contrôleur de domaine (généralement, l'émulateur de contrôleur de domaine principal) comme faisant autorité. Ainsi, les autres contrôleurs de domaine peuvent reprendre un fonctionnement normal :  
>   
> [Utilisation de la clé de Registre BurFlags pour réinitialiser le jeu de réplica du service de réplication de fichiers](https://support.microsoft.com/kb/290762)  
>   
> [Comment forcer une synchronisation faisant autorité et ne faisant pas autorité pour un SYSVOL répliqué par DFSR (comme « D4/D2 » pour le service FRS)](https://support.microsoft.com/kb/2218556)  
  
> [!WARNING]  
> Ne faites pas s'exécuter tous les contrôleurs de domaine d'une forêt ou d'un domaine sur le même hyperviseur hôte. Cela introduit un point de défaillance unique qui paralyse les services AD DS, Exchange, SQL et d'autres opérations d'entreprise, chaque fois que l'hyperviseur passe en mode hors connexion. Cela revient au même que d'utiliser un seul contrôleur de domaine pour tout un domaine ou toute une forêt. Plusieurs contrôleurs de domaine sur plusieurs plateformes permettent de fournir des fonctionnalités de redondance et de tolérance de panne.  
  
#### <a name="post-snapshot-replication"></a>Réplication postérieure à une capture instantanée  
Ne restaurez pas les captures instantanées tant que tous les changements effectués localement depuis la création de la capture instantanée n'ont pas été répliqués en sortie. Les changements effectués à l'origine sont perdus à jamais si d'autres contrôleurs de domaine ne les ont pas reçus au préalable par réplication.  
  
Utilisez Repadmin.exe pour afficher les changements sortants non répliqués entre un contrôleur de domaine et ses partenaires :  
  
1.  Retournez les noms des partenaires du contrôleur de domaine et les GUID d'objet DSA avec :  
  
    ```  
    Repadmin.exe /showrepl <DC Name of the partner> /repsto  
    ```  
  
2.  Retournez la réplication entrante en attente du contrôleur de domaine partenaire au contrôleur de domaine à restaurer :  
  
    ```  
    Repadmin.exe /showchanges < Name of partner DC><DSA Object GUID of the domain controller being restored><naming context to compare>  
    ```  
  
Sinon, il est possible de voir simplement le nombre de changements non répliqués :  
  
```  
Repadmin.exe /showchanges <Name of partner DC><DSA Object GUID of the domain controller being restored><naming context to compare> /statistics  
```  
  
Par exemple (avec une sortie changée pour une meilleure lisibilité et les entrées importantes en ***italique***), vous pouvez voir ici les partenariats de réplication de DC4 :  
  
```  
C:\>repadmin.exe /showrepl dc4.corp.contoso.com /repsto  
  
Default-First-Site-Name\DC4  
DSA Options: IS_GC  
Site Options: (none)  
DSA object GUID: 5d083398-4bd3-48a4-a80d-fb2ebafb984f  
DSA invocationID: 730fafec-b6d4-4911-88f2-5b64e48fc2f1  
  
==== OUTBOUND NEIGHBORS FOR CHANGE NOTIFICATIONS ============  
  
DC=corp,DC=contoso,DC=com  
    Default-First-Site-Name\DC3 via RPC  
        DSA object GUID: f62978a8-fcf7-40b5-ac00-40aa9c4f5ad3  
        Last attempt @ 2011-11-11 15:04:12 was successful.  
    Default-First-Site-Name\DC2 via RPC  
        DSA object GUID: 3019137e-d223-4b62-baaa-e241a0c46a11  
        Last attempt @ 2011-11-11 15:04:15 was successful.  
```  
  
À présent, vous savez qu'il se réplique avec DC2 et DC3. Vous affichez ensuite la liste des changements que DC2 indique ne pas avoir encore reçus de la part de DC4, et vous constatez qu'il existe un nouveau groupe :  
  
```  
C:\>repadmin /showchanges dc2.corp.contoso.com 5d083398-4bd3-48a4-a80d-fb2ebafb984f dc=corp,dc=contoso,dc=com  
  
==== SOURCE DSA: (null) ====  
Objects returned: 1  
(0) add CN=newgroup4,CN=Users,DC=corp,DC=contoso,DC=com  
    1> parentGUID: 55fc995a-04f4-4774-b076-d6a48ac1af99  
    1> objectGUID: 96b848a2-df1d-433c-a645-956cfbf44086  
    2> objectClass: top; group  
    1> instanceType: 0x4 = ( WRITE )  
    1> whenCreated: 11/11/2011 3:03:57 PM Eastern Standard Time  
```  
  
Vous pouvez également tester l'autre partenaire pour vous assurer qu'il n'est pas déjà répliqué.  
  
Toutefois, si cela ne vous intéresse pas de savoir quels sont les objets non répliqués et si vous êtes seulement intéressé par les objets en attente, utilisez l'option **/statistics** :  
  
```  
C:\>repadmin /showchanges dc2.corp.contoso.com 5d083398-4bd3-48a4-a80d-fb2ebafb984f dc=corp,dc=contoso,dc=com /statistics  
  
***********************************************  
********* Grand total *************************  
Packets:              1  
Objects:              1Object Additions:     1Object Modifications: 0Object Deletions:     0Object Moves:         0Attributes:           12Values:               13  
```  
  
> [!IMPORTANT]  
> Testez tous les partenaires accessibles en écriture si vous constatez des défaillances ou une réplication en attente. Tant qu'il existe au moins une convergence, vous pouvez restaurer sans risque la capture instantanée, car la réplication compare et modifie en conséquence les autres serveurs.  
>   
> Veillez à noter les erreurs de réplication indiquées par /showchanges et corrigez-les avant de continuer.  
  
### <a name="windows-powershell-snapshot-cmdlets"></a>Applets de commande Windows PowerShell pour captures instantanées  
Les applets de commande suivantes du module Windows PowerShell pour Hyper-V fournissent des fonctionnalités de capture instantanée dans Windows Server 2012 :  
  
```  
Checkpoint-VM  
Export-VMSnapshot  
Get-VMSnapshot  
Remove-VMSnapshot  
Rename-VMSnapshot  
Restore-VMSnapshot  
```  
  


