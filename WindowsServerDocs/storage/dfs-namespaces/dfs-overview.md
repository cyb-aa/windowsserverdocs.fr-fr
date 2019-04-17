---
title: "Vue d’ensemble des espaces de nomsDFS"
ms.prod: windows-server-threshold
ms.author: jgerend
ms.manager: brianlic
ms.technology: storage
ms.topic: article
author: jasongerend
ms.date: 07/10/2017
description: "Cette rubrique décrit la fonctionnalité Espaces de nomsDFS qui est un service de rôle de WindowsServer. Elle permet de grouper des dossiers partagés qui se trouvent sur des serveurs différents en un ou plusieurs espaces de noms logiquement structurés."
ms.openlocfilehash: f3a76208fa1d6e1207edd699f830fc05a360bee5
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="dfs-namespaces-overview"></a>Vue d’ensemble des espaces de nomsDFS

> S’applique à: WindowsServer (canal semi-annuel), WindowsServer2016, WindowsServer2012R2, WindowsServer2012, WindowsServer2008R2, WindowsServer2008

La fonctionnalité Espaces de nomsDFS est un service de rôle de Windows Server. Elle permet de grouper des dossiers partagés qui se trouvent sur des serveurs différents en un ou plusieurs espaces de noms logiquement structurés. Cela permet de donner aux utilisateurs une représentation virtuelle des dossiers partagés, dans laquelle un seul chemin d’accès conduit à des fichiers situés sur plusieurs serveurs, comme illustré dans la figure suivante:

