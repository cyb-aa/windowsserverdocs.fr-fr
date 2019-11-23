---
title: Vue d'ensemble de la mise à jour adaptée aux clusters
ms.topic: article
ms.prod: windows-server
ms.manager: dongill
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 08/06/2018
description: La mise à jour adaptée aux clusters automatise l’installation des mises à jour logicielles sur des clusters exécutant Windows Server.
ms.assetid: 3c2993b4-aa81-452b-a5c3-3724ad95d892
ms.openlocfilehash: e96223e0b4b44e87ade9dc8eb875f9aa7104f451
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361254"
---
# <a name="cluster-aware-updating-overview"></a>Vue d'ensemble de la mise à jour adaptée aux clusters

> S’applique à : Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Cette rubrique fournit une vue d’ensemble de la mise à jour adaptée aux\-de cluster \(la mise à jour adaptée aux clusters\), une fonctionnalité qui automatise le processus de mise à jour des logiciels sur les serveurs en cluster tout en maintenant la disponibilité.

> [!NOTE]
> Lors de la mise à jour d' [espaces de stockage direct](../storage/storage-spaces/storage-spaces-direct-overview.md) clusters, nous vous recommandons d’utiliser la mise à jour adaptée aux clusters.
  
## <a name="BKMK_OVER"></a>Description de la fonctionnalité  
La mise à jour adaptée aux clusters est une fonctionnalité automatisée qui vous permet de mettre à jour des serveurs dans un [cluster de basculement](failover-clustering-overview.md) avec peu ou pas de perte de disponibilité pendant le processus de mise à jour. Pendant une exécution de mise à jour, la mise à jour adaptée aux clusters effectue en toute transparence les tâches suivantes :  

1. Place chaque nœud du cluster en mode de maintenance du nœud.
2. Déplace les rôles en cluster du nœud.
3. Installe les mises à jour et toutes les mises à jour dépendantes.
4. Effectue un redémarrage si nécessaire.
5. Remet le nœud en mode maintenance.
6. Restaure les rôles en cluster sur le nœud.
7. Déplace pour mettre à jour le nœud suivant.

Pour de nombreux rôles de cluster du cluster, le processus de mise à jour automatique entraîne un basculement planifié. Cela peut provoquer une interruption de service temporaire pour les clients connectés. Toutefois, dans le cas de charges de travail disponibles en continu, telles que hyper\-V avec migration dynamique ou serveur de fichiers avec basculement transparent SMB, la mise à jour adaptée aux clusters peut coordonner les mises à jour de cluster sans aucun impact sur la disponibilité du service.    
  
## <a name="practical-applications"></a>Cas pratiques  
  
-   La mise à jour adaptée aux clusters réduit les interruptions de service dans les services en cluster, réduit le besoin de solutions manuelles de contournement et rend le\-final à\-processus de mise à jour de cluster plus fiable pour l’administrateur. Lorsque la fonctionnalité de mise à jour adaptée aux clusters est utilisée conjointement avec les charges de travail de cluster disponibles en continu, telles que les serveurs de fichiers disponibles en continu \(charge de travail de serveur de fichiers avec basculement transparent SMB\) ou hyper\-V, les mises à jour de cluster peuvent être effectuées sans impact sur la disponibilité des services pour les clients.  
  
-   La mise à jour adaptée aux clusters facilite l’adoption de processus informatiques cohérents au sein de l’entreprise. Les profils d’exécution de mise à jour peuvent être créés pour différentes classes de clusters de basculement, puis gérés de manière centralisée sur un partage de fichiers pour s’assurer que les déploiements de la mise à jour adaptée aux clusters dans l’organisation informatique appliquent les mises à jour de manière cohérente, même si les clusters sont gérés par des lignes différentes\-de\-entreprise  
  
-   La mise à jour adaptée aux clusters permet de planifier des exécutions de mises à jour à intervalles réguliers (par exemple, selon une fréquence quotidienne, hebdomadaire ou mensuelle) pour faciliter la coordination des mises à jour de cluster en fonction d’autres processus de gestion informatiques.  
  
