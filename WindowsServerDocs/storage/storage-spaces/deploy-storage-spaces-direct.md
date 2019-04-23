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
description: Obtenir des instructions détaillées pour déployer le stockage à définition logicielle avec les espaces de stockage Direct dans Windows Server en tant qu’infrastructure hyperconvergé ou convergé infrastructure (également appelé désagrégé).
ms.localizationpriority: medium
ms.openlocfilehash: 55cfa0e066506d7174f9e5b1e61cc0aa290706d7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865410"
---
# <a name="deploy-storage-spaces-direct"></a>Déployer des espaces de stockage direct

> S’applique à : Windows Server 2019, Windows Server 2016

Cette rubrique fournit des instructions détaillées pour déployer [espaces de stockage Direct](storage-spaces-direct-overview.md).

> [!Tip]
> Cherchez à acquérir Hyper-Converged Infrastructure ? Microsoft recommande ces [défini par le logiciel Windows Server](https://microsoft.com/wssd) solutions proposées par nos partenaires. Elles sont conçues, assemblées et validés par rapport à notre architecture de référence pour garantir la compatibilité et la fiabilité, afin de vous être opérationnel rapidement opérationnel.

> [!Tip]
> Vous pouvez utiliser des machines virtuelles de Hyper-V, y compris dans Microsoft Azure, à [évaluer les espaces de stockage Direct sans matériel](storage-spaces-direct-in-vm.md). Vous pouvez également passer en revue les pratiques [scripts de déploiement de laboratoire rapide de Windows Server](https://aka.ms/wslab), que nous utilisons à des fins de formation.

## <a name="before-you-start"></a>Avant de commencer

Examinez le [espaces de stockage Direct configuration matérielle requise](Storage-Spaces-Direct-Hardware-Requirements.md) et ce document pour vous familiariser avec l’approche globale et les notes importantes associées à certaines étapes de cette rubrique.

Collectez les informations suivantes :

- **Option de déploiement.** Prend en charge les espaces de stockage Direct [deux options de déploiement : hyperconvergées et convergé](storage-spaces-direct-overview.md#deployment-options), également appelé désagrégé. Familiarisez-vous avec les avantages de chacun d’eux à décider ce qui vous convient. Étapes 1 à 3 ci-dessous s’appliquent à ces deux options de déploiement. Étape 4 est uniquement nécessaire pour le déploiement convergé.

- **Noms de serveur.** Vous familiariser avec les stratégies d’affectation de noms de votre organisation pour les ordinateurs, les fichiers, les chemins d’accès et les autres ressources. Vous devez configurer plusieurs serveurs, chacun avec des noms uniques.

- **Nom de domaine.** Vous familiariser avec les stratégies de votre organisation pour l’affectation de noms de domaine et de jonction de domaine.  Vous allez joindre les serveurs à votre domaine, et vous devez spécifier le nom de domaine. 

- **Mise en réseau RDMA.** Il existe deux types de protocoles RDMA : iWarp et RoCE. Remarque lequel vos cartes réseau utilisent, et si RoCE, Notez également la version (v1 ou v2). Pour le RoCE, Notez également le modèle de votre commutateur top-of-rack.

- **ID DE VLAN.** Notez l’ID de VLAN à utiliser pour les cartes réseau de gestion du système d’exploitation sur les serveurs, le cas échéant. Vous devriez pouvoir l’obtenir auprès de votre administrateur réseau.

## <a name="step-1-deploy-windows-server"></a>Étape 1 : Déployer Windows Server

### <a name="step-11-install-the-operating-system"></a>Étape 1.1 : Installer le système d’exploitation

La première étape consiste à installer Windows Server sur chaque serveur qui figurera dans le cluster. Espaces de stockage Direct nécessite Windows Server 2016 Datacenter Edition. Vous pouvez utiliser l’option d’installation Server Core ou le serveur avec expérience utilisateur.

Lorsque vous installez Windows Server à l’aide de l’Assistant installation, vous pouvez choisir entre *Windows Server* (qui fait référence à Server Core) et *Windows Server (serveur avec expérience utilisateur)*, ce qui équivaut à de la *complète* option d’installation disponible dans Windows Server 2012 R2. Si vous ne choisissez pas, vous obtiendrez l’option d’installation Server Core. Pour plus d’informations, consultez [Options d’Installation pour Windows Server 2016](../../get-started/Windows-Server-2016.md).

### <a name="step-12-connect-to-the-servers"></a>Étape 1.2 : Se connecter aux serveurs

Ce guide concentre l’option d’installation Server Core et le déploiement/la gestion à distance à partir d’un système de gestion distinct, qui doit avoir :

- Windows Server 2016 avec les mêmes mises à jour que les serveurs qu’il gère
- Connectivité réseau vers les serveurs qu’il gère
- Joint au même domaine ou un domaine entièrement fiable
- Outils d’administration de serveur distant et modules PowerShell pour Hyper-V et le clustering de basculement. Outils de serveur distant et modules PowerShell sont disponibles sur Windows Server et peuvent être installés sans installer d’autres fonctionnalités. Vous pouvez également installer le [outils d’Administration de serveur distant](https://www.microsoft.com/download/details.aspx?id=45520) sur un PC de gestion Windows 10.

Sur le système de gestion, installez le cluster de basculement et les outils de gestion Hyper-V. Vous pouvez effectuer cette opération à l’aide du Gestionnaire de serveur en utilisant l’**Assistant Ajout de rôles et de fonctionnalités**. Dans la page **Fonctionnalités**, sélectionnez **Outils d’administration de serveur distant**, puis sélectionnez les outils à installer.

Ouvrez la session PowerShell, puis utilisez le nom du serveur ou l’adresse IP du nœud auquel vous souhaitez vous connecter. Vous serez invité à entrer un mot de passe après avoir exécuté cette commande, entrez le mot de passe administrateur que vous avez spécifié lors de la configuration Windows.

   ```PowerShell
   Enter-PSSession -ComputerName <myComputerName> -Credential LocalHost\Administrator
   ```

   Voici un exemple de faire la même chose d’une manière qui est plus utile dans les scripts, au cas où vous deviez effectuer cette opération plusieurs fois :

   ```PowerShell
   $myServer1 = "myServer-1"
   $user = "$myServer1\Administrator"

   Enter-PSSession -ComputerName $myServer1 -Credential $user
   ```

> [!TIP]
> Si vous déployez à distance à partir d’un système de gestion, vous pouvez obtenir une erreur telle que *WinRM ne peut pas traiter la demande.* Pour résoudre ce problème, utilisez Windows PowerShell pour ajouter chaque serveur à la liste des hôtes approuvés sur votre ordinateur de gestion :  
>   
> `Set-Item WSMAN:\Localhost\Client\TrustedHosts -Value Server01 -Force`
>  
> Remarque : la liste des hôtes approuvés prend en charge des caractères génériques, tels que `Server*`.
>
> Pour afficher votre liste d’hôtes approuvés, tapez `Get-Item WSMAN:\Localhost\Client\TrustedHosts`.  
>   
> Pour vider la liste, tapez `Clear-Item WSMAN:\Localhost\Client\TrustedHost`.  

### <a name="step-13-join-the-domain-and-add-domain-accounts"></a>Étape 1.3 : Joindre le domaine et ajouter des comptes de domaine

Jusqu'à présent, vous avez configuré les serveurs individuels avec le compte administrateur local, `<ComputerName>\Administrator`.

Pour gérer les espaces de stockage Direct, vous devrez joindre les serveurs à un domaine et utiliser un compte de domaine Active Directory Domain Services qui se trouve dans le groupe Administrateurs sur chaque serveur.

À partir du système de gestion, ouvrez une console PowerShell avec des privilèges d’administrateur. Utilisez `Enter-PSSession` pour vous connecter à chaque serveur et exécutez la cmdlet suivante, en remplaçant votre propre nom de l’ordinateur, nom de domaine et les informations d’identification de domaine :

```PowerShell  
Add-Computer -NewName "Server01" -DomainName "contoso.com" -Credential "CONTOSO\User" -Restart -Force  
```

Si votre compte d’administrateur de stockage n’est pas membre du groupe Admins du domaine, ajoutez votre compte d’administrateur de stockage au groupe Administrateurs local sur chaque nœud - ou mieux encore, ajoutez le groupe que vous utilisez pour les administrateurs de stockage. Vous pouvez utiliser la commande suivante (ou écrivez une fonction de Windows PowerShell pour faire - voir [utiliser PowerShell pour ajouter des utilisateurs de domaine à un groupe Local](http://blogs.technet.com/b/heyscriptingguy/archive/2010/08/19/use-powershell-to-add-domain-users-to-a-local-group.aspx) pour plus d’informations) :

```
Net localgroup Administrators <Domain\Account> /add
```

### <a name="step-14-install-roles-and-features"></a>Étape 1.4 : Installer des rôles et fonctionnalités

L’étape suivante consiste à installer des rôles de serveur sur chaque serveur. Vous pouvez le faire à l’aide de [Windows Admin Center](../../manage/windows-admin-center/use/manage-servers.md), [le Gestionnaire de serveur](../../administration/server-manager/install-or-uninstall-roles-role-services-or-features.md)), ou de PowerShell. Voici les rôles à installer :

- Clustering de basculement
- Hyper-V
- Serveur de fichiers (si vous souhaitez héberger les partages de fichiers, par exemple, pour un déploiement convergé)
- Data-Center-Bridging (si vous utilisez RoCEv2 au lieu des cartes réseau iWARP)
- RSAT-Clustering-PowerShell
- Hyper-V-PowerShell

Pour installer via PowerShell, utilisez le [Install-WindowsFeature](https://docs.microsoft.com/powershell/module/microsoft.windows.servermanager.migration/install-windowsfeature) applet de commande. Vous pouvez l’utiliser sur un seul serveur comme suit :

```PowerShell
Install-WindowsFeature -Name "Hyper-V", "Failover-Clustering", "Data-Center-Bridging", "RSAT-Clustering-PowerShell", "Hyper-V-PowerShell", "FS-FileServer"
```

Pour exécuter la commande sur tous les serveurs du cluster en même temps, utilisez ce petit script, la modification de la liste des variables au début du script adaptée à votre environnement.

```PowerShell
# Fill in these variables with your values
$ServerList = "Server01", "Server02", "Server03", "Server04"
$FeatureList = "Hyper-V", "Failover-Clustering", "Data-Center-Bridging", "RSAT-Clustering-PowerShell", "Hyper-V-PowerShell", "FS-FileServer"

# This part runs the Install-WindowsFeature cmdlet on all servers in $ServerList, passing the list of features into the scriptblock with the "Using" scope modifier so you don't have to hard-code them here.
Invoke-Command ($ServerList) {
    Install-WindowsFeature -Name $Using:Featurelist
}
```

## <a name="step-2-configure-the-network"></a>Étape 2 : Configurer le réseau

Si vous déployez des espaces de stockage Direct à l’intérieur de machines virtuelles, ignorez cette section.

Espaces de stockage Direct nécessite la bande passante élevée, faible latence réseau entre les serveurs du cluster. Mise en réseau de moins de 10 GbE est requis et l’accès direct à la mémoire à distance (RDMA) est recommandé. Vous pouvez utiliser iWARP ou RoCE tant qu’il a le logo Windows Server 2016, mais iWARP est généralement plus facile à configurer.

> [!Important]
> En fonction de votre équipement réseau et en particulier avec RoCE v2, une configuration du commutateur top-of-rack peut être nécessaire. Configuration de commutateur correct est importante de garantir la fiabilité et les performances des espaces de stockage Direct.

Windows Server 2016 introduit incorporées commutateur association (SET) au sein du commutateur virtuel Hyper-V. Ainsi, les mêmes ports de carte réseau physiques à utiliser pour tout le trafic réseau lors de l’utilisation de RDMA, réduisant le nombre de ports de carte réseau physiques requis. L’association de cartes incorporé de commutateur est recommandée pour espaces de stockage Direct.

Pour obtenir des instructions configurer la mise en réseau pour les espaces de stockage Direct, consultez [Windows Server 2016 convergé carte d’interface réseau et le Guide de déploiement de RDMA invité](https://github.com/Microsoft/SDN/blob/master/Diagnostics/S2D%20WS2016_ConvergedNIC_Configuration.docx).

## <a name="step-3-configure-storage-spaces-direct"></a>Étape 3 : Configurer les espaces de stockage direct

Les étapes suivantes sont effectuées sur un système de gestion qui est de la même version que les serveurs en cours de configuration. Les étapes suivantes ne doivent pas être exécutés à distance à l’aide d’une session PowerShell, mais est exécutés dans une session PowerShell locale sur le système de gestion, avec des autorisations administratives.

### <a name="step-31-clean-drives"></a>Étape 3.1 : Nettoyage des lecteurs

Avant d’activer les espaces de stockage Direct, assurez-vous que vos lecteurs sont vides : aucune partition ancienne ou autres données. Exécutez le script suivant, en remplaçant les noms d’ordinateur, pour supprimer les anciennes partitions ou autres données.

> [!Warning]
> Ce script supprime définitivement toutes les données sur des lecteurs autres que le lecteur de démarrage du système d’exploitation !

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

La sortie se présente comme suit, où **nombre** est le nombre de lecteurs de chaque modèle dans chaque serveur :

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

### <a name="step-32-validate-the-cluster"></a>Étape 3.2 : Valider le cluster

Dans cette étape, vous allez exécuter l’outil de validation de cluster pour vous assurer que les nœuds de serveur sont configurés correctement pour créer un cluster à l’aide d’espaces de stockage Direct. Lorsque la validation du cluster (`Test-Cluster`) est exécutée avant la création du cluster, il exécute les tests qui vérifient que la configuration est appropriée pour fonctionner correctement comme un cluster de basculement. L’exemple ci-dessous utilise la `-Include` paramètre, puis les catégories de tests spécifiques sont définies. Cela garantit que les tests spécifiques des espaces de stockage direct sont inclus dans la validation.

Utilisez la commande PowerShell suivante pour valider un ensemble de serveurs à utiliser comme cluster d’espaces de stockage direct.

```PowerShell
Test-Cluster –Node <MachineName1, MachineName2, MachineName3, MachineName4> –Include "Storage Spaces Direct", "Inventory", "Network", "System Configuration"
```

### <a name="step-33-create-the-cluster"></a>Étape 3.3 : créer le cluster

Dans cette étape, vous allez créer un cluster avec les nœuds que vous avez validés pour la création du cluster à l’étape précédente à l’aide de l’applet de commande PowerShell suivante.

Lors de la création du cluster, vous obtiendrez un avertissement indiquant : « vous rencontrez des problèmes pendant la création du rôle en cluster qui peut-être empêcher son démarrage. Pour de plus amples informations, consultez le fichier de rapport ci-dessous. » Vous pouvez ignorer cet avertissement en toute sécurité. Cela est dû au fait qu’aucun disque n’est disponible pour le quorum du cluster. Il est recommandé de configurer un témoin de partage de fichiers ou un témoin de cloud après la création du cluster.

> [!Note]
> Si les serveurs utilisent des adresses IP statiques, modifiez la commande suivante afin de refléter l’adresse IP statique en ajoutant le paramètre suivant et en spécifiant l’adresse IP : –StaticAddress &lt;X.X.X.X&gt;.
> Dans la commande suivante, l’espace réservé ClusterName doit être remplacé par un nom NetBIOS unique et d’au maximum 15 caractères.
> ```PowerShell
> New-Cluster –Name <ClusterName> –Node <MachineName1,MachineName2,MachineName3,MachineName4> –NoStorage
> ```

Une fois le cluster créé, la réplication de l’entrée DNS pour le nom du cluster peut demander du temps. Cette durée dépend de l’environnement et la configuration de la réplication DNS. Si la résolution du cluster échoue, dans la plupart des cas, vous pouvez réussir en utilisant le nom d’ordinateur d’un nœud est un membre actif du cluster à la place du nom du cluster.

### <a name="step-34-configure-a-cluster-witness"></a>Étape 3.4 : Configurer un témoin de cluster

Nous vous recommandons de configurer un témoin pour le cluster, afin de clusters avec trois serveurs ou plus peuvent résister à deux serveurs défaillant ou hors connexion. Un déploiement à deux serveurs nécessite un témoin de cluster, sinon des serveurs hors connexion entraîne l’autre indisponibilité ainsi. Avec ces systèmes, vous pouvez utiliser un partage de fichiers en tant que témoin, ou utiliser un témoin de cloud. 

Pour plus d’informations, voir les rubriques suivantes :

- [Configurer et gérer le quorum](../../failover-clustering/manage-cluster-quorum.md)
- [Déployer un témoin Cloud pour un Cluster de basculement](../../failover-clustering/deploy-cloud-witness.md)

### <a name="step-35-enable-storage-spaces-direct"></a>Étape 3.5 : Activer les espaces de stockage direct

Après avoir créé le cluster, utilisez le `Enable-ClusterStorageSpacesDirect` applet de commande PowerShell, ce qui permettra de mettre le système de stockage dans le mode d’espaces de stockage Direct et automatiquement les opérations suivantes :

-   **Créer un pool :** Crée un seul pool volumineux qui porte un nom semblable à « S2D sur Cluster1 ».

-   **Configure les caches des espaces de stockage Direct :** Si plus d’un média (lecteur) type disponible pour une utilisation directe des espaces de stockage, il permet la plus rapide en tant que périphériques cache (lire et écrire dans la plupart des cas)

-   **Niveaux :** Crée deux niveaux comme niveaux par défaut. L’un est appelé « Capacité » et l’autre est appelé « Performances ». L’applet de commande analyse les périphériques et configure chaque niveau avec une combinaison de types de périphériques et de résilience.

À partir du système de gestion, dans une fenêtre de commande PowerShell ouverte avec des privilèges d’administrateur, lancez la commande suivante. Le nom de cluster est celui du cluster que vous avez créé aux étapes précédentes. Si cette commande est exécutée localement sur l’un des nœuds, le paramètre -CimSession n’est pas nécessaire.

```PowerShell
Enable-ClusterStorageSpacesDirect –CimSession <ClusterName>
```

Pour activer les espaces de stockage direct à l’aide de la commande ci-dessus, vous pouvez également utiliser le nom du nœud à la place du nom du cluster. L’utilisation du nom du nœud peut être plus fiable en raison de retards de réplication DNS qui peuvent se produire avec le nom du cluster nouvellement créé.

Quand l’exécution de cette commande est terminée, ce qui peut prendre plusieurs minutes, le système est prêt pour la création de volumes.

### <a name="step-36-create-volumes"></a>Étape 3.6 : Créer des volumes

Nous vous recommandons d’utiliser le `New-Volume` applet de commande qu’elle fournit l’expérience la plus simple et rapide. Cet applet de commande unique crée et formate automatiquement le disque virtuel et les partitions, crée le volume avec le nom correspondant et l’ajoute aux volumes partagés de cluster – le tout en une seule étape facile.

Pour plus d’informations, consultez [Création de volumes dans les espaces de stockage direct](create-volumes.md).

### <a name="step-37-optionally-enable-the-csv-cache"></a>Étape 3.7 : Si vous le souhaitez activer le cache de volume partagé de cluster

Vous pouvez éventuellement activer le cache de volume (CSV) partagé de cluster à utiliser la mémoire système (RAM) comme un cache à double écriture au niveau du bloc d’opérations de lecture qui ne sont pas déjà mis en cache par le Gestionnaire de cache de Windows. Cela peut améliorer les performances pour les applications telles que Hyper-V. Le cache de volume partagé de cluster peut améliorer les performances des demandes de lecture et est également utile pour les scénarios de serveur de fichiers avec montée en puissance.

L’activation du cache de volume partagé de cluster permet de réduire la quantité de mémoire disponible pour exécuter des machines virtuelles sur un cluster hyperconvergé, vous devez donc équilibrer les performances de stockage avec la mémoire disponible pour les disques durs virtuels.

Pour définir la taille du cache de volume partagé de cluster, ouvrez une session PowerShell sur le système de gestion avec un compte disposant des autorisations d’administrateur sur le cluster de stockage et ensuite utiliser ce script, en changeant le `$ClusterName` et `$CSVCacheSize` variables comme cela (cela est approprié Il montre un cache de volume partagé de cluster de 2 Go par serveur) :

```PowerShell
$ClusterName = "StorageSpacesDirect1"
$CSVCacheSize = 2048 #Size in MB

Write-Output "Setting the CSV cache..."
(Get-Cluster $ClusterName).BlockCacheSize = $CSVCacheSize

$CSVCurrentCacheSize = (Get-Cluster $ClusterName).BlockCacheSize
Write-Output "$ClusterName CSV cache size: $CSVCurrentCacheSize MB"
```

Pour plus d’informations, consultez [à l’aide de la CSV en mémoire cache de lecture](csv-cache.md).

### <a name="step-38-deploy-virtual-machines-for-hyper-converged-deployments"></a>Étape 3.8 : Déployer des machines virtuelles pour les déploiements hyperconvergés

Si vous déployez un cluster hyperconvergé, la dernière étape consiste à approvisionner des machines virtuelles sur le cluster d’espaces de stockage Direct.

Les fichiers de la machine virtuelle doivent être stockés sur l’espace de noms de volume partagé de cluster de systèmes (exemple : c:\\ClusterStorage\\Volume1) exactement comme les machines virtuelles en cluster sur les clusters de basculement.

Vous pouvez utiliser les outils fournis ou autres outils pour gérer le stockage et les machines virtuelles, tels que System Center Virtual Machine Manager.

## <a name="step-4-deploy-scale-out-file-server-for-converged-solutions"></a>Étape 4 : Déployer le serveur de fichiers avec montée en puissance pour les solutions convergées

Si vous déployez une solution convergence, l’étape suivante consiste à créer une instance de serveur de fichiers avec montée en puissance et de configurer des partages de fichiers. Si vous déployez un cluster hyperconvergé - vous avez terminé et que vous n’avez pas besoin cette section.

### <a name="step-41-create-the-scale-out-file-server-role"></a>Étape 4.1 : Créer le rôle de serveur de fichiers avec montée en puissance

L’étape suivante de la configuration des services de cluster pour votre serveur de fichiers crée le rôle de serveur de fichiers en cluster, c'est-à-dire lorsque vous créez l’instance de serveur de fichiers avec montée en puissance sur lequel se trouvent vos partages de fichiers disponibles en permanence.

#### <a name="to-create-a-scale-out-file-server-role-by-using-server-manager"></a>Pour créer un rôle de serveur de fichiers avec montée en puissance à l’aide du Gestionnaire de serveur

1.  Dans le Gestionnaire de Cluster de basculement, sélectionnez le cluster, accédez à **rôles**, puis cliquez sur **configurer un rôle...** .<br>L’Assistant haute disponibilité s’affiche.
2.  Sur le **sélectionner un rôle** , cliquez sur **serveur de fichiers**.
3.  Sur le **Type de serveur de fichiers** , cliquez sur **Scale-Out File Server pour les données d’application**.
4.  Sur le **Point d’accès Client** , tapez un nom pour le serveur de fichiers de montée en puissance.
5.  Vérifiez que le rôle a été correctement configuré en accédant à **rôles** et confirme que le **état** colonne affiche **en cours d’exécution** en regard du rôle de serveur de fichiers en cluster que vous avez créé, comme indiqué dans la Figure 1.

    ![Capture d’écran du Gestionnaire de Cluster de basculement affiche le serveur de fichiers de montée en puissance](media\Hyper-converged-solution-using-Storage-Spaces-Direct-in-Windows-Server-2016\SOFS_in_FCM.png "Gestionnaire du Cluster de basculement affiche le serveur de fichiers de montée en puissance")

     **Figure 1** Gestionnaire du Cluster de basculement affiche le serveur de fichiers de montée en puissance avec l’état en cours d’exécution

> [!NOTE]
>  Après avoir créé le rôle en cluster, il peut y avoir un réseau des retards de propagation qui peuvent vous empêcher de créer des partages de fichiers dessus pendant quelques minutes, ou potentiellement longue.  
  
#### <a name="to-create-a-scale-out-file-server-role-by-using-windows-powershell"></a>Pour créer un rôle de serveur de fichiers avec montée en puissance à l’aide de Windows PowerShell

 Dans une session Windows PowerShell qui est connectée au cluster de serveur de fichiers, entrez les commandes suivantes pour créer le rôle de serveur de fichiers avec montée en puissance, modification *FSCLUSTER* corresponde au nom de votre cluster, et *SOFS* doit correspondre au nom que vous souhaitez attribuer le rôle de serveur de fichiers avec montée en puissance :

```PowerShell
Add-ClusterScaleOutFileServerRole -Name SOFS -Cluster FSCLUSTER
```

> [!NOTE]
>  Après avoir créé le rôle en cluster, il peut y avoir un réseau des retards de propagation qui peuvent vous empêcher de créer des partages de fichiers dessus pendant quelques minutes, ou potentiellement longue. Si le rôle de serveur SOFS échoue immédiatement et ne démarre pas, il peut être, car l’objet d’ordinateur du cluster n’est pas autorisé à créer un compte d’ordinateur pour le rôle de serveur SOFS. Pour obtenir une aide, consultez ce billet de blog : [Montée en puissance fichier serveur rôle ne parvient pas à démarrer avec l’ID d’événement 1205, 1069 et 1194](http://www.aidanfinn.com/?p=14142).

### <a name="step-42-create-file-shares"></a>Étape 4.2 : Créer des partages de fichiers

Une fois que vous avez créé vos disques virtuels et ajoutés à des volumes partagés de cluster, il est temps de créer des partages de fichiers dessus - un partage de fichiers par volume partagé de cluster par disque virtuel. System Center Virtual Machine Manager (VMM) est probablement le côtés moyen pour ce faire, car il gère les autorisations pour vous, mais si vous ne l’avez dans votre environnement, vous pouvez utiliser Windows PowerShell pour automatiser partiellement le déploiement.

Utiliser les scripts inclus dans le [Configuration du partage SMB pour les charges de travail Hyper-V](http://gallery.technet.microsoft.com/SMB-Share-Configuration-4a36272a) script, lequel partiellement automatise le processus de création de partages et groupes. Il est écrit pour les charges de travail Hyper-V, donc si vous déployez des autres charges de travail, vous devrez peut-être modifier les paramètres ou effectuer des étapes supplémentaires après avoir créé les partages. Par exemple, si vous utilisez Microsoft SQL Server, le compte de service SQL Server doit pouvoir un contrôle total sur le partage et le système de fichiers.

> [!NOTE]
>  Vous devrez mettre à jour l’appartenance au groupe lorsque vous ajoutez des nœuds de cluster, sauf si vous utilisez System Center Virtual Machine Manager pour créer vos partages.

Pour créer des partages de fichiers à l’aide de scripts PowerShell, procédez comme suit :

1. Télécharger les scripts inclus dans [Configuration du partage SMB pour les charges de travail Hyper-V](http://gallery.technet.microsoft.com/SMB-Share-Configuration-4a36272a) à un des nœuds du cluster de serveur de fichiers.
2. Ouvrez une session Windows PowerShell avec les informations d’identification d’administrateur de domaine sur le système de gestion, puis utilisez le script suivant pour créer un groupe Active Directory pour les objets ordinateur Hyper-V, en modifiant les valeurs des variables en fonction de votre environnement :

    ```PowerShell
    # Replace the values of these variables
    $HyperVClusterName = "Compute01"
    $HyperVObjectADGroupSamName = "Hyper-VServerComputerAccounts" <#No spaces#>
    $ScriptFolder = "C:\Scripts\SetupSMBSharesWithHyperV"

    # Start of script itself
    CD $ScriptFolder
    .\ADGroupSetup.ps1 -HyperVObjectADGroupSamName $HyperVObjectADGroupSamName -HyperVClusterName $HyperVClusterName
    ```
3. Ouvrez une session Windows PowerShell avec les informations d’identification d’administrateur sur l’un des nœuds de stockage et ensuite utiliser le script suivant pour créer des partages pour chaque volume partagé de cluster et accorder des autorisations administratives pour les partages au groupe Admins du domaine et le cluster de calcul.

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

### <a name="step-43-enable-kerberos-constrained-delegation"></a>Activer l’authentification Kerberos étape 4.3 la délégation contrainte

Pour configurer la délégation contrainte Kerberos pour gestion de scénario à distance et une sécurité accrue Migration dynamique, à partir d’un des nœuds de cluster de stockage, utilisent le script KCDSetup.ps1 inclus dans [Configuration du partage SMB pour les charges de travail Hyper-V](http://gallery.technet.microsoft.com/SMB-Share-Configuration-4a36272a). Voici un petit wrapper pour le script :

```PowerShell
$HyperVClusterName = "Compute01"
$ScaleOutFSName = "SOFS"
$ScriptFolder = "C:\Scripts\SetupSMBSharesWithHyperV"

CD $ScriptFolder
.\KCDSetup.ps1 -HyperVClusterName $HyperVClusterName -ScaleOutFSName $ScaleOutFSName -EnableLM
```

## <a name="next-steps"></a>Étapes suivantes

Après avoir déployé votre serveur de fichiers en cluster, il est recommandé de tester les performances de votre solution à l’aide de charges de travail synthétiques avant de mettre les charges de travail réelles. Cela vous permet de confirmer que la solution s’exécute correctement et résolvez les problèmes en attente avant d’ajouter la complexité des charges de travail. Pour plus d’informations, consultez [Test stockage espaces performances à l’aide de charges de travail synthétiques](https://technet.microsoft.com/library/dn894707.aspx).

## <a name="see-also"></a>Voir aussi

-   [Espaces de stockage Direct dans Windows Server 2016](storage-spaces-direct-overview.md)
-   [Comprendre le cache dans les espaces de stockage Direct](understand-the-cache.md)
-   [Planification des volumes dans les espaces de stockage Direct](plan-volumes.md)
-   [Tolérance des espaces de stockage](storage-spaces-fault-tolerance.md)
-   [Espaces de stockage Direct configuration matérielle requise](Storage-Spaces-Direct-Hardware-Requirements.md)
-   [To RDMA, or not to RDMA – that is the question](https://blogs.technet.microsoft.com/filecab/2017/03/27/to-rdma-or-not-to-rdma-that-is-the-question/) (blog TechNet)
