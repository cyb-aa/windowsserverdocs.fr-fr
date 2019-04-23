---
title: Vue d'ensemble de la mise à jour adaptée aux clusters
ms.topic: article
ms.prod: windows-server-threshold
ms.manager: dongill
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 08/06/2018
description: La mise à jour adaptée aux clusters (CAU) automatise l’installation de mise à jour de logiciels sur les clusters exécutant Windows Server.
ms.assetid: 3c2993b4-aa81-452b-a5c3-3724ad95d892
ms.openlocfilehash: 77ccfe7dc9a769d602ff069d5f1d8e77522aa001
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827420"
---
# <a name="cluster-aware-updating-overview"></a>Vue d'ensemble de la mise à jour adaptée aux clusters

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette rubrique fournit une vue d’ensemble du Cluster\-prenant en charge la mise à jour \(adaptée aux clusters\), une fonctionnalité qui automatise le logiciel de mise à jour sur les serveurs en cluster tout en conservant la disponibilité.

> [!NOTE]
> Lors de la mise à jour [espaces de stockage Direct](../storage/storage-spaces/storage-spaces-direct-overview.md) clusters, nous vous recommandons d’utiliser la mise à jour adaptée aux clusters.
  
## <a name="BKMK_OVER"></a>Description de la fonctionnalité  
La mise à jour adaptée aux clusters est une fonctionnalité qui vous permet de mettre à jour des serveurs dans un [cluster de basculement](failover-clustering-overview.md) avec peu ou aucune perte de disponibilité au cours du processus de mise à jour. Pendant une exécution mise à jour, la mise à jour adaptée aux clusters effectue de façon transparente les tâches suivantes :  

1. Place chaque nœud du cluster en mode de maintenance de nœud.
2. Déplace les rôles en cluster du nœud.
3. Installe les mises à jour et les mises à jour dépendantes.
4. Effectue un redémarrage si nécessaire.
5. Sort le nœud en mode maintenance.
6. Restaure les rôles en cluster sur le nœud.
7. Déplace la mise à jour le nœud suivant.

Pour de nombreux rôles de cluster du cluster, le processus de mise à jour automatique entraîne un basculement planifié. Cela peut provoquer une interruption de service temporaire pour les clients connectés. Toutefois, dans le cas de charges de travail disponibles en permanence, notamment Hyper\-V avec live migration ou serveur de fichiers avec basculement Transparent SMB, la mise à jour adaptée aux clusters peut coordonner les mises à jour de cluster sans aucun impact sur la disponibilité du service.    
  
## <a name="practical-applications"></a>Cas pratiques  
  
-   Adaptée aux clusters réduit les interruptions des services en cluster, réduit le besoin de mise à jour des solutions de contournement manuelle et rend la fin\-à\-mise à jour de processus plus fiable pour l’administrateur de cluster de fin. Lorsque la fonctionnalité d’adaptée aux clusters est utilisée conjointement avec les charges de travail de cluster disponibles en continu, tels que les serveurs de fichiers disponibles en permanence \(le charge de travail du serveur de fichiers avec basculement Transparent SMB\) ou Hyper\-V, le cluster mises à jour peuvent être effectuées sans impact sur la disponibilité du service pour les clients.  
  
-   La mise à jour adaptée aux clusters facilite l’adoption de processus informatiques cohérents au sein de l’entreprise. La mise à jour des profils d’exécution peut créé pour les différentes classes de clusters de basculement et puis gérée de façon centralisée sur un partage de fichiers pour vous assurer que les déploiements adaptée aux clusters dans toute l’organisation informatique appliquent les mises à jour de manière cohérente, même si les clusters sont gérés par les différentes lignes \-de\-entreprise ou administrateurs.  
  
-   La mise à jour adaptée aux clusters permet de planifier des exécutions de mises à jour à intervalles réguliers (par exemple, selon une fréquence quotidienne, hebdomadaire ou mensuelle) pour faciliter la coordination des mises à jour de cluster en fonction d’autres processus de gestion informatiques.  
  
