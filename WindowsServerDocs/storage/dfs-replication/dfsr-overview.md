---
title: Présentation de réplication DFS
ms.date: 03/08/2019
ms.prod: windows-server
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 22f9e25763217cbbfdfd8a4ab099344f23138344
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75949715"
---
# <a name="dfs-replication-overview"></a>Présentation de réplication DFS

> S’applique à : Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008, Windows Server (canal semi-annuel)

Réplication DFS est un service de rôle de Windows Server qui vous permet de répliquer efficacement des dossiers (y compris ceux référencés par un chemin d’accès d’espace de noms DFS) sur plusieurs serveurs et sites. La réplication DFS correspond à un moteur de réplication multimaître efficace qui vous permet d'assurer la synchronisation des dossiers entre des serveurs par le biais de connexions réseau dont la bande passante est limitée. Il remplace le service de réplication de fichiers (FRS) comme moteur de réplication pour les espaces de noms DFS, ainsi que pour la réplication du dossier SYSVOL Active Directory Domain Services (AD DS) dans des domaines qui utilisent le niveau fonctionnel de domaine Windows Server 2008 ou version ultérieure.

La réplication DFS utilise un algorithme de compression appelé compression différentielle à distance (RDC). L’algorithme RDC détecte les modifications des données d’un fichier et permet à la réplication DFS de répliquer uniquement les blocs de fichier modifiés à la place du fichier entier.

Pour plus d’informations sur la réplication de SYSVOL à l’aide de réplication DFS, consultez [migrer la réplication SYSVOL vers réplication DFS](migrate-sysvol-to-dfsr.md).

Pour utiliser réplication DFS, vous devez créer des groupes de réplication et ajouter des dossiers répliqués aux groupes. Les groupes de réplication, les dossiers répliqués et les membres sont illustrés dans la figure suivante.

![Groupe de réplication contenant une connexion entre deux membres, chacun d’entre eux ayant deux dossiers répliqués](media/dfsr-overview.gif)

Cette illustration montre qu’un groupe de réplication est un ensemble de serveurs, appelés membres, qui participent à la réplication d’un ou plusieurs dossiers répliqués. Un dossier répliqué est un dossier qui reste synchronisé sur chaque membre. Dans la figure, il existe deux dossiers répliqués : projets et propositions. À mesure que les données changent dans chaque dossier répliqué, les modifications sont répliquées sur les connexions entre les membres du groupe de réplication. Les connexions entre tous les membres forment la topologie de réplication.
La création de plusieurs dossiers répliqués dans un même groupe de réplication simplifie le processus de déploiement des dossiers répliqués, car la topologie, la planification et la limitation de la bande passante pour le groupe de réplication sont appliquées à chaque dossier répliqué. Pour déployer des dossiers répliqués supplémentaires, vous pouvez utiliser Dfsradmin. exe ou un suivant les instructions de l’Assistant pour définir le chemin d’accès local et les autorisations pour le nouveau dossier répliqué.

Chaque dossier répliqué possède des paramètres uniques, tels que des filtres de fichiers et de sous-dossiers, afin que vous puissiez filtrer différents fichiers et sous-dossiers pour chaque dossier répliqué.

Les dossiers répliqués stockés sur chaque membre peuvent se trouver sur des volumes différents dans le membre, et les dossiers répliqués n’ont pas besoin d’être des dossiers partagés ou une partie d’un espace de noms. Toutefois, le composant logiciel enfichable de gestion DFS permet de partager facilement les dossiers répliqués et de les publier éventuellement dans un espace de noms existant.

Vous pouvez administrer réplication DFS à l’aide de la gestion DFS, des commandes DfsrAdmin et Dfsrdiag ou des scripts qui appellent WMI.

## <a name="requirements"></a>Conditions préalables

Avant de pouvoir déployer la réplication DFS, vous devez configurer vos serveurs comme suit :

- Mettez à jour le schéma Active Directory Domain Services (AD DS) pour inclure les ajouts de schéma Windows Server 2003 R2 ou version ultérieure. Vous ne pouvez pas utiliser de dossiers répliqués en lecture seule avec les ajouts de schéma Windows Server 2003 R2 ou antérieurs.
- Assurez-vous que tous les serveurs présents dans un groupe de réplication se trouvent dans la même forêt. Vous ne pouvez pas activer la réplication entre des serveurs de forêts différentes.
- Installez la réplication DFS sur tous les serveurs qui joueront le rôle de membres d’un groupe de réplication.
- Contactez votre éditeur de logiciels antivirus pour vérifier que votre logiciel antivirus est compatible avec la réplication DFS.
- Recherchez tous les dossiers à répliquer sur les volumes formatés avec le système de fichiers NTFS. La réplication DFS ne prend pas en charge le système ReFS (Resilient File System) ni le système de fichiers FAT. De même, elle ne prend pas en charge la réplication du contenu stocké sur des volumes partagés de cluster.

## <a name="interoperability-with-azure-virtual-machines"></a>Interopérabilité avec les machines virtuelles Azure

L’utilisation de réplication DFS sur une machine virtuelle dans Azure a été testée avec Windows Server. Toutefois, il existe certaines limitations et exigences que vous devez suivre.

