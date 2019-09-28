---
title: Surveiller les serveurs et configurer des alertes avec Azure Monitor à partir du centre d’administration Windows
description: Le centre d’administration Windows (Project Honolulu) s’intègre à Azure Monitor
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.localizationpriority: medium
ms.prod: windows-server
ms.date: 03/24/2019
ms.openlocfilehash: 28108a79bbdc654f6437a698c158a3f74d4423ba
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407010"
---
# <a name="monitor-servers-and-configure-alerts-with-azure-monitor-from-windows-admin-center"></a>Surveiller les serveurs et configurer des alertes avec Azure Monitor à partir du centre d’administration Windows

[En savoir plus sur l’intégration d’Azure avec le centre d’administration Windows.](../plan/azure-integration-options.md)

[Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/overview) est une solution qui collecte, analyse et agit sur la télémétrie à partir de diverses ressources, notamment les serveurs Windows et les machines virtuelles, localement et dans le Cloud. Bien que Azure Monitor extrait des données à partir de machines virtuelles Azure et d’autres ressources Azure, cet article se concentre sur la façon dont Azure Monitor fonctionne avec les serveurs et les machines virtuelles locaux, en particulier avec le centre d’administration Windows. Si vous souhaitez savoir comment utiliser Azure Monitor pour obtenir des alertes par courrier électronique sur votre cluster hyper-convergé, consultez la rubrique [utilisation de Azure Monitor pour envoyer des courriers électroniques pour les erreurs de service de contrôle d’intégrité](https://docs.microsoft.com/windows-server/storage/storage-spaces/configure-azure-monitor).

## <a name="how-does-azure-monitor-work"></a>Comment Azure Monitor fonctionne-t-il ?
les données ![img @ no__t-1 générées à partir de serveurs Windows locaux sont collectées dans un espace de travail Log Analytics dans Azure Monitor. Dans un espace de travail, vous pouvez activer diverses solutions de surveillance, c’est-à-dire des ensembles de logiques qui fournissent des Insights pour un scénario particulier. Par exemple, Azure Update Management, Azure Security Center et Azure Monitor pour machines virtuelles sont des solutions de surveillance qui peuvent être activées dans un espace de travail. 

Lorsque vous activez une solution de surveillance dans un espace de travail Log Analytics, tous les serveurs qui signalent à cet espace de travail commencent à collecter des données pertinentes pour cette solution, afin que la solution puisse générer des informations pour tous les serveurs de l’espace de travail. 

Pour collecter des données de télémétrie sur un serveur local et les transmettre à l’espace de travail Log Analytics, Azure Monitor nécessite l’installation de la Microsoft Monitoring Agent ou de la valeur MMA. Certaines solutions de surveillance requièrent également un agent secondaire. Par exemple, Azure Monitor pour machines virtuelles dépend également d’un agent ServiceMap pour les fonctionnalités supplémentaires fournies par cette solution. 

Certaines solutions, comme Azure Update Management, dépendent également de Azure Automation, qui vous permet de gérer de manière centralisée les ressources dans les environnements Azure et non-Azure. Par exemple, Azure Update Management utilise Azure Automation pour planifier et orchestrer l’installation des mises à jour sur les ordinateurs de votre environnement, de manière centralisée, à partir de l’Portail Azure.


## <a name="how-does-windows-admin-center-enable-you-to-use-azure-monitor"></a>Comment le centre d’administration Windows vous permet-il d’utiliser des Azure Monitor ?

À partir de WAC, vous pouvez activer deux solutions de surveillance :

- [Update Management Azure](azure-update-management.md) (dans l’outil mises à jour)
- Azure Monitor pour machines virtuelles (dans les paramètres du serveur), a. k. un ordinateur virtuel Insights

Vous pouvez commencer à utiliser Azure Monitor à partir de l’un de ces outils. Si vous n’avez jamais utilisé Azure Monitor avant, WAC approvisionne automatiquement un espace de travail Log Analytics (et un compte Azure Automation, si nécessaire), puis installe et configure le Microsoft Monitoring Agent (MMA) sur le serveur cible. Il installe ensuite la solution correspondante dans l’espace de travail. 

Par exemple, si vous accédez pour la première fois à l’outil mises à jour pour configurer Azure Update Management, WAC :

1. Installer MMA sur la machine
2. Créez l’espace de travail Log Analytics et le compte Azure Automation (puisqu’un compte Azure Automation est nécessaire dans ce cas)
3. Installez la solution Update Management dans l’espace de travail nouvellement créé.

Si vous souhaitez ajouter une autre solution de surveillance à partir de WAC sur le même serveur, WAC installera simplement cette solution dans l’espace de travail existant auquel ce serveur est connecté. WAC installe également tous les autres agents nécessaires.

Si vous vous connectez à un autre serveur, mais que vous avez déjà configuré un espace de travail Log Analytics (via WAC ou manuellement dans le portail Azure), vous pouvez également installer l’agent MMA sur le serveur et le connecter à un espace de travail existant. Lorsque vous connectez un serveur à un espace de travail, il démarre automatiquement la collecte de données et la création de rapports pour les solutions installées dans cet espace de travail.

## <a name="azure-monitor-for-virtual-machines-aka-virtual-machine-insights"></a>Azure Monitor pour les machines virtuelles (également appelé Insights des machines virtuelles)
>S'applique à : Windows Admin Center Preview

Quand vous configurez Azure Monitor pour machines virtuelles dans les paramètres du serveur, le centre d’administration Windows active la solution Azure Monitor pour machines virtuelles, également appelée Virtual Machine Insights. Cette solution vous permet de surveiller l’intégrité et les événements du serveur, de créer des alertes par courrier électronique, d’obtenir une vue consolidée des performances du serveur dans l’ensemble de votre environnement et de visualiser des applications, des systèmes et des services connectés à un serveur donné.

> [!NOTE]
> En dépit de son nom, la machine virtuelle Insights fonctionne pour les serveurs physiques, ainsi que pour les machines virtuelles.

Avec la disponibilité de 5 Go de données/mois/mois par le Azure Monitor, vous pouvez facilement essayer cela pour un ou deux serveurs sans vous soucier de la facturation. Lisez ce qui suit pour voir les autres avantages de l’intégration des serveurs dans Azure Monitor, par exemple obtenir une vue consolidée des performances des systèmes sur les serveurs de votre environnement.

### <a name="set-up-your-server-for-use-with-azure-monitor"></a>**Configuration de votre serveur pour une utilisation avec Azure Monitor**

Dans la page vue d’ensemble d’une connexion au serveur, cliquez sur le bouton nouveau « gérer les alertes », ou accédez à paramètres du serveur > la surveillance et les alertes. Dans cette page, intégrez votre serveur à Azure Monitor en cliquant sur « configurer » et en complétant le volet d’installation. Le centre d’administration s’occupe de l’approvisionnement de l’espace de travail Azure Log Analytics, de l’installation de l’agent nécessaire et de la configuration de la solution VM Insights. Une fois que vous avez terminé, votre serveur envoie les données du compteur de performance à Azure Monitor, ce qui vous permet d’afficher et de créer des alertes par courrier électronique basées sur ce serveur, à partir du Portail Azure.

### <a name="create-email-alerts"></a>**Créer des alertes par courrier électronique**

Une fois que vous avez attaché votre serveur à Azure Monitor, vous pouvez utiliser les liens hypertexte intelligents dans les paramètres > page surveillance et alertes pour accéder au portail Azure. Le centre d’administration active automatiquement les compteurs de performances à collecter. vous pouvez ainsi [créer facilement une nouvelle alerte](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log) en personnalisant l’une des nombreuses requêtes prédéfinies ou en écrivant vos propres requêtes.

### <a name="get-a-consolidated-view-across-multiple-servers-"></a>\* * Obtenir une vue consolidée sur plusieurs serveurs * *

Si vous intégrez plusieurs serveurs à un seul Log Analytics espace de travail dans Azure Monitor, vous pouvez obtenir une vue consolidée de tous ces serveurs à partir de la [solution machines virtuelles](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-overview) dans Azure Monitor.  (Notez que seuls les onglets performances et mappages de machines virtuelles Insights pour Azure Monitor fonctionnent avec les serveurs locaux : l’onglet intégrité ne fonctionne qu’avec les machines virtuelles Azure.) Pour l’afficher dans la Portail Azure, accédez à Azure Monitor > machines virtuelles (sous Insights), puis accédez aux onglets « performance » ou « Maps ».

### <a name="visualize-apps-systems-and-services-connected-to-a-given-server"></a>**Visualiser des applications, des systèmes et des services connectés à un serveur donné**

Lorsque le centre d’administration intègre un serveur dans la solution VM Insights dans Azure Monitor, il s’illumine également d’une fonctionnalité appelée [service Map](https://docs.microsoft.com/azure/azure-monitor/insights/service-map). Cette fonctionnalité Découvre automatiquement les composants de l’application et mappe la communication entre les services afin que vous puissiez visualiser facilement les connexions entre les serveurs avec des détails importants du Portail Azure. Vous pouvez le trouver en accédant au Portail Azure > Azure Monitor > machines virtuelles (sous Insights) et en accédant à l’onglet « Maps » (cartes).

> [!NOTE]
> Les visualisations de Virtual Machines Insights pour Azure Monitor sont actuellement proposées dans 6 régions publiques.  Pour obtenir les informations les plus récentes, consultez la [documentation de Azure Monitor pour machines virtuelles](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-onboard#log-analytics).  Vous devez déployer l’espace de travail Log Analytics dans l’une des régions prises en charge pour bénéficier des autres avantages offerts par la solution machines virtuelles décrite ci-dessus.

## <a name="disabling-monitoring"></a>Désactivation de l’analyse

Pour déconnecter complètement votre serveur de l’espace de travail Log Analytics, désinstallez l’agent MMA. Cela signifie que ce serveur n’enverra plus de données à l’espace de travail, et que toutes les solutions installées dans cet espace de travail ne collectent et ne traitent plus les données de ce serveur. Toutefois, cela n’affecte pas l’espace de travail lui-même. toutes les ressources qui sont reportées à cet espace de travail continueront à le faire. Pour désinstaller l’agent MMA dans WAC, accédez à applications & fonctionnalités, recherchez le Microsoft Monitoring Agent, puis cliquez sur désinstaller.

Si vous souhaitez désactiver une solution spécifique au sein d’un espace de travail, vous devez [Supprimer la solution de surveillance de la portail Azure](https://docs.microsoft.com/azure/azure-monitor/insights/solutions#remove-a-management-solution). La suppression d’une solution de surveillance signifie que les Insights créées par cette solution ne seront plus générées pour l' _un_ des serveurs Reporting à cet espace de travail. Par exemple, si je désinstalle la solution Azure Monitor pour machines virtuelles, je ne vois plus d’informations sur les performances des machines VIRTUELles ou des serveurs de l’une des machines connectées à mon espace de travail.