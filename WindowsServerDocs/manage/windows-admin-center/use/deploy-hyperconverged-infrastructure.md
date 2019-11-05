---
title: Déployer une infrastructure hypervergé avec le centre d’administration Windows
ms.topic: article
author: cosmosdarwin
ms.author: cosdar
ms.prod: windows-server
ms.technology: manage
ms.date: 11/04/2019
ms.openlocfilehash: 62bf21dd0afcb99aa77cff8a733e80fc4cffe2fb
ms.sourcegitcommit: 1da993bbb7d578a542e224dde07f93adfcd2f489
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73587229"
---
# <a name="deploy-hyperconverged-infrastructure-with-windows-admin-center"></a>Déployer une infrastructure hypervergé avec le centre d’administration Windows

> S’applique à : Centre d’administration Windows, version préliminaire du centre d’administration Windows

Vous pouvez utiliser le centre d’administration Windows [version 1910](https://docs.microsoft.com/windows-server/manage/windows-admin-center/understand/windows-admin-center) ou ultérieure pour déployer une infrastructure hypervergé à l’aide de deux serveurs Windows ou plus appropriés. Cette nouvelle fonctionnalité prend la forme d’un flux de travail à plusieurs étapes qui vous guide tout au long de l’installation des fonctionnalités, de la configuration de la mise en réseau, de la création du cluster et du déploiement de espaces de stockage direct et/ou de la mise en réseau définie par logiciel (SDN), si elle est sélectionnée.

![Capture d’écran du flux de travail de création de cluster](../media/deploy-hyperconverged-infrastructure/deployment-workflow-screenshot.png)

  > [!Important]
  > Cette fonctionnalité est en cours de développement actif. Elle est disponible en version préliminaire, ce qui vous permet de l’essayer tôt et de partager vos commentaires.

## <a name="preview-the-workflow"></a>Afficher un aperçu du flux de travail

### <a name="1-prerequisites"></a>1. conditions préalables

Le flux de travail de création de cluster dans le centre d’administration Windows n’effectue pas d’installation complète du système d’exploitation. par conséquent, vous devez d’abord installer Windows Server sur chaque serveur. Les versions prises en charge sont Windows Server 2016, Windows Server 2019 et Windows Server Insider preview. Vous devez également joindre chaque serveur au même domaine de Active Directory que le centre d’administration Windows en cours d’exécution avant de démarrer le flux de travail.

### <a name="2-install-windows-admin-center"></a>2. installer le centre d’administration Windows
 
Suivez les instructions pour [Télécharger et installer](https://docs.microsoft.com/windows-server/manage/windows-admin-center/understand/windows-admin-center) la dernière version du centre d’administration Windows.

### <a name="3-install-the-cluster-creation-extension"></a>3. installer l’extension de création de cluster

Le flux de travail de création de cluster est remis et mis à jour via le flux d’extension du centre d’administration Windows. C’est pourquoi nous pouvons traiter vos commentaires et apporter des améliorations plus rapidement au cours de la phase préliminaire.

Pour installer l’extension, lancez le centre d’administration Windows, puis :

1.  Dans le coin supérieur droit, sélectionnez l’icône Paramètres.
2.  Accéder aux extensions
3.  Rechercher l’extension de création de cluster
4.  Sélectionnez Installer

![Captures d’écran des quatre étapes pour installer l’extension de création de cluster](../media/deploy-hyperconverged-infrastructure/how-to-install.png)

Une fois installé, sélectionnez l’extension de création de cluster dans la liste déroulante en haut à gauche :

![Capture d’écran du lancement de l’extension de création de cluster](../media/deploy-hyperconverged-infrastructure/how-to-launch.png)

## <a name="limitations-of-the-preview-release"></a>Limitations de la version préliminaire

Le flux de travail de création de cluster est en cours de développement actif. en d’autres termes, il n’est pas terminé.

Voici quelques-unes des limitations de la version préliminaire actuelle :

1.  **Configuration requise pour le domaine.** Le workflow requiert actuellement que chaque serveur que vous ajoutez soit déjà joint au même domaine de Active Directory que le centre d’administration Windows en cours d’exécution.
2.  **Afficher le script.** Avec la préversion actuelle, vous ne pouvez pas voir les scripts exécutés par le flux de travail à l’aide de la fonctionnalité afficher le script dans le centre d’administration Windows, ni télécharger un rapport complet de tout ce qui s’est produit à la fin. Nous savons que cela est important pour de nombreux utilisateurs, restez en contact. En attendant, utilisez les applets de commande fournies ci-dessous pour [voir ce qui se passe](#see-whats-happening).
3.  **Reprendre ou recommencer.** Bien que vous puissiez quitter la version préliminaire du flux de travail, la préversion actuelle ne prend pas en charge la reprise à l’endroit où vous l’avez quittée et que vous ne pouvez pas le nettoyer complètement avant de commencer. Si vous souhaitez effectuer un déploiement à plusieurs reprises sur les mêmes serveurs à des fins de test, utilisez les applets de commande fournies ci-dessous pour [Annuler et recommencer](#undo-and-start-over).
4.  **Accès direct à la mémoire à distance (RDMA).** La version préliminaire actuelle ne peut pas activer la mise en réseau RDMA, qui est couramment utilisée avec l’infrastructure hypervergé. Pour le moment, terminez le flux de travail sans activer RDMA, puis utilisez d’autres outils pour activer RDMA de la même façon que vous le feriez.

Au-delà de ces limitations, il existe des fonctionnalités potentielles innombrables qui peuvent être ajoutées dans les versions ultérieures : application des mises à jour Windows, configuration d’un témoin pour le quorum de cluster, importation de machines virtuelles, etc. Pour nous aider à hiérarchiser les fonctionnalités que vous souhaitez, veuillez [partager vos commentaires](#feedback) comme décrit ci-dessous.

## <a name="see-whats-happening"></a>Voir ce qui se passe

Utilisez ces applets de commande Windows PowerShell pour suivre et voir ce que fait le Workflow.

Pour connaître les fonctionnalités Windows installées, utilisez l’applet de commande `Get-WindowsFeature`. Exemple :

```PowerShell
Get-WindowsFeature "Hyper-V", "Failover-Clustering", "Data-Center-Bridging", "BitLocker"
```

![Capture d’écran de la sortie PowerShell](../media/deploy-hyperconverged-infrastructure/script-out-1.png)

  > [!Note]
  > Les fonctionnalités installées dépendent du type de cluster que vous sélectionnez.

Pour afficher les cartes réseau et leurs propriétés, telles que le nom, les adresses IPv4 et l’ID de réseau local virtuel :

```PowerShell
Get-NetAdapter | Where Status -Eq "Up" | Sort InterfaceAlias | Format-Table Name, InterfaceDescription, Status, LinkSpeed, VLANID, MacAddress
Get-NetAdapter | Where Status -Eq "Up" | Get-NetIPAddress -AddressFamily IPv4 -ErrorAction SilentlyContinue | Sort InterfaceAlias | Format-Table InterfaceAlias, IPAddress, PrefixLength
```

![Capture d’écran de la sortie PowerShell](../media/deploy-hyperconverged-infrastructure/script-out-2.png)

Pour afficher les commutateurs virtuels Hyper-V et le mode d’association des cartes réseau physiques :

```PowerShell
Get-VMSwitch
Get-VMSwitchTeam
```

![Capture d’écran de la sortie PowerShell](../media/deploy-hyperconverged-infrastructure/script-out-3.png)

Pour afficher les cartes réseau virtuelles hôtes, le cas échéant, exécutez :

```PowerShell
Get-VMNetworkAdapter -ManagementOS | Format-Table Name, IsManagementOS, SwitchName
Get-VMNetworkAdapterTeamMapping -ManagementOS | Format-Table NetAdapterName, ParentAdapter
```

![Capture d’écran de la sortie PowerShell](../media/deploy-hyperconverged-infrastructure/script-out-4.png)

Pour afficher le cluster de basculement actuel et ses nœuds membres, exécutez :

```PowerShell
Get-Cluster
Get-ClusterNode
```

![Capture d’écran de la sortie PowerShell](../media/deploy-hyperconverged-infrastructure/script-out-5.png)

Pour déterminer si espaces de stockage direct est activé et le pool de stockage par défaut créé automatiquement :

```PowerShell
Get-ClusterStorageSpacesDirect
Get-StoragePool -IsPrimordial $False
```

![Capture d’écran de la sortie PowerShell](../media/deploy-hyperconverged-infrastructure/script-out-6.png)

## <a name="undo-and-start-over"></a>Annuler et recommencer

Utilisez ces applets de commande Windows PowerShell pour annuler les modifications apportées par le flux de travail et recommencer.

### <a name="remove-virtual-machines-or-other-clustered-resources"></a>Supprimer des machines virtuelles ou d’autres ressources en cluster

Si vous avez créé des machines virtuelles ou d’autres ressources en cluster, telles que les contrôleurs de réseau pour la mise en réseau définie par logiciel, supprimez-les en premier.

Par exemple, pour supprimer des ressources par nom, utilisez :

```PowerShell
Get-ClusterResource -Name "<NAME>" | Remove-ClusterResource
```

### <a name="undo-the-storage-steps"></a>Annuler les étapes de stockage

Si vous avez activé espaces de stockage direct, désactivez-le avec le script suivant :

> [!Warning]
> Ces applets de commande suppriment définitivement toutes les données dans espaces de stockage direct volumes. Cette opération ne peut pas être annulée.

```PowerShell
Get-VirtualDisk | Remove-VirtualDisk
Get-StoragePool -IsPrimordial $False | Remove-StoragePool
Disable-ClusterS2D
```

### <a name="undo-the-clustering-steps"></a>Annuler les étapes de clustering

Si vous avez créé un cluster, supprimez-le avec cette applet de commande :

```PowerShell
Remove-Cluster -CleanUpAD
```

Pour supprimer également les rapports de validation de cluster, exécutez cette applet de commande sur chaque serveur qui faisait partie du cluster :

```PowerShell
Get-ChildItem C:\Windows\cluster\Reports\ | Remove-Item
```

### <a name="undo-the-networking-steps"></a>Annuler les étapes de mise en réseau

Exécutez ces applets de commande sur chaque serveur qui faisait partie du cluster.

Si vous avez créé un commutateur virtuel Hyper-V :

```PowerShell
Get-VMSwitch | Remove-VMSwitch
```

> [!Note]
> L’applet de commande `Remove-VMSwitch` supprime automatiquement tous les adaptateurs virtuels et annule l’Association d’une association de cartes physiques à Switch-Embedded.

Si vous avez modifié les propriétés de la carte réseau, telles que le nom, l’adresse IPv4 et l’ID de réseau local virtuel :

> [!Warning]
> Ces applets de commande suppriment les noms de cartes réseau et les adresses IP. Assurez-vous d’avoir les informations dont vous avez besoin pour vous connecter par la suite, par exemple un adaptateur de gestion exclu du script ci-dessous. Assurez-vous également que vous savez comment les serveurs sont connectés en termes de propriétés physiques telles que l’adresse MAC, et pas seulement le nom de la carte dans Windows.

```PowerShell
Get-NetAdapter | Where Name -Ne "Management" | Rename-NetAdapter -NewName $(Get-Random)
Get-NetAdapter | Where Name -Ne "Management" | Get-NetIPAddress -ErrorAction SilentlyContinue | Where AddressFamily -Eq IPv4 | Remove-NetIPAddress
Get-NetAdapter | Where Name -Ne "Management" | Set-NetAdapter -VlanID 0
```

Vous êtes maintenant prêt à démarrer le flux de travail.

## <a name="feedback"></a>Retour d’expérience

Cette version préliminaire concerne vos commentaires. Voici quelques méthodes permettant d’atteindre notre équipe :

- [Envoyer et voter pour les demandes de fonctionnalités sur UserVoice](https://windowsserver.uservoice.com/forums/295071/category/319162?query=%5Bhci%5D)
- [Rejoignez le Forum du centre d’administration Windows sur la communauté Microsoft Tech](https://techcommunity.microsoft.com/t5/Windows-Server-Management/bd-p/WindowsServerManagement)
- Messagerie HCI-déploiement [at] microsoft.com
- Tweet à [@servermgmt](https://twitter.com/servermgmt)

## <a name="report-an-issue"></a>Signaler un problème

Utilisez les canaux listés ci-dessus pour signaler un problème au niveau du flux de travail de création du cluster.

Si possible, incluez les informations suivantes pour nous aider à reproduire et résoudre rapidement votre problème :

- Type de cluster que vous avez sélectionné (exemple : *« hyperconvergentd »* )
- Étape où vous avez rencontré le problème (exemple : *« 3,2 Create cluster »* )
- Version de l’extension de création du cluster. Accédez à **paramètres** > **Extensions** > **extensions installées** et consultez la colonne **version** (exemple : *« 1.0.30 »* ).
- Des messages d’erreur s’affichent à l’écran ou dans la console du navigateur, que vous pouvez ouvrir en appuyant sur **F12**.
- Toutes autres informations pertinentes sur votre environnement 

## <a name="see-also"></a>Articles associés

- [Bonjour, Centre d’administration Windows](https://docs.microsoft.com/windows-server/manage/windows-admin-center/understand/windows-admin-center)
- [Déployer des espaces de stockage direct](https://docs.microsoft.com/windows-server/storage/storage-spaces/deploy-storage-spaces-direct)
