---
title: Configurer des stratégies de demande de connexion
description: Cette rubrique fournit des informations sur la configuration des stratégies de demande de connexion dans le serveur NPS dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: f62c6a67-4dda-47f8-8bdf-9b76c37953e6
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9677e147bdaea4de71a054cd6c52d81126e005d1
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="configure-connection-request-policies"></a>Configurer des stratégies de demande de connexion

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette rubrique pour créer et configurer des stratégies de demande de connexion qui indiquer si le serveur NPS local traite les demandes de connexion ou les transmet au serveur RADIUS pour le traitement.

Stratégies de demande de connexion sont des ensembles de conditions et les paramètres qui permettent aux administrateurs réseau de désigner les serveurs Authentication Dial-In utilisateur RADIUS (Remote Service) qui effectuent l’authentification et l’autorisation des demandes de connexion que le serveur exécutant le serveur de stratégie réseau \(NPS\) reçoit des clients RADIUS.

La stratégie de demande de connexion par défaut utilise NPS comme serveur RADIUS et traite toutes les demandes d’authentification localement.

Pour configurer un serveur qui exécute NPS pour agir en tant que proxy RADIUS et les demandes de connexion directe à d’autres serveurs NPS ou RADIUS, vous devez configurer un groupe de serveurs RADIUS distant en plus d’ajouter une nouvelle stratégie de demande de connexion qui spécifie les conditions et les paramètres que les demandes de connexion doivent correspondre.

Vous pouvez créer un nouveau groupe de serveurs RADIUS distant pendant la création d’une nouvelle stratégie de demande de connexion avec l’Assistant Nouvelle stratégie de demande de connexion.

Si vous ne souhaitez pas que le serveur NPS qui agissent comme une connexion de processus et le serveur RADIUS demande localement, vous pouvez supprimer la stratégie de demande de connexion par défaut.

Si vous souhaitez que le serveur NPS pour jouer à la fois un serveur RADIUS, le traitement des demandes de connexion localement et en tant que proxy RADIUS, en envoyant des demandes de connexion à un groupe de serveurs RADIUS distants, ajoutez une nouvelle stratégie à l’aide de la procédure suivante, puis vérifiez que la stratégie de demande de connexion par défaut est la dernière stratégie traitée en le plaçant dernière dans la liste des stratégies.

## <a name="add-a-connection-request-policy"></a>Ajouter une stratégie de demande de connexion

L’appartenance au groupe **Admins du domaine**, ou équivalente, est la condition minimale requise pour effectuer cette procédure.

### <a name="to-add-a-new-connection-request-policy"></a>Pour ajouter une nouvelle stratégie de demande de connexion 

1. Dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **serveur NPS** pour ouvrir la console NPS. 
2. Dans l’arborescence de la console, double-cliquez sur **stratégies**.
3. Avec le bouton droit **stratégies de demande de connexion**, puis cliquez sur **nouvelle stratégie de demande de connexion**.
4. Utilisez l’Assistant de stratégie de demande de nouvelle connexion pour configurer votre connexion de stratégie de demande et, si pas précédemment configuré, un groupe de serveurs RADIUS distants.


Pour plus d’informations sur la gestion du serveur NPS, voir [gérer un serveur de stratégie réseau](nps-manage-top.md).

Pour plus d’informations sur le serveur NPS, voir [serveur NPS (Network Policy)](nps-top.md).

