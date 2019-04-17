---
title: Groupes de serveurs RADIUS distants
description: Cette rubrique fournit une vue d’ensemble du réseau stratégie serveur RADIUS groupes de serveurs distants dans Windows Server2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d81678a7-be21-48f2-9b3f-5a75d6aef013
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1f27b5e501f110a038264cd54d75c8b8f9566a64
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="remote-radius-server-groups"></a>Groupes de serveurs RADIUS distants

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Lorsque vous configurez le serveur NPS (Network Policy) en tant que proxy Authentication Dial-In utilisateur RADIUS (Remote Service), vous utilisez NPS pour transférer les demandes de connexion à des serveurs RADIUS sont capables de traiter les demandes de connexion, car ils peuvent effectuer l’authentification et autorisation dans le domaine où se trouve le compte d’utilisateur ou ordinateur. Par exemple, si vous souhaitez transférer les demandes de connexion à un ou plusieurs serveurs RADIUS dans des domaines non approuvés, vous pouvez configurer NPS en tant que proxy RADIUS pour transférer les demandes aux serveurs RADIUS distants dans le domaine non approuvé.

>[!NOTE]
>Les groupes de serveurs RADIUS distants sont pas liée à et distincts des groupes Windows.

Pour configurer NPS en tant que proxy RADIUS, vous devez créer une stratégie de demande de connexion qui contient toutes les informations requises pour le serveur NPS pour évaluer les messages à transférer et où envoyer les messages.

Lorsque vous configurez un groupe de serveurs RADIUS distants dans NPS, et vous configurez une stratégie de demande de connexion avec le groupe, vous désignez l’emplacement où NPS doit transférer les demandes de connexion.

## <a name="configuring-radius-servers-for-a-group"></a>La configuration des serveurs RADIUS pour un groupe

Un groupe de serveurs RADIUS distants est un groupe nommé qui contient un ou plusieurs serveurs RADIUS. Si vous configurez plusieurs serveurs, vous pouvez spécifier des paramètres soit pour déterminer l’ordre dans lequel les serveurs sont utilisés par le serveur proxy ou pour distribuer le flux des messages RADIUS sur tous les serveurs dans le groupe afin d’éviter la surcharge d’un ou plusieurs serveurs avec un trop grand nombre de demandes de connexion d’équilibrage de charge.

Chaque serveur dans le groupe possède les paramètres suivants.

- **Nom ou l’adresse**. Chaque membre du groupe doit avoir un nom unique au sein du groupe. Le nom peut être une adresse IP ou un nom qui peut être résolu sur son adresse IP.

- **L’authentification et la gestion des comptes**. Vous pouvez transférer les demandes d’authentification, les demandes de gestion ou à la fois pour chaque membre du groupe de serveurs RADIUS distants.

- **L’équilibrage de charge**. Un paramètre de priorité est utilisé pour indiquer les membres du groupe est le serveur principal (la priorité est définie sur 1). Pour les membres du groupe qui ont la même priorité, un paramètre de poids est utilisé pour calculer la fréquence à laquelle les messages RADIUS sont envoyés à chaque serveur. Vous pouvez utiliser des paramètres supplémentaires pour configurer la manière dont le serveur NPS détecte lorsqu’un membre du groupe tout d’abord n’est pas disponible et lorsqu’il est disponible après avoir déterminé ne soit ne pas disponible.

Après avoir configuré un groupe de serveurs RADIUS distants, vous pouvez spécifier le groupe dans l’authentification et les paramètres de gestion d’une stratégie de demande de connexion. Pour cette raison, vous pouvez configurer tout d’abord un groupe de serveurs RADIUS distants. Ensuite, vous pouvez configurer la stratégie de demande de connexion pour utiliser le groupe de serveurs RADIUS distants qui vient d’être configuré. Vous pouvez également utiliser l’Assistant Nouvelle stratégie de demande de connexion pour créer un nouveau groupe de serveurs RADIUS distant pendant que vous créez la stratégie de demande de connexion.

Pour plus d’informations sur le serveur NPS, voir [serveur NPS (Network Policy)](nps-top.md).
