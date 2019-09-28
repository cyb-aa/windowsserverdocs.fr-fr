---
title: Configurer des stratégies de requête de connexion
description: Cette rubrique fournit des informations sur la configuration des stratégies de demande de connexion dans le serveur NPS (Network Policy Server) dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: f62c6a67-4dda-47f8-8bdf-9b76c37953e6
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d62beb3106141d4683c957020bc96e4a7dfb306f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405479"
---
# <a name="configure-connection-request-policies"></a>Configurer des stratégies de requête de connexion

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour créer et configurer des stratégies de demande de connexion qui désignent si le serveur NPS local traite les demandes de connexion ou les transfère au serveur RADIUS distant pour traitement.

Les stratégies de demande de connexion sont des ensembles de conditions et de paramètres qui permettent aux administrateurs réseau de désigner les serveurs protocole RADIUS (Remote Authentication Dial-In User Service) (RADIUS) qui effectuent l’authentification et l’autorisation des demandes de connexion que le le serveur exécutant le serveur de stratégie réseau \(NPS @ no__t-1 reçoit des clients RADIUS.

La stratégie de demande de connexion par défaut utilise NPS en tant que serveur RADIUS et traite toutes les demandes d’authentification localement.

Pour configurer un serveur exécutant NPS en tant que proxy RADIUS et transférer les demandes de connexion à d’autres serveurs NPS ou RADIUS, vous devez configurer un groupe de serveurs RADIUS distants en plus de l’ajout d’une nouvelle stratégie de demande de connexion qui spécifie les conditions et les paramètres qui les demandes de connexion doivent correspondre.

Vous pouvez créer un groupe de serveurs RADIUS distants lorsque vous créez une stratégie de demande de connexion à l’aide de l’Assistant Nouvelle stratégie de demande de connexion.

Si vous ne souhaitez pas que le serveur NPS agisse en tant que serveur RADIUS et traite les demandes de connexion en local, vous pouvez supprimer la stratégie de demande de connexion par défaut.

Si vous souhaitez que le serveur NPS agisse à la fois en tant que serveur RADIUS, en traitant les demandes de connexion localement et en tant que proxy RADIUS, en transférant certaines demandes de connexion à un groupe de serveurs RADIUS distants, ajoutez une nouvelle stratégie à l’aide de la procédure suivante, puis vérifiez que la valeur par défaut la stratégie de demande de connexion correspond à la dernière stratégie traitée en la plaçant en dernier dans la liste des stratégies.

## <a name="add-a-connection-request-policy"></a>Ajouter une stratégie de demande de connexion

L'appartenance au groupe **Admins du domaine**, ou équivalent, est la condition minimale requise pour effectuer cette procédure.

### <a name="to-add-a-new-connection-request-policy"></a>Pour ajouter une nouvelle stratégie de demande de connexion 

1. Dans Gestionnaire de serveur, cliquez sur **Outils**, puis sur **serveur de stratégie réseau** pour ouvrir la console NPS. 
2. Dans l’arborescence de la console, double-cliquez sur **stratégies**.
3. Cliquez avec le bouton droit sur **stratégies de demande de connexion**, puis cliquez sur **nouvelle stratégie de demande de connexion**.
4. Utilisez l’Assistant Nouvelle stratégie de demande de connexion pour configurer votre stratégie de demande de connexion et, si elle n’a pas été configurée, un groupe de serveurs RADIUS distants.


Pour plus d’informations sur la gestion de NPS, consultez [gérer un serveur de stratégie réseau](nps-manage-top.md).

Pour plus d’informations sur NPS, consultez [serveur NPS (Network Policy Server)](nps-top.md).