-   La mise à jour adaptée aux clusters fournit une architecture extensible permettant de mettre à jour l’inventaire logiciel du cluster dans un cluster\-la prise en charge. Ils peuvent être utilisés par les serveurs de publication pour coordonner l’installation des mises à jour logicielles qui ne sont pas publiées sur Windows Update ou Microsoft Update ou qui ne sont pas disponibles auprès de Microsoft, par exemple les mises à jour des pilotes de périphériques non\-Microsoft.  
  
-   Le mode de mise à jour\-automatique de la mise à jour adaptée aux clusters permet à un appareil « cluster dans une boîte » \(un ensemble de machines physiques en cluster, généralement empaqueté dans un châssis\) de se mettre à jour lui-même. Ce type de système est généralement déployé dans des filiales disposant d’un support technique local minimal pour gérer les clusters. Le mode de mise à jour automatique de\-offre une grande valeur dans ces scénarios de déploiement.  
  
## <a name="important-functionality"></a>Fonctionnalités importantes  
Vous trouverez ci-dessous une description des fonctionnalités importantes de mise à jour adaptée aux clusters :  
  
-   Une interface utilisateur \(interface\)-la fenêtre de mise à jour adaptée aux clusters, ainsi qu’un ensemble d’applets de commande que vous pouvez utiliser pour prévisualiser, appliquer, surveiller et créer des rapports sur les mises à jour.  
  
-   Fin\-pour\-l’automatisation de l’opération de mise à jour\-du cluster \(d’une exécution de mise à jour\), orchestrée par un ou plusieurs ordinateurs coordinateurs de mise à jour  
  
-   Un plug-in par défaut\-dans qui s’intègre à l’agent Windows Update existant \(WUA\) et Windows Server Update Services \(infrastructure de\) WSUS dans Windows Server pour appliquer les mises à jour Microsoft importantes  
  
-   Un deuxième plug-in\-dans qui peut être utilisé pour appliquer des correctifs Microsoft et qui peut être personnalisé pour appliquer des mises à jour Microsoft non\-  
  
-   Des profils d’exécution de mise à jour dans lesquels vous définissez les paramètres des options d’exécution de mise à jour, telles que le nombre maximal de tentatives de mise à jour autorisées par nœud. Grâce à ces profils, vous pouvez réutiliser facilement les mêmes paramètres de mise à jour entre chaque exécution de mise à jour ou pour d’autres clusters de basculement ;  
  
-   Architecture extensible qui prend en charge le nouveau plug\-en développement pour coordonner d’autres nœuds\-la mise à jour d’outils dans le cluster, tels que les programmes d’installation de logiciels personnalisés, les outils de mise à jour du BIOS et la carte réseau ou l’adaptateur de bus hôte \(HBA\) les outils de mise à jour.  
  
La mise à jour adaptée aux clusters peut coordonner l’opération de mise à jour complète du cluster dans deux modes :  
  
-   **Mode de mise à jour de l’auto\-** Pour ce mode, le rôle en cluster de la mise à jour adaptée aux clusters est configuré en tant que charge de travail sur le cluster de basculement qui doit être mis à jour et une planification de mise à jour associée est définie. Le cluster effectue ses propres mises à jour aux moments planifiés sur la base du profil d’exécution de mise à jour par défaut ou personnalisé. Lors de l’exécution de mise à jour, le processus Coordinateur de mise à jour de la mise à jour adaptée aux clusters commence par mettre à jour le nœud qui est actuellement propriétaire du rôle en cluster Mise à jour adaptée aux clusters, puis il met à jour les nœuds suivants du cluster, à tour de rôle. Pour mettre à jour le nœud de cluster actif, le rôle en cluster de la mise à jour adaptée aux clusters bascule sur un autre nœud de cluster et un nouveau processus Coordinateur de mise à jour sur ce nœud prend le contrôle de l’exécution de mise à jour. En mode de mise à jour automatique\-, la mise à jour adaptée aux clusters peut mettre à jour le cluster de basculement à l’aide d’un\-final entièrement automatisé et\-processus de mise à jour. Un administrateur peut également déclencher des mises à jour sur\-demande dans ce mode, ou simplement utiliser l’approche de mise à jour\-à distance, le cas échéant. En mode de mise à jour automatique des\-, un administrateur peut obtenir des informations récapitulatives sur une exécution de mise à jour en cours en se connectant au cluster et en exécutant l’applet de commande Windows PowerShell **obtenir\-CauRun** .  
  
