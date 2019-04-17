---
title: Surveiller les serveurs et de configurer des alertes avec le moniteur Azure à partir de Windows Admin Center
description: Windows Admin Center (projet Honolulu) s’intègre avec le moniteur Azure
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 03/24/2019
ms.openlocfilehash: 6ada708bf7dd8cd08e1bc2620be5244a07beac7d
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296933"
---
# Surveiller les serveurs et de configurer des alertes avec le moniteur Azure à partir de Windows Admin Center

[En savoir plus sur l’intégration d’Azure avec Windows Admin Center.](../plan/azure-integration-options.md)

[Moniteur Azure](https://docs.microsoft.com/azure/azure-monitor/overview) est une solution qui collecte, analyse et agit sur les données de télémétrie à partir d’une variété de ressources, y compris les serveurs Windows et les ordinateurs virtuels, à la fois sur site et dans le cloud. Bien que le moniteur Azure extrait des données à partir d’ordinateurs virtuels Azure et d’autres ressources Azure, cet article se concentre sur le moniteur Azure fonctionnement avec des serveurs locaux et les ordinateurs virtuels, en particulier avec Windows Admin Center. Si vous avez besoin savoir comment vous pouvez utiliser le moniteur Azure pour obtenir les alertes par courrier électronique concernant votre cluster hyperconvergé, lire sur [l’utilisation de moniteur Azure pour envoyer des e-mails pour les pannes de Service d’intégrité](https://docs.microsoft.com/windows-server/storage/storage-spaces/configure-azure-monitor).

## Comment fonctionne le moniteur Azure?
![IMG](../media/azure-monitor-diagram.png) générées à partir de serveurs de Windows en local les données sont collectées dans un espace de travail Analytique du journal dans le moniteur Azure. Au sein d’un espace de travail, vous pouvez activer différentes solutions de surveillance: définit de logique qui fournissent des informations sur un scénario spécifique. Par exemple, la gestion des mises à jour Azure, Azure Security Center et le moniteur Azure pour les ordinateurs virtuels sont toutes les solutions de surveillance qui peuvent être activées au sein d’un espace de travail. 

Lorsque vous activez une solution d’analyse dans un espace de travail Analytique du journal, tous les serveurs de création de rapports à cet espace de travail démarrera la collecte de données pertinentes pour que cette solution, afin que la solution peut générer des informations pour tous les serveurs dans l’espace de travail. 

Pour collecter des données de télémétrie sur un serveur local et il renvoyer à l’espace de travail Analytique du journal, surveiller Azure nécessite l’installation de l’Agent mma ou l’agent MMA. Certaines solutions de surveillance requièrent également un agent secondaire. Par exemple, moniteur Azure pour les ordinateurs virtuels varie également selon un agent ServiceMap pour que cette solution offre des fonctionnalités supplémentaires. 

Certaines solutions, comme la gestion de mise à jour Azure, dépendent également Automation Azure, ce qui vous permet de gérer les ressources de manière centralisée sur Azure et les environnements non Azure. Par exemple, gestion des mises à jour Azure utilise Azure Automation pour planifier et orchestrer l’installation des mises à jour sur les ordinateurs de votre environnement, de manière centralisée, à partir du portail Azure.


## Comment Windows Admin Center vous permette d’utiliser le moniteur Azure?

À partir de WAC, vous pouvez activer à deux solutions d’analyse:

- [Mettre à jour Azure Gestion](azure-update-management.md) (dans l’outil de mises à jour)
- Le moniteur Azure pour les ordinateurs virtuels (dans les paramètres de serveur), ou des informations de Machines virtuelles

Vous pouvez commencer à l’aide de moniteur Azure à partir d’un de ces outils. Si vous n’avez jamais utilisé moniteur d’Azure avant, WAC configurera automatiquement un espace de travail de journal Analytique (et un compte Azure Automation, si nécessaire) et installer et configurer Microsoft Monitoring Agent (MMA) sur le serveur cible. Il va ensuite installer la solution correspondante dans l’espace de travail. 

Par exemple, si vous passez tout d’abord à l’outil de mises à jour pour configurer la gestion des mises à jour Azure, WAC sera:

1. Installer l’agent MMA sur l’ordinateur
2. Créer l’espace de travail Analytique du journal et le compte Azure Automation (dans la mesure où un compte Azure Automation est nécessaire dans ce cas)
3. Installer la solution de gestion de la mise à jour dans l’espace de travail nouvellement créé.

Si vous souhaitez ajouter une autre solution de surveillance au sein de WAC sur le même serveur, WAC installera simplement cette solution dans l’espace de travail existant auquel ce serveur est connecté. WAC installera également tous les autres agents nécessaires.

Si vous vous connecter à un autre serveur, mais que vous avez déjà configuré un espace de travail de journal Analytique (soit par le biais de WAC, soit manuellement dans le portail Azure), vous pouvez également installer l’agent MMA sur le serveur et le connecter jusqu'à un espace de travail existant. Lorsque vous connectez un serveur dans un espace de travail, il démarre automatiquement collecte de données et solutions installées dans cet espace de travail de rapports.

## Surveiller les Azure pour les ordinateurs virtuels (également appelée) Informations de la Machine virtuelle)
>S’applique à: Windows Admin Center Preview

Lorsque vous configurez Azure moniteur pour les ordinateurs virtuels dans les paramètres de serveur, Windows Admin Center permet le moniteur Azure pour la solution de machines virtuelles, également connue sous les informations de Machine virtuelle. Cette solution vous permet de surveiller les événements et l’intégrité du serveur, créer des alertes par courrier électronique, obtenir une vue consolidée des performances du serveur au sein de votre environnement et visualiser les applications, les systèmes et les services connectés à un serveur donné.

> [!NOTE]
> Malgré son nom, informations sur une machine virtuelle fonctionne pour les serveurs physiques, ainsi que des machines virtuelles.

Avec du moniteur Azure gratuite 5 Go d’allocation des données/mois/client, vous pouvez facilement tester cela pour un serveur ou deux sans de bloquer l’obtention facturé. Lisez la suite pour voir des avantages supplémentaires des serveurs d’intégration dans Azure moniteur, par exemple, l’obtention d’une vue consolidée des performances des systèmes sur les serveurs dans votre environnement.

### **Configurer votre serveur pour une utilisation avec le moniteur Azure**

À partir de la page de présentation d’une connexion serveur, cliquez sur le nouveau bouton «Gérer les alertes» ou accédez à paramètres du serveur > surveillance et des alertes. Dans cette page, intégrer votre serveur Azure moniteur en cliquant sur «Configurer» et en effectuant le volet du programme d’installation. Centre d’administration s’occupe de l’approvisionnement de l’espace de travail Azure Log Analytique, l’installation de l’agent nécessaire et vérifier que la solution d’informations sur une machine virtuelle est configurée. Une fois terminé, votre serveur envoie les données de compteur de performances à surveiller Azure, ce qui vous permet d’afficher et de créer des alertes par courrier électronique basés sur ce serveur, à partir du portail Azure.

### **Créer des alertes par courrier électronique**

Une fois que votre serveur que vous avez joint à Azure moniteur, vous pouvez utiliser les liens hypertexte intelligents dans la page > surveillance et des alertes pour naviguer vers le portail Azure. Centre d’administration active automatiquement les compteurs de performances pour être collectés, afin que vous pouvez facilement [créer une nouvelle alerte](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log) en personnalisant l’un des nombreux requêtes prédéfinies, ou créé par vos soins.

### ** Obtenir une vue consolidée sur plusieurs serveurs **

Si vous intégré plusieurs serveurs pour un espace de travail Analytique du journal unique au sein de moniteur Azure, vous pouvez obtenir une vue consolidée de tous ces serveurs à partir de la [solution d’informations sur les Machines virtuelles](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-overview) au sein de moniteur Azure.  (Notez que seuls les onglets performances et cartes d’informations de Machines virtuelles pour Azure moniteur fonctionne avec des serveurs locaux – les fonctions d’onglet intégrité uniquement avec les machines virtuelles Azure). Pour l’afficher dans le portail Azure, accédez à Azure moniteur > Machines virtuelles (sous Insights) et accédez à l’onglet «Performances» ou «Maps».

### **Visualiser les applications, les systèmes et les services connectés à un serveur donné**

Lorsque onboards centre d’administration un serveur à la solution insights de machine virtuelle Azure moniteur, il met également en avant une fonctionnalité appelée [Carte de Service](https://docs.microsoft.com/azure/azure-monitor/insights/service-map). Cette fonctionnalité détecte des composants d’application automatiquement et mappe la communication entre les services afin que vous pouvez facilement visualiser les connexions entre les serveurs avec détail à partir du portail Azure. Vous pouvez le rechercher en accédant à la > portail Azure Azure moniteur > Machines virtuelles (sous Insights) et navigation vers l’onglet «Maps».

> [!NOTE]
> Les visualisations pour des renseignements de Machines virtuelles Azure moniteur sont proposées dans les régions publiques 6 actuellement.  Pour obtenir les informations les plus récentes, vérifiez si le [Moniteur Azure pour obtenir une documentation machines virtuelles](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-onboard#log-analytics).  Vous devez déployer l’espace de travail Analytique du journal dans l’une des régions pris en charge pour obtenir les avantages supplémentaires fournies par la solution de Machines virtuelles Insights décrite ci-dessus.

## Désactivation de l’analyse

Pour déconnecter complètement votre serveur à partir de l’espace de travail Analytique du journal, vous pouvez désinstaller l’agent MMA. Cela signifie que ce serveur n’est plus envoie des données à l’espace de travail, et toutes les solutions installées dans cet espace de travail ne seront plus collecter et traitent les données à partir de ce serveur. Toutefois, cela n’affecte pas l’espace de travail lui-même – toutes les ressources de cet espace de travail de rapports continuera à le faire. Pour désinstaller l’agent MMA au sein de WAC, accédez aux fonctionnalités de & des applications, recherchez l’Agent mma, puis cliquez sur Désinstaller.

Si vous souhaitez désactiver une solution spécifique au sein d’un espace de travail, vous devez [Supprimer la solution d’analyse à partir du portail Azure](https://docs.microsoft.com/azure/azure-monitor/insights/solutions#remove-a-management-solution). Suppression d’une solution de surveillance signifie que les informations créées par que solution n’est plus sera générée pour _n’importe quel_ des serveurs et cet espace de travail. Par exemple, si j’ai désinstallez le moniteur Azure pour la solution de machines virtuelles, je ne verrez plus les informations relatives aux performances de machine virtuelle ou un serveur à partir d’un des ordinateurs connectés à mon espace de travail.