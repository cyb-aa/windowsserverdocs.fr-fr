---
title: "Vue d’ensemble de la mise à jour adaptée aux clusters"
ms.topic: article
ms.prod: windows-server-threshold
ms.manager: dongill
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 3/23/2017
description: "Mise à jour adaptée aux clusters (CAU) automatise l’installation de mise à jour logicielles sur des clusters exécutant Windows Server."
ms.assetid: 3c2993b4-aa81-452b-a5c3-3724ad95d892
ms.openlocfilehash: 68336741accabd6e4cb0da5dbd708be707f68df6
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="cluster-aware-updating-overview"></a>Vue d’ensemble de la mise à jour adaptée aux clusters

> S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016, Windows Server2012R2, Windows Server2012

Cette rubrique fournit une vue d’ensemble de la mise à jour adaptée aux Cluster\ \(CAU\), une fonctionnalité qui automatise le logiciel de mise à jour des processus sur les serveurs en cluster tout en conservant la disponibilité.

> [!NOTE]
> Lors de la mise à jour [espaces de stockage Direct](../storage/storage-spaces/storage-spaces-direct-overview.md) clusters, nous vous recommandons d’utiliser la mise à jour adaptée aux clusters.
  
## <a name="BKMK_OVER"></a>Description de la fonctionnalité  
Mise à jour adaptée aux clusters est une fonctionnalité qui vous permet de mettre à jour des serveurs dans un [cluster de basculement](failover-clustering-overview.md) avec peu ou aucune perte de disponibilité pendant le processus de mise à jour. Pendant une mise à jour d’exécution, la mise à jour adaptée aux clusters effectue de façon transparente les tâches suivantes:  

1. Place chaque nœud du cluster en mode de maintenance de nœud.
2. Déplace les rôles en cluster du nœud.
3. Installe les mises à jour et les mises à jour dépendantes.
4. Effectue un redémarrage si nécessaire.
5. Sort le nœud en mode de maintenance.
6. Restaure les rôles en cluster sur le nœud.
7. Déplace vers le nœud suivant.

De nombreux rôles de cluster du cluster, le processus de mise à jour automatique déclenche un basculement planifié. Cela peut entraîner une interruption de service temporaire pour les clients connectés. Cependant, dans le cas de charges de travail disponibles en continu, telles que Hyper\-V avec migration dynamique ou serveur de fichiers avec basculement Transparent SMB, la mise à jour adaptée aux clusters coordonne les mises à jour de cluster sans aucun impact sur la disponibilité du service.    
  
## <a name="practical-applications"></a>Applications pratiques  
  
-   Adaptée aux clusters réduit les interruptions des services en cluster, réduit la nécessité pour les solutions de contournement de mise à jour manuelle et rend le cluster end\ celle-ci bout mise à jour la plus fiable pour l’administrateur. Lorsque la fonctionnalité d’adaptée aux clusters est utilisée conjointement avec les charges de travail de cluster disponibles en continu, telles que les serveurs de fichiers disponibles en permanence \ (charge de travail avec Failover\ Transparent SMB) ou Hyper\-V, le cluster de mises à jour peuvent être effectuées sans aucun impact sur la disponibilité du service pour les clients.  
  
-   Adaptée aux clusters facilite l’adoption de processus informatiques cohérents au sein de l’entreprise. Mise à jour des profils d’exécution de création pour différentes catégories de clusters de basculement et puis de centraliser sur un partage de fichiers pour vous assurer que les déploiements adaptée aux clusters dans toute l’organisation informatique appliquent des mises à jour de façon cohérente, même si les clusters sont gérés par différente lignes vente\ maximum\ métier ou des administrateurs.  
  
-   Adaptée aux clusters peut planifier des exécutions de mise à jour régulièrement quotidienne, hebdomadaire ou mensuelle afin de coordonner les mises à jour de cluster avec d’autres processus de gestion informatique.  
  
