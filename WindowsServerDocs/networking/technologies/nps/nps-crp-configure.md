---
title: Configurer des stratégies de requête de connexion
description: Cette rubrique fournit des informations sur la façon de configurer des stratégies de demande de connexion dans le serveur NPS dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: f62c6a67-4dda-47f8-8bdf-9b76c37953e6
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4f80f9fb8be0c44cfb5685e5b9cc489282e4961d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884510"
---
# <a name="configure-connection-request-policies"></a>Configurer des stratégies de requête de connexion

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour créer et configurer des stratégies de demande de connexion qui définit si le serveur NPS local traite les demandes de connexion ou les transmet au serveur RADIUS à distance pour le traitement.

Les stratégies de demande de connexion sont des ensembles de conditions et paramètres qui permettent aux administrateurs réseau de désigner les serveurs d’authentification Dial-In Service RADIUS (Remote User), effectuez l’authentification et autorisation de connexion demande que le serveur exécutant le serveur NPS \(NPS\) reçoit des clients RADIUS.

La stratégie de demande de connexion par défaut utilise NPS comme serveur RADIUS et traite toutes les demandes d’authentification localement.

Pour configurer un serveur qui exécute NPS pour agir en tant que proxy RADIUS et les demandes de connexion directe à d’autres serveurs NPS ou RADIUS, vous devez configurer un groupe de serveurs RADIUS distant en plus d’ajouter une nouvelle stratégie de demande de connexion qui spécifie les conditions et les paramètres qui les demandes de connexion doivent correspondre.

Vous pouvez créer un nouveau groupe de serveurs RADIUS distant pendant la création d’une nouvelle stratégie de demande de connexion avec l’Assistant Nouvelle stratégie de demande de connexion.

Si vous ne souhaitez pas que le serveur NPS pour agir comme une connexion de serveur et les processus RADIUS les requêtes localement, vous pouvez supprimer la stratégie de demande de connexion par défaut.

Si vous souhaitez que le serveur NPS pour jouer le rôle à la fois un serveur RADIUS, traitement des demandes de connexion localement et en tant que proxy RADIUS, transfert des demandes de connexion à un groupe de serveurs RADIUS distants, ajoutez une nouvelle stratégie à l’aide de la procédure suivante, puis vérifier que la valeur par défaut stratégie de demande de connexion est la dernière stratégie traitée en plaçant dernière dans la liste des stratégies.

## <a name="add-a-connection-request-policy"></a>Ajouter une stratégie de demande de connexion

L'appartenance au groupe **Admins du domaine**, ou équivalent, est la condition minimale requise pour effectuer cette procédure.

### <a name="to-add-a-new-connection-request-policy"></a>Pour ajouter une nouvelle stratégie de demande de connexion 

1. Dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **Network Policy Server** pour ouvrir la console NPS. 
2. Dans l’arborescence de la console, double-cliquez sur **stratégies**.
3. Avec le bouton droit **stratégies de demande de connexion**, puis cliquez sur **nouvelle stratégie de demande de connexion**.
4. Utilisez l’Assistant de stratégie de demande de nouvelle connexion pour configurer votre connexion demandent une stratégie et, dans le cas contraire précédemment configuré, un groupe de serveurs RADIUS distants.


Pour plus d’informations sur la gestion de serveur NPS, consultez [gérer un serveur de stratégie réseau](nps-manage-top.md).

Pour plus d’informations sur le serveur NPS, consultez [serveur NPS (Network Policy Server)](nps-top.md).

