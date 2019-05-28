---
title: Vue d’ensemble des espaces de noms DFS
ms.prod: windows-server-threshold
ms.author: jgerend
ms.manager: daveba
ms.technology: storage
ms.topic: article
author: jasongerend
ms.date: 05/09/2019
description: Cette rubrique décrit la fonctionnalité Espaces de noms DFS qui est un service de rôle de Windows Server. Elle permet de grouper des dossiers partagés qui se trouvent sur des serveurs différents en un ou plusieurs espaces de noms logiquement structurés.
ms.openlocfilehash: 33b5a36c9f13179286a95735621272f79983231c
ms.sourcegitcommit: 75f257d97d345da388cda972ccce0eb29e82d3bc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65613186"
---
# <a name="dfs-namespaces-overview"></a>Vue d’ensemble des espaces de noms DFS

> S’applique à : Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008, Windows Server (canal semi-annuel)

La fonctionnalité Espaces de noms DFS est un service de rôle de Windows Server. Elle permet de grouper des dossiers partagés qui se trouvent sur des serveurs différents en un ou plusieurs espaces de noms logiquement structurés. Cela permet de donner aux utilisateurs une représentation virtuelle des dossiers partagés, dans laquelle un seul chemin d’accès conduit à des fichiers situés sur plusieurs serveurs, comme illustré dans la figure suivante :

