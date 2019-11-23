---
title: Déployer des espaces de stockage direct
ms.prod: windows-server
manager: eldenc
ms.author: stevenek
ms.technology: storage-spaces
ms.topic: get-started-article
ms.assetid: 20fee213-8ba5-4cd3-87a6-e77359e82bc0
author: stevenek
ms.date: 06/07/2019
description: Instructions pas à pas pour déployer le stockage défini par logiciel avec espaces de stockage direct dans Windows Server en tant qu’infrastructure hyper-convergée ou en tant qu’infrastructure convergée (également appelée infrastructure désagrégée).
ms.localizationpriority: medium
ms.openlocfilehash: 0ab96f737f7700e202c9d0382c06859c4ea84118
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402818"
---
# <a name="deploy-storage-spaces-direct"></a>Déployer des espaces de stockage direct

> S’applique à : Windows Server 2019, Windows Server 2016

Cette rubrique fournit des instructions pas à pas pour déployer [espaces de stockage direct](storage-spaces-direct-overview.md).

> [!Tip]
> Vous cherchez à acquérir une infrastructure hyper-convergée ? Microsoft recommande d’acheter une solution matérielle/logicielle validée auprès de nos partenaires, y compris des outils et des procédures de déploiement. Ces solutions sont conçues, assemblées et validées par rapport à notre architecture de référence pour garantir la compatibilité et la fiabilité, ce qui vous permet d’être opérationnel rapidement. Pour les solutions Windows Server 2019, visitez le [site web Azure Stack solutions HCI](https://azure.microsoft.com/overview/azure-stack/hci). Pour plus d’informations sur les solutions Windows Server 2016, consultez la [définition du logiciel Windows Server](https://microsoft.com/wssd).

> [!Tip]
> Vous pouvez utiliser des machines virtuelles Hyper-V, y compris dans Microsoft Azure, pour [évaluer des espaces de stockage direct sans matériel](storage-spaces-direct-in-vm.md). Vous pouvez également consulter les [scripts de déploiement Windows Server Rapid Lab](https://aka.ms/wslab)pratiques que nous utilisons à des fins de formation.

## <a name="before-you-start"></a>Avant de commencer

Passez en revue les [espaces de stockage direct configuration matérielle requise](Storage-Spaces-Direct-Hardware-Requirements.md) et plongez dans ce document pour vous familiariser avec l’approche globale et les remarques importantes associées à certaines étapes.

Rassemblez les informations suivantes :

- **Option de déploiement.** Espaces de stockage direct prend en charge [deux options de déploiement : Hyper-convergé et convergé](storage-spaces-direct-overview.md#deployment-options), également appelées « désagrégées ». Familiarisez-vous avec les avantages de chacun pour décider de ce qui vous convient le plus. Les étapes 1-3 ci-dessous s’appliquent aux deux options de déploiement. L’étape 4 n’est nécessaire que pour le déploiement convergé.

- **Noms des serveurs.** Familiarisez-vous avec les stratégies d’affectation de noms de votre organisation pour les ordinateurs, les fichiers, les chemins d’accès et autres ressources. Vous devez configurer plusieurs serveurs, chacun avec des noms uniques.

- **Nom de domaine.** Familiarisez-vous avec les stratégies de votre organisation relatives à l’attribution de noms de domaine et à la jonction de domaine.  Vous allez joindre les serveurs à votre domaine, et vous devez spécifier le nom de domaine. 

- **Mise en réseau RDMA.** Il existe deux types de protocoles RDMA : iWarp et RoCE. Notez l’une des cartes réseau utilisées et, si RoCE, Notez également la version (v1 ou v2). Pour RoCE, Notez également le modèle de votre commutateur haut de la rack.

- **ID DE RÉSEAU LOCAL VIRTUEL.** Notez l’ID de réseau local virtuel à utiliser pour les cartes réseau du système d’exploitation de gestion sur les serveurs, le cas échéant. Vous devriez pouvoir l’obtenir auprès de votre administrateur réseau.

## <a name="step-1-deploy-windows-server"></a>Étape 1 : Déployer Windows Server

### <a name="step-11-install-the-operating-system"></a>Étape 1,1 : installer le système d’exploitation

La première étape consiste à installer Windows Server sur chaque serveur qui sera dans le cluster. Espaces de stockage direct nécessite Windows Server 2016 Datacenter Edition. Vous pouvez utiliser l’option d’installation minimale ou le serveur avec expérience utilisateur.

Quand vous installez Windows Server à l’aide de l’Assistant Installation, vous pouvez choisir entre *Windows Server* (qui fait référence à Server Core) et *Windows Server (serveur avec expérience utilisateur)* , qui est l’équivalent de l’option d’installation *complète* disponible dans Windows Server 2012 R2. Si vous ne le faites pas, vous obtiendrez l’option d’installation Server Core. Pour plus d’informations, consultez [options d’installation de Windows Server 2016](../../get-started/Windows-Server-2016.md).

### <a name="step-12-connect-to-the-servers"></a>Étape 1,2 : connexion aux serveurs

Ce guide met l’accent sur l’option d’installation Server Core et sur le déploiement et la gestion à distance à partir d’un système de gestion distinct, qui doit disposer des éléments suivants :

- Windows Server 2016 avec les mêmes mises à jour que les serveurs qu’il gère
- Connectivité réseau aux serveurs qu’il gère
- Joint au même domaine ou à un domaine entièrement approuvé
- Outils d’administration de serveur distant et modules PowerShell pour Hyper-V et le clustering de basculement. Les outils RSAT et les modules PowerShell sont disponibles sur Windows Server et peuvent être installés sans installer d’autres fonctionnalités. Vous pouvez également installer le [Outils d’administration de serveur distant](https://www.microsoft.com/download/details.aspx?id=45520) sur un PC de gestion Windows 10.

Sur le système de gestion, installez le cluster de basculement et les outils de gestion Hyper-V. Vous pouvez effectuer cette opération à l’aide du Gestionnaire de serveur en utilisant l’**Assistant Ajout de rôles et de fonctionnalités**. Dans la page **Fonctionnalités**, sélectionnez **Outils d’administration de serveur distant**, puis sélectionnez les outils à installer.

Ouvrez la session PowerShell, puis utilisez le nom du serveur ou l’adresse IP du nœud auquel vous souhaitez vous connecter. Une fois que vous avez exécuté cette commande, vous êtes invité à entrer un mot de passe, entrez le mot de passe d’administrateur que vous avez spécifié lors de la configuration de Windows.

   ```PowerShell
   Enter-PSSession -ComputerName <myComputerName> -Credential LocalHost\Administrator
   ```

   Voici un exemple d’utilisation de la même chose d’une manière plus utile dans les scripts, au cas où vous devriez le faire plusieurs fois :

   ```PowerShell
   $myServer1 = "myServer-1"
   $user = "$myServer1\Administrator"

   Enter-PSSession -ComputerName $myServer1 -Credential $user
   ```

> [!TIP]
> Si vous effectuez un déploiement à distance à partir d’un système de gestion, vous pouvez obtenir une erreur telle que *WinRM ne peut pas traiter la demande.* Pour résoudre ce problème, utilisez Windows PowerShell pour ajouter chaque serveur à la liste des hôtes approuvés sur votre ordinateur de gestion :  
>   
> `Set-Item WSMAN:\Localhost\Client\TrustedHosts -Value Server01 -Force`
>  
> Remarque : la liste des hôtes approuvés prend en charge les caractères génériques, comme `Server*`.
>
> Pour afficher votre liste d’hôtes approuvés, tapez `Get-Item WSMAN:\Localhost\Client\TrustedHosts`.  
>   
> Pour vider la liste, tapez `Clear-Item WSMAN:\Localhost\Client\TrustedHost`.  

### <a name="step-13-join-the-domain-and-add-domain-accounts"></a>Étape 1,3 : joindre le domaine et ajouter des comptes de domaine

Jusqu’à présent, vous avez configuré les serveurs individuels avec le compte d’administrateur local, `<ComputerName>\Administrator`.

Pour gérer espaces de stockage direct, vous devez joindre les serveurs à un domaine et utiliser un compte de domaine Active Directory Domain Services qui se trouve dans le groupe Administrateurs sur chaque serveur.

À partir du système de gestion, ouvrez une console PowerShell avec des privilèges d’administrateur. Utilisez `Enter-PSSession` pour vous connecter à chaque serveur et exécutez l’applet de commande suivante, en remplaçant le nom de votre ordinateur, le nom de domaine et les informations d’identification de domaine :

```PowerShell  
Add-Computer -NewName "Server01" -DomainName "contoso.com" -Credential "CONTOSO\User" -Restart -Force  
```

Si votre compte d’administrateur de stockage n’est pas membre du groupe Admins du domaine, ajoutez votre compte d’administrateur de stockage au groupe Administrateurs local sur chaque nœud, ou mieux encore, ajoutez le groupe que vous utilisez pour les administrateurs du stockage. Vous pouvez utiliser la commande suivante (ou écrire une fonction Windows PowerShell pour ce faire, consultez [Utiliser PowerShell pour ajouter des utilisateurs de domaine à un groupe local](http://blogs.technet.com/b/heyscriptingguy/archive/2010/08/19/use-powershell-to-add-domain-users-to-a-local-group.aspx) pour plus d’informations) :

```
Net localgroup Administrators <Domain\Account> /add
```

### <a name="step-14-install-roles-and-features"></a>Étape 1,4 : installer les rôles et les fonctionnalités

L’étape suivante consiste à installer les rôles serveur sur chaque serveur. Pour ce faire, vous pouvez utiliser le [Centre d’administration Windows](../../manage/windows-admin-center/use/manage-servers.md), [Gestionnaire de serveur](../../administration/server-manager/install-or-uninstall-roles-role-services-or-features.md)) ou PowerShell. Voici les rôles à installer :

- Clustering de basculement
- Hyper-V
- Serveur de fichiers (si vous souhaitez héberger des partages de fichiers, par exemple pour un déploiement convergé)
- Data-Center-Bridging (si vous utilisez RoCEv2 au lieu des cartes réseau iWARP)
- RSAT-Clustering-PowerShell
- Hyper-V-PowerShell

Pour procéder à l’installation via PowerShell, utilisez l’applet de commande [install-WindowsFeature](https://docs.microsoft.com/powershell/module/microsoft.windows.servermanager.migration/install-windowsfeature) . Vous pouvez l’utiliser sur un serveur unique comme suit :

```PowerShell
Install-WindowsFeature -Name "Hyper-V", "Failover-Clustering", "Data-Center-Bridging", "RSAT-Clustering-PowerShell", "Hyper-V-PowerShell", "FS-FileServer"
```

Pour exécuter la commande sur tous les serveurs du cluster en même temps, utilisez ce petit peu de script, en modifiant la liste des variables au début du script pour l’adapter à votre environnement.

```PowerShell
# Fill in these variables with your values
$ServerList = "Server01", "Server02", "Server03", "Server04"
$FeatureList = "Hyper-V", "Failover-Clustering", "Data-Center-Bridging", "RSAT-Clustering-PowerShell", "Hyper-V-PowerShell", "FS-FileServer"

# This part runs the Install-WindowsFeature cmdlet on all servers in $ServerList, passing the list of features into the scriptblock with the "Using" scope modifier so you don't have to hard-code them here.
Invoke-Command ($ServerList) {
    Install-WindowsFeature -Name $Using:Featurelist
}
```

## <a name="step-2-configure-the-network"></a>Étape 2 : Configurer le réseau

Si vous déployez des espaces de stockage direct à l’intérieur de machines virtuelles, ignorez cette section.

Espaces de stockage direct nécessite un réseau à bande passante élevée et à faible latence entre les serveurs du cluster. Au moins 10 GbE Networking est requis et l’accès direct à la mémoire à distance (RDMA) est recommandé. Vous pouvez utiliser iWARP ou RoCE, à condition qu’il dispose du logo Windows Server 2016, mais iWARP est généralement plus facile à configurer.

> [!Important]
> Selon votre équipement réseau, et en particulier avec RoCE v2, une configuration du commutateur haut de la rack peut être nécessaire. Une configuration de commutateur correcte est importante pour garantir la fiabilité et les performances de espaces de stockage direct.

Windows Server 2016 introduit Switch-Embedded Teaming (SET) dans le commutateur virtuel Hyper-V. Cela permet d’utiliser les mêmes ports de carte réseau physique pour tout le trafic réseau tout en utilisant RDMA, ce qui réduit le nombre de ports de carte réseau physique requis. Switch-Embedded Teaming est recommandé pour les espaces de stockage direct.

Interconnexions de nœuds commutées ou non
- Basculement : les commutateurs réseau doivent être correctement configurés pour gérer la bande passante et le type de réseau. Si vous utilisez RDMA qui implémente le protocole RoCE, la configuration du commutateur et du périphérique réseau est encore plus importante.
- Sans changement : les nœuds peuvent être interconnectés à l’aide de connexions directes, évitant l’utilisation d’un commutateur. Chaque nœud a une connexion directe avec tous les autres nœuds du cluster.

Pour obtenir des instructions sur la configuration de la mise en réseau pour espaces de stockage direct, consultez le Guide de déploiement d’une [carte réseau Windows Server 2016 convergée et d’invité RDMA](https://github.com/Microsoft/SDN/blob/master/Diagnostics/S2D%20WS2016_ConvergedNIC_Configuration.docx).

## <a name="step-3-configure-storage-spaces-direct"></a>Étape 3 : Configurer les espaces de stockage direct

Les étapes suivantes sont effectuées sur un système de gestion qui est de la même version que les serveurs en cours de configuration. Les étapes suivantes ne doivent pas être exécutées à distance à l’aide d’une session PowerShell, mais plutôt dans une session PowerShell locale sur le système de gestion, avec des autorisations d’administration.

### <a name="step-31-clean-drives"></a>Étape 3,1 : nettoyer les lecteurs

Avant d’activer espaces de stockage direct, assurez-vous que vos lecteurs sont vides : il n’y a pas d’anciennes partitions ou d’autres données. Exécutez le script suivant, en remplaçant les noms de votre ordinateur, pour supprimer toutes les anciennes partitions ou d’autres données.

> [!Warning]
> Ce script supprime définitivement toutes les données sur les lecteurs autres que le lecteur de démarrage du système d’exploitation.

```PowerShell
# Fill in these variables with your values
$ServerList = "Server01", "Server02", "Server03", "Server04"

Invoke-Command ($ServerList) {
    Update-StorageProviderCache
    Get-StoragePool | ? IsPrimordial -eq $false | Set-StoragePool -IsReadOnly:$false -ErrorAction SilentlyContinue
    Get-StoragePool | ? IsPrimordial -eq $false | Get-VirtualDisk | Remove-VirtualDisk -Confirm:$false -ErrorAction SilentlyContinue
    Get-StoragePool | ? IsPrimordial -eq $false | Remove-StoragePool -Confirm:$false -ErrorAction SilentlyContinue
    Get-PhysicalDisk | Reset-PhysicalDisk -ErrorAction SilentlyContinue
    Get-Disk | ? Number -ne $null | ? IsBoot -ne $true | ? IsSystem -ne $true | ? PartitionStyle -ne RAW | % {
        $_ | Set-Disk -isoffline:$false
        $_ | Set-Disk -isreadonly:$false
        $_ | Clear-Disk -RemoveData -RemoveOEM -Confirm:$false
        $_ | Set-Disk -isreadonly:$true
        $_ | Set-Disk -isoffline:$true
    }
    Get-Disk | Where Number -Ne $Null | Where IsBoot -Ne $True | Where IsSystem -Ne $True | Where PartitionStyle -Eq RAW | Group -NoElement -Property FriendlyName
} | Sort -Property PsComputerName, Count
```

La sortie doit ressembler à ceci, où **Count** est le nombre de lecteurs de chaque modèle dans chaque serveur :

```
Count Name                          PSComputerName
----- ----                          --------------
4     ATA SSDSC2BA800G4n            Server01
10    ATA ST4000NM0033              Server01
4     ATA SSDSC2BA800G4n            Server02
10    ATA ST4000NM0033              Server02
4     ATA SSDSC2BA800G4n            Server03
10    ATA ST4000NM0033              Server03
4     ATA SSDSC2BA800G4n            Server04
10    ATA ST4000NM0033              Server04
```

### <a name="step-32-validate-the-cluster"></a>Étape 3,2 : valider le cluster

Dans cette étape, vous allez exécuter l’outil de validation de cluster pour vous assurer que les nœuds de serveur sont configurés correctement pour créer un cluster à l’aide de espaces de stockage direct. Lorsque la validation de cluster (`Test-Cluster`) est exécutée avant la création du cluster, elle exécute les tests qui vérifient que la configuration semble appropriée pour fonctionner correctement en tant que cluster de basculement. L’exemple ci-dessous utilise le paramètre `-Include`, puis les catégories de tests spécifiques sont spécifiées. Cela garantit que les tests spécifiques des espaces de stockage direct sont inclus dans la validation.

Utilisez la commande PowerShell suivante pour valider un ensemble de serveurs à utiliser comme cluster d’espaces de stockage direct.

```PowerShell
Test-Cluster –Node <MachineName1, MachineName2, MachineName3, MachineName4> –Include "Storage Spaces Direct", "Inventory", "Network", "System Configuration"
```

### <a name="step-33-create-the-cluster"></a>Étape 3,3 : créer le cluster

Dans cette étape, vous allez créer un cluster avec les nœuds que vous avez validés pour la création du cluster à l’étape précédente à l’aide de l’applet de commande PowerShell suivante.

Lorsque vous créez le cluster, vous obtenez un avertissement indiquant que des problèmes se sont produits lors de la création du rôle en cluster qui peut l’empêcher de démarrer. Pour de plus amples informations, consultez le fichier de rapport ci-dessous. » Vous pouvez ignorer cet avertissement en toute sécurité. Cela est dû au fait qu’aucun disque n’est disponible pour le quorum du cluster. Il est recommandé de configurer un témoin de partage de fichiers ou un témoin de cloud après la création du cluster.

> [!Note]
> Si les serveurs utilisent des adresses IP statiques, modifiez la commande suivante afin de refléter l’adresse IP statique en ajoutant le paramètre suivant et en spécifiant l’adresse IP : –StaticAddress &lt;X.X.X.X&gt;.
> Dans la commande suivante, l’espace réservé ClusterName doit être remplacé par un nom NetBIOS unique et d’au maximum 15 caractères.
> ```PowerShell
> New-Cluster –Name <ClusterName> –Node <MachineName1,MachineName2,MachineName3,MachineName4> –NoStorage
> ```

Une fois le cluster créé, la réplication de l’entrée DNS pour le nom du cluster peut demander du temps. Cette durée dépend de l’environnement et la configuration de la réplication DNS. Si la résolution du cluster échoue, dans la plupart des cas, vous pouvez réussir en utilisant le nom d’ordinateur d’un nœud est un membre actif du cluster à la place du nom du cluster.

### <a name="step-34-configure-a-cluster-witness"></a>Étape 3,4 : configurer un témoin de cluster

Nous vous recommandons de configurer un témoin pour le cluster, de sorte que les clusters avec trois serveurs ou plus peuvent résister à l’échec ou à la mise hors connexion de deux serveurs. Un déploiement sur deux serveurs requiert un témoin de cluster, sinon l’un des serveurs en mode hors connexion provoque également l’indisponibilité de l’autre. Avec ces systèmes, vous pouvez utiliser un partage de fichiers en tant que témoin, ou utiliser un témoin de cloud. 

Pour plus d’informations, voir les rubriques suivantes :

- [Configurer et gérer le quorum](../../failover-clustering/manage-cluster-quorum.md)
- [Déployer un témoin de Cloud pour un cluster de basculement](../../failover-clustering/deploy-cloud-witness.md)

### <a name="step-35-enable-storage-spaces-direct"></a>Étape 3.5 : Activer les espaces de stockage direct

Après avoir créé le cluster, utilisez l’applet de commande PowerShell `Enable-ClusterStorageSpacesDirect`, qui met le système de stockage en mode espaces de stockage direct et effectue automatiquement les opérations suivantes :

-   **Créer un pool :** crée un seul pool volumineux qui porte un nom semblable à « S2D sur Cluster1 ».

-   **Configurer les caches des espaces de stockage direct :** si plusieurs types de média (lecteur) sont disponibles pour l’utilisation des espaces de stockage direct, les plus rapides en tant que périphériques cache sont activés (en lecture et en écriture dans la plupart des cas).

-   **Niveaux :** Crée deux niveaux en tant que niveaux par défaut. L’un est appelé « Capacité » et l’autre est appelé « Performances ». L’applet de commande analyse les périphériques et configure chaque niveau avec une combinaison de types de périphériques et de résilience.

À partir du système de gestion, dans une fenêtre de commande PowerShell ouverte avec des privilèges d’administrateur, lancez la commande suivante. Le nom de cluster est celui du cluster que vous avez créé aux étapes précédentes. Si cette commande est exécutée localement sur l’un des nœuds, le paramètre -CimSession n’est pas nécessaire.

```PowerShell
Enable-ClusterStorageSpacesDirect –CimSession <ClusterName>
```

Pour activer vos espaces de stockage direct à l’aide de la commande ci-dessus, vous pouvez également utiliser le nom du nœud à la place du nom du cluster. L’utilisation du nom du nœud peut être plus fiable en raison de retards de réplication DNS qui peuvent se produire avec le nom du cluster nouvellement créé.

Quand l’exécution de cette commande est terminée, ce qui peut prendre plusieurs minutes, le système est prêt pour la création de volumes.

### <a name="step-36-create-volumes"></a>Étape 3.6 : Créer des volumes

Nous vous recommandons d’utiliser l’applet de commande `New-Volume`, car elle offre l’expérience la plus rapide et la plus simple. Cet applet de commande unique crée et formate automatiquement le disque virtuel et les partitions, crée le volume avec le nom correspondant et l’ajoute aux volumes partagés de cluster – le tout en une seule étape facile.

Pour plus d’informations, consultez [Création de volumes dans les espaces de stockage direct](create-volumes.md).

### <a name="step-37-optionally-enable-the-csv-cache"></a>Étape 3,7 : activer éventuellement le cache de volume partagé de cluster

Vous pouvez éventuellement activer le cache de volume partagé de cluster pour utiliser la mémoire système (RAM) en tant que cache au niveau du bloc d’écriture directe des opérations de lecture qui ne sont pas encore mises en cache par le gestionnaire de cache Windows. Cela peut améliorer les performances des applications telles que Hyper-V. Le cache de volume partagé de cluster peut améliorer les performances des requêtes de lecture et est également utile pour les scénarios de Serveur de fichiers avec montée en puissance parallèle.

L’activation du cache de volume partagé de cluster réduit la quantité de mémoire disponible pour l’exécution de machines virtuelles sur un cluster hyper-convergé. vous devez donc équilibrer les performances de stockage avec la mémoire disponible pour les disques durs virtuels.

Pour définir la taille du cache de volume partagé de cluster, ouvrez une session PowerShell sur le système de gestion avec un compte disposant d’autorisations d’administrateur sur le cluster de stockage, puis utilisez ce script, en modifiant les variables `$ClusterName` et `$CSVCacheSize` selon le cas (cet exemple définit un cache CSV de 2 Go par serveur) :

```PowerShell
$ClusterName = "StorageSpacesDirect1"
$CSVCacheSize = 2048 #Size in MB

Write-Output "Setting the CSV cache..."
(Get-Cluster $ClusterName).BlockCacheSize = $CSVCacheSize

$CSVCurrentCacheSize = (Get-Cluster $ClusterName).BlockCacheSize
Write-Output "$ClusterName CSV cache size: $CSVCurrentCacheSize MB"
```

Pour plus d’informations, consultez [utilisation du cache de lecture en mémoire du volume partagé de](csv-cache.md)cluster.

### <a name="step-38-deploy-virtual-machines-for-hyper-converged-deployments"></a>Étape 3,8 : déployer des machines virtuelles pour les déploiements hyper-convergent

Si vous déployez un cluster hyper-convergé, la dernière étape consiste à approvisionner des machines virtuelles sur le cluster espaces de stockage direct.

Les fichiers de la machine virtuelle doivent être stockés dans l’espace de noms CSV du système (exemple : c :\\ClusterStorage\\volume1) comme les machines virtuelles en cluster sur les clusters de basculement.

Vous pouvez utiliser les outils intégrés ou d’autres outils pour gérer le stockage et les machines virtuelles, par exemple System Center Virtual Machine Manager.

## <a name="step-4-deploy-scale-out-file-server-for-converged-solutions"></a>Étape 4 : déployer des Serveur de fichiers avec montée en puissance parallèle pour les solutions convergées

Si vous déployez une solution convergée, l’étape suivante consiste à créer une instance Serveur de fichiers avec montée en puissance parallèle et à configurer certains partages de fichiers. Si vous déployez un cluster hyper-convergé, vous avez terminé et vous n’avez pas besoin de cette section.

### <a name="step-41-create-the-scale-out-file-server-role"></a>Étape 4,1 : créer le rôle Serveur de fichiers avec montée en puissance parallèle

L’étape suivante de la configuration des services de cluster pour votre serveur de fichiers consiste à créer le rôle de serveur de fichiers en cluster, ce qui est le cas lorsque vous créez l’instance de Serveur de fichiers avec montée en puissance parallèle sur laquelle vos partages de fichiers disponibles en continu sont hébergés.

#### <a name="to-create-a-scale-out-file-server-role-by-using-server-manager"></a>Pour créer un rôle de Serveur de fichiers avec montée en puissance parallèle à l’aide de Gestionnaire de serveur

1. Dans Gestionnaire du cluster de basculement, sélectionnez le cluster, accédez à **rôles**, puis cliquez sur **configurer le rôle...** .<br>L’Assistant haute disponibilité s’affiche.
2. Dans la page **Sélectionner un rôle** , cliquez sur serveur de **fichiers**.
3. Sur la page **type de serveur de fichiers** , cliquez sur **serveur de fichiers avec montée en puissance parallèle pour les données d’application**.
4. Dans la page **point d’accès client** , tapez un nom pour le serveur de fichiers avec montée en puissance parallèle.
5. Vérifiez que le rôle a été correctement configuré en accédant à **rôles** et en confirmant que la colonne **État** indique **s’exécuter** en regard du rôle de serveur de fichiers en cluster que vous avez créé, comme illustré à la figure 1.

   ![Capture d’écran de gestionnaire du cluster de basculement montrant le Serveur de fichiers avec montée en puissance parallèle](media/Hyper-converged-solution-using-Storage-Spaces-Direct-in-Windows-Server-2016/SOFS_in_FCM.png "Gestionnaire du cluster de basculement montrant les serveur de fichiers avec montée en puissance parallèle")

    **Figure 1** Gestionnaire du cluster de basculement de l’Serveur de fichiers avec montée en puissance parallèle avec l’État en cours d’exécution

> [!NOTE]
>  Après avoir créé le rôle en cluster, il peut y avoir des retards de propagation du réseau qui peuvent vous empêcher de créer des partages de fichiers sur ce dernier pendant quelques minutes, ou potentiellement plus longtemps.  
  
#### <a name="to-create-a-scale-out-file-server-role-by-using-windows-powershell"></a>Pour créer un rôle de Serveur de fichiers avec montée en puissance parallèle à l’aide de Windows PowerShell

 Dans une session Windows PowerShell qui est connectée au cluster de serveurs de fichiers, entrez les commandes suivantes pour créer le rôle Serveur de fichiers avec montée en puissance parallèle, modifiez *FSCLUSTER* pour qu’il corresponde au nom de votre cluster et *SOFS* pour qu’il corresponde au nom que vous souhaitez attribuer au rôle serveur de fichiers avec montée en puissance parallèle :

```PowerShell
Add-ClusterScaleOutFileServerRole -Name SOFS -Cluster FSCLUSTER
```

> [!NOTE]
>  Après avoir créé le rôle en cluster, il peut y avoir des retards de propagation du réseau qui peuvent vous empêcher de créer des partages de fichiers sur ce dernier pendant quelques minutes, ou potentiellement plus longtemps. Si le rôle SOFS échoue immédiatement et ne démarre pas, cela peut être dû au fait que l’objet ordinateur du cluster n’est pas autorisé à créer un compte d’ordinateur pour le rôle SOFS. Pour obtenir de l’aide, consultez ce billet de blog : [serveur de fichiers avec montée en puissance parallèle rôle ne parvient pas à démarrer avec les ID d’événement 1205, 1069 et 1194](http://www.aidanfinn.com/?p=14142).

### <a name="step-42-create-file-shares"></a>Étape 4,2 : créer des partages de fichiers

Une fois que vous avez créé vos disques virtuels et que vous les avez ajoutés à CSV, il est temps de créer des partages de fichiers sur chacun d’eux : un partage de fichiers par CSV par disque virtuel. System Center Virtual Machine Manager (VMM) est probablement le moyen handiest de le faire car il gère des autorisations pour vous, mais si vous ne l’avez pas dans votre environnement, vous pouvez utiliser Windows PowerShell pour automatiser partiellement le déploiement.

Utilisez les scripts inclus dans le script [configuration du partage SMB pour les charges de travail Hyper-V](http://gallery.technet.microsoft.com/SMB-Share-Configuration-4a36272a) , qui automatise partiellement le processus de création de groupes et de partages. Il est écrit pour les charges de travail Hyper-V. par conséquent, si vous déployez d’autres charges de travail, vous devrez peut-être modifier les paramètres ou effectuer des étapes supplémentaires après avoir créé les partages. Par exemple, si vous utilisez Microsoft SQL Server, le compte de service SQL Server doit disposer du contrôle total sur le partage et le système de fichiers.

> [!NOTE]
>  Vous devrez mettre à jour l’appartenance au groupe lorsque vous ajoutez des nœuds de cluster, sauf si vous utilisez System Center Virtual Machine Manager pour créer vos partages.

Pour créer des partages de fichiers à l’aide de scripts PowerShell, procédez comme suit :

1. Téléchargez les scripts inclus dans la [configuration du partage SMB pour les charges de travail Hyper-V](http://gallery.technet.microsoft.com/SMB-Share-Configuration-4a36272a) vers l’un des nœuds du cluster de serveurs de fichiers.
2. Ouvrez une session Windows PowerShell avec des informations d’identification d’administrateur de domaine sur le système de gestion, puis utilisez le script suivant pour créer un groupe de Active Directory pour les objets ordinateur Hyper-V, en modifiant les valeurs des variables en fonction de vos environnement

    ```PowerShell
    # Replace the values of these variables
    $HyperVClusterName = "Compute01"
    $HyperVObjectADGroupSamName = "Hyper-VServerComputerAccounts" <#No spaces#>
    $ScriptFolder = "C:\Scripts\SetupSMBSharesWithHyperV"

    # Start of script itself
    CD $ScriptFolder
    .\ADGroupSetup.ps1 -HyperVObjectADGroupSamName $HyperVObjectADGroupSamName -HyperVClusterName $HyperVClusterName
    ```
3. Ouvrez une session Windows PowerShell avec des informations d’identification d’administrateur sur l’un des nœuds de stockage, puis utilisez le script suivant pour créer des partages pour chaque volume partagé de cluster et octroyer des autorisations d’administration aux partages au groupe Admins du domaine et au cluster de calcul.

    ```PowerShell
    # Replace the values of these variables
    $StorageClusterName = "StorageSpacesDirect1"
    $HyperVObjectADGroupSamName = "Hyper-VServerComputerAccounts" <#No spaces#>
    $SOFSName = "SOFS"
    $SharePrefix = "Share"
    $ScriptFolder = "C:\Scripts\SetupSMBSharesWithHyperV"

    # Start of the script itself
    CD $ScriptFolder
    Get-ClusterSharedVolume -Cluster $StorageClusterName | ForEach-Object
    {
        $ShareName = $SharePrefix + $_.SharedVolumeInfo.friendlyvolumename.trimstart("C:\ClusterStorage\Volume")
        Write-host "Creating share $ShareName on "$_.name "on Volume: " $_.SharedVolumeInfo.friendlyvolumename
        .\FileShareSetup.ps1 -HyperVClusterName $StorageClusterName -CSVVolumeNumber $_.SharedVolumeInfo.friendlyvolumename.trimstart("C:\ClusterStorage\Volume") -ScaleOutFSName $SOFSName -ShareName $ShareName -HyperVObjectADGroupSamName $HyperVObjectADGroupSamName
    }
    ```

### <a name="step-43-enable-kerberos-constrained-delegation"></a>Étape 4,3 activer la délégation Kerberos avec restriction

Pour configurer la délégation Kerberos confrontée pour la gestion de scénario à distance et une sécurité accrue Migration dynamique, à partir de l’un des nœuds du cluster de stockage, utilisez le script KCDSetup. ps1 inclus dans la [configuration du partage SMB pour les charges de travail Hyper-V](http://gallery.technet.microsoft.com/SMB-Share-Configuration-4a36272a). Voici un petit wrapper pour le script :

```PowerShell
$HyperVClusterName = "Compute01"
$ScaleOutFSName = "SOFS"
$ScriptFolder = "C:\Scripts\SetupSMBSharesWithHyperV"

CD $ScriptFolder
.\KCDSetup.ps1 -HyperVClusterName $HyperVClusterName -ScaleOutFSName $ScaleOutFSName -EnableLM
```

## <a name="next-steps"></a>Étapes suivantes

Après le déploiement de votre serveur de fichiers en cluster, nous vous recommandons de tester les performances de votre solution à l’aide de charges de travail synthétiques avant de mettre en place des charges de travail réelles. Cela vous permet de vérifier que la solution fonctionne correctement et de résoudre les problèmes en attente avant d’ajouter la complexité des charges de travail. Pour plus d’informations, consultez [tester les performances des espaces de stockage à l’aide de charges de travail synthétiques](https://technet.microsoft.com/library/dn894707.aspx).

## <a name="see-also"></a>Voir également

-   [espaces de stockage direct dans Windows Server 2016](storage-spaces-direct-overview.md)
-   [Comprendre le cache dans espaces de stockage direct](understand-the-cache.md)
-   [Planification des volumes dans espaces de stockage direct](plan-volumes.md)
-   [Tolérance aux pannes des espaces de stockage](storage-spaces-fault-tolerance.md)
-   [espaces de stockage direct configuration matérielle requise](Storage-Spaces-Direct-Hardware-Requirements.md)
-   [To RDMA, or not to RDMA – that is the question](https://blogs.technet.microsoft.com/filecab/2017/03/27/to-rdma-or-not-to-rdma-that-is-the-question/) (blog TechNet)