-   **Mode de mise à jour des\-à distance** Pour ce mode, un ordinateur distant, appelé coordinateur de mise à jour, est configuré avec les outils de la mise à jour adaptée aux clusters. Le Coordinateur de mise à jour n’est pas membre du cluster qui est mis à jour pendant l’exécution de mise à jour. À partir de l’ordinateur distant, l’administrateur déclenche une exécution de mise à jour à la demande sur\-à l’aide d’un profil d’exécution de mise à jour par défaut ou personnalisé. Le mode de mise à jour des\-distants est utile pour surveiller la progression réelle du\-lors de l’exécution de mise à jour, et pour les clusters qui s’exécutent sur des installations minimales.  
  
## <a name="hardware-and-software-requirements"></a>Configuration matérielle et logicielle requise  

La mise à jour adaptée aux clusters peut être utilisée sur toutes les éditions de Windows Server, y compris les installations Server Core. Pour plus d’informations sur la configuration requise, consultez [Configuration requise pour la mise à jour adaptée aux clusters et meilleures pratiques](cluster-aware-updating-requirements.md).

### <a name="installing-cluster-aware-updating"></a>Installation de la mise à jour adaptée aux clusters
Pour utiliser la mise à jour adaptée aux clusters, installez la fonctionnalité de clustering de basculement dans Windows Server et créez un cluster de basculement. Les composants prenant en charge la fonctionnalité de mise à jour adaptée aux clusters sont automatiquement installés sur chaque nœud du cluster.

