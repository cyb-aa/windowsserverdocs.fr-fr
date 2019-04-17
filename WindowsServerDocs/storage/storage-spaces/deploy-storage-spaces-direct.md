---
title: Déployer des espaces de stockage direct
ms.prod: windows-server-threshold
manager: eldenc
ms.author: stevenek
ms.technology: storage-spaces
ms.topic: get-started-article
ms.assetid: 20fee213-8ba5-4cd3-87a6-e77359e82bc0
author: stevenek
ms.date: 8/16/2018
description: Instructions pas à pas pour déployer le stockage à définition logicielle avec des espaces de stockage Direct dans Windows Server en tant qu’infrastructure hyperconvergée ou convergé infrastructure (également appelé désagrégée).
ms.localizationpriority: medium
ms.openlocfilehash: 55cfa0e066506d7174f9e5b1e61cc0aa290706d7
ms.sourcegitcommit: 65e4f760be73104e67847f77e834e7b5e065211b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/23/2018
ms.locfileid: "5429041"
---
# Déployer des espaces de stockage direct

> S’applique à: Windows Server 2019, Windows Server 2016

Cette rubrique fournit des instructions détaillées pour déployer des [Espaces de stockage Direct](storage-spaces-direct-overview.md).

> [!Tip]
> Vous cherchez à acquérir une Infrastructure hyperconvergée? Microsoft recommande de ces solutions [Windows Server Software-Defined](https://microsoft.com/wssd) de nos partenaires. Ils sont conçues, assemblées et validées par rapport à notre architecture de référence pour assurer la compatibilité et la fiabilité, afin de vous préparer et rapidement opérationnels.

> [!Tip]
> Vous pouvez utiliser des machines virtuelles de Hyper-V, notamment dans Microsoft Azure, pour [évaluer les espaces de stockage Direct sans matériel](storage-spaces-direct-in-vm.md). Vous pouvez également passer en revue les pratiques [scripts de déploiement de laboratoire rapide de Windows Server](https://aka.ms/wslab), que nous utilisons à des fins de formation.

## Avant de commencer

Passez en revue les [espaces de stockage Direct configuration matérielle requise](Storage-Spaces-Direct-Hardware-Requirements.md) et Parcourir ce document pour vous familiariser avec l’approche globale et des remarques importantes associées à certaines étapes.

Collecter les informations suivantes:

- **Option de déploiement.** Prend en charge les espaces de stockage Direct [deux options de déploiement: hyperconvergé et convergé](storage-spaces-direct-overview.md#deployment-options), également appelé désagrégée. Vous devez vous familiariser avec les avantages de chaque déterminer ce qui vous convient. Étapes 1 à 3 ci-dessous s’appliquent à ces deux options de déploiement. Étape 4 est uniquement nécessaire pour le déploiement convergé.

- **Noms de serveur.** Familiarisez-vous avec les stratégies d’attribution de noms de votre organisation pour les ordinateurs, les fichiers, les chemins d’accès et les autres ressources. Vous devez configurer plusieurs serveurs, chacun avec des noms uniques.

- **Nom de domaine.** Familiarisez-vous avec les stratégies de votre organisation pour l’attribution de noms de domaine et jonction de domaine.  Vous allez joindre les serveurs de votre domaine, et vous devez spécifier le nom de domaine. 

- **Mise en réseau RDMA.** Il existe deux types de protocoles RDMA: iWarp et RoCE. Notez celle qui permet de vos cartes réseau, et si RoCE, Notez également la version (v1 ou v2). Pour RoCE, Notez également le modèle de votre commutateur top-of-rack.

- **ID DE RÉSEAU LOCAL VIRTUEL.** Notez l’ID de réseau local virtuel pour être utilisé pour les cartes réseau de gestion du système d’exploitation sur les serveurs, le cas échéant. Vous devriez pouvoir l’obtenir auprès de votre administrateur réseau.

## Étape1: Déployer Windows Server

### Étape 1.1: Installer le système d’exploitation

La première étape consiste à installer Windows Server sur chaque serveur qui sera dans le cluster. Windows Server 2016 Datacenter Edition requiert les espaces de stockage Direct. Vous pouvez utiliser l’option d’installation Server Core ou serveur avec expérience utilisateur.

Lorsque vous installez Windows Server à l’aide de l’Assistant installation, vous pouvez choisir entre *Windows Server* (référence à Server Core) et *Windows Server (serveur avec expérience utilisateur)*, qui est l’équivalent de l’option d’installation *complète* disponible dans Windows Server 2012 R2. Si vous ne choisissez pas, vous obtiendrez l’option d’installation Server Core. Pour plus d’informations, voir les [Options d’Installation pour Windows Server 2016](../../get-started/Windows-Server-2016.md).

### Étape 1.2: Se connecter aux serveurs

Ce guide concentre l’option d’installation Server Core et le déploiement/gestion à distance à partir d’un système de gestion distincte, qui doit avoir:

- Windows Server 2016 avec les mêmes mises à jour que les serveurs qu’il gère
- Connectivité réseau aux serveurs qu’il gère
- Joint au même domaine ou à un domaine entièrement fiable
- Outils d’administration de serveur distant et modules PowerShell pour Hyper-V et le clustering de basculement. Outils de serveur distant et modules PowerShell sont disponibles sur Windows Server et peuvent être installées sans installer d’autres fonctionnalités. Vous pouvez également installer les [Outils d’Administration de serveur distant](https://www.microsoft.com/download/details.aspx?id=45520) sur un PC de gestion de Windows 10.

Sur le système de gestion, installez le cluster de basculement et les outils de gestion Hyper-V. Vous pouvez effectuer cette opération à l’aide du Gestionnaire de serveur en utilisant l’**Assistant Ajout de rôles et de fonctionnalités**. Dans la page **Fonctionnalités**, sélectionnez **Outils d’administration de serveur distant**, puis sélectionnez les outils à installer.

Ouvrez la session PowerShell, puis utilisez le nom du serveur ou l’adresseIP du nœud auquel vous souhaitez vous connecter. Vous serez invité à entrer un mot de passe après avoir exécuté cette commande, entrez le mot de passe administrateur que vous avez spécifié lors de la configuration de Windows.

   ```PowerShell
   Enter-PSSession -ComputerName <myComputerName> -Credential LocalHost\Administrator
   ```

   Voici un exemple d’exécution de la même chose d’une façon qui est plus utile dans les scripts, au cas où vous avez besoin effectuer cette opération plusieurs fois:

   ```PowerShell
   $myServer1 = "myServer-1"
   $user = "$myServer1\Administrator"

   Enter-PSSession -ComputerName $myServer1 -Credential $user
   ```

> [!TIP]
> Si vous déployez à distance à partir d’un système de gestion, vous risquez d’obtenir une erreur comme *WinRM ne peut pas traiter la demande.* Pour résoudre ce problème, utilisez Windows PowerShell pour ajouter chaque serveur à la liste des hôtes approuvés sur votre ordinateur de gestion:  
>   
> `Set-Item WSMAN:\Localhost\Client\TrustedHosts -Value Server01 -Force`
>  
> Remarque: la liste des hôtes approuvés prend en charge les caractères génériques, par exemple `Server*`.
>
> Pour afficher votre liste des hôtes approuvés, tapez `Get-Item WSMAN:\Localhost\Client\TrustedHosts`.  
>   
> Pour vider la liste, tapez `Clear-Item WSMAN:\Localhost\Client\TrustedHost`.  

### Étape 1.3: Joindre le domaine et ajouter des comptes de domaine

Jusqu'à présent, vous avez configuré les serveurs individuels avec le compte administrateur local, `<ComputerName>\Administrator`.

Pour gérer les espaces de stockage Direct, vous devez joindre les serveurs à un domaine et utiliser un compte de domaine Active Directory Domain Services qui se trouve dans le groupe d’administrateurs sur chaque serveur.

À partir du système de gestion, ouvrez une console PowerShell avec des privilèges d’administrateur. Utilisez `Enter-PSSession` pour vous connecter à chaque serveur, exécutez l’applet de commande suivante, en remplaçant votre propre nom d’ordinateur, nom de domaine et les informations d’identification de domaine:

```PowerShell  
Add-Computer -NewName "Server01" -DomainName "contoso.com" -Credential "CONTOSO\User" -Restart -Force  
```

Si votre compte d’administrateur de stockage n’est pas membre du groupe Admins du domaine, ajoutez votre compte d’administrateur de stockage au groupe Administrateurs local sur chaque nœud - ou mieux encore, ajoutez le groupe que vous utilisez pour les administrateurs de stockage. Vous pouvez utiliser la commande suivante (ou écrire une fonction de Windows PowerShell pour effectuer cette opération - pour plus d’informations, voir [Utiliser PowerShell pour ajouter des utilisateurs de domaine à un groupe Local](http://blogs.technet.com/b/heyscriptingguy/archive/2010/08/19/use-powershell-to-add-domain-users-to-a-local-group.aspx) ):

```
Net localgroup Administrators <Domain\Account> /add
```

### Étape 1.4: Installer des rôles et fonctionnalités

L’étape suivante consiste à installer des rôles de serveur sur chaque serveur. Vous pouvez le faire à l’aide de [Windows Admin Center](../../manage/windows-admin-center/use/manage-servers.md), [Le Gestionnaire de serveur](../../administration/server-manager/install-or-uninstall-roles-role-services-or-features.md)), ou PowerShell. Voici les rôles à installer:

- Clustering de basculement
- Hyper-V
- Serveur de fichiers (si vous souhaitez héberger les partages de fichiers, par exemple pour un déploiement convergé)
- Data-Center-Bridging (si vous utilisez RoCEv2 au lieu des cartes réseau iWARP)
- RSAT-Clustering-PowerShell
- Hyper-V-PowerShell

Pour installer via PowerShell, utilisez l’applet de commande [Install-WindowsFeature](https://docs.microsoft.com/powershell/module/microsoft.windows.servermanager.migration/install-windowsfeature) . Vous pouvez l’utiliser sur un seul serveur comme suit:

```PowerShell
Install-WindowsFeature -Name "Hyper-V", "Failover-Clustering", "Data-Center-Bridging", "RSAT-Clustering-PowerShell", "Hyper-V-PowerShell", "FS-FileServer"
```

Pour exécuter la commande sur tous les serveurs du cluster en même temps, utilisez ce peu de script, modification de la liste des variables au début du script pour s’adapter à votre environnement.

```PowerShell
# Fill in these variables with your values
$ServerList = "Server01", "Server02", "Server03", "Server04"
$FeatureList = "Hyper-V", "Failover-Clustering", "Data-Center-Bridging", "RSAT-Clustering-PowerShell", "Hyper-V-PowerShell", "FS-FileServer"

# This part runs the Install-WindowsFeature cmdlet on all servers in $ServerList, passing the list of features into the scriptblock with the "Using" scope modifier so you don't have to hard-code them here.
Invoke-Command ($ServerList) {
    Install-WindowsFeature -Name $Using:Featurelist
}
```

## Étape2: Configurer le réseau

Si vous déployez des espaces de stockage Direct à l’intérieur des machines virtuelles, ignorez cette section.

Bande passante élevée, faible latence réseau entre les serveurs du cluster requiert les espaces de stockage Direct. Mise en réseau GbE au moins 10 est nécessaire et un accès direct à la mémoire à distance (RDMA) est recommandé. Vous pouvez utiliser iWARP ou RoCE tant qu’elles présentent le logo Windows Server 2016, mais iWARP est généralement plus facile à configurer.

> [!Important]
> Selon votre matériel de mise en réseau et en particulier avec RoCE v2, une configuration du commutateur top-of-rack peut être nécessaire. Configuration de commutateur correct est importante de s’assurer de la fiabilité et les performances des espaces de stockage Direct.

Windows Server 2016 introduit switch embedded association (SET) au sein du commutateur virtuel Hyper-V. Ainsi, les mêmes ports de carte réseau physiques à utiliser pour tout le trafic réseau lors de l’utilisation RDMA, en réduisant le nombre de ports de carte réseau physique requis. Set (switch embedded Teaming la) est recommandée pour les espaces de stockage Direct.

Pour savoir comment configurer un réseau pour les espaces de stockage Direct, consultez [Windows Server 2016 convergé NIC et Guide de déploiement RDMA invité](https://github.com/Microsoft/SDN/blob/master/Diagnostics/S2D%20WS2016_ConvergedNIC_Configuration.docx).

## Étape3: Configurer les espaces de stockage direct

Les étapes suivantes sont effectuées sur un système de gestion qui est de la même version que les serveurs en cours de configuration. Les étapes suivantes ne doivent pas être exécutés à distance à l’aide d’une session PowerShell, mais est exécutées dans une session PowerShell locale sur le système de gestion, avec des autorisations administratives.

### Étape 3.1: Nettoyage des lecteurs.

Avant d’activer les espaces de stockage Direct, vérifiez vos disques sont vides: aucune partition ancienne ou autres données. Exécutez le script suivant, en remplaçant vos noms d’ordinateur, pour supprimer toutes les partitions anciennes ou autres données.

> [!Warning]
> Ce script supprimera définitivement toutes les données sur les lecteurs autres que le lecteur de démarrage du système d’exploitation!

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

La sortie doit ressembler à ceci, où le **nombre** est le nombre de disques de chaque modèle de chaque serveur:

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

### Étape 3.2: Valider le cluster

Dans cette étape, vous allez exécuter l’outil de validation de cluster pour garantir que les nœuds de serveur sont correctement configurés pour créer un cluster à l’aide d’espaces de stockage Direct. Validation de cluster lorsque (`Test-Cluster`) est exécutée avant la création du cluster, il exécute les tests qui vérifient que la configuration est appropriée pour fonctionner correctement comme un cluster de basculement. L’exemple ci-dessous utilise le `-Include` paramètre, puis les catégories de tests spécifiques sont définies. Cela garantit que les tests spécifiques des espaces de stockage direct sont inclus dans la validation.

Utilisez la commande PowerShell suivante pour valider un ensemble de serveurs à utiliser comme cluster d'espaces de stockage direct.

```PowerShell
Test-Cluster –Node <MachineName1, MachineName2, MachineName3, MachineName4> –Include "Storage Spaces Direct", "Inventory", "Network", "System Configuration"
```

### Étape 3.3: Créer le cluster

Dans cette étape, vous allez créer un cluster avec les nœuds que vous avez validés pour la création du cluster à l’étape précédente à l’aide de l’applet de commande PowerShell suivante.

Lors de la création du cluster, vous obtiendrez un avertissement indiquant: «des problèmes se sont alors que la création du rôle en cluster qui peut ne pas démarrer. Pour de plus amples informations, consultez le fichier de rapport ci-dessous.» Vous pouvez ignorer cet avertissement en toute sécurité. Cela est dû au fait qu’aucun disque n’est disponible pour le quorum du cluster. Il est recommandé de configurer un témoin de partage de fichiers ou un témoin de cloud après la création du cluster.

> [!Note]
> Si les serveurs utilisent des adressesIP statiques, modifiez la commande suivante afin de refléter l’adresseIP statique en ajoutant le paramètre suivant et en spécifiant l’adresseIP: –StaticAddress&lt;X.X.X.X&gt;.
> Dans la commande suivante, l’espace réservé ClusterName doit être remplacé par un nom NetBIOS unique et d’au maximum 15caractères.
> ```PowerShell
> New-Cluster –Name <ClusterName> –Node <MachineName1,MachineName2,MachineName3,MachineName4> –NoStorage
> ```

Une fois le cluster créé, la réplication de l’entréeDNS pour le nom du cluster peut demander du temps. Cette durée dépend de l’environnement et la configuration de la réplicationDNS. Si la résolution du cluster échoue, dans la plupart des cas, vous pouvez réussir en utilisant le nom d’ordinateur d’un nœud est un membre actif du cluster à la place du nom du cluster.

### Étape 3.4: Configurer un témoin de cluster

Nous vous recommandons de configurer un témoin pour le cluster, afin que les clusters au moins trois serveurs peuvent résister aux deux serveurs Échec ou à la mise hors connexion. Un déploiement à deux serveurs nécessite un témoin de cluster, dans le cas contraire mode serveur hors connexion oblige l’autre à devenir indisponible également. Avec ces systèmes, vous pouvez utiliser un partage de fichiers en tant que témoin, ou utiliser un témoin de cloud. 

Pour plus d’informations, voir les rubriques suivantes:

- [Configurer et gérer le quorum](../../failover-clustering/manage-cluster-quorum.md)
- [Déployer un témoin de cloud pour un cluster de basculement](../../failover-clustering/deploy-cloud-witness.md)

### Étape3.5: Activer les espaces de stockage direct

Après avoir créé le cluster, utilisez le `Enable-ClusterStorageSpacesDirect` applet de commande PowerShell, ce qui mettra le système de stockage dans le mode d’espaces de stockage Direct et automatiquement les opérations suivantes:

-   **Créer un pool:** crée un seul pool volumineux qui porte un nom semblable à «S2D sur Cluster1».

-   **Configurer les caches des espaces de stockage direct:** si plusieurs types de média (disque) sont disponibles pour l’utilisation d'espaces de stockage direct, les plus rapides en tant que périphériques cache sont activés (en lecture et en écriture dans la plupart des cas).

-   **Niveaux:** Crée deux niveaux comme niveaux par défaut. L’un est appelé «Capacité» et l’autre est appelé «Performances». L’applet de commande analyse les périphériques et configure chaque niveau avec une combinaison de types de périphériques et de résilience.

À partir du système de gestion, dans une fenêtre de commande PowerShell ouverte avec des privilèges d’administrateur, lancez la commande suivante. Le nom de cluster est celui du cluster que vous avez créé aux étapes précédentes. Si cette commande est exécutée localement sur l’un des nœuds, le paramètre -CimSession n’est pas nécessaire.

```PowerShell
Enable-ClusterStorageSpacesDirect –CimSession <ClusterName>
```

Pour activer vos espaces de stockage direct à l’aide de la commande ci-dessus, vous pouvez également utiliser le nom du nœud à la place du nom du cluster. L’utilisation du nom du nœud peut être plus fiable en raison de retards de réplication DNS qui peuvent se produire avec le nom du cluster nouvellement créé.

Quand l’exécution de cette commande est terminée, ce qui peut prendre plusieurs minutes, le système est prêt pour la création de volumes.

### Étape 3.6: Créer des volumes

Nous vous recommandons d’utiliser le `New-Volume` applet de commande telle qu’elle assure une expérience plus rapide et plus simple. Cet applet de commande unique crée et formate automatiquement le disque virtuel et les partitions, crée le volume avec le nom correspondant et l’ajoute aux volumes partagés de cluster – le tout en une seule étape facile.

Pour plus d’informations, consultez [Création de volumes dans les espaces de stockage direct](create-volumes.md).

### Étape 3.7: Éventuellement activer le cache de volume partagé de cluster

Vous pouvez éventuellement activer le cache de volume (CSV) partagé de cluster à utiliser la mémoire système (RAM) sert de cache au niveau du bloc écriture à l’aide d’opérations de lecture qui ne sont pas déjà mis en cache par le Gestionnaire de cache de Windows. Cela peut améliorer les performances pour les applications telles que Hyper-V. Le cache de volume partagé de cluster peut améliorer les performances des demandes de lecture et est également utile pour les scénarios de serveur de fichiers avec montée en.

Activation du cache de volume partagé de cluster permet de réduire la quantité de mémoire disponible pour exécuter des ordinateurs virtuels sur un cluster hyperconvergé, vous devez donc trouver un équilibre entre performances de stockage avec la mémoire disponible pour les disques durs virtuels.

Pour définir la taille du cache de volume partagé de cluster, ouvrez une session PowerShell sur le système de gestion avec un compte disposant des autorisations d’administrateur sur le cluster de stockage et ensuite utiliser ce script, en modifiant le `$ClusterName` et `$CSVCacheSize` variables de façon adéquate (cet exemple définit un 2 Go CSV cache par serveur):

```PowerShell
$ClusterName = "StorageSpacesDirect1"
$CSVCacheSize = 2048 #Size in MB

Write-Output "Setting the CSV cache..."
(Get-Cluster $ClusterName).BlockCacheSize = $CSVCacheSize

$CSVCurrentCacheSize = (Get-Cluster $ClusterName).BlockCacheSize
Write-Output "$ClusterName CSV cache size: $CSVCurrentCacheSize MB"
```

Pour plus d’informations, voir [l’utilisation du volume partagé de cluster en mémoire cache de lecture](csv-cache.md).

### Étape 3.8: Déployer des machines virtuelles pour les déploiements hyperconvergés

Si vous déployez un cluster hyperconvergé, la dernière étape consiste à configurer des ordinateurs virtuels sur le cluster d’espaces de stockage Direct.

Les fichiers de la machine virtuelle doivent être stockés dans l’espace de nomsCSV du système (par exemple: c:\\ClusterStorage\\Volume1) exactement comme les machines virtuelles en cluster sur les clusters de basculement.

Vous pouvez utiliser les outils fournis ou autres outils pour gérer le stockage et les machines virtuelles, tels que System Center Virtual Machine Manager.

## Étape 4: Déployer le serveur de fichiers avec montée en pour les solutions convergées

Si vous déployez une solution convergée, l’étape suivante consiste à créer une instance de serveur de fichiers avec montée en et d’installation des partages de fichiers. Si vous déployez un cluster hyperconvergé - vous avez terminé et que vous n’avez pas besoin cette section.

### Étape 4.1: Créer le rôle de serveur de fichiers avec montée en

L’étape suivante dans la configuration du cluster services pour votre serveur de fichiers est créer le rôle de serveur de fichiers en cluster, lorsque vous créez l’instance de serveur de fichiers avec montée en qui héberge vos partages de fichiers disponibles en permanence.

#### Pour créer un rôle de serveur de fichiers avec montée en à l’aide du Gestionnaire de serveur

1.  Dans le Gestionnaire de Cluster de basculement, sélectionnez le cluster, accédez aux **rôles**, puis cliquez sur **Configurer un rôle**.<br>L’Assistant haute disponibilité s’affiche.
2.  Sur la page **Sélectionner un rôle** , cliquez sur le **Serveur de fichiers**.
3.  Dans la page **Type de serveur de fichiers** , cliquez sur **Le serveur de fichiers avec montée en données d’application**.
4.  Sur la page de **Point d’accès Client** , tapez un nom pour le serveur de fichiers avec montée en.
5.  Vérifiez que le rôle a été correctement configuré en accédant à des **rôles** et confirmer que la colonne **état** affiche **en cours d’exécution** en regard du rôle de serveur de fichiers en cluster que vous avez créé, comme illustré dans la Figure 1.

    ![Capture d’écran du Gestionnaire du Cluster de basculement montrant le serveur de fichiers avec montée en](media\Hyper-converged-solution-using-Storage-Spaces-Direct-in-Windows-Server-2016\SOFS_in_FCM.png "Failover Cluster Manager showing the Scale-Out File Server")

     **Figure 1** Gestionnaire du Cluster de basculement montrant le serveur de fichiers avec montée en avec le statut en cours d’exécution

> [!NOTE]
>  Après la création du rôle en cluster, il peut exister certaines réseau retards de propagation de qui vous empêchent de créer des partages de fichiers dessus pendant quelques minutes, ou potentiellement plus.  
  
#### Pour créer un rôle de serveur de fichiers avec montée en à l’aide de Windows PowerShell

 Dans une session Windows PowerShell qui est connectée au cluster de serveurs de fichier, entrez les commandes suivantes pour créer le rôle de serveur de fichiers avec montée en modifiant *FSCLUSTER* pour correspondre au nom de votre cluster et *en puissance* pour correspondre au nom que vous souhaitez donner le Rôle de serveur de fichiers avec montée en:

```PowerShell
Add-ClusterScaleOutFileServerRole -Name SOFS -Cluster FSCLUSTER
```

> [!NOTE]
>  Après la création du rôle en cluster, il peut exister certaines réseau retards de propagation de qui vous empêchent de créer des partages de fichiers dessus pendant quelques minutes, ou potentiellement plus. Si le rôle en puissance échoue immédiatement et ne démarre pas, il peut être parce que l’objet d’ordinateur du cluster n’est pas autorisé à créer un compte d’ordinateur pour le rôle en puissance. Pour faciliter la que, consultez ce billet de blog: [avec montée en fichier serveur rôle échoue à démarrer With événement ID 1205, 1069 et 1194](http://www.aidanfinn.com/?p=14142).

### Étape 4.2: Créer des partages de fichiers

Une fois que vous avez créé vos disques virtuels et ajoutés au volumes partagés de cluster, il est temps de créer des partages de fichiers sur chacun d’eux - partage un seul fichier par volume partagé de cluster par disque virtuel. System Center Virtual Machine Manager (VMM) est probablement le moyen côté de le faire, car il gère les autorisations pour vous, mais si vous ne l’avez pas dans votre environnement, vous pouvez utiliser Windows PowerShell pour automatiser partiellement le déploiement.

Utiliser les scripts inclus dans le script de [Configuration du partage SMB pour les charges de travail Hyper-V](http://gallery.technet.microsoft.com/SMB-Share-Configuration-4a36272a) , qui partiellement automatise le processus de création des groupes et des partages. Elle est destinée aux charges de travail Hyper-V, donc si vous déployez des autres charges de travail, vous devrez peut-être modifier les paramètres ou des étapes supplémentaires après avoir créé les partages. Par exemple, si vous utilisez Microsoft SQL Server, le compte de service SQL Server doit se voir attribuer un contrôle total sur le partage et le système de fichiers.

> [!NOTE]
>  Vous devez mettre à jour l’appartenance au groupe lorsque vous ajoutez des nœuds de cluster, sauf si vous utilisez System Center Virtual Machine Manager pour créer vos partages.

Pour créer des partages de fichiers à l’aide de scripts PowerShell, procédez comme suit:

1. Téléchargez les scripts inclus dans la [Configuration du partage SMB pour les charges de travail Hyper-V](http://gallery.technet.microsoft.com/SMB-Share-Configuration-4a36272a) à l’un des nœuds du cluster de serveurs de fichiers.
2. Ouvrez une session Windows PowerShell avec des droits d’administrateur de domaine sur le système de gestion et ensuite utiliser le script suivant pour créer un groupe d’Active Directory pour les objets ordinateur Hyper-V, la modification des valeurs pour les variables en fonction de votre environnement:

    ```PowerShell
    # Replace the values of these variables
    $HyperVClusterName = "Compute01"
    $HyperVObjectADGroupSamName = "Hyper-VServerComputerAccounts" <#No spaces#>
    $ScriptFolder = "C:\Scripts\SetupSMBSharesWithHyperV"

    # Start of script itself
    CD $ScriptFolder
    .\ADGroupSetup.ps1 -HyperVObjectADGroupSamName $HyperVObjectADGroupSamName -HyperVClusterName $HyperVClusterName
    ```
3. Ouvrez une session Windows PowerShell en tant qu’administrateur sur l’un des nœuds de stockage et ensuite utiliser le script suivant pour créer des partages pour chaque volume partagé de cluster et accorder des autorisations d’administration pour les partages au groupe Admins du domaine et le cluster de calcul.

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

### Activer l’authentification Kerberos étape 4.3 la délégation contrainte

Pour configurer la délégation contrainte Kerberos pour la gestion de scénario à distance et une sécurité accrue Live Migration, à partir d’un des nœuds du cluster de stockage, utilisez le script KCDSetup.ps1 inclus dans la [Configuration du partage SMB pour les charges de travail Hyper-V](http://gallery.technet.microsoft.com/SMB-Share-Configuration-4a36272a). Voici un wrapper peu pour le script:

```PowerShell
$HyperVClusterName = "Compute01"
$ScaleOutFSName = "SOFS"
$ScriptFolder = "C:\Scripts\SetupSMBSharesWithHyperV"

CD $ScriptFolder
.\KCDSetup.ps1 -HyperVClusterName $HyperVClusterName -ScaleOutFSName $ScaleOutFSName -EnableLM
```

## Étapes suivantes

Après le déploiement de votre serveur de fichiers en cluster, nous vous recommandons de tester les performances de votre solution à l’aide de charges de travail synthétiques avant de vous appelez les charges de travail réels. Cela vous permet de confirmer que la solution s’exécute correctement et fonctionnent tous les points en attente avant d’ajouter la complexité des charges de travail. Pour plus d’informations, consultez [Test stockage espaces performances à l’aide de synthétique charges de travail](https://technet.microsoft.com/library/dn894707.aspx).

## Articles associés

-   [Espaces de stockage direct dans WindowsServer2016](storage-spaces-direct-overview.md)
-   [Fonctionnement du cache dans les espaces de stockage direct](understand-the-cache.md)
-   [Planification des volumes dans les espaces de stockage direct](plan-volumes.md)
-   [Tolérance de panne des espaces de stockage](storage-spaces-fault-tolerance.md)
-   [Configuration matérielle requise pour les espaces de stockage direct](Storage-Spaces-Direct-Hardware-Requirements.md)
-   [To RDMA, or not to RDMA – that is the question](https://blogs.technet.microsoft.com/filecab/2017/03/27/to-rdma-or-not-to-rdma-that-is-the-question/) (blog TechNet)
