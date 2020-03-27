---
title: Groupes de serveurs RADIUS distants
description: Cette rubrique fournit une vue d’ensemble des groupes de serveurs RADIUS distants du serveur de stratégie réseau dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: d81678a7-be21-48f2-9b3f-5a75d6aef013
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 8275c3e8902ed78d77d01a2ff5d769d3e99abf97
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80316177"
---
# <a name="remote-radius-server-groups"></a>Groupes de serveurs RADIUS distants

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Quand vous configurez le serveur NPS (Network Policy Server) en tant que proxy protocole RADIUS (Remote Authentication Dial-In User Service) (RADIUS), vous utilisez NPS pour transférer les demandes de connexion aux serveurs RADIUS capables de traiter les demandes de connexion, car ils peuvent effectuer les opérations suivantes : authentification et autorisation dans le domaine où se trouve le compte d’utilisateur ou d’ordinateur. Par exemple, si vous souhaitez transférer des demandes de connexion à un ou plusieurs serveurs RADIUS dans des domaines non approuvés, vous pouvez configurer NPS en tant que proxy RADIUS pour transférer les demandes aux serveurs RADIUS distants dans le domaine non approuvé.

>[!NOTE]
>Les groupes de serveurs RADIUS distants sont indépendants et séparés des groupes Windows.

Pour configurer NPS en tant que proxy RADIUS, vous devez créer une stratégie de demande de connexion qui contient toutes les informations requises pour que le serveur NPS évalue les messages à transférer et l’emplacement d’envoi des messages.

Quand vous configurez un groupe de serveurs RADIUS distants dans NPS et que vous configurez une stratégie de demande de connexion avec le groupe, vous désignez l’emplacement où NPS doit transférer les demandes de connexion.

## <a name="configuring-radius-servers-for-a-group"></a>Configuration des serveurs RADIUS pour un groupe

Un groupe de serveurs RADIUS distants est un groupe nommé qui contient un ou plusieurs serveurs RADIUS. Si vous configurez plusieurs serveurs, vous pouvez spécifier des paramètres d’équilibrage de charge pour déterminer l’ordre dans lequel les serveurs sont utilisés par le proxy ou pour distribuer le workflow des messages RADIUS sur tous les serveurs du groupe pour empêcher la surcharge d’un ou de plusieurs serveurs. avec un trop grand nombre de demandes de connexion.

Chaque serveur du groupe a les paramètres suivants.

- **Nom ou adresse**. Chaque membre du groupe doit avoir un nom unique au sein du groupe. Le nom peut être une adresse IP ou un nom qui peut être résolu en son adresse IP.

- **L’authentification et la gestion des comptes**. Vous pouvez transférer les demandes d’authentification, les demandes de gestion des comptes, ou les deux à chaque membre du groupe de serveurs RADIUS distants.

- **Équilibrage de charge**. Un paramètre de priorité est utilisé pour indiquer le membre du groupe qui est le serveur principal (la priorité est définie sur 1). Pour les membres du groupe qui ont la même priorité, un paramètre de pondération est utilisé pour calculer la fréquence à laquelle les messages RADIUS sont envoyés à chaque serveur. Vous pouvez utiliser des paramètres supplémentaires pour configurer la façon dont le serveur NPS détecte lorsqu’un membre du groupe est d’abord indisponible et lorsqu’il est disponible après avoir été déterminé comme étant indisponible.

Une fois que vous avez configuré un groupe de serveurs RADIUS distants, vous pouvez spécifier le groupe dans les paramètres d’authentification et de gestion des comptes d’une stratégie de demande de connexion. Pour cette raison, vous pouvez d’abord configurer un groupe de serveurs RADIUS distants. Ensuite, vous pouvez configurer la stratégie de demande de connexion pour utiliser le groupe de serveurs RADIUS distants que vous venez de configurer. Vous pouvez également utiliser l’Assistant Nouvelle stratégie de demande de connexion pour créer un groupe de serveurs RADIUS distants pendant que vous créez la stratégie de demande de connexion.

Pour plus d’informations sur NPS, consultez [serveur NPS (Network Policy Server)](nps-top.md).