-   Adaptée aux clusters fournit une architecture extensible pour mettre à jour de l’inventaire logiciel de cluster de manière prenant en charge cluster\. Cela peut servir par des éditeurs pour coordonner l’installation des mises à jour de logiciels qui ne sont pas publiées sur Windows Update ou MicrosoftUpdate ou qui ne sont pas disponibles auprès de Microsoft, par exemple, les mises à jour des pilotes de périphériques autres que Microsoft.  
  
-   Mode de mise à jour intégrée adaptée aux clusters permet à un système «cluster dans une zone» \ (un ensemble de machines physiques en cluster, regroupées dans un seul chassis\) pour mettre à jour lui-même. En règle générale, ces appareils sont déployés dans les filiales avec minimale support technique local pour gérer les clusters. Mode de mise à jour intégrée est particulièrement intéressant dans ces scénarios de déploiement.  
  
## <a name="important-functionality"></a>Fonctionnalités importantes  
Voici une description des fonctionnalités importantes de mise à jour adaptée aux clusters:  
  
-   Une interface utilisateur \(UI\) - la fenêtre de mise à jour adaptée aux clusters - et un ensemble d’applets de commande que vous pouvez utiliser pour afficher un aperçu, appliquer, surveiller et créer des rapports sur les mises à jour  
  
-   Une automation end\-celle-ci-fin de l’opération de mise à jour cluster\ \ (une mise à jour exécuter), orchestrée par un ou plusieurs ordinateurs coordinateur de mise à jour  
  
-   Par défaut un plug-in qui s’intègre à l’infrastructure WindowsServerUpdateServices \(WSUS\) dans Windows Server et d’existant \(WUA\) l’Agent Windows Update pour appliquer des mises à jour Microsoft importantes  
  
-   Un deuxième un plug-in qui peut être utilisé pour appliquer des correctifs Microsoft, et qui peut être personnalisé pour appliquer des mises à jour des éditeurs autres que Microsoft  
  
-   Mise à jour des profils d’exécution que vous configurez avec les paramètres d’options d’exécution de mise à jour, telles que le nombre maximal de fois que la mise à jour sera relancé par nœud. Profils d’exécution de mise à jour permettent de rapidement réutiliser les mêmes paramètres entre chaque exécution de mise à jour et de partager facilement les paramètres de mise à jour avec les autres clusters de basculement.  
  
-   Une architecture extensible qui prend en charge un plug-in développement de nouveaux pour coordonner d’autres outils de mise à jour node\ du cluster, tels que des programmes d’installation personnalisée de logiciels, outils, de mise à jour de BIOS et outils de réseau hôte ou adaptateur de bus carte \(HBA\) mise à jour.  
  
Mise à jour adaptée aux clusters peut coordonner l’opération dans deux modes de mise à jour complète de cluster:  
  
-   **Mode de mise à jour intégrée** pour ce mode, le rôle en cluster adaptée aux clusters est configuré comme une charge de travail sur le cluster de basculement qui doit être mis à jour, et un planning de mise à jour associé est défini. Le cluster met à jour lui-même aux heures planifiées à l’aide par défaut ou un profil d’exécution de mise à jour de personnalisé. La mise à jour de l’exécution, le processus coordinateur de mise à jour adaptée aux clusters démarre sur le nœud qui possède actuellement le rôle en cluster adaptée aux clusters, et le processus effectue séquentiellement mises à jour sur chaque nœud du cluster. Pour mettre à jour le nœud de cluster actuel, le rôle en cluster adaptée aux clusters bascule vers un autre nœud de cluster, et un nouveau processus coordinateur de mise à jour sur ce nœud prend le contrôle de l’exécution de mise à jour. En mode de mise à jour intégrée, adaptée aux clusters peut mettre à jour le cluster de basculement à l’aide d’un processus de mise à jour entièrement automatisé, end\ bout celle-ci. Un administrateur peut également déclencher des mises à jour à la demande dans\ dans ce mode, ou utiliser simplement la remote\-mise à jour approche si vous le souhaitez. En mode de mise à jour intégrée, un administrateur peut obtenir des informations résumées sur l’exécution de mise à jour en cours en se connectant au cluster et en cours d’exécution le **applet de commande Get-CauRun** applet de commande Windows PowerShell.  
  