-   Adaptée aux clusters fournit une architecture extensible pour mettre à jour de l’inventaire logiciel de cluster dans un cluster\-manière éclairée. Cela peut servir par les éditeurs pour coordonner l’installation de mises à jour logicielles qui ne sont pas publiées pour Windows Update ou Microsoft Update ou qui ne sont pas disponibles auprès de Microsoft, par exemple, pour les mises à jour non\-des pilotes de périphériques Microsoft.  
  
-   Adaptée aux clusters self\-en mode de mise à jour permet à un « cluster dans une zone de » \(un ensemble d’ordinateurs physiques en cluster, regroupées dans un seul châssis\) pour mettre à jour lui-même. Ce type de système est généralement déployé dans des filiales disposant d’un support technique local minimal pour gérer les clusters. Self\-en mode de mise à jour est particulièrement intéressant dans ces scénarios de déploiement.  
  
## <a name="important-functionality"></a>Fonctionnalités importantes  
Voici une description des fonctionnalités importantes de la mise à jour adaptée aux clusters :  
  
-   Une interface utilisateur \(l’interface utilisateur\) -la fenêtre de la mise à jour prenant en charge du Cluster - et un ensemble d’applets de commande que vous pouvez utiliser pour afficher un aperçu, appliquer, surveiller et créer des rapports sur les mises à jour  
  
-   Une fin\-à\-fin automation du cluster\-mise à jour \(une exécution de la mise à jour\)orchestrées par un ou plusieurs ordinateurs coordinateur de mise à jour  
  
-   Une prise par défaut\-dans qui s’intègre à l’Agent de mise à jour Windows existant \(WUA\) et Windows Server Update Services \(WSUS\) infrastructure dans Windows Server pour appliquer Microsoft importante mises à jour  
  
-   Un second plug\-dans qui peut être utilisé pour appliquer des correctifs Microsoft, et qui peut être personnalisé à appliquer non\-mises à jour Microsoft  
  
-   Des profils d’exécution de mise à jour dans lesquels vous définissez les paramètres des options d’exécution de mise à jour, telles que le nombre maximal de tentatives de mise à jour autorisées par nœud. Grâce à ces profils, vous pouvez réutiliser facilement les mêmes paramètres de mise à jour entre chaque exécution de mise à jour ou pour d’autres clusters de basculement ;  
  
-   Une architecture extensible qui prend en charge de nouveaux plug\-dans le développement pour coordonner l’autre nœud\-la mise à jour des outils au sein du cluster, tels que des programmes d’installation de logiciels personnalisés, la mise à jour des outils et d’une carte réseau ou d’adaptateur de bus hôte deBIOS\( HBA\) la mise à jour des outils.  
  
La mise à jour adaptée aux clusters peut coordonner le processus complet de clusters selon deux modes de mise à jour :  
  
-   **Self\-la mise à jour en mode** pour ce mode, le rôle en cluster adaptée aux clusters est configuré comme une charge de travail sur le cluster de basculement qui doit être mis à jour, et un planning de mise à jour associé est défini. Le cluster effectue ses propres mises à jour aux moments planifiés sur la base du profil d’exécution de mise à jour par défaut ou personnalisé. Lors de l’exécution de mise à jour, le processus Coordinateur de mise à jour de la mise à jour adaptée aux clusters commence par mettre à jour le nœud qui est actuellement propriétaire du rôle en cluster Mise à jour adaptée aux clusters, puis il met à jour les nœuds suivants du cluster, à tour de rôle. Pour mettre à jour le nœud de cluster actif, le rôle en cluster de la mise à jour adaptée aux clusters bascule sur un autre nœud de cluster et un nouveau processus Coordinateur de mise à jour sur ce nœud prend le contrôle de l’exécution de mise à jour. Dans self\-mise à jour du mode, adaptée aux clusters peut mettre à jour le cluster de basculement à l’aide d’entièrement automatisé, fin\-à\-mise à jour de fin. Un administrateur peut également déclencher des mises à jour sur\-à la demande dans ce mode, ou simplement utiliser la télécommande\-la mise à jour approche si vous le souhaitez. Dans self\-mise à jour du mode, un administrateur peut obtenir des informations résumées sur l’exécution de la mise à jour en cours d’exécution en vous connectant au cluster et en cours d’exécution le **obtenir\-CauRun** applet de commande Windows PowerShell.  
  
