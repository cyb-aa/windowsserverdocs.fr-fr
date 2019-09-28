---
title: Afficher les détails et notifications de la tâche
description: Gestionnaire de serveur
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-server-manager
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 95117407-2dd3-4f9a-841f-4331be3544c3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a3dcbac95e60fce75316f8a4427aef54bdfad15b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383019"
---
# <a name="view-task-details-and-notifications"></a>Afficher les détails et notifications de la tâche

>S'applique à : Windows Server 2016

Dans Gestionnaire de serveur de Windows Server 2012 R2 ou Windows Server 2012, lorsque vous effectuez des tâches de gestion telles que l’ajout de rôles et de fonctionnalités, le démarrage de services, l’actualisation des données affichées dans la console Gestionnaire de serveur ou la création d’un groupe de serveurs personnalisé, la notification s’affiche dans la zone **notifications** de l’en-tête de la console Gestionnaire de serveur. Les notifications et la boîte de dialogue détails de la **tâche** que vous pouvez ouvrir à partir du menu **notifications** en cliquant sur l’icône d’indicateur, afficher l’état des tâches ou des demandes de l’utilisateur, vous indiquer si une tâche a échoué et vous aider à résoudre les problèmes en pointant sur messages d’erreur détaillés concernant les échecs de tâches.

## <a name="the-notifications-area"></a>Zone Notifications
La zone **notifications** de la barre de menus gestionnaire de serveur, marquée par une icône d’indicateur, affiche les résultats des tâches que vous démarrez dans Gestionnaire de serveur. Les notifications vous indiquent si les tâches que vous avez démarrées dans Gestionnaire de serveur ont réussi ou échoué. Lorsque les notifications sont consultables, le nombre de notifications disponibles est affiché en regard de l’icône d’indicateur. Si une tâche a échoué, n’est que partiellement terminée (par exemple, si elle n’a pas pu être effectuée sur tous les serveurs distants sur lesquels vous vouliez l’exécuter) ou s’est terminée avec des avertissements, l’indicateur **Notifications** devient rouge. Les tâches pour lesquelles des notifications s’affichent sont notamment les suivantes.

-   Actualisation manuelle des données affichées dans Gestionnaire de serveur (les notifications sont affichées pour les actualisations automatiques uniquement en cas d’échec des actualisations)

-   démarrage ou arrêt des services

-   Installation ou désinstallation de rôles, de services de rôle et de fonctionnalités

-   démarrage d’une analyse Best Practices Analyzer (BPA)

-   ajout de serveurs distants à gérer (les notifications s’affichent pour les échecs de contact ou d’actualisation des données affichées pour les serveurs distants)

Les éléments du menu **Notifications** affichent une barre de progression, une brève description de la tâche, le nom du serveur cible pour la tâche (ou l’un des serveurs cibles, si plusieurs serveurs cibles ont été sélectionnés), un lien vers une boîte de dialogue ou un contrôle associé le cas échéant, et un menu **Tâches**. Le menu **Tâches** affiche des commandes qui s’appliquent à la notification active (la notification sur laquelle la souris pointe). Par exemple, si vous arrêtez un service, vous pouvez cliquer sur **Redémarrer** dans le menu **Tâches** de la notification pour redémarrer le service.

Les notifications sont particulièrement utiles pour installer ou désinstaller des rôles, des services de rôle et des fonctionnalités. Par exemple, si vous démarrez l’installation d’une fonctionnalité sur un serveur distant, vous pouvez fermer l’Assistant Ajout de rôles et de fonctionnalités pendant que l’installation est toujours en cours, mais la tâche active reste dans la liste **des notifications** . L’élément **notifications** affiche une barre de progression pour l’installation et vous permet de rouvrir l’Assistant Ajout de rôles et de fonctionnalités, si nécessaire, en cliquant sur **Assistant Ajout de rôles et de fonctionnalités**. Les éléments de cette liste vous avertissent si une installation a échoué ou si d’autres étapes de configuration sont requises pour terminer le déploiement de la fonctionnalité.

Les notifications jouent également un rôle IMPORTANT dans la résolution des problèmes liés aux tâches ou aux processus dans Gestionnaire de serveur. Pour plus d’informations sur l’utilisation des messages dans la zone **notifications** et la boîte de dialogue détails de la **tâche** afin de résoudre les problèmes de tâches ou de processus ayant échoué, consultez le [Guide de résolution des problèmes de gestionnaire de serveur](https://social.technet.microsoft.com/wiki/contents/articles/13443.windows-server-2012-server-manager-troubleshooting-guide-part-i-overview.aspx).

Pour supprimer une notification que vous ne voulez plus voir dans la liste **notifications** , pointez sur la notification, puis cliquez sur supprimer la **tâche** (**X**).

## <a name="viewing-and-troubleshooting-tasks-by-using-task-details"></a>Affichage et dépannage des tâches à l’aide des détails des tâches
La commande détails de la **tâche** au bas du menu **notifications** ouvre la boîte de dialogue détails de la **tâche** , qui fournit des descriptions complètes des événements de tâches (démarrage, arrêt, avertissements, réussites ou échecs). Comme les autres contrôles de liste dans Gestionnaire de serveur, tels que les vignettes **événements**, **services**et **Best Practices Analyzer** , vous pouvez filtrer et créer des requêtes à exécuter sur les tâches qui sont affichées dans la boîte de dialogue détails de la **tâche** . (pour plus d’informations sur le filtrage et la création de requêtes sur les contrôles de liste, consultez [Filtrer, trier et interroger des données dans des vignettes gestionnaire de serveur](filter-sort-and-query-data-in-server-manager-tiles.md).) Dans le volet supérieur, vous pouvez passer en revue les notifications qui ont été affichées dans le menu **Notifications** et voir le nombre de notifications qui ont été générées pour la même tâche. la sélection d’une notification dans le volet supérieur affiche des informations complètes sur la notification dans le volet inférieur.

Le volet inférieur est particulièrement utile pour dépanner les tâches qui ont échoué. Si Gestionnaire de serveur ne pouvez pas vous connecter ou obtenir des données pour un serveur qui est membre du pool de serveurs, les entrées de ce volet contiennent souvent des messages détaillés, y compris le texte complet de la gestion à distance de Windows (WinRM) sous-jacente, la mise en réseau ou les problèmes de sécurité qui empêcher Gestionnaire de serveur de communiquer avec un serveur cible.

## <a name="see-also"></a>Voir aussi
[Filtrer, trier et interroger des données dans Gestionnaire de serveur vignettes](filter-sort-and-query-data-in-server-manager-tiles.md)
[Gestionnaire de serveur Guide de résolution des problèmes](https://social.technet.microsoft.com/wiki/contents/articles/13443.windows-server-2012-server-manager-troubleshooting-guide-part-i-overview.aspx)