-   **Mode de mise à jour Remote\** pour ce mode, un ordinateur distant, appelé coordinateur de mise à jour, est configuré avec les outils adaptée aux clusters. Le coordinateur de mise à jour n’est pas membre du cluster qui est mis à jour pendant l’exécution de mise à jour. À partir de l’ordinateur distant, l’administrateur déclenche une exécution de mise à jour dans\ à la demande à l’aide par défaut ou un profil d’exécution de mise à jour de personnalisé. Mode de mise à jour Remote\ est utile pour surveiller la progression de l’heure de real\ pendant l’exécution de mise à jour et pour les clusters qui sont en cours d’exécution sur les installations Server Core.  
  
## <a name="hardware-and-software-requirements"></a>Configuration matérielle et logicielle requise  

Adaptée aux clusters peut être utilisée sur toutes les éditions de Windows Server, y compris les installations Server Core. Pour plus d’informations d’exigences détaillées, voir [les meilleures pratiques et les exigences de mise à jour adaptée aux clusters](cluster-aware-updating-requirements.md).

### <a name="installing-cluster-aware-updating"></a>L’installation de la mise à jour adaptée aux clusters
Pour utiliser adaptée aux clusters, installez la fonctionnalité Clustering avec basculement dans Windows Server et créer un cluster de basculement. Les composants qui prennent en charge la fonctionnalité adaptée aux clusters sont automatiquement installés sur chaque nœud du cluster.

