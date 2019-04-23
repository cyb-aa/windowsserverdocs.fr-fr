---
title: Groupes de serveurs RADIUS distants
description: Cette rubrique fournit une vue d’ensemble du réseau stratégie serveur RADIUS groupes de serveurs distants dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d81678a7-be21-48f2-9b3f-5a75d6aef013
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9912927a7b75e4c9f04aa3d24eb7ed46c73a7dd2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855260"
---
# <a name="remote-radius-server-groups"></a>Groupes de serveurs RADIUS distants

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Lorsque vous configurez le serveur NPS (Network Policy Server) comme un proxy d’authentification Dial-In Service RADIUS (Remote User), vous utilisez NPS pour transférer les demandes de connexion pour les serveurs RADIUS qui sont capables de traiter les demandes de connexion, car ils peuvent effectuer authentification et autorisation dans le domaine où se trouve le compte d’utilisateur ou ordinateur. Par exemple, si vous souhaitez transférer les demandes de connexion à un ou plusieurs serveurs RADIUS dans des domaines non approuvés, vous pouvez configurer NPS en tant que proxy RADIUS pour transférer les demandes aux serveurs RADIUS distants dans le domaine non approuvé.

>[!NOTE]
>Les groupes de serveurs RADIUS distants sont pas liées à et distinct à partir de groupes de Windows.

Pour configurer NPS en tant que proxy RADIUS, vous devez créer une stratégie de demande de connexion qui contient toutes les informations requises pour le serveur NPS pour évaluer les messages à transférer et où envoyer les messages.

Lorsque vous configurez un groupe de serveurs RADIUS distants dans NPS, et vous configurez une stratégie de demande de connexion avec le groupe, vous désignez l’emplacement où NPS doit transférer les demandes de connexion.

## <a name="configuring-radius-servers-for-a-group"></a>Configuration des serveurs RADIUS pour un groupe

Un groupe de serveurs RADIUS distants est un groupe nommé qui contient un ou plusieurs serveurs RADIUS. Si vous configurez plusieurs serveurs, vous pouvez spécifier des paramètres soit pour déterminer l’ordre dans lequel les serveurs sont utilisés par le proxy ou à distribuer le flux de messages RADIUS entre tous les serveurs du groupe pour éviter de surcharger un ou plusieurs serveurs d’équilibrage de charge avec trop de demandes de connexion.

Chaque serveur dans le groupe possède les paramètres suivants.

- **Nom ou l’adresse**. Chaque membre du groupe doit avoir un nom unique au sein du groupe. Le nom peut être une adresse IP ou un nom qui peut être résolu à son adresse IP.

- **L’authentification et comptabilité**. Vous pouvez transférer les demandes d’authentification, les demandes de gestion ou les deux à chaque membre du groupe de serveurs RADIUS distants.

- **L’équilibrage de charge**. Un paramètre de priorité est utilisé pour indiquer quel membre du groupe est le serveur principal (la priorité est définie sur 1). Pour les membres du groupe qui ont la même priorité, un paramètre de poids est utilisé pour calculer la fréquence à laquelle les messages RADIUS sont envoyés à chaque serveur. Vous pouvez utiliser des paramètres supplémentaires pour configurer la manière dont le serveur NPS détecte lorsqu’un membre du groupe tout d’abord devient indisponible et lorsqu’elle sera disponible une fois qu’il a été déterminé pour ne pas être disponibles.

Une fois que vous avez configuré un groupe de serveurs RADIUS distants, vous pouvez spécifier le groupe dans l’authentification et les paramètres de gestion d’une stratégie de demande de connexion. Pour cette raison, vous pouvez configurer tout d’abord un groupe de serveurs RADIUS distants. Ensuite, vous pouvez configurer la stratégie de demande de connexion pour utiliser le groupe de serveurs RADIUS distant qui vient d’être configuré. Vous pouvez également utiliser l’Assistant Nouvelle stratégie de demande de connexion pour créer un nouveau groupe de serveurs RADIUS distant lors de la création de la stratégie de demande de connexion.

Pour plus d’informations sur le serveur NPS, consultez [serveur NPS (Network Policy Server)](nps-top.md).