Pour installer la fonctionnalité Clustering avec basculement, vous avez le choix entre les outils ci-dessous :
- Assistant Ajout de rôles et de fonctionnalités dans le Gestionnaire de serveur
- [Install-WindowsFeature](https://docs.microsoft.com/powershell/module/servermanager/Install-WindowsFeature?view=winserver2012r2-ps&viewFallbackFrom=win10-ps) l’applet de commande Windows PowerShell
- Outil en ligne de commande Gestion et maintenance des images de déploiement (DISM)

Pour plus d’informations, consultez [installer la fonctionnalité de clustering de basculement](create-failover-cluster.md#install-the-failover-clustering-feature).

Vous devez également installer les outils de clustering de basculement, qui font partie de la Outils d’administration de serveur distant et sont installés par défaut lorsque vous installez la fonctionnalité de clustering de basculement dans Gestionnaire de serveur. Les outils de clustering avec basculement incluent l’interface utilisateur de mise à jour adaptée aux clusters et les applets de commande PowerShell. 

Vous devez installer les outils de clustering de basculement comme suit pour prendre en charge les différents modes de la Mise à jour adaptée aux clusters :

- Pour utiliser la mise à jour adaptée aux clusters en mode auto\-, installez les outils de clustering de basculement sur chaque nœud de cluster.   
  
- Pour activer le mode de mise à jour\-à distance, installez les outils de clustering de basculement sur un ordinateur qui dispose d’une connectivité réseau avec le cluster de basculement.  
  
> [!NOTE]  
> -   Vous ne pouvez pas utiliser les outils de clustering avec basculement sur Windows Server 2012 pour gérer la mise à jour adaptée aux clusters sur une version plus récente de Windows Server. 
> -   Pour utiliser la mise à jour adaptée aux clusters uniquement en mode de mise à jour\-à distance, l’installation des outils de clustering de basculement sur les nœuds du cluster n’est pas nécessaire. Cependant, certaines fonctionnalités de la mise à jour adaptée aux clusters ne seront pas disponibles. Pour plus d’informations, consultez [Configuration requise et meilleures pratiques pour la mise à jour adaptée aux clusters\-](cluster-aware-updating-requirements.md).  
> -   Sauf si vous utilisez la mise à jour adaptée aux clusters uniquement en mode de mise à jour automatique\-, l’ordinateur sur lequel les outils de la mise à jour adaptée aux clusters sont installés et qui coordonne les mises à jour ne peuvent pas être membres du cluster de basculement.  
  
### <a name="enabling-self-updating-mode"></a>Activation du mode de mise à jour automatique
Pour activer le mode de mise à jour automatique, vous devez ajouter le rôle en cluster mise à jour adaptée aux clusters au cluster de basculement. Pour ce faire, utilisez l’une des méthodes suivantes :
- Dans Gestionnaire de serveur, sélectionnez **outils** > **mise à jour adaptée**aux clusters, puis dans la fenêtre mise à jour adaptée aux clusters, sélectionnez **configurer les options de mise à jour automatique du cluster**. 
- Dans une session PowerShell, exécutez l’applet de commande [Add-CauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Add-CauClusterRole?view=win10-ps) .  
  
Pour désinstaller la mise à jour adaptée aux clusters, désinstallez la fonctionnalité de clustering avec basculement ou les outils de clustering avec basculement à l’aide de Gestionnaire de serveur, de l’applet de commande [Uninstall-WindowsFeature](https://docs.microsoft.com/powershell/module/servermanager/Uninstall-WindowsFeature?view=win10-ps) ou de la commande DISM\-outils  
  
### <a name="additional-requirements-and-best-practices"></a>Configuration requise et meilleures pratiques supplémentaires  

Pour garantir une mise à jour correcte des nœuds de cluster par la mise à jour adaptée aux clusters et pour de l’aide supplémentaire sur la configuration de votre environnement de cluster de basculement pour une utilisation de la mise à jour adaptée aux clusters, exécutez l’outil Best Practices Analyzer de la mise à jour adaptée aux clusters.  
  
Pour plus d’informations sur la configuration requise et Best Practices Analyzer les meilleures pratiques pour l’utilisation de la mise à jour adaptée aux [clusters, consultez Configuration requise et meilleures pratiques pour la mise à jour avec prise en charge des\-de cluster](cluster-aware-updating-requirements.md).  
  
### <a name="starting-cluster-aware-updating"></a>Démarrage de la mise à jour adaptée aux clusters  
  
##### <a name="to-start-cluster-aware-updating-from-server-manager"></a>Pour démarrer la mise à jour adaptée aux clusters à partir de Gestionnaire de serveur  
  
1.  Démarrez le Gestionnaire de serveur.  
  
2.  Effectuez l’une des opérations suivantes :  
  
    -   Dans le menu **Outils** , cliquez sur mise **à jour adaptée au cluster\-** .  
  
    -   Si un ou plusieurs nœuds de cluster, ou le cluster, sont ajoutés à Gestionnaire de serveur, dans la page **tous les serveurs** , cliquez avec le bouton\-droit sur le nom d’un nœud \(ou le nom de l'\)de cluster, puis cliquez sur **mettre à jour le cluster**.  
  
## <a name="see-also"></a>Voir également  
Les liens suivants fournissent des informations supplémentaires sur l’utilisation de la mise à jour adaptée aux clusters.  
  
-   [Configuration requise et meilleures pratiques pour la mise à jour adaptée aux\-de cluster](cluster-aware-updating.md)  
  
-   [Mise à jour adaptée aux\-de cluster : Forum aux questions](cluster-aware-updating-faq.md)  
  
-   [Options avancées et profils d’exécution de mise à jour pour la mise à jour adaptée aux clusters](cluster-aware-updating-options.md)  
  
-   [Comment fonctionne la mise en œuvre de la mise à jour adaptée aux\-](cluster-aware-updating-plug-ins.md)  
  
-   [Applets de commande de mise à jour adaptée aux clusters\-dans Windows PowerShell](https://docs.microsoft.com/powershell/module/clusterawareupdating/?view=win10-ps&viewFallbackFrom=winserverr2-ps)  
  
-   [Prise en charge de la mise à jour des\-du plug-in de cluster\-en référence](https://docs.microsoft.com/previous-versions/windows/desktop/mscs/cluster-aware-update-plug-in-interfaces-and-classes)  
  