Pour installer la fonctionnalité de Clustering de basculement, vous pouvez utiliser les outils suivants:
- Assistant Ajout de rôles et fonctionnalités dans le Gestionnaire de serveur
- [Install-WindowsFeature](https://technet.microsoft.com/itpro/powershell/windows/server-manager/install-windowsfeature) applet de commande Windows PowerShell
- Outil de ligne de commande de maintenance des images de déploiement et de gestion (DISM)

Pour plus d’informations, voir [installer ou désinstaller des rôles, Services de rôle ou fonctionnalités](https://technet.microsoft.com/library/hh831809(v=ws.11).aspx).

Vous devez également installer les outils de Clustering de basculement qui font partie des outils d’Administration de serveur distant et sont installés par défaut lorsque vous installez la fonctionnalité Clustering avec basculement dans le Gestionnaire de serveur. Les outils de clustering avec basculement incluent l’interface utilisateur de la mise à jour adaptée aux clusters et les applets de commande PowerShell. 

Vous devez installer les outils de Clustering de basculement comme suit pour prendre en charge les différents modes de mise à jour adaptée aux clusters:

- Pour utiliser adaptée aux clusters en mode de mise à jour intégrée, installez les outils de Clustering de basculement sur chaque nœud du cluster.   
  
- Pour activer le mode de mise à jour remote\, installez les outils de Clustering de basculement sur un ordinateur qui dispose d’une connectivité réseau au cluster de basculement.  
  
> [!NOTE]  
> -   Vous ne pouvez pas utiliser les outils de Clustering de basculement Windows Server2012 pour gérer la mise à jour adaptée aux clusters sur une version plus récente de Windows Server. 
> -   Pour utiliser adaptée aux clusters exclusivement en mode de mise à jour remote\, installation des outils de Clustering de basculement sur les nœuds de cluster n’est pas nécessaire. Toutefois, certaines fonctionnalités adaptée aux clusters ne seront pas disponibles. Pour plus d’informations, voir [configuration requise et meilleures pratiques pour la mise à jour adaptée aux Cluster\](cluster-aware-updating-requirements.md).  
> -   Sauf si vous utilisez adaptée aux clusters exclusivement en mode de mise à jour intégrée, l’ordinateur sur lequel les outils adaptée aux clusters sont installés et qui coordonne les mises à jour ne peut pas être un membre du cluster de basculement.  
  
### <a name="enabling-self-updating-mode"></a>L’activation du mode de mise à jour automatique
Pour activer le mode de mise à jour automatique, vous devez ajouter le rôle en cluster mise à jour adaptée aux clusters au cluster de basculement. Pour ce faire, utilisez une des méthodes suivantes:
- Dans le Gestionnaire de serveur, sélectionnez **outils** > **mise à jour adaptée aux clusters**, puis dans la fenêtre de la mise à jour adaptée aux clusters, sélectionnez **cluster configurer mise à jour automatique des options**. 
- Dans une session PowerShell, exécutez le [Add-CauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/add-cauclusterrole) applet de commande.  
  
Pour désinstaller adaptée aux clusters, désinstallez la fonctionnalité Clustering avec basculement ou les outils de Clustering de basculement à l’aide du Gestionnaire de serveur, le [Uninstall-WindowsFeature](https://technet.microsoft.com/itpro/powershell/windows/server-manager/uninstall-windowsfeature) applet de commande, ou les outils de ligne de commande DISM.  
  
### <a name="additional-requirements-and-best-practices"></a>Configuration supplémentaire requise et meilleures pratiques  

Pour vous assurer qu’adaptée aux clusters peut mettre à jour les nœuds de cluster avec succès et pour obtenir des conseils supplémentaires pour la configuration de votre environnement de cluster de basculement pour une utilisation adaptée aux clusters, vous pouvez exécuter le Best Practices Analyzer adaptée aux clusters.  
  
Pour les exigences détaillées et des meilleures pratiques adaptée aux clusters, et des informations sur l’exécution adaptée aux clusters Best Practices Analyzer, voir [configuration requise et meilleures pratiques pour la mise à jour adaptée aux Cluster\](cluster-aware-updating-requirements.md).  
  
### <a name="starting-cluster-aware-updating"></a>À partir de la mise à jour adaptée aux clusters  
  
##### <a name="to-start-cluster-aware-updating-from-server-manager"></a>Pour démarrer la mise à jour adaptée aux clusters à partir du Gestionnaire de serveur  
  
1.  Démarrez le Gestionnaire de serveur.  
  
2.  Effectuez l’une des opérations suivantes:  
  
    -   Sur le **outils** menu, cliquez sur **mise à jour adaptée aux Cluster\**.  
  
    -   Si un ou plusieurs nœuds de cluster, ou le cluster, est ajouté au Gestionnaire de serveur, sur le **tous les serveurs** page, le nom d’un nœud de clic droit \ (ou le nom de le cluster\), puis cliquez sur **mise à jour de Cluster**.  
  
## <a name="see-also"></a>Voir aussi  
Les liens suivants fournissent plus d’informations sur l’utilisation de la mise à jour adaptée aux clusters.  
  
-   [Configuration requise et meilleures pratiques pour la mise à jour adaptée aux Cluster\](cluster-aware-updating.md)  
  
-   [Mise à jour prenant en charge Cluster\: Forum aux Questions](cluster-aware-updating-faq.md)  
  
-   [Options avancées et profils d’exécution mise à jour pour adaptée aux clusters](cluster-aware-updating-options.md)  
  
-   [Fonctionnement adaptée aux clusters un plug-ins](cluster-aware-updating-plug-ins.md)  
  
-   [Applets de commande de mise à jour prenant en charge Cluster\ dans Windows PowerShell](https://go.microsoft.com/fwlink/p/?LinkId=237675)  
  
-   [Cluster\ prenant en charge la mise à jour d’un plug-in de référence](https://msdn.microsoft.com/library/hh418084.aspx)  
  

