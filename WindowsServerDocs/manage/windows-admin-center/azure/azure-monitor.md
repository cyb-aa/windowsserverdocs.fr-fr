---
title: Surveiller les serveurs et de configurer des alertes avec Azure Monitor à partir de Windows Admin Center
description: Windows Admin Center (projet Honolulu) s’intègre avec Azure Monitor
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 03/24/2019
ms.openlocfilehash: 8f7baba465071cc95ab7492037ff25c5cd58219e
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280436"
---
# <a name="monitor-servers-and-configure-alerts-with-azure-monitor-from-windows-admin-center"></a>Surveiller les serveurs et de configurer des alertes avec Azure Monitor à partir de Windows Admin Center

[En savoir plus sur l’intégration d’Azure avec Windows Admin Center.](../plan/azure-integration-options.md)

[Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/overview) est une solution qui collecte, analyse et agit sur les données de télémétrie à partir d’un éventail de ressources, y compris les serveurs Windows et les machines virtuelles, à la fois en local et dans le cloud. Bien que Azure Monitor extrait des données à partir de machines virtuelles Azure et d’autres ressources Azure, cet article se concentre sur le fonctionnement d’Azure Monitor avec les serveurs locaux et les machines virtuelles, en particulier avec Windows Admin Center. Si vous vous intéressez savoir comment vous pouvez utiliser Azure Monitor pour obtenir des alertes par courrier électronique sur votre cluster hyperconvergé, en savoir plus sur [à l’aide d’Azure Monitor pour envoyer des e-mails pour les erreurs de Service de contrôle d’intégrité](https://docs.microsoft.com/windows-server/storage/storage-spaces/configure-azure-monitor).

## <a name="how-does-azure-monitor-work"></a>Comment fonctionne Azure Monitor ?
![IMG](../media/azure-monitor-diagram.png) généré à partir de serveurs de Windows locaux sont collectées dans un espace de travail Analytique de journal dans Azure Monitor. Dans un espace de travail, vous pouvez activer les différentes solutions de surveillance — définit de la logique qui fournissent des analyses pour un scénario particulier. Par exemple, la gestion de mise à jour d’Azure, Azure Security Center et Azure Monitor pour les machines virtuelles sont toutes les solutions de surveillance qui peuvent être activées au sein d’un espace de travail. 

Lorsque vous activez une solution de surveillance dans un espace de travail Analytique de journal, tous les serveurs de rapports à cet espace de travail va commencer à collecter des données pertinentes à cette solution, afin que la solution peut générer des informations pour tous les serveurs dans l’espace de travail. 

Pour collecter des données de télémétrie sur un serveur local et placez-la dans l’espace de travail Analytique de journal, Azure Monitor nécessite l’installation de l’agent Microsoft Monitoring Agent, ou l’agent MMA. Certaines solutions de surveillance nécessitent également un agent secondaire. Par exemple, Azure Monitor pour les machines virtuelles varie également sur un agent ServiceMap pour cette solution fournit des fonctionnalités supplémentaires. 

Certaines solutions, comme la gestion de mise à jour d’Azure, dépendent également Azure Automation, qui vous permet de gérer de manière centralisée des ressources sur Azure et les environnements non-Azure. Par exemple, Azure Update Management utilise Azure Automation pour planifier et orchestrer l’installation des mises à jour sur plusieurs ordinateurs dans votre environnement, de manière centralisée, à partir du portail Azure.


## <a name="how-does-windows-admin-center-enable-you-to-use-azure-monitor"></a>Comment Windows Admin Center vous permette d’utiliser Azure Monitor ?

À partir de WAC, vous pouvez activer à deux solutions de surveillance :

- [Gestion de mise à jour Azure](azure-update-management.md) (dans l’outil de mises à jour)
- Azure Monitor pour les machines virtuelles (dans les paramètres de serveur), appelées Machines virtuelles insights

Vous pouvez commencer à utiliser Azure Monitor à partir d’un de ces outils. Si vous n’avez jamais utilisé Azure Monitor avant, WAC configurera automatiquement un espace de travail Analytique de journal (et un compte Azure Automation, si nécessaire) et installer et configurer Microsoft Monitoring Agent (MMA) sur le serveur cible. Il va ensuite installer la solution correspondante dans l’espace de travail. 

Par exemple, si vous accédez pour la première fois à l’outil de mises à jour pour configurer la gestion de mise à jour d’Azure, WAC sera :

1. Installer l’agent MMA sur la machine
2. Créer l’espace de travail Analytique de journal et le compte Azure Automation (dans la mesure où un compte Azure Automation est nécessaire dans ce cas)
3. Installer la solution de gestion de la mise à jour dans l’espace de travail nouvellement créé.

Si vous souhaitez ajouter une autre solution de surveillance à partir dans WAC sur le même serveur, WAC installera simplement cette solution dans l’espace de travail existant auquel ce serveur est connecté. WAC installe en outre tous les autres agents nécessaires.

Si vous vous connecter à un autre serveur, mais que vous avez déjà configuré un espace de travail Analytique de journal (soit par le biais WAC ou manuellement dans le portail Azure), vous pouvez également installer l’agent MMA sur le serveur et vous connecter à un espace de travail existant. Lorsque vous vous connectez à un serveur dans un espace de travail, il démarre automatiquement la collecte de données et création de rapports pour les solutions installées dans cet espace de travail.

## <a name="azure-monitor-for-virtual-machines-aka-virtual-machine-insights"></a>Azure Monitor pour les ordinateurs virtuels (également appelé) Insights de la Machine virtuelle)
>S'applique à : Windows Admin Center Preview

Lorsque vous configurez Azure Monitor pour les machines virtuelles dans les paramètres de serveur, Windows Admin Center permet l’analyse Azure pour la solution de machines virtuelles, également connu sous les informations de Machine virtuelle. Cette solution vous permet de surveiller les événements et l’intégrité du serveur, créer des alertes par courrier électronique, obtenir une vue consolidée des performances du serveur au sein de votre environnement, et visualiser les applications, systèmes et services connectés à un serveur donné.

> [!NOTE]
> Malgré son nom, les insights de machine virtuelle fonctionne pour les serveurs physiques, ainsi que des machines virtuelles.

Avec Azure Monitor gratuit 5 Go de l’allocation de données/mois/client, vous pouvez facilement tester cela pour un serveur ou deux sans crainte d’être facturés. Lisez la suite pour voir d’autres avantages de l’intégration des serveurs dans Azure Monitor, telles que l’obtention d’une vue consolidée des performances des systèmes entre les serveurs dans votre environnement.

### <a name="set-up-your-server-for-use-with-azure-monitor"></a>**Configurer votre serveur pour une utilisation avec Azure Monitor**

Dans la page Vue d’ensemble d’une connexion serveur, cliquez sur le nouveau bouton « Gérer les alertes », ou accédez aux paramètres de serveur > surveillance et alertes. Dans cette page, intégrer votre serveur à Azure Monitor en cliquant sur « Configurer » et le volet de configuration à la fin. Centre d’administration prend en charge l’approvisionnement de l’espace de travail Analytique des journaux Azure, installation de l’agent nécessaires et en garantissant la solution d’insights de machine virtuelle est configurée. Une fois terminé, votre serveur envoie les données de compteur de performances pour Azure Monitor, ce qui vous permet d’afficher et créer des alertes par courrier électronique en fonction de ce serveur, à partir du portail Azure.

### <a name="create-email-alerts"></a>**Créer des alertes par courrier électronique**

Une fois que vous avez attaché votre serveur à Azure Monitor, vous pouvez utiliser les liens hypertexte intelligents dans les paramètres > page de surveillance et alertes pour accéder au portail Azure. Centre d’administration active automatiquement les compteurs de performance à collecter, vous pouvez donc facilement [créer une nouvelle alerte](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log) en personnalisant l’un de nombreuses requêtes prédéfinies, ou en écrivant votre propre.

### <a name="get-a-consolidated-view-across-multiple-servers-"></a>** Obtenir une vue consolidée sur plusieurs serveurs **

Si vous intégrez plusieurs serveurs à un seul espace de travail Analytique de journal dans Azure Monitor, vous pouvez obtenir une vue consolidée de tous ces serveurs à partir de la [solution Insights de Machines virtuelles](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-overview) dans Azure Monitor.  (Notez que les onglets performances et des mappages d’Insights de Machines virtuelles pour Azure Monitor fonctionne avec des serveurs locaux : les fonctions d’onglet de contrôle d’intégrité uniquement avec les machines virtuelles Azure). Pour les afficher dans le portail Azure, accédez à Azure Monitor > Machines virtuelles (sous Insights) et accédez à l’onglet « Performances » ou « Cartes ».

### <a name="visualize-apps-systems-and-services-connected-to-a-given-server"></a>**Visualiser les applications, systèmes et services connectés à un serveur donné**

Lorsque centre d’administration intègre un serveur dans la solution d’insights de machine virtuelle dans Azure Monitor, il active une fonctionnalité appelée [Service Map](https://docs.microsoft.com/azure/azure-monitor/insights/service-map). Cette fonctionnalité détecte les composants d’application automatiquement et mappe la communication entre les services afin que vous puissiez visualiser facilement les connexions entre les serveurs avec moindres détails à partir du portail Azure. Vous pouvez le trouver en accédant au portail Azure > Azure Monitor > Machines virtuelles (sous Insights) et en accédant à l’onglet « Cartes ».

> [!NOTE]
> Les visualisations pour les Machines virtuelles Insights pour Azure Monitor sont actuellement proposées dans les régions publiques 6.  Pour plus d’informations, consultez le [Azure Monitor pour obtenir une documentation machines virtuelles](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-onboard#log-analytics).  Vous devez déployer l’espace de travail Analytique de journal dans une des régions prises en charge pour obtenir les avantages supplémentaires fournies par la solution de Machines virtuelles Insights décrite ci-dessus.

## <a name="disabling-monitoring"></a>Désactivation de la surveillance

Pour déconnecter complètement votre serveur à partir de l’espace de travail Analytique de journal, désinstallez l’agent MMA. Cela signifie que ce serveur n’envoie plus de données à l’espace de travail, et toutes les solutions installées dans cet espace de travail ne seront plus collecter et traitent les données de ce serveur. Toutefois, cela n’affecte pas l’espace de travail – toutes les ressources associées à cet espace de travail continuera à le faire. Pour désinstaller l’agent MMA dans WAC, accédez à des applications et fonctionnalités, recherchez Microsoft Monitoring Agent, puis cliquez sur Désinstaller.

Si vous souhaitez désactiver une solution spécifique au sein d’un espace de travail, vous devrez [supprimer la solution de surveillance à partir du portail Azure](https://docs.microsoft.com/azure/azure-monitor/insights/solutions#remove-a-management-solution). Suppression d’une solution de surveillance signifie que les informations créées par cette solution n’est plus seront générées pour _n’importe quel_ des serveurs de rapports à cet espace de travail. Par exemple, si la désinstallation de l’analyse Azure pour la solution de machines virtuelles, je ne voyez plus informations sur les performances de machine virtuelle ou serveur à partir de tous les ordinateurs connectés à mon espace de travail.