![Éléments de technologie d'espaces de noms DFS](media/dfs-overview.png)

Voici une description des éléments qui constituent un espace de noms DFS:

- **Serveur d’espaces de noms**: un serveur d’espace de noms héberge un espace de noms. Le serveur d’espace de noms peut être un serveur membre ou contrôleur de domaine.
- **Racine de l'espace de noms**: la racine de l'espace de noms est le point de départ de l’espace de noms. Dans la figure précédente, le nom de la racine est Public, et le chemin d’accès à l’espace de noms est \\\\\Contoso\\Public. Ce type d’espace de noms est un espace de noms basé sur un domaine, car il commence par un nom de domaine (par exemple, Contoso) et ses métadonnées sont stockées dans les Services de domaine ActiveDirectory (ADDS). Même si un serveur d’espace de noms unique est indiqué dans la figure précédente, un espace de noms basé sur un domaine peut être hébergé sur plusieurs serveurs d’espace de noms pour accroître la disponibilité de l’espace de noms.
- **Dossier**: les dossiers sans cibles de dossier ajoutent une structure et une hiérarchie à l’espace de noms et les dossiers avec cibles de dossier donnent un contenu réel aux utilisateurs. Lorsque les utilisateurs accèdent à un dossier dont l’espace de noms est doté de cibles, l'ordinateur client reçoit une référence qui redirige l’ordinateur client de façon transparente vers une des cibles de dossier.
- **Cibles de dossier**: une cible de dossier représente un chemin d’accès UNC (Universal Naming Convention) d’un dossier partagé ou d’un autre espace de noms associé à un dossier dans un espace de noms. La cible de dossier se trouve à l'endroit où sont stockés les données et le contenu. Dans la figure précédente, le dossier nommé Tools a deux cibles de dossiers, une à Londres et une à New York, et le dossier nommé Training Guides a une seule cible de dossier à New York. Un utilisateur qui accède à \\\\Contoso\\Public\\Software\\Tools est redirigé de façon transparente vers le dossier partagé \\\\LDN-SVR-01\\Tools ou \\\\NYC-SVR-01\\Tools, selon le site sur lequel l’utilisateur se trouve actuellement.

Cette rubrique explique comment installer le système de fichiersDFS, quelles sont les nouveautés et où trouver des informations d’évaluation et de déploiement.

Vous pouvez administrer les espaces de noms à l’aide de la gestion DFS, des [applets de commande des espaces de nomsDFS (DFSN) dans Windows PowerShell](https://technet.microsoft.com/library/jj884270.aspx), de la commande **DfsUtil** ou des scripts qui appellent WMI.

## <a name="server-requirements-and-limits"></a>Configuration requise et limites du serveur

Aucune configuration matérielle ou logicielle supplémentaire n’est requise pour l’exécution de la gestion DFS ou des espaces de noms DFS.

Un serveur d’espace de noms est un contrôleur de domaine ou un serveur membre qui héberge un espace de noms. Le nombre d’espaces de noms que vous pouvez héberger sur un serveur est déterminé par le système d’exploitation exécuté sur le serveur d’espace de noms.

Les serveurs qui exécutent les systèmes d’exploitation suivants peuvent héberger plusieurs espaces de noms basés sur un domaine en plus d’un espace de noms autonome. 

- WindowsServer (canal semi-annuel)
- WindowsServer2016 
- WindowsServer2012R2
- WindowsServer2012
- Windows Server2008R2 Datacenter/Entreprise

Les serveurs qui exécutent les systèmes d’exploitation suivants peuvent héberger un espace de noms autonome:

- WindowsServer2008R2 Standard


Le tableau suivant décrit les autres facteurs à prendre en compte lors du choix des serveurs pour héberger un espace de noms.

|Serveur hébergeant des espaces de noms autonomes|Serveur hébergeant des espaces de noms basés sur un domaine|
|---|---|
|Doit contenir un volume NTFS pour héberger l’espace de noms.|Doit contenir un volume NTFS pour héberger l’espace de noms.|
|Peut être un serveur membre ou un contrôleur de domaine.|Doit être un serveur membre ou un contrôleur de domaine dans le domaine où l’espace de noms est configuré. (Cette condition s’applique à chaque serveur d’espace de noms qui héberge un espace de noms basé sur un domaine donné.)|
|Peut être hébergé par un cluster de basculement pour accroître la disponibilité de l’espace de noms.|L’espace de noms ne peut pas être une ressource en cluster dans un cluster de basculement. Toutefois, vous pouvez trouver l’espace de noms sur un serveur qui fonctionne également comme un nœud dans un cluster de basculement si vous configurez l’espace de noms pour utiliser uniquement les ressources locales sur le serveur.|

## <a name="installing-dfs-namespaces"></a>Installation des espaces de noms DFS

Les espaces de nomsDFS et la réplicationDFS sont intégrés au rôle Services de fichiers et de stockage. Les outils de gestion consacrés au système de fichiers DFS (Gestion DFS, module Espaces de noms DFS pour Windows PowerShell et les outils en ligne de commande) sont installés séparément dans le cadre des outils d’administration de serveur distant.

Installez des espaces de noms DFS en utilisant le Gestionnaire de serveur ou PowerShell, comme décrit dans les sections suivantes.

### <a name="to-install-dfs-by-using-server-manager"></a>Pour installerDFS à l’aide du Gestionnaire de serveur

1. Ouvrez le Gestionnaire de serveur, cliquez sur **Gérer**, puis sur **Ajouter des rôles et des fonctionnalités**. L’Assistant Ajout de rôles et de fonctionnalités apparaît.

2. Dans la page **Sélection du serveur**, choisissez le serveur ou le disque dur virtuel (VHD) d'un ordinateur virtuel hors connexion sur lequel vous voulez installer le système de fichiersDFS.

3. Sélectionnez les services de rôle et les fonctionnalités que vous souhaitez installer.

    - Pour installer le service d’espaces de noms DFS, dans la page **Rôles du serveur**, sélectionnez **Espaces de noms DFS**.

    - Pour installer uniquement les outils de gestionDFS, dans la page **Fonctionnalités**, développez successivement **Outils d’administration de serveur distant**, **Outils d’administration de rôles** et **Outils de services de fichiers**, puis sélectionnez **Outils de gestionDFS**.

         La fonctionnalité**Outils de gestion DFS** installe le composant logiciel enfichable Gestion DFS, le module Espaces de noms DFS pour Windows PowerShell et les outils en ligne de commande, mais elle n’installe pas de services DFS sur le serveur.

### <a name="to-install-dfs-by-using-windows-powershell"></a>Pour installerDFS à l’aide de WindowsPowerShell

Ouvrez une session WindowsPowerShell avec des droits d’utilisateur élevés, puis tapez la commande suivante où <name\> désigne le service de rôle ou la fonctionnalité à installer (reportez-vous au tableau qui suit pour obtenir une liste des noms de services de rôle ou de fonctionnalités appropriés):

```PowerShell
Install-WindowsFeature <name>
```

|Service de rôle ou fonctionnalité|Nom|
|---|---|
|Espaces de noms DFS|`FS-DFS-Namespace`|
|Outils de gestion DFS|`RSAT-DFS-Mgmt-Con`|

Par exemple, pour installer l’option Outils du système de fichiers DFS de la fonctionnalité Outils d’administration de serveur distant, tapez la commande suivante:

```PowerShell
Install-WindowsFeature RSAT-DFS-Mgmt-Con
```

Pour installer les espaces de noms DFS et l’option Outils du système de fichiers DFS de la fonctionnalité Outils d’administration de serveur distant, tapez la commande suivante:

```PowerShell
Install-WindowsFeature FS-DFS-Namespace, RSAT-DFS-Mgmt-Con
```

## <a name="interoperability-with-azure-virtual-machines"></a>Interopérabilité avec les machines virtuelles Azure

L’utilisation des espaces de nomsDFS sur une machine virtuelle dans Microsoft Azure a été testée. Toutefois, vous devez respecter certaines limites et certaines conditions.

- Vous ne pouvez pas mettre en cluster des espaces de noms autonomes dans des machines virtuelles Azure.

- Vous pouvez héberger des espaces de noms basés sur un domaine dans des machines virtuelles Azure, y compris des environnements avec Azure Active Directory, bien qu’un espace de noms unique ne puisse pas inclure à la fois des serveurs d’espace de noms locaux et des serveurs d’espace de noms hébergés dans des machines virtuelles Azure, et ce, même si vous utilisez les services de fédération Active Directory (AD FS).

Pour en savoir plus sur la prise en main des machines virtuelles Azure, voir [Documentation sur les machines virtuelles Azure](https://docs.microsoft.com/azure/virtual-machines/).

## <a name="see-also"></a>Articles associés

Pour plus d’informations connexes, voir les ressources suivantes.

|Type de contenu|Références|
|------------------|----------------|
|**Évaluation du produit**|[Nouveautés des espaces de noms DFS et de la réplication DFS dans Windows Server](https://technet.microsoft.com/library/dn281957(v=ws.11).aspx)|
|**Déploiement**|[Considérations d’extensibilité des espaces de noms DFS](http://blogs.technet.com/b/filecab/archive/2012/08/26/dfs-namespace-scalability-considerations.aspx)|
|**Opérations**|[Espaces de noms DFS: Forum Aux Questions](http://technet.microsoft.com/library/ee404780.aspx)|
|**Ressources de la communauté**|[Forum TechNet sur le stockage et les services de fichiers](http://social.technet.microsoft.com/forums/winserverfiles/threads/)|
|**Protocoles**|[Protocoles Windows Server de services de fichiers](http://msdn.microsoft.com/library/cc239875.aspx)|
|**Technologies associées**| [Clustering de basculement](../../failover-clustering/failover-clustering-overview.md)|