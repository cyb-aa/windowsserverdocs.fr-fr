---
title: Afficher les détails et notifications de la tâche
description: Gestionnaire de serveur
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 44fd23b917d08aad663c0d3fb8da4bebef47783e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831510"
---
# <a name="view-task-details-and-notifications"></a>Afficher les détails et notifications de la tâche

>S'applique à : Windows Server 2016

Dans le Gestionnaire de serveur dans Windows Server 2012 R2 ou Windows Server 2012, lorsque vous effectuez des tâches de gestion telles que l’ajout de rôles et fonctionnalités, démarrer des services, l’actualisation des données qui s’affiche dans la console du Gestionnaire de serveur ou la création d’un groupe personnalisé de serveurs, un notification s’affiche dans le **Notifications** zone de l’en-tête de la console Gestionnaire de serveur. Notifications et le **les détails des tâches** boîte de dialogue que vous pouvez ouvrir à partir de la **Notifications** menu en cliquant sur l’icône d’indicateur affichent l’état des demandes ou des tâches de l’utilisateur, vous indiquent si une tâche a échoué et vous aider à résoudre les problèmes en pointant sur les messages d’erreur détaillés sur les échecs de tâche.

## <a name="the-notifications-area"></a>Zone Notifications
Le **Notifications** zone dans la barre de menu Gestionnaire de serveur, marquée par une icône d’indicateur, affiche les résultats des tâches que vous démarrez dans le Gestionnaire de serveur. Les notifications vous indiquent si les tâches que vous avez démarrées dans le Gestionnaire de serveur ont réussi ou échoué. Lorsque les notifications sont consultables, le nombre de notifications disponibles est affiché en regard de l’icône d’indicateur. Si une tâche a échoué, n’est que partiellement terminée (par exemple, si elle n’a pas pu être effectuée sur tous les serveurs distants sur lesquels vous vouliez l’exécuter) ou s’est terminée avec des avertissements, l’indicateur **Notifications** devient rouge. Les tâches pour lesquelles des notifications s’affichent sont notamment les suivantes.

-   Actualisation manuelle des données affichées dans le Gestionnaire de serveur (les notifications sont affichées pour les actualisations automatiques uniquement si l’échec des actualisations)

-   démarrage ou arrêt des services

-   Installation ou désinstallation des rôles, services de rôle et fonctionnalités

-   démarrage d’une analyse Best Practices Analyzer (BPA)

-   Ajout de serveurs distants à gérer (les notifications sont affichées pour les échecs de contacter ou actualiser les données affichées pour les serveurs distants)

Les éléments du menu **Notifications** affichent une barre de progression, une brève description de la tâche, le nom du serveur cible pour la tâche (ou l’un des serveurs cibles, si plusieurs serveurs cibles ont été sélectionnés), un lien vers une boîte de dialogue ou un contrôle associé le cas échéant, et un menu **Tâches**. Le menu **Tâches** affiche des commandes qui s’appliquent à la notification active (la notification sur laquelle la souris pointe). Par exemple, si vous arrêtez un service, vous pouvez cliquer sur **Redémarrer** dans le menu **Tâches** de la notification pour redémarrer le service.

Notifications sont particulièrement utiles pour l’installation ou désinstallation des rôles, services de rôle et fonctionnalités. Par exemple, si vous démarrez l’installation d’une fonctionnalité sur un serveur distant, vous pouvez fermer l’ajouter des rôles et fonctionnalités Assistant pendant que l’installation est toujours en cours, mais la tâche active demeure dans les **Notifications** liste. Le **Notifications** élément affiche une barre de progression pour l’installation et vous permet de rouvrir l’ajouter Assistant rôles et fonctionnalités, si nécessaire, en cliquant sur **Assistant Ajout de rôles et fonctionnalités**. Les éléments de cette liste vous avertissent si une installation a échoué ou si d’autres étapes de configuration sont requises pour terminer le déploiement de la fonctionnalité.

Les notifications jouent également un rôle IMPORTANT dans la résolution des problèmes avec les tâches ou les processus dans le Gestionnaire de serveur. Pour plus d’informations sur la façon d’utiliser des messages dans la **Notifications** zone et **les détails des tâches** boîte de dialogue pour résoudre les tâches ayant échoués ou les processus, consultez le [le Gestionnaire de serveur Guide de dépannage](https://social.technet.microsoft.com/wiki/contents/articles/13443.windows-server-2012-server-manager-troubleshooting-guide-part-i-overview.aspx).

Pour supprimer une notification que vous ne souhaitez plus afficher à partir de la **Notifications** répertorier, pointez votre souris sur la notification et puis cliquez sur **supprimer tâche** (**X**).

## <a name="viewing-and-troubleshooting-tasks-by-using-task-details"></a>Affichage et dépannage des tâches à l’aide des détails de la tâche
Le **les détails des tâches** commande en bas de la **Notifications** menu s’ouvre le **les détails des tâches** boîte de dialogue, qui fournit des descriptions complètes des événements de tâches (démarrage, l’arrêt, avertissements, réussites ou échecs). Comme d’autres contrôles de liste dans le Gestionnaire de serveur, telles que **événements**, **Services**, et **Best Practices Analyzer** vignettes, vous pouvez filtrer et créer des requêtes à exécuter sur les tâches qui sont affichés dans le **les détails des tâches** boîte de dialogue. (pour plus d’informations sur le filtrage et la création de requêtes sur les contrôles de liste, consultez [filtrer, trier et interroger des données dans les vignettes du Gestionnaire de serveur](filter-sort-and-query-data-in-server-manager-tiles.md).) Dans le volet supérieur, vous pouvez passer en revue les notifications qui ont été affichées dans le menu **Notifications** et voir le nombre de notifications qui ont été générées pour la même tâche. sélection d’une notification dans le volet supérieur affiche des détails complets sur la notification dans le volet inférieur.

Le volet inférieur est particulièrement utile pour dépanner les tâches qui ont échoué. Si le Gestionnaire de serveur ne peut pas se connecter à ou obtenir des données pour un serveur qui est membre du pool de serveurs, les entrées de ce volet contiennent souvent des messages détaillés, y compris le texte intégral de sous-jacent de gestion à distance de Windows (WinRM), mise en réseau ou des problèmes de sécurité qui empêcher le Gestionnaire de serveur de communiquer avec un serveur cible.

## <a name="see-also"></a>Voir aussi
[Filtrer, trier et interroger des données dans les vignettes du Gestionnaire de serveur](filter-sort-and-query-data-in-server-manager-tiles.md)
[Guide de résolution des problèmes de gestionnaire de serveur](https://social.technet.microsoft.com/wiki/contents/articles/13443.windows-server-2012-server-manager-troubleshooting-guide-part-i-overview.aspx)