-   **À distance\-la mise à jour en mode** pour ce mode, un ordinateur distant, appelé coordinateur de mise à jour, est configuré avec les outils d’adaptée aux clusters. Le Coordinateur de mise à jour n’est pas membre du cluster qui est mis à jour pendant l’exécution de mise à jour. À partir de l’ordinateur distant, l’administrateur déclenche un sur\-exigent la mise à jour d’exécuter à l’aide d’une par défaut ou le profil d’exécution de la mise à jour de personnalisé. À distance\-mise à jour du mode est utile pour surveiller réel\-progression du temps lors de l’exécution de la mise à jour et pour les clusters qui sont exécutent sur les installations Server Core.  
  
## <a name="hardware-and-software-requirements"></a>Configuration matérielle et logicielle requise  

Adaptée aux clusters peut être utilisée sur toutes les éditions de Windows Server, y compris les installations Server Core. Pour connaître la configuration requise d’informations, consultez [configuration requise de la mise à jour adaptée aux clusters et les meilleures pratiques](cluster-aware-updating-requirements.md).

### <a name="installing-cluster-aware-updating"></a>L’installation de la mise à jour adaptée aux clusters
Pour utiliser adaptée aux clusters, installez la fonctionnalité Clustering avec basculement dans Windows Server et créer un cluster de basculement. Les composants prenant en charge la fonctionnalité de mise à jour adaptée aux clusters sont automatiquement installés sur chaque nœud du cluster.