![Éléments de technologie d'espaces de noms DFS](media/dfs-overview.png)

Voici une description des éléments qui constituent un espace de noms DFS :

- **Serveur d’espaces de noms** : un serveur d’espace de noms héberge un espace de noms. Le serveur d’espace de noms peut être un serveur membre ou contrôleur de domaine.
- **Racine de l'espace de noms** : la racine de l'espace de noms est le point de départ de l’espace de noms. Dans la figure précédente, le nom de la racine est Public, et le chemin d’accès de l’espace de noms \\ \\Contoso\\publique. Ce type d’espace de noms est un espace de noms de domaine, car elle commence par un nom de domaine (par exemple, Contoso) et ses métadonnées sont stockées dans les Services de domaine Active Directory (AD DS). Même si un serveur d’espace de noms unique est indiqué dans la figure précédente, un espace de noms basé sur un domaine peut être hébergé sur plusieurs serveurs d’espace de noms pour accroître la disponibilité de l’espace de noms.
- **Dossier** : les dossiers sans cibles de dossier ajoutent une structure et une hiérarchie à l’espace de noms et les dossiers avec cibles de dossier donnent un contenu réel aux utilisateurs. Lorsque les utilisateurs accèdent à un dossier dont l’espace de noms est doté de cibles, l'ordinateur client reçoit une référence qui redirige l’ordinateur client de façon transparente vers une des cibles de dossier.
- **Cibles de dossier** : une cible de dossier représente un chemin d’accès UNC (Universal Naming Convention) d’un dossier partagé ou d’un autre espace de noms associé à un dossier dans un espace de noms. La cible de dossier se trouve à l'endroit où sont stockés les données et le contenu. Dans la figure précédente, le dossier nommé Tools a deux cibles de dossiers, une à Londres et une à New York, et le dossier nommé Training Guides a une seule cible de dossier à New York. Un utilisateur qui accède à \\ \\Contoso\\Public\\logiciel\\outils est redirigée de façon transparente vers le dossier partagé \\ \\LDN-SVR-01\\outils ou \\ \\NYC-SVR-01\\outils, en fonction du site auquel l’utilisateur se trouve actuellement dans.

Elle explique comment installer le système de fichiers DFS, quelles sont les nouveautés et où trouver des informations d’évaluation et de déploiement.

Vous pouvez administrer les espaces de noms à l’aide de la gestion DFS, des [applets de commande des espaces de noms DFS (DFSN) dans Windows PowerShell](https://docs.microsoft.com/powershell/module/dfsn/?view=win10-ps), de la commande **DfsUtil** ou des scripts qui appellent WMI.

## <a name="server-requirements-and-limits"></a>Configuration requise et limites du serveur

Aucune configuration matérielle ou logicielle supplémentaire n’est requise pour l’exécution de la gestion DFS ou des espaces de noms DFS.

Un serveur d’espace de noms est un contrôleur de domaine ou un serveur membre qui héberge un espace de noms. Le nombre d’espaces de noms que vous pouvez héberger sur un serveur est déterminé par le système d’exploitation exécuté sur le serveur d’espace de noms.

Les serveurs qui exécutent les systèmes d’exploitation suivants peuvent héberger plusieurs espaces de noms basés sur un domaine en plus d’un espace de noms autonome. 

- Windows Server 2019
- Windows Server 2016
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2 Datacenter et Enterprise Edition
- Windows Server (canal semi-annuel)

Les serveurs qui exécutent les systèmes d’exploitation suivants peuvent héberger un espace de noms autonome :

- Windows Server 2008 R2 Standard

Le tableau suivant décrit les autres facteurs à prendre en compte lors du choix des serveurs pour héberger un espace de noms.

|Serveur hébergeant des espaces de noms autonomes|Serveur hébergeant des espaces de noms basés sur un domaine|
|---|---|
|Doit contenir un volume NTFS pour héberger l’espace de noms.|Doit contenir un volume NTFS pour héberger l’espace de noms.|
|Peut être un serveur membre ou un contrôleur de domaine.|Doit être un serveur membre ou un contrôleur de domaine dans le domaine où l’espace de noms est configuré. (Cette condition s’applique à chaque serveur d’espace de noms qui héberge un espace de noms basé sur un domaine donné.)|
|Peut être hébergé par un cluster de basculement pour accroître la disponibilité de l’espace de noms.|L’espace de noms ne peut pas être une ressource en cluster dans un cluster de basculement. Toutefois, vous pouvez trouver l’espace de noms sur un serveur qui fonctionne également comme un nœud dans un cluster de basculement si vous configurez l’espace de noms pour utiliser uniquement les ressources locales sur le serveur.|

## <a name="installing-dfs-namespaces"></a>Installation des espaces de noms DFS

Les espaces de noms DFS et la réplication DFS sont intégrés au rôle Services de fichiers et de stockage. Les outils de gestion consacrés au système de fichiers DFS (Gestion DFS, module Espaces de noms DFS pour Windows PowerShell et les outils en ligne de commande) sont installés séparément dans le cadre des outils d’administration de serveur distant.

Installer les espaces de noms DFS à l’aide de [Windows Admin Center](../../manage/windows-admin-center/understand/windows-admin-center.md), Gestionnaire de serveur ou de PowerShell, comme décrit dans les sections suivantes.

### <a name="to-install-dfs-by-using-server-manager"></a>Pour installer DFS à l’aide du Gestionnaire de serveur

1. Ouvrez le Gestionnaire de serveur, cliquez sur **Gérer**, puis sur **Ajouter des rôles et des fonctionnalités**. L’Assistant Ajout de rôles et de fonctionnalités apparaît.

2. Dans la page **Sélection du serveur** , choisissez le serveur ou le disque dur virtuel (VHD) d’ordinateur virtuel hors connexion sur lequel vous cherchez à installer le système de fichiers DFS.

3. Sélectionnez les services de rôle et les fonctionnalités que vous souhaitez installer.

    - Pour installer le service d’espaces de noms DFS, dans la page **Rôles du serveur**, sélectionnez **Espaces de noms DFS**.

    - Pour installer uniquement les outils de gestion DFS, dans la page **Fonctionnalités**, développez successivement **Outils d’administration de serveur distant**, **Outils d’administration de rôles** et **Outils de services de fichiers**, puis sélectionnez **Outils de gestion DFS**.

         La fonctionnalité**Outils de gestion DFS** installe le composant logiciel enfichable Gestion DFS, le module Espaces de noms DFS pour Windows PowerShell et les outils en ligne de commande mais elle n’installe pas de services DFS sur le serveur.

### <a name="to-install-dfs-by-using-windows-powershell"></a>Pour installer DFS à l’aide de Windows PowerShell

Ouvrez une session Windows PowerShell avec des droits utilisateur élevés et tapez la commande suivante, où < nom\> est le service de rôle ou une fonctionnalité que vous souhaitez installer (voir le tableau suivant pour obtenir la liste des noms de service ou une fonctionnalité de rôle pertinentes) :

```PowerShell
Install-WindowsFeature <name>
```

|Service de rôle ou fonctionnalité|Nom|
|---|---|
|Espaces de noms DFS|`FS-DFS-Namespace`|
|Outils de gestion DFS|`RSAT-DFS-Mgmt-Con`|

Par exemple, pour installer l’option Outils du système de fichiers DFS de la fonctionnalité Outils d’administration de serveur distant, tapez la commande suivante :

```PowerShell
Install-WindowsFeature "RSAT-DFS-Mgmt-Con"
```

Pour installer les espaces de noms DFS et l’option Outils du système de fichiers DFS de la fonctionnalité Outils d’administration de serveur distant, tapez la commande suivante :

```PowerShell
Install-WindowsFeature "FS-DFS-Namespace", "RSAT-DFS-Mgmt-Con"
```

## <a name="interoperability-with-azure-virtual-machines"></a>Interopérabilité avec les machines virtuelles Azure

L’utilisation des espaces de noms DFS sur une machine virtuelle dans Microsoft Azure a été testée. Toutefois, vous devez respecter certaines limites et certaines conditions.

- Vous ne pouvez pas mettre en cluster des espaces de noms autonomes dans des machines virtuelles Azure.

- Vous pouvez héberger des espaces de noms basés sur le domaine dans des machines virtuelles Azure, y compris les environnements avec Azure Active Directory.

Pour en savoir plus sur la prise en main des machines virtuelles Azure, voir [Documentation sur les machines virtuelles Azure](https://docs.microsoft.com/azure/virtual-machines/).

## <a name="see-also"></a>Voir aussi

Pour plus d’informations connexes, voir les ressources suivantes.

|Type de contenu|Références|
|------------------|----------------|
|**Évaluation du produit**|[Quelles sont les nouveautés dans les espaces de noms DFS et réplication DFS dans Windows Server](https://technet.microsoft.com/library/dn281957(v=ws.11).aspx)|
|**Déploiement**|[Considérations d’extensibilité Namespace DFS](http://blogs.technet.com/b/filecab/archive/2012/08/26/dfs-namespace-scalability-considerations.aspx)|
|**Opérations**|[Espaces de noms DFS : Forum Aux Questions](https://technet.microsoft.com/library/ee404780.aspx)|
|**Ressources de la communauté**|[Les Services de fichiers et le forum TechNet sur le stockage](https://social.technet.microsoft.com/forums/winserverfiles/threads/)|
|**Protocoles**|[Protocoles dans Windows Server des Services de fichiers](https://msdn.microsoft.com/en-us/library/cc239318.aspx) (déconseillée)|
|**Technologies connexes**| [Clustering de basculement](../../failover-clustering/failover-clustering-overview.md)|
|**Support technique**|[Windows IT Pro prise en charge](https://www.microsoft.com/itpro/windows/support)|
