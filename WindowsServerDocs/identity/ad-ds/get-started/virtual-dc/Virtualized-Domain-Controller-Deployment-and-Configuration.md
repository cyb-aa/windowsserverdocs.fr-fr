---
ms.assetid: b146f47e-3081-4c8e-bf68-d0f993564db2
title: "Configuration et déploiement de contrôleur de domaine virtualisés"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: dcb377cef003458bcdf2e4b3564167cee4310020
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="virtualized-domain-controller-deployment-and-configuration"></a>Configuration et déploiement de contrôleur de domaine virtualisés

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Cette rubrique couvre:  
  
-   [Remarques concernant l’installation](../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Deployment-and-Configuration.md#BKMK_InstallConsiderations)  
  
    Cela inclut les spécifications de plateforme et d’autres contraintes importantes.  
  
-   [Domaine virtualisés le clonage du contrôleur](../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Deployment-and-Configuration.md#BKMK_VDCCloning)  
  
    Cette rubrique explique en détail le contrôleur de domaine virtualisé entier du processus de clonage.  
  
-   [Restauration sécurisée des contrôleurs de domaine virtualisés](../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Deployment-and-Configuration.md#BKMK_VDCSafeRestore)  
  
    Cette rubrique explique en détail les validations effectuées durant la restauration sécurisée des contrôleurs de domaine virtualisés.  
  
## <a name="BKMK_InstallConsiderations"></a>Remarques concernant l’installation  
Il n’existe aucun rôle spécial ou l’installation des composants pour les contrôleurs de domaine virtualisés; tous les contrôleurs de domaine contiennent automatiquement les fonctions de restauration de clonage et en toute sécurité. Vous ne pouvez pas supprimer ou désactiver ces fonctionnalités.  
  
Utilisation de contrôleurs de domaine Windows Server 2012 nécessite un schéma de service d’annuaire Active Directory Windows Server 2012 version 56 ou supérieure et le niveau fonctionnel de forêt supérieur ou égal à Windows Server 2003 natif.  
  
Les deux contrôleurs de domaine accessible en écriture et en lecture seule prennent en charge tous les aspects de contrôleur de domaine virtualisé, comme les catalogues globaux et les rôles FSMO.  
  
> [!IMPORTANT]  
> Le détenteur du rôle FSMO d’émulateur de contrôleur de domaine principal doit être en ligne quand le clonage commence.  
  
### <a name="BKMK_PlatformReqs"></a>Exigences de plates-formes  
Le clonage de contrôleur de domaine virtualisé nécessite:  
  
-   Rôle FSMO d’émulateur de contrôleur de domaine principal hébergé sur un contrôleur de domaine Windows Server 2012  
  
-   Émulateur de contrôleur de domaine principal disponible durant les opérations de clonage.  
  
À la fois clonage et la restauration sécurisée nécessitent:  
  
-   Invités virtualisés Windows Server 2012  
  
-   Plateforme hôte de virtualisation prend en charge les ID de génération d’ordinateur virtuel (VMGID)  
  
Passez en revue le tableau ci-dessous pour les produits de virtualisation et si elles prennent en charge les contrôleurs de domaine virtualisés et l’ID de génération d’ordinateur virtuel.  
  
|||  
|-|-|  
|**Solution de virtualisation**|**Prend en charge les contrôleurs de domaine virtualisés et VMGID**|  
|**Serveur Microsoft Windows Server 2012 avec Hyper-V**|Oui|  
|**Microsoft Windows Server 2012 Hyper-V Server**|Oui|  
|**Fonctionnalité de Microsoft Windows 8 avec Hyper-V Client**|Oui|  
|**Windows Server 2008 R2 et Windows Server 2008**|N°|  
|**Solutions de virtualisation non Microsoft**|Contactez le fournisseur|  
  
Même si Microsoft prend en charge Windows 7 Virtual PC, Virtual PC 2007, Virtual PC 2004 et Virtual Server 2005, ils ne peuvent pas exécuter d’invités 64 bits, ni qu’ils prennent en charge ID de génération.  
  
Aide sur les produits de virtualisation tiers et leurs possibilités de prise en charge avec les contrôleurs de domaine virtualisés, contactez directement ce fournisseur.  
  
Pour plus d’informations, consultez la politique de Support pour [logiciels Microsoft qui exécutent des logiciels de virtualisation matérielle non Microsoft](https://support.microsoft.com/kb/897615).  
  
### <a name="critical-caveats"></a>Avertissements critiques  
Effectuez des contrôleurs de domaine virtualisés *pas* prennent en charge la restauration sécurisée des opérations suivantes:  
  
-   Fichiers VHD et VHDX copiés manuellement les fichiers de disque dur virtuel existants  
  
-   Fichiers VHD et VHDX restaurés à l’aide du logiciel de sauvegarde de disque complet ou de sauvegarde de fichiers  
  
> [!NOTE]  
> Fichiers VHDX sont nouveaux pour Windows Server 2012 Hyper-V.  
  
Aucune de ces opérations n’est couverte par la sémantique de l’ID de génération et par conséquent, ne modifiez pas l’ID de génération d’ordinateur virtuel. La restauration peuvent entraîner l’utilisation de ces méthodes de contrôleurs de domaine dans une restauration USN et une mise en quarantaine du contrôleur de domaine ou introduire des objets en attente et la nécessité pour les opérations de nettoyage à l’échelle de forêt.  
  
> [!WARNING]  
> Restauration sécurisée des contrôleurs de domaine virtualisés n’est pas un remplacement pour les sauvegardes de l’état système et la Corbeille AD DS.  
>   
> Après la restauration d’une capture instantanée, les deltas des changements non répliqués précédemment en provenance de ce contrôleur de domaine après la capture instantanée sont définitivement perdues. Restauration sécurisée implémente une restauration ne faisant pas autorité automatisée pour éviter la mise en quarantaine du contrôleur de domaine accidentelle *uniquement*.  
  
Pour plus d’informations sur les bulles USN et les objets en attente, voir [les opérations de résolution des problèmes Active Directory qui échouent avec l’erreur 8606: «des attributs insuffisants ont été donnés pour créer un objet»](https://support.microsoft.com/kb/2028495).  
  
## <a name="BKMK_VDCCloning"></a>Domaine virtualisés le clonage du contrôleur  
Il existe un nombre d’étapes et les étapes pour le clonage d’un contrôleur de domaine virtualisé, indépendamment de l’utilisation des outils graphiques ou Windows PowerShell. À un niveau élevé, les trois phases sont:  
  
**Préparer l’environnement**  
  
-   Étape 1: Vérifier que l’hyperviseur prend en charge les ID de génération et par conséquent, le clonage  
  
-   Étape 2: Vérifier que le rôle d’émulateur de contrôleur de domaine principal est hébergé par un contrôleur de domaine qui exécute Windows Server 2012 et qu’il est en ligne et accessible par le contrôleur de domaine cloné durant le clonage.  
  
**Préparer le contrôleur de domaine source**  
  
-   Étape 3: Autoriser le contrôleur de domaine source pour le clonage  
  
-   Étape 4: Supprimer des services ou programmes incompatibles, ou les ajouter au fichier CustomDCCloneAllowList.xml.  
  
-   Étape 5: Créer DCCloneConfig.xml  
  
-   Étape 6: Mettre le contrôleur de domaine source hors connexion  
  
**Créer le contrôleur de domaine cloné**  
  
-   Étape 7: Copier ou exporter l’ordinateur virtuel source et ajoutez le code XML s’il n’a pas déjà été copié  
  
-   Étape 8: Créer un nouvel ordinateur virtuel à partir de la copie  
  
-   Étape 9: Démarrer le nouvel ordinateur virtuel pour commencer le clonage  
  
Il n’existe aucune différence dans l’opération procédurales lors de l’utilisation des outils graphiques tels que la Console de gestion Hyper-V ou des outils de ligne de commande tels que Windows PowerShell, afin de ces étapes sont présentées qu’une seule fois pour les deux interfaces. Cette rubrique fournit des exemples de Windows PowerShell pour vous permettre d’Explorer l’automatisation de bout en bout du processus de clonage; ils ne sont pas requis pour toutes les étapes. Il n’existe aucun outil de gestion graphique pour les contrôleurs de domaine virtualisés inclus dans Windows Server 2012.  
  
Il existe plusieurs points dans la procédure où vous avez le choix pour la création de l’ordinateur cloné et comment ajouter les fichiers xml; ces étapes sont décrites dans les détails ci-dessous. Le processus est irréversible.  
  
Le diagramme suivant illustre le processus contrôleur clonage de domaine virtualisé, où le domaine existe déjà.  
  
![Déploiement de contrôleur de domaine virtualisé](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_CloningProcessFlow.png)  
  
### <a name="step-1---validate-the-hypervisor"></a>Étape 1 - valider l’hyperviseur  
Vérifiez que le contrôleur de domaine source est en cours d’exécution sur un hyperviseur pris en charge en consultant la documentation du fournisseur. Contrôleurs de domaine virtualisés sont indépendants de l’hyperviseur et ne nécessitent pas d’Hyper-V.  
  
Si l’hyperviseur est Microsoft Hyper-V, assurez-vous qu’il s’exécute sur Windows Server 2012. Vous pouvez effectuer cette vérification à l’aide de la gestion des périphériques  
  
Ouvrez **Devmgmt.msc** et examinez les **périphériques système** installé pour appareils Microsoft Hyper-V et pilotes. Le périphérique système spécifique nécessaire à un contrôleur de domaine virtualisé est le **compteur de génération Microsoft Hyper-V** (pilote: vmgencounter.sys).  
  
![Déploiement de contrôleur de domaine virtualisé](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVVMGenIDCounter.png)  
  
### <a name="step-2---verify-the-pdce-fsmo-role"></a>Étape 2: vérifier le rôle FSMO PDCE  
Avant de tenter de cloner un contrôleur de domaine, vous devez vérifier que le contrôleur de domaine qui héberge le FSMO d’émulateur de contrôleur de domaine principal exécute Windows Server 2012. L’émulateur de contrôleur de domaine principal (PDCE) est nécessaire pour plusieurs raisons:  
  
1.  L’émulateur crée spéciale **contrôleurs de domaine clonables** de groupe et définit son autorisation à la racine du domaine pour permettre à un contrôleur de domaine à se cloner.  
  
2.  Le contrôleur de domaine de clonage contacte l’émulateur directement à l’aide du protocole RPC DRSUAPI, afin de créer des objets ordinateur pour le contrôleur de domaine clone.  
  
    > [!NOTE]  
    > Windows Server 2012 étend le protocole distant Service de réplication de répertoire (DRS) existant (UUID **E3514235-4B06-11D1-AB04-00C04FC2DCD2**) à inclure une nouvelle méthode RPC **IDL_DRSAddCloneDC** (Opnum **28**). Le **IDL_DRSAddCloneDC** méthode crée un nouvel objet de contrôleur de domaine en copiant les attributs d’un objet de contrôleur de domaine existant.  
    >   
    > Les États d’un contrôleur de domaine sont composés d’objets ordinateur, serveur, NTDS les paramètres, FRS, DFSR et connexion conservées pour chaque contrôleur de domaine. Lors de la duplication d’un objet, cette méthode RPC remplace toutes les références au contrôleur de domaine d’origine par les objets correspondants du nouveau contrôleur de domaine. L’appelant doit avoir le contrôle d’accès droit DS-Clone-contrôleur de domaine sur le contexte d’attribution de noms de domaine.  
    >   
    > L’utilisation de cette nouvelle méthode nécessite toujours un accès direct au contrôleur de domaine d’émulateur PDC à partir de l’appelant.  
    >   
    > Étant donné que cette méthode RPC est nouvelle, votre logiciel d’analyse réseau nécessite des analyseurs mis à jour pour inclure les champs du nouvel Opnum 28 dans l’UUID E3514235-4B06 - 11D 1-AB04-00C04FC2DCD2 existant. Dans le cas contraire, vous ne pouvez pas analyser ce trafic.  
    >   
    > Pour plus d’informations, voir [4.1.29 IDL_DRSAddCloneDC (Opnum 28)](https://msdn.microsoft.com/en-us/library/hh554213(v=prot.13).aspx).  
  
***Cela signifie également qu’à l’aide des réseaux non routés complètement, le clonage de contrôleur de domaine virtualisé demande des segments de réseau d’accéder à l’émulateur***. Il est possible de déplacer un contrôleur de domaine cloné vers un autre réseau après le clonage - comme un contrôleur de domaine physique - tant que vous ne veillez à mettre à jour les informations de site logique AD DS.  
  
> [!IMPORTANT]  
> Lors du clonage d’un domaine qui contient uniquement un seul contrôleur de domaine, vous devez vérifier que le contrôleur de domaine source est remis en ligne avant le démarrage des copies clones. Un domaine de production doit toujours contenir au moins deux contrôleurs de domaine.  
  
#### <a name="active-directory-users-and-computers-method"></a>Utilisateurs Active Directory et la méthode d’ordinateurs  
  
1.  À l’aide du composant logiciel enfichable DSA.msc, cliquez avec le bouton droit sur le domaine et cliquez sur **maîtres d’opérations**. Notez le contrôleur de domaine nommé sous l’onglet CDP et fermer la boîte de dialogue.  
  
2.  Avec le bouton droit d’objet d’ordinateur du contrôleur de domaine, puis cliquez sur **propriétés**, puis de valider les informations du système d’exploitation.  
  
#### <a name="windows-powershell-method"></a>Méthode de Windows PowerShell  
Vous pouvez combiner les applets de commande Module Active Directory Windows PowerShell suivante pour retourner la version de l’émulateur de contrôleur de domaine principal:  
  
```  
Get-adddomaincontroller  
Get-adcomputer  
```  
  
Si non fourni le domaine, ces applets de commande Supposons que le domaine de l’ordinateur sur lequel exécuter.  
  
La commande suivante retourne les informations PDCE et système d’exploitation:  
  
```  
get-adcomputer(Get-ADDomainController -Discover -Service "PrimaryDC").name -property * | format-list dnshostname,operatingsystem,operatingsystemversion  
```  
  
Cet exemple ci-dessous montre comment spécifier le nom de domaine et filtrer les propriétés retournées avant le pipeline Windows PowerShell:  
  
![Déploiement de contrôleur de domaine virtualisé](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PDCOSInfo.png)  
  
### <a name="step-3---authorize-a-source-dc"></a>Étape 3 - autoriser une contrôleur de domaine Source  
Le contrôleur de domaine source doit avoir le contrôle droit d’accès (voiture) **autoriser un contrôleur de domaine créer un clone de lui-même** sur le sommet du contexte de nommage de domaine. Par défaut, le groupe bien connu **contrôleurs de domaine clonables** a cette autorisation et ne contient aucun membre. L’émulateur crée ce groupe lorsque le rôle FSMO est transféré à un contrôleur de domaine Windows Server 2012.  
  
#### <a name="active-directory-administrative-center-method"></a>Méthode du centre d’administration Active Directory  
  
1.  Démarrez Dsac.exe et accédez au contrôleur de domaine source, puis ouvrez sa page de détails.  
  
2.  Dans le **membre de** section, ajoutez le **contrôleurs de domaine clonables** groupe pour ce domaine.  
  
#### <a name="windows-powershell-method"></a>Méthode de Windows PowerShell  
Vous pouvez combiner les applets de commande suivantes du Module Active Directory Windows PowerShell **get-adcomputer** et **ajouter-adgroupmember** pour ajouter un contrôleur de domaine pour le **contrôleurs de domaine clonables** groupe:  
  
```  
Get-adcomputer <dc name> | %{add-adgroupmember "cloneable domain controllers" $_.samaccountname}  
```  
  
Par exemple, cela ajoute le serveur DC1 au groupe, sans avoir à spécifier le nom unique du membre du groupe:  
  
![Déploiement de contrôleur de domaine virtualisé](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_AddDcToGroup.png)  
  
#### <a name="rebuilding-default-permissions"></a>La reconstruction des autorisations par défaut  
Si vous supprimez cette autorisation de l’en-tête du domaine, le clonage échoue. Vous pouvez recréer l’autorisation à l’aide du centre d’administration Active Directory ou Windows PowerShell.  
  
##### <a name="active-directory-administrative-center-method"></a>Méthode du centre d’administration Active Directory  
  
1.  Ouvrez **centre d’administration Active Directory**, cliquez sur l’en-tête du domaine, cliquez sur **propriétés**, cliquez sur le **Extensions** , cliquez sur **sécurité**, puis cliquez sur **avancé**. Cliquez sur **cet objet uniquement**.  
  
2.  Cliquez sur **ajouter**, sous **permet d’entrer le nom d’objet à sélectionner**, tapez le nom du groupe **contrôleurs de domaine clonables.**  
  
3.  Sous autorisations, cliquez sur **autoriser un contrôleur de domaine créer un clone de lui-même**, puis cliquez sur **OK**.  
  
> [!NOTE]  
> Vous pouvez également supprimer l’autorisation par défaut et ajouter des contrôleurs de domaine individuels. Cela est susceptible de provoquer des problèmes de maintenance, toutefois, où les nouveaux administrateurs ne seront pas informés de cette personnalisation. Modifier le paramètre par défaut n’augmente pas la sécurité et est déconseillée.  
  
##### <a name="windows-powershell-method"></a>Méthode de Windows PowerShell  
Utilisez les commandes suivantes dans une invite de la console Windows PowerShell administrateur élevés. Ces commandes détectent le nom de domaine et restaurent les autorisations par défaut:  
  
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
  
Vous pouvez également exécuter l’exemple [FixVDCPermissions.ps1](../../../ad-ds/reference/virtual-dc/Virtualized-Domain-Controller-Technical-Reference-Appendix.md#BKMK_FixPDCPerms) dans une console Windows PowerShell, où commence de la console d’administrateur élevés sur un contrôleur de domaine du domaine affecté. Il définie automatiquement les autorisations. L’exemple se trouve dans l’annexe de ce module.  
  
### <a name="step-4---remove-incompatible-applications-or-services-if-not-using-customdccloneallowlistxml"></a>Étape 4 - supprimer applications ou services incompatibles (si vous n’utilisez pas CustomDCCloneAllowList.xml)  
Tous les programmes ou services retournés précédemment par Get-ADDCCloningExcludedApplicationList - *et non ajoutés au fichier CustomDCCloneAllowList.xml* -doivent être supprimés avant le clonage. Désinstallation de l’application ou le service est la méthode recommandée.  
  
> [!WARNING]  
> N’importe quel programme incompatible ou service pas désinstallé ou ajouté au fichier CustomDCCloneAllowList.xml empêche le clonage.  
  
Utilisez l’applet de commande Get-AdComputerServiceAccount pour localiser les comptes de Service autonomes gérés (MSAs) dans le domaine et si cet ordinateur est à l’aide d’un d’eux. Si un compte de service administré est installé, utilisez l’applet de commande Uninstall-ADServiceAccount pour supprimer le compte de service installé localement. Une fois que vous avez fini de mettre le contrôleur de domaine source hors connexion à l’étape 6, vous pouvez rajouter le compte de service administré via Install-ADServiceAccount lorsque le serveur est remis en ligne. Pour plus d’informations, voir [Uninstall-ADServiceAccount](https://technet.microsoft.com/en-us/library/hh852310).  
  
> [!IMPORTANT]  
> Comptes de service administrés autonomes - apparus dans Windows Server 2008 R2 - ont été remplacés dans Windows Server 2012 avec les comptes de service administrés groupe. Comptes de service administrés groupe prend en charge le clonage.  
  
### <a name="step-5---create-dccloneconfigxml"></a>Étape 5 - créer DCCloneConfig.xml  
Le fichier DcCloneConfig.xml est requis pour le clonage des contrôleurs de domaine. Son contenu vous permettre de spécifier des détails uniques comme le nouveau nom d’ordinateur et l’adresse IP.  
  
Le fichier CustomDCCloneAllowList.xml est facultatif, sauf si vous installez des applications ou services Windows potentiellement incompatibles sur le contrôleur de domaine source. Les fichiers nécessitent précis d’attribution de noms, de mise en forme et la sélection élective; dans le cas contraire, le clonage échoue.  
  
Pour cette raison, vous devez toujours utiliser les applets de commande Windows PowerShell pour créer les fichiers XML et les placer à l’emplacement approprié.  
  
#### <a name="generating-with-new-addccloneconfigfile"></a>Génération avec New-ADDCCloneConfigFile  
Le module Active Directory pour Windows PowerShell contient une nouvelle applet de commande dans Windows Server 2012:  
  
```  
New-ADDCCloneConfigFile  
```  
  
Vous exécutez l’applet de commande sur le contrôleur de domaine source que vous souhaitez cloner. L’applet de commande prend en charge plusieurs arguments et lorsqu’il est utilisé, elle teste toujours l’ordinateur et l’environnement dans lequel elle est exécutée, sauf si vous spécifiez l’argument - offline.  
  
||||  
|-|-|-|  
|**Active Directory**<br /><br />**Applet de commande**|**Arguments**|**Explication**|  
|**New-ADDCCloneConfigFile**|*<no argument specified>*|Crée un fichier DcCloneConfig.xml vide dans le répertoire de travail DSA (par défaut: % systemroot%\ntds)|  
||-CloneComputerName|Spécifie le nom d’ordinateur du contrôleur de domaine clone. Type de données chaîne.|  
||-Chemin d’accès|Spécifie le dossier pour créer le fichier DcCloneConfig.xml. Si non spécifié, écrit dans le répertoire de travail DSA (par défaut: % systemroot%\ntds). Type de données chaîne.|  
||-SiteName|Spécifie le nom du site logique Active Directory à joindre durant la création du compte ordinateur cloné. Type de données chaîne.|  
||-IPv4Address|Spécifie l’adresse IPv4 statique de l’ordinateur cloné. Type de données chaîne.|  
||-IPv4SubnetMask|Spécifie le masque de sous-réseau IPv4 statique de l’ordinateur cloné. Type de données chaîne.|  
||-IPv4DefaultGateway|Spécifie l’adresse de passerelle par défaut IPv4 statique de l’ordinateur cloné. Type de données chaîne.|  
||-IPv4DNSResolver|Spécifie les entrées DNS IPv4 statiques de l’ordinateur cloné dans une liste séparée par des virgules. Type de données de tableau. Quatre entrées au maximum peuvent être fournies.|  
||-PreferredWINSServer|Spécifie l’adresse IPv4 statique du serveur WINS principal. Type de données chaîne.|  
||-AlternateWINSServer|Spécifie l’adresse IPv4 statique du serveur WINS secondaire. Type de données chaîne.|  
||-IPv6DNSResolver|Spécifie les entrées DNS IPv6 statiques de l’ordinateur cloné dans une liste séparée par des virgules. Il n’existe aucun moyen de définir des informations Ipv6 statiques de clonage de contrôleur de domaine virtualisé. Type de données de tableau.|  
||-Hors connexion|N’effectue pas les tests de validation et remplace tout fichier dccloneconfig.xml existant. N’a aucun paramètre. Pour plus d’informations, voir [en cours d’exécution de New-ADDCCloneConfigFile en mode hors connexion](../../../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#BKMK_OfflineMode).|  
||*-Statiques*|Obligatoire si vous spécifiez des arguments IP statiques IPv4SubnetMask, IPv4SubnetMask ou IPv4DefaultGateway. N’a aucun paramètre.|  
  
Tests effectués durant l’exécution en mode en ligne:  
  
-   Émulateur de contrôleur de domaine principal est Windows Server 2012 ou version ultérieure  
  
-   Contrôleur de domaine source est un membre du groupe de contrôleurs de domaine clonables  
  
-   Contrôleur de domaine source n’inclut pas toutes les applications ou services exclus  
  
-   Contrôleur de domaine source ne contient-elle pas déjà un fichier DcCloneConfig.xml dans le chemin d’accès spécifié  
  
![Déploiement de contrôleur de domaine virtualisé](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSNewDCCloneConfig.png)  
  
### <a name="step-6---take-the-source-domain-controller-offline"></a>Étape 6 - mettre le contrôleur de domaine Source hors connexion  
Vous ne pouvez pas copier une source en cours d’exécution du contrôleur de domaine; Il doit être arrêté de façon fluide. Ne clonez pas un contrôleur de domaine arrêté par la suite d’une coupure.  
  
#### <a name="graphical-method"></a>Méthode graphique  
Utilisez le bouton d’arrêt dans le contrôleur de domaine en cours d’exécution, ou le bouton d’arrêt du Gestionnaire Hyper-V.  
  
![Déploiement de contrôleur de domaine virtualisé](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_Shutdown.png)  
  
![Déploiement de contrôleur de domaine virtualisé](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVShutdown.png)  
  
#### <a name="windows-powershell-method"></a>Méthode de Windows PowerShell  
Vous pouvez arrêter un ordinateur virtuel à l’aide des applets de commande suivantes:  
  
```  
Stop-computer  
Stop-vm  
```  
  
Stop-computer est une applet de commande qui prend en charge l’arrêt des ordinateurs indépendamment de la virtualisation et est similaire à l’ancien utilitaire Shutdown.exe. Stop-vm est une nouvelle applet de commande dans le module Windows PowerShell de Windows Server 2012 Hyper-V et équivaut aux options d’alimentation dans le Gestionnaire Hyper-V. Ce dernier est utile dans les environnements lab où le contrôleur de domaine opère souvent sur un réseau virtualisé privé.  
  
![Déploiement de contrôleur de domaine virtualisé](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_StopComputer2.png)  
  
![Déploiement de contrôleur de domaine virtualisé](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_StopVM.png)  
  
### <a name="step-7---copy-disks"></a>Étape 7 - copier les disques  
Un choix d’administration est requise dans la phase de copie:  
  
-   Copier les disques manuellement sans Hyper-V  
  
-   Exporter l’ordinateur virtuel, à l’aide d’Hyper-V  
  
-   Exporter les disques fusionnés avec Hyper-V  
  
Tous les disques d’un ordinateur virtuel doivent être copiés, pas seulement le lecteur système. Si le contrôleur de domaine source utilise des disques de différenciation et si vous envisagez de déplacer votre contrôleur de domaine cloné vers un autre hôte Hyper-V, vous devez exporter.  
  
Copie manuelle de disques est recommandée si le contrôleur de domaine source a uniquement *un* lecteur. Exportation/importation est recommandée pour les ordinateurs virtuels avec *plusieurs* lecteur ou autres personnalisations complexes du matériel virtualisé, telles que plusieurs cartes réseau.  
  
Si vous copiez manuellement les fichiers, supprimez les captures instantanées avant de copier. Si vous exportez l’ordinateur virtuel, supprimez les captures instantanées avant l’exportation ou les supprimer de la nouvelle machine virtuelle après l’importation.  
  
> [!WARNING]  
> Captures instantanées sont des disques qui peuvent retourner un contrôleur de domaine à un état antérieur de différenciation. Si vous clonez un contrôleur de domaine et restaurez ensuite sa capture instantanée antérieure au clonage, vous obtiendriez contrôleurs de domaine dupliqués dans la forêt. Il n’existe aucune valeur dans les captures instantanées antérieures sur un contrôleur de domaine qui vient d’être cloné.  
  
#### <a name="manually-copying-disks"></a>Copie manuelle des disques  
  
##### <a name="hyper-v-manager-method"></a>Méthode de Gestionnaire Hyper-V  
Utilisez le composant logiciel enfichable Gestionnaire Hyper-V pour déterminer quels disques sont associés au contrôleur de domaine source. Utilisez l’option inspecter pour vérifier si le contrôleur de domaine utilise des disques de différenciation (ce qui nécessite que vous copiez également le disque parent)  
  
![Déploiement de contrôleur de domaine virtualisé](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVInspect.png)  
  
Pour supprimer les captures instantanées, sélectionnez un ordinateur virtuel et supprimez la sous-arborescence de capture instantanée.  
  
![Déploiement de contrôleur de domaine virtualisé](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVDeleteSnapshot.gif)  
  
Vous pouvez copier puis manuellement les fichiers VHD ou VHDX à l’aide de l’Explorateur Windows, Xcopy.exe ou Robocopy.exe. Aucune étape particulière n’est requis. Il est conseillé de modifier les noms de fichiers, même si le déplacement vers un autre dossier.  
  
> [!NOTE]  
> Si une copie entre des ordinateurs hôtes sur un réseau local (1 Gbit ou plus), la **Xcopy.exe /J** option copie les fichiers VHD/VHDX beaucoup plus rapidement que n’importe quel autre outil, au prix de quantité supérieure utilisation de la bande passante.  
  
##### <a name="windows-powershell-method"></a>Méthode de Windows PowerShell  
Pour déterminer les disques à l’aide de Windows PowerShell, utilisez les Modules Hyper-V:  
  
```  
Get-vmidecontroller  
Get-vmscsicontroller  
Get-vmfibrechannelhba  
Get-vmharddiskdrive  
```  
  
Par exemple, vous pouvez retourner tous les disques durs IDE d’un ordinateur virtuel nommé **DC2** avec l’exemple suivant:  
  
![Déploiement de contrôleur de domaine virtualisé](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_ReturnIDE.png)  
  
Si le chemin d’accès du disque pointe vers un fichier AVHD ou AVHDX, il est un instantané. Pour supprimer les captures instantanées associées à un disque et fusionner dans le VHD ou VHDX réels, utilisez les applets de commande:  
  
```  
Get-VMSnapshot  
Remove-VMSnapshot  
```  
  
Par exemple, pour supprimer toutes les captures instantanées d’un ordinateur virtuel nommé DC2-SOURCECLONE:  
  
![Déploiement de contrôleur de domaine virtualisé](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_DelSnapshots.png)  
  
Pour copier les fichiers à l’aide de Windows PowerShell, utilisez l’applet de commande suivante:  
  
```  
Copy-Item  
```  
  
Combiner des applets de commande de machine virtuelle sous forme de pipelines pour faciliter l’automatisation. Le pipeline est un canal utilisé entre plusieurs applets de commande pour transmettre des données. Par exemple, pour copier le lecteur d’un contrôleur de domaine source hors connexion nommé DC2-SOURCECLONE vers un nouveau disque appelé c:\temp\copy.vhd sans avoir besoin de connaître le chemin d’accès exact de son lecteur système:  
  
```  
Get-VMIdeController dc2-sourceclone | Get-VMHardDiskDrive | select-Object {copy-item -path $_.path -destination c:\temp\copy.vhd}  
```  
  
![Déploiement de contrôleur de domaine virtualisé](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSCopyDrive.png)  
  
> [!IMPORTANT]  
> Vous ne pouvez pas utiliser de disques passthru avec le clonage, car ils n’utilisent pas un fichier de disque virtuel mais plutôt un disque dur réel.  
  
> [!NOTE]  
> Pour plus d’informations sur d’autres opérations Windows PowerShell avec des pipelines, voir [et utilisation des pipelines dans Windows PowerShell](https://technet.microsoft.com/en-us/library/ee176927.aspx).  
  
#### <a name="exporting-the-vm"></a>Exportez la machine virtuelle  
Comme alternative à copier les disques, vous pouvez exporter la totalité VM Hyper-V en tant qu’une copie. L’exportation crée automatiquement un dossier nommé pour l’ordinateur virtuel et contenant tous les disques et les informations de configuration.  
  
![Déploiement de contrôleur de domaine virtualisé](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVExport.png)  
  
##### <a name="hyper-v-manager-method"></a>Méthode de Gestionnaire Hyper-V  
Pour exporter un ordinateur virtuel avec le Gestionnaire Hyper-V:  
  
1.  Cliquez sur le contrôleur de domaine source, cliquez sur **exporter**.  
  
2.  Sélectionnez un dossier existant comme conteneur d’exportation.  
  
3.  Attendez la fin de la colonne État cesse d’afficher **exportation**.  
  
##### <a name="windows-powershell-method"></a>Méthode de Windows PowerShell  
Pour exporter un ordinateur virtuel à l’aide du module Windows PowerShell Hyper-V, utilisez l’applet de commande:  
  
```  
Export-vm  
```  
  
Par exemple, pour exporter un ordinateur virtuel nommé DC2-SOURCECLONE vers un dossier nommé C:\VM:  
  
![Déploiement de contrôleur de domaine virtualisé](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSExport.png)  
  
> [!NOTE]  
> Prend en charge de Windows Server 2012 Hyper-V nouveau exporter et importer des fonctionnalités qui sont en dehors de l’étendue de cette formation. Pour plus d’informations, voir TechNet.  
  
#### <a name="exporting-merged-disks-using-hyper-v"></a>Exportation de disques fusionnés avec Hyper-V  
La dernière possibilité consiste à utiliser les options de fusion et de conversion de disques d’Hyper-V. Il vous autorise à effectuer une copie d’une structure de disque existante - même en incluant des fichiers AVHD/AVHDX de capture instantanée - dans un seul nouveau disque. Comme dans le scénario de copie manuelle de disque, cela est conçu pour ordinateurs virtuels qui utilisent un seul lecteur, tels que C:\\ plus simples. Son seul avantage est que, contrairement à la copie manuelle, il ne nécessite pas tout d’abord supprimer les captures instantanées. Cette opération est nécessairement plus lente que la simple suppression des captures instantanées et copie de disques.  
  
##### <a name="hyper-v-manager-method"></a>Méthode de Gestionnaire Hyper-V  
Pour créer un disque fusionné à l’aide du Gestionnaire Hyper-V:  
  
1.  Cliquez sur **modifier le disque**.  
  
2.  Recherchez le disque enfant plus bas. Par exemple, si vous utilisez un disque de différenciation, le disque enfant est l’enfant le plus bas. Si l’ordinateur virtuel possède une capture instantanée (ou plusieurs), la capture instantanée actuellement sélectionnée est le disque enfant plus bas.  
  
3.  Sélectionnez le **fusionner** option pour créer un disque unique à partir de la structure parent-enfant entière.  
  
4.  Sélectionnez un nouveau disque dur virtuel et indiquez un chemin d’accès. Cela rapproche les fichiers VHD/VHDX existants dans une nouvelle unité portable unique qui se trouve pas au risque de restauration des captures instantanées.  
  
##### <a name="windows-powershell-method"></a>Méthode de Windows PowerShell  
Pour créer un disque fusionné à partir d’un jeu complexe de parents via le module Hyper-V Windows PowerShell, utilisez l’applet de commande:  
  
```  
Convert-vm  
```  
  
Par exemple, pour exporter l’ensemble de la chaîne de captures instantanées de disque de l’ordinateur virtuel (ce temps ne pas y compris les disques de différenciation) et le parent du disque dans un nouveau disque unique nommé DC4-CLONED. VHDX:  
  
![Déploiement de contrôleur de domaine virtualisé](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSConvertVhd.png)  
  
#### <a name="BKMK_Offline"></a>Ajout de code XML pour le disque système hors connexion  
Si vous avez copié le fichier Dccloneconfig.xml pour le contrôleur de domaine source en cours d’exécution, vous devez copier le fichier dccloneconfig.xml mis à jour sur le disque système copié/exporté hors connexion maintenant. Selon les applications installées détectées avec Get-ADDCCloningExcludedApplicationList plus tôt, vous devez également copier le fichier CustomDCCloneAllowList.xml sur le disque.  
  
Les emplacements suivants peuvent contenir le fichier DcCloneConfig.xml:  
  
1.  Répertoire de travail de DSA  
  
2.  %windir%\ntds  
  
3.  Média amovible en lecture/écriture, dans l’ordre de la lettre de lecteur, à la racine du lecteur  
  
Ces chemins d’accès ne sont pas configurables. Une fois le clonage commence, le clonage vérifie ces emplacements dans un ordre spécifique et utilise le fichier DcCloneConfig.xml premier fichier trouvé, quel que soit le contenu de l’autre dossier.  
  
Les emplacements suivants peuvent contenir le fichier CustomDCCloneAllowList.xml:  
  
1.  HKey_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters  
  
    AllowListFolder (*REG_SZ*)  
  
2.  Répertoire de travail de DSA  
  
3.  %windir%\ntds  
  
4.  Média amovible en lecture/écriture, dans l’ordre de la lettre de lecteur, à la racine du lecteur  
  
Vous pouvez exécuter New-ADDCCloneConfigFile avec le **-hors connexion** argument (également appelé mode hors connexion) pour créer le fichier DcCloneConfig.xml et le placer dans un emplacement approprié. Les exemples suivants montrent comment exécuter New-ADDCCloneConfigFile en mode hors connexion.  
  
Pour créer un contrôleur de domaine clone appelé CloneDC1 en mode hors connexion, dans un site nommé «REDMOND» avec l’adresse IPv4 statique, tapez:  
  
```  
New-ADDCCloneConfigFile -Offline -CloneComputerName CloneDC1 -SiteName REDMOND -IPv4Address "10.0.0.2" -IPv4DNSResolver "10.0.0.1" -IPv4SubnetMask "255.255.0.0" -IPv4DefaultGateway "10.0.0.1" -Static -Path F:\Windows\NTDS  
```  
  
Pour créer un contrôleur de domaine clone appelé Clone2 en mode hors connexion avec IPv4 statiques et des paramètres IPv6 statiques, tapez:  
  
```  
New-ADDCCloneConfigFile -Offline -IPv4Address "10.0.0.2" -IPv4DNSResolver "10.0.0.1" -IPv4SubnetMask "255.255.0.0" -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -CloneComputerName "Clone2" -PreferredWINSServer "10.0.0.1" -AlternateWINSServer "10.0.0.3" -Path F:\Windows\NTDS  
```  
  
Pour créer un contrôleur de domaine clone en mode hors connexion avec IPv4 statiques et des paramètres IPv6 dynamiques et spécifier plusieurs serveurs DNS pour les paramètres de résolution DNS, tapez:  
  
```  
New-ADDCCloneConfigFile -Offline -IPv4Address "10.0.0.10" -IPv4SubnetMask "255.255.0.0" -IPv4DefaultGateway "10.0.0.1" -IPv4DNSResolver @( "10.0.0.1","10.0.0.2" ) -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -Path F:\Windows\NTDS   
```  
  
Pour créer un contrôleur de domaine clone appelé Clone1 en mode hors connexion avec les paramètres IPv4 dynamiques et des paramètres IPv6 statiques, tapez:  
  
```  
New-ADDCCloneConfigFile -Offline -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -CloneComputerName "Clone1" -PreferredWINSServer "10.0.0.1" -AlternateWINSServer "10.0.0.3" -SiteName "REDMOND" -Path F:\Windows\NTDS  
```  
  
Pour créer un contrôleur de domaine clone en mode hors connexion avec les paramètres IPv4 dynamiques et des paramètres IPv6 dynamiques, tapez:  
  
```  
New-ADDCCloneConfigFile -Offline -IPv4DNSResolver "10.0.0.1" -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -Path F:\Windows\NTDS  
  
```  
  
##### <a name="windows-explorer-method"></a>Méthode de l’Explorateur Windows  
Windows Server 2012 offre désormais une option graphique pour le montage des fichiers VHD et VHDX. Cela nécessite l’installation de la fonctionnalité Expérience Bureau sur Windows Server 2012.  
  
1.  Cliquez sur le fichier VHD/VHDX récemment copié qui contient le lecteur système ou le dossier d’emplacement de répertoire de travail DSA du contrôleur de domaine source, puis cliquez sur **monter** à partir de la **outils d’Image disque** menu.  
  
2.  Dans le lecteur monté, copiez les fichiers XML à un emplacement valide. Vous pouvez être invité par des autorisations pour le dossier.  
  
3.  Cliquez sur le lecteur monté, puis cliquez sur **éjecter** à partir de la **outils disque** menu.  
  
![Déploiement de contrôleur de domaine virtualisé](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVClickMountedDrive.png)  
  
![Déploiement de contrôleur de domaine virtualisé](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVDetailsMountedDrive.gif)  
  
![Déploiement de contrôleur de domaine virtualisé](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVEjectMountedDrive.gif)  
  
##### <a name="windows-powershell-method"></a>Méthode de Windows PowerShell  
Sinon, vous pouvez monter le disque hors connexion et copier le fichier XML à l’aide des applets de commande Windows PowerShell:  
  
```  
mount-vhd  
get-disk  
get-partition  
get-volume  
Add-PartitionAccessPath  
Copy-Item  
```  
  
Cela vous permet de contrôler totalement le processus. Par exemple, le lecteur peut être monté avec une lettre de lecteur spécifique, le fichier copié, puis démonter le lecteur.  
  
```  
mount-vhd <disk path> -passthru -nodriveletter | get-disk | get -partition | get-volume | get-partition | where {$_.partition number -eq 2} | Add-PartitionAccessPath -accesspath <drive letter>  
  
copy-item <xml file path><destination path>\dccloneconfig.xml  
  
dismount-vhd <disk path>  
```  
  
Par exemple:  
  
![Déploiement de contrôleur de domaine virtualisé](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSMountVHD.png)  
  
Vous pouvez également utiliser la nouvelle **Mount-DiskImage** applet de commande pour monter un fichier de disque dur virtuel (ou ISO).  
  
### <a name="step-8---create-the-new-virtual-machine"></a>Étape 8 - créer le nouvel ordinateur virtuel  
La dernière étape de configuration avant de commencer le processus de clonage consiste à créer un nouvel ordinateur virtuel qui utilise les disques à partir du contrôleur de domaine source copié. Selon le choix effectué durant la phase de copie des disques, vous avez deux possibilités:  
  
1.  Associer un nouvel ordinateur virtuel au disque copié  
  
2.  Importer l’ordinateur virtuel exporté  
  
#### <a name="associating-a-new-vm-with-copied-disks"></a>Association d’un nouvel ordinateur virtuel à des disques copiés  
Si vous avez copié le disque système manuellement, vous devez créer un nouvel ordinateur virtuel à l’aide du disque copié. L’hyperviseur définit automatiquement l’ID de génération d’ordinateur virtuel lors de la création d’un nouvel ordinateur virtuel; aucune modification de configuration n’est nécessaires dans l’hôte d’ordinateur virtuel ou Hyper-V.  
  
![Déploiement de contrôleur de domaine virtualisé](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVConnectVHD.gif)  
  
##### <a name="hyper-v-manager-method"></a>Méthode de Gestionnaire Hyper-V  
  
1.  Créer un nouvel ordinateur virtuel.  
  
2.  Spécifiez le nom de l’ordinateur virtuel, la mémoire et réseau.  
  
3.  Sur la page connecter un disque dur virtuel, spécifiez le disque système copié.  
  
4.  Terminez l’Assistant pour créer l’ordinateur virtuel.  
  
S’il existe plusieurs disques, cartes réseau, ou autres personnalisations, configurez-les avant de démarrer le contrôleur de domaine. La méthode «Exportation / importation» de la copie de disques est recommandée pour les ordinateurs virtuels complexes.  
  
##### <a name="windows-powershell-method"></a>Méthode de Windows PowerShell  
Vous pouvez utiliser le module Hyper-V Windows PowerShell pour automatiser la création d’ordinateurs virtuels dans Windows Server 2012, à l’aide de l’applet de commande suivante:  
  
```  
New-VM  
```  
  
Par exemple, ici l’ordinateur virtuel DC4-CLONEDFROMDC2 est créé, à l’aide de 1 Go de RAM, le démarrage à partir du fichier c:\vm\dc4-systemdrive-clonedfromdc2.vhd et l’utilisation du réseau virtuel 10.0:  
  
![Déploiement de contrôleur de domaine virtualisé](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSNewVM.png)  
  
#### <a name="import-vm"></a>Importer l’ordinateur virtuel  
Si vous avez précédemment exporté votre ordinateur virtuel, vous devez maintenant importer reconnectez-vous en tant qu’une copie. Il utilise le fichier XML exporté pour recréer l’ordinateur à l’aide de tous les paramètres précédents, lecteurs, réseaux et les paramètres de mémoire.  
  
Si vous envisagez de créer des copies supplémentaires à partir du même ordinateur virtuel exporté, assurez-vous autant de copies de l’ordinateur virtuel exporté en fonction des besoins. Puis utilisez Import pour chaque copie.  
  
> [!IMPORTANT]  
> Il est important d’utiliser le **copie** option, car l’exportation conserve toutes les informations de la source; l’importation du serveur avec **déplacer** ou **en Place** entraîne la collision des informations s’effectue sur le même serveur hôte Hyper-V.  
  
##### <a name="hyper-v-manager-method"></a>Méthode de Gestionnaire Hyper-V  
Pour importer à l’aide du composant logiciel enfichable Gestionnaire Hyper-V:  
  
1.  Cliquez sur **importer l’ordinateur virtuel**  
  
2.  Sur le **localiser le dossier** , sélectionnez le fichier de définition de machine virtuelle exporté à l’aide du bouton Parcourir  
  
3.  Sur le **sélectionner l’ordinateur virtuel** , cliquez sur l’ordinateur source.  
  
4.  Sur le **choisir le Type** , cliquez sur **copier l’ordinateur virtuel (créer un nouvel ID unique)**, puis cliquez sur **Terminer**.  
  
5.  Renommer l’ordinateur virtuel importé si l’importation sur le même hôte Hyper-V; il aura le même nom que le contrôleur de domaine source exporté.  
  
![Déploiement de contrôleur de domaine virtualisé](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVImportLocateFolder.png)  
  
![Déploiement de contrôleur de domaine virtualisé](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVImportSelectVM.png)  
  
![Déploiement de contrôleur de domaine virtualisé](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVImportChooseType.gif)  
  
N’oubliez pas de supprimer des captures instantanées importées, à l’aide du composant logiciel enfichable Gestion Hyper-V:  
  
![Déploiement de contrôleur de domaine virtualisé](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVImportDelSnap.gif)  
  
> [!WARNING]  
> La suppression des captures instantanées importées est extrêmement importante; Si appliquées, elles restaurent le contrôleur de domaine cloné à l’état d’un contrôleur de domaine précédente et éventuellement dynamique, ce qui conduit à l’échec de réplication, les informations IP en double et d’autres problèmes.  
  
##### <a name="windows-powershell-method"></a>Méthode de Windows PowerShell  
Vous pouvez utiliser le module Hyper-V Windows PowerShell pour automatiser l’importation d’ordinateurs virtuels dans Windows Server 2012, à l’aide des applets de commande suivantes:  
  
```  
Import-VM  
Rename-VM  
```  
  
Par exemple, voici l’ordinateur virtuel VM DC2-CLONED est importé à l’aide de son fichier XML déterminé automatiquement, puis renommé immédiatement son nouveau nom d’ordinateur virtuel DC5-CLONEDFROMDC2:  
  
![Déploiement de contrôleur de domaine virtualisé](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSImportVM.png)  
  
N’oubliez pas de supprimer des captures instantanées importées, à l’aide des applets de commande suivantes:  
  
```  
Get-VMSnapshot  
Remove-VMSnapshot  
```  
  
Par exemple:  
  
![Déploiement de contrôleur de domaine virtualisé](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSGetVMSnap.png)  
  
> [!WARNING]  
> Assurez-vous que, lorsque vous importez l’ordinateur, des adresses MAC statiques ont été affectés pas au contrôleur de domaine source. Si un ordinateur source avec une adresse MAC statique est cloné, les ordinateurs copiés n’est pas correctement envoie ou reçoit tout le trafic réseau. Définissez une nouvelle unique statique ou dynamique adresse MAC si c’est le cas. Vous pouvez voir si un ordinateur virtuel utilise des adresses MAC statiques avec la commande:  
>   
> **Get-VM - VMName.**   
>  ***test-vm* | Get-VMNetworkAdapter | fl \ ***  
  
### <a name="step-9---clone-the-new-virtual-machine"></a>Étape 9 - cloner le nouvel ordinateur virtuel  
Si vous le souhaitez, avant de commencer le clonage, redémarrez le contrôleur de domaine source clone hors connexion. Vérifiez que l’émulateur PDC est en ligne, quel que soit.  
  
Pour commencer le clonage, il vous suffit de démarrer le nouvel ordinateur virtuel. Le processus démarre automatiquement et le contrôleur de domaine redémarre automatiquement après que le clonage est terminé.  
  
> [!IMPORTANT]  
> Mise hors tension pendant une période prolongée des contrôleurs de domaine n’est pas recommandé et si le clone se joint au même site que son contrôleur de domaine source, l’intra initiale et la topologie de réplication intersite peuvent prendre plus de temps, si le contrôleur de domaine source est hors connexion.  
  
Si vous utilisez Windows PowerShell pour démarrer un ordinateur virtuel, la nouvelle applet de commande du Module Hyper-V est:  
  
```  
Start-VM  
```  
  
Par exemple:  
  
![Déploiement de contrôleur de domaine virtualisé](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSStartVM.png)  
  
Une fois que l’ordinateur redémarre après que le clonage terminé, il est un contrôleur de domaine et vous pouvez vous connecter normalement pour vérifier son bon fonctionnement. S’il existe des erreurs, le serveur est configuré pour démarrer en Mode restauration des Services d’annuaire pour examen.  
  
## <a name="BKMK_VDCSafeRestore"></a>Dispositifs de protection  
Clonage de contrôleur de domaine virtualisé, à la différence des dispositifs de protection de la virtualisation Windows Server 2012 n’ont aucune étape de configuration. La fonctionnalité est opérationnelle sans intervention, que vous respecter certaines conditions simples:  
  
-   L’hyperviseur prend en charge les ID de génération  
  
-   Il existe un contrôleur de domaine partenaire valide un contrôleur de domaine restauré permettre répliquer les modifications de ne faisant pas autorité.  
  
### <a name="validate-the-hypervisor"></a>Valider l’hyperviseur  
Vérifiez que le contrôleur de domaine source est en cours d’exécution sur un hyperviseur pris en charge en consultant la documentation du fournisseur. Contrôleurs de domaine virtualisés sont indépendants de l’hyperviseur et ne nécessitent pas d’Hyper-V.  
  
Passez en revue la précédente [spécifications de plateforme](../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Deployment-and-Configuration.md#BKMK_PlatformReqs) section prise en charge des ID de génération connus.  
  
Si vous migrez des ordinateurs virtuels à partir d’un hyperviseur source vers un hyperviseur cible distinct, les dispositifs de protection peuvent ou ne peuvent pas être déclenchées selon si les hyperviseurs prennent en charge les ID de génération d’ordinateur virtuel, comme expliqué dans le tableau suivant.  
  
|Hyperviseur source|Hyperviseur cible|Résultat|  
|---------------------|---------------------|----------|  
|ID de génération d’ordinateur virtuel prend en charge|Ne prend pas en charge les ID de génération|Dispositifs de protection non déclenchés (si un fichier DCCloneConfigFile.xml est présent, contrôleur de domaine démarre en mode DSRM)|  
|Ne prend pas en charge les ID de génération|ID de génération d’ordinateur virtuel prend en charge|Dispositifs de protection déclenchés|  
|ID de génération d’ordinateur virtuel prend en charge|ID de génération d’ordinateur virtuel prend en charge|Dispositifs de protection non déclenchés étant donné que la définition de l’ordinateur virtuel n’a pas changé, ce qui signifie que pour l’ID de génération reste le même|  
  
### <a name="validate-the-replication-topology"></a>Valider la topologie de réplication  
Dispositifs de lancer une réplication entrante ne faisant pas autorité pour le delta de la réplication Active Directory, ainsi que la resynchronisation ne faisant pas autorité tous du contenu de SYSVOL. Cela garantit le contrôleur de domaine retourne à partir d’une capture instantanée avec toutes les fonctionnalités et finalement cohérente avec le reste de l’environnement.  
  
Cette nouvelle fonctionnalité s’accompagne de plusieurs conditions requises et restrictions:  
  
-   Un contrôleur de domaine restauré doit être en mesure de contacter un contrôleur de domaine accessible en écriture  
  
-   Tous les contrôleurs de domaine dans un domaine ne doivent pas être restaurés simultanément  
  
-   Tout changement provenant d’un contrôleur de domaine restauré qui n’ont pas encore répliqués sortant depuis la capture instantanée est définitivement perdues  
  
Alors que la section de résolution des problèmes couvre ces scénarios, ci-dessous. Vérifiez que vous ne créez pas une topologie qui puisse causer des problèmes.  
  
#### <a name="writable-domain-controller-availability"></a>Disponibilité du contrôleur de domaine accessible en écriture  
Si restaurée, un contrôleur de domaine doit être connectés à un contrôleur de domaine accessible en écriture; un contrôleur de domaine en lecture seule ne peuvent pas envoyer le delta des mises à jour. La topologie est probablement correcte déjà, comme un contrôleur de domaine accessible en écriture toujours besoin d’un partenaire accessible en écriture. Toutefois, si vous restaurez simultanément tous les contrôleurs de domaine accessible en écriture, aucun d’eux ne peut trouver une source valide. En va de même si les contrôleurs de domaine accessible en écriture sont hors ligne pour maintenance ou inaccessibles via le réseau.  
  
#### <a name="simultaneous-restore"></a>Restauration simultanée  
Ne restaurez pas tous les contrôleurs de domaine dans un domaine unique simultanément. Si toutes les captures instantanées restaurés en même temps, fonctionnement de la réplication Active Directory normalement, mais la réplication SYSVOL s’arrête. L’architecture de restauration des services FRS et DFSR requièrent leur réplica d’instance pour le mode de synchronisation ne faisant pas autorité. Si tous les contrôleurs de domaine restaurés en même temps, et chaque contrôleur de domaine se marque comme ne faisant pas autorité pour SYSVOL, qu’ils soient tous seront puis essayez de synchroniser des stratégies de groupe et les scripts à partir d’un partenaire faisant autorité; à ce stade, cependant, tous les partenaires sont également ne faisant pas autorité.  
  
> [!IMPORTANT]  
> Si tous les contrôleurs de domaine sont restaurés en même temps, utilisez les articles suivants pour définir un contrôleur de domaine - généralement l’émulateur PDC - comme faisant autorité, afin que les autres contrôleurs de domaine peuvent renvoyer à une opération normale:  
>   
> [Jeux de réplicas à l’aide de la clé de Registre BurFlags pour réinitialiser le Service de réplication de fichiers](https://support.microsoft.com/kb/290762)  
>   
> [Comment forcer une synchronisation faisant autorité et ne faisant pas autorité pour SYSVOL répliqué DFSR (comme «D4/D2» pour le service FRS)](https://support.microsoft.com/kb/2218556)  
  
> [!WARNING]  
> N’exécutez pas tous les contrôleurs de domaine dans une forêt ou un domaine sur le même hôte hyperviseur. Cela introduit un seul point de défaillance qui paralyse les services AD DS, Exchange, SQL et autres opérations d’entreprise chaque fois que l’hyperviseur passe en mode hors connexion. Il s’agit pas différente de celle qu’un seul contrôleur de domaine pour un domaine entier ou une forêt. Plusieurs contrôleurs de domaine sur plusieurs plateformes d’aider à fournir la redondance et une tolérance de panne.  
  
#### <a name="post-snapshot-replication"></a>Réplication après les captures instantanée  
Ne restaurez pas les captures instantanées jusqu'à ce que tous les changements effectués localement depuis la création de capture instantanée ont réplication sortantes. Toutes les modifications d’origine sont définitivement perdues si d’autres contrôleurs de domaine n’a pas déjà reçu les par le biais de la réplication.  
  
Utilisez Repadmin.exe pour afficher toutes les modifications sortantes changements non répliquées entre un contrôleur de domaine et ses partenaires:  
  
1.  Retourner partenaire du contrôleur de domaine noms et les GUID d’objet DSA avec:  
  
    ```  
    Repadmin.exe /showrepl <DC Name of the partner> /repsto  
    ```  
  
2.  Retournez la réplication entrante en attente du contrôleur de domaine partenaire au contrôleur de domaine à restaurer:  
  
    ```  
    Repadmin.exe /showchanges < Name of partner DC><DSA Object GUID of the domain controller being restored><naming context to compare>  
    ```  
  
Vous pouvez également, juste pour voir le nombre de changements non répliqués:  
  
```  
Repadmin.exe /showchanges <Name of partner DC><DSA Object GUID of the domain controller being restored><naming context to compare> /statistics  
```  
  
Par exemple (avec une sortie changée pour une meilleure lisibilité et les entrées importantes ***en italique***), vous regardez ici les partenariats de réplication de DC4:  
  
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
  
Maintenant, vous savez qu’il se réplique avec DC2 et DC3. Vous affichez ensuite la liste des changements que DC2 indique toujours pas à partir de DC4 et ne constatez qu’il existe un nouveau groupe:  
  
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
  
Vous pouvez également tester l’autre partenaire pour vous assurer qu’il n’est pas déjà répliqué.  
  
Si vous intéresse pas les objets non répliqués et êtes seulement intéressé que tous les objets ont été exceptionnelles, vous pouvez utiliser le **/statistics** option:  
  
```  
C:\>repadmin /showchanges dc2.corp.contoso.com 5d083398-4bd3-48a4-a80d-fb2ebafb984f dc=corp,dc=contoso,dc=com /statistics  
  
***********************************************  
********* Grand total *************************  
Packets:              1  
Objects:              1Object Additions:     1Object Modifications: 0Object Deletions:     0Object Moves:         0Attributes:           12Values:               13  
```  
  
> [!IMPORTANT]  
> Tester tous les partenaires accessibles en écriture si vous constatez des défaillances ou la réplication en attente. Tant qu’au moins une convergence, il est généralement possible de restaurer la capture instantanée, comme la réplication transitive rapproche finalement les autres serveurs.  
>   
> Veillez à noter les erreurs de réplication indiquées par /showchanges et ne pas poursuivre jusqu'à ce qu’ils sont résolus.  
  
### <a name="windows-powershell-snapshot-cmdlets"></a>Applets de commande Windows PowerShell pour captures instantanées  
Les applets de commande du module Windows PowerShell Hyper-V suivantes fournissent des fonctionnalités de capture instantanée dans Windows Server 2012:  
  
```  
Checkpoint-VM  
Export-VMSnapshot  
Get-VMSnapshot  
Remove-VMSnapshot  
Rename-VMSnapshot  
Restore-VMSnapshot  
```  
  