- L’utilisation des captures instantanées ou des états de mise en mémoire pour restaurer un serveur exécutant la réplication DFS pour répliquer tout élément différent du dossier SYSVOL entraîne l’échec de la réplication DFS, ce qui requiert des étapes de récupération de base de données particulières. De même, n’exportez, ne clonez pas et ne copiez pas les machines virtuelles. Pour plus d’informations, voir l’article [2517913](https://support.microsoft.com/kb/2517913) de la Base de connaissances Microsoft et la rubrique [Virtualisation DFSR sûre](https://blogs.technet.microsoft.com/filecab/2013/04/05/safely-virtualizing-dfsr/).
- Lorsque vous sauvegardez des données dans un dossier répliqué hébergé sur une machine virtuelle, vous devez utiliser le logiciel de sauvegarde de la machine virtuelle invitée.
- Réplication DFS nécessite l’accès à des contrôleurs de domaine physiques ou virtualisés, il ne peut pas communiquer directement avec Azure AD.
- La réplication DFS nécessite une connexion VPN entre les membres du groupe de réplication local et tous les membres hébergés dans des machines virtuelles Azure. Vous devez également configurer le routeur local (par exemple, Forefront Threat Management Gateway) pour autoriser le mappeur de point de terminaison RPC (port 135) et un port attribué de façon aléatoire compris entre 49152 et 65535 à passer sur la connexion VPN. Vous pouvez utiliser l’applet de commande Set-DfsrMachineConfiguration ou l’outil en ligne de commande Dfsrdiag pour spécifier un port statique au lieu du port aléatoire. Pour plus d’informations sur la façon de spécifier un port statique pour la réplication DFS, voir [Set-DfsrServiceConfiguration](https://docs.microsoft.com/powershell/module/dfsr/set-dfsrserviceconfiguration). Pour plus d’informations sur les ports associés à ouvrir pour la gestion de Windows Server, consultez l’article [832017](https://support.microsoft.com/kb/832017) de la Base de connaissances Microsoft.

Pour en savoir plus sur la prise en main des machines virtuelles Azure, visitez le [site web Microsoft Azure](https://docs.microsoft.com/azure/virtual-machines/).

## <a name="installing-dfs-replication"></a>Installation de réplication DFS

Réplication DFS fait partie du rôle Services de fichiers et de stockage. Les outils de gestion pour DFS (gestion DFS, le module réplication DFS pour Windows PowerShell et les outils en ligne de commande) sont installés séparément dans le cadre du Outils d’administration de serveur distant.

Installez réplication DFS à l’aide du [Centre d’administration Windows](../../manage/windows-admin-center/understand/windows-admin-center.md), gestionnaire de serveur ou PowerShell, comme décrit dans les sections suivantes.

### <a name="to-install-dfs-by-using-server-manager"></a>Pour installer DFS à l’aide du Gestionnaire de serveur

1. Ouvrez le Gestionnaire de serveur, cliquez sur **Gérer**, puis sur **Ajouter des rôles et des fonctionnalités**. L’Assistant Ajout de rôles et de fonctionnalités apparaît.

2. Dans la page **Sélection du serveur**, choisissez le serveur ou le disque dur virtuel (VHD) d'un ordinateur virtuel hors connexion sur lequel vous voulez installer le système de fichiers DFS.

3. Sélectionnez les services de rôle et les fonctionnalités que vous souhaitez installer.

    - Pour installer le service réplication DFS, dans la page **rôles de serveur** , sélectionnez **réplication DFS**.

    - Pour installer uniquement les outils de gestion DFS, dans la page **Fonctionnalités** , développez successivement **Outils d’administration de serveur distant**, **Outils d’administration de rôles**et **Outils de services de fichiers**, puis sélectionnez **Outils de gestion DFS**.

         Les **outils de gestion DFS** installent le composant logiciel enfichable de gestion DFS, les modules réplication DFS et espaces de noms DFS pour Windows PowerShell, ainsi que les outils en ligne de commande, mais il n’installe pas de services DFS sur le serveur.

### <a name="to-install-dfs-replication-by-using-windows-powershell"></a>Pour installer réplication DFS à l’aide de Windows PowerShell

Ouvrez une session Windows PowerShell avec des droits d’utilisateur élevés, puis tapez la commande suivante, où < nom\> correspond au service de rôle ou à la fonctionnalité que vous souhaitez installer (consultez le tableau suivant pour obtenir une liste des noms de services de rôle ou de fonctionnalités appropriés) :

```PowerShell
Install-WindowsFeature <name>
```

|Service de rôle ou fonctionnalité|Nom|
|---|---|
|Réplication DFS|`FS-DFS-Replication`|
|Outils de gestion DFS|`RSAT-DFS-Mgmt-Con`|

Par exemple, pour installer l’option Outils du système de fichiers DFS de la fonctionnalité Outils d’administration de serveur distant, tapez la commande suivante :

```PowerShell
Install-WindowsFeature "RSAT-DFS-Mgmt-Con"
```

Pour installer les réplication DFS et les composants système de fichiers DFS des outils de la fonctionnalité Outils d’administration de serveur distant, tapez :

```PowerShell
Install-WindowsFeature "FS-DFS-Replication", "RSAT-DFS-Mgmt-Con"
```

## <a name="see-also"></a>Articles associés

- [Vue d’ensemble des espaces de noms DFS et réplication DFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj127250(v%3dws.11))
- [Liste de vérification : déployer réplication DFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc772201(v%3dws.11))
- [Liste de vérification : gérer les réplication DFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc755035(v%3dws.11))
- [Déploiement de réplication DFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770925(v%3dws.11))
- [Gestion des réplication DFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770925(v%3dws.11))
- [Résolution des problèmes réplication DFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732802(v%3dws.11))