Pour installer la fonctionnalité Clustering avec basculement, vous avez le choix entre les outils ci-dessous :
- Assistant Ajout de rôles et de fonctionnalités dans le Gestionnaire de serveur
- [Install-WindowsFeature](https://docs.microsoft.com/powershell/module/servermanager/Install-WindowsFeature?view=winserver2012r2-ps&viewFallbackFrom=win10-ps) applet de commande Windows PowerShell
- Outil en ligne de commande Gestion et maintenance des images de déploiement (DISM)

Pour plus d’informations, consultez [installer la fonctionnalité Clustering avec basculement](create-failover-cluster.md#install-the-failover-clustering-feature).

Vous devez également installer les outils de Clustering de basculement, qui font partie des outils d’Administration de serveur distant et sont installés par défaut lorsque vous installez la fonctionnalité Clustering avec basculement dans le Gestionnaire de serveur. Les outils de Clustering de basculement incluent l’interface utilisateur de la mise à jour adaptée aux clusters et les applets de commande PowerShell. 

Vous devez installer les outils de clustering de basculement comme suit pour prendre en charge les différents modes de la Mise à jour adaptée aux clusters :

- Pour utiliser adaptée aux clusters dans self\-mise à jour du mode, installez les outils de Clustering de basculement sur chaque nœud du cluster.   
  
- Pour activer à distance\-mise à jour du mode, installez les outils de Clustering de basculement sur un ordinateur qui dispose d’une connectivité réseau au cluster de basculement.  
  
> [!NOTE]  
> -   Vous ne pouvez pas utiliser les outils de Clustering de basculement Windows Server 2012 pour gérer la mise à jour adaptée aux clusters sur une version plus récente de Windows Server. 
> -   Pour utiliser adaptée aux clusters uniquement dans remote\-la mise à jour en mode, installation des outils de Clustering de basculement sur les nœuds de cluster n’est pas requise. Cependant, certaines fonctionnalités de la mise à jour adaptée aux clusters ne seront pas disponibles. Pour plus d’informations, consultez [configuration requise et meilleures pratiques pour le Cluster\-prenant en charge la mise à jour](cluster-aware-updating-requirements.md).  
> -   Sauf si vous utilisez adaptée uniquement dans self\-la mise à jour en mode, l’ordinateur sur lequel les outils adaptée aux clusters sont installés et qui coordonne les mises à jour ne peut pas être un membre du cluster de basculement.  
  
### <a name="enabling-self-updating-mode"></a>L’activation du mode de mise à jour automatique
Pour activer le mode de mise à jour automatique, vous devez ajouter le rôle en cluster de la mise à jour adaptée aux clusters au cluster de basculement. Pour ce faire, utilisez une des méthodes suivantes :
- Dans le Gestionnaire de serveur, sélectionnez **outils** > **la mise à jour adaptée aux clusters**, puis dans la fenêtre de la mise à jour adaptée aux clusters, sélectionnez **cluster configurer mise à jour automatique des options**. 
- Dans une session PowerShell, exécutez le [Add-CauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Add-CauClusterRole?view=win10-ps) applet de commande.  
  
Pour désinstaller l’adaptée aux clusters, désinstallez la fonctionnalité Clustering avec basculement ou les outils de Clustering de basculement à l’aide du Gestionnaire de serveur, le [Uninstall-WindowsFeature](https://docs.microsoft.com/powershell/module/servermanager/Uninstall-WindowsFeature?view=win10-ps) applet de commande, ou la commande DISM\-outils en ligne.  
  
### <a name="additional-requirements-and-best-practices"></a>Configuration requise et meilleures pratiques supplémentaires  

Pour garantir une mise à jour correcte des nœuds de cluster par la mise à jour adaptée aux clusters et pour de l’aide supplémentaire sur la configuration de votre environnement de cluster de basculement pour une utilisation de la mise à jour adaptée aux clusters, exécutez l’outil Best Practices Analyzer de la mise à jour adaptée aux clusters.  
  
Pour connaître la configuration requise et meilleures pratiques pour l’utilisation adaptée aux clusters et des informations sur l’exécution adaptée aux clusters Best Practices Analyzer, consultez [configuration requise et meilleures pratiques pour le Cluster\-prenant en charge la mise à jour](cluster-aware-updating-requirements.md).  
  
### <a name="starting-cluster-aware-updating"></a>Démarrage de la mise à jour adaptée aux clusters  
  
##### <a name="to-start-cluster-aware-updating-from-server-manager"></a>Pour démarrer la mise à jour adaptée aux clusters à partir du Gestionnaire de serveur  
  
1.  Démarrez le Gestionnaire de serveur.  
  
2.  Faites une des actions suivantes :  
  
    -   Sur le **outils** menu, cliquez sur **Cluster\-prenant en charge la mise à jour**.  
  
    -   Si un ou plusieurs nœuds de cluster ou le cluster, est ajouté au Gestionnaire de serveur, sur le **tous les serveurs** page droite\-cliquez sur le nom d’un nœud \(ou le nom du cluster\), puis cliquez sur  **Mettre à jour de Cluster**.  
  
## <a name="see-also"></a>Voir aussi  
Les liens suivants fournissent plus d’informations sur l’utilisation de la mise à jour adaptée aux clusters.  
  
-   [Configuration requise et meilleures pratiques pour le Cluster\-prenant en charge la mise à jour](cluster-aware-updating.md)  
  
-   [Cluster\-prenant en charge la mise à jour : Forum aux Questions](cluster-aware-updating-faq.md)  
  
-   [Les Options avancées et profils d’exécution la mise à jour pour adaptée aux clusters](cluster-aware-updating-options.md)  
  
-   [Comment adaptée aux clusters Branchez\-ins travail](cluster-aware-updating-plug-ins.md)  
  
-   [Cluster\-prenant en charge la mise à jour des applets de commande dans Windows PowerShell](https://docs.microsoft.com/powershell/module/clusterawareupdating/?view=win10-ps&viewFallbackFrom=winserverr2-ps)  
  
-   [Cluster\-prenant en charge la mise à jour Plug\-dans la référence](https://docs.microsoft.com/previous-versions/windows/desktop/mscs/cluster-aware-update-plug-in-interfaces-and-classes)  
  

