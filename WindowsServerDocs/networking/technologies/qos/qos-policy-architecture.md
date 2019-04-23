---
title: Architecture de la stratégie de QoS
description: Cette rubrique fournit une vue d’ensemble de la stratégie de qualité de Service (QoS), qui vous permet d’utiliser la stratégie de groupe pour classer par priorité de la bande passante du trafic réseau des applications spécifiques et des services dans Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 25097cb8-b9b1-41c9-b3c7-3610a032e0d8
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bad37ba3558137b02ae495fe8dd9be2c903cdd97
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843140"
---
# <a name="qos-policy-architecture"></a>Architecture de la stratégie de QoS

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour en savoir plus sur l’architecture de la stratégie QoS.

La figure suivante illustre l’architecture de QoS basée sur la stratégie.

![Architecture de stratégie de QoS](../../media/QoS/QoS-Policy-Architecture.jpg)

L’architecture de QoS basée sur la stratégie comprend les composants suivants :

- **Service Client stratégie de groupe**. Un service Windows qui gère les paramètres de stratégie de groupe configuration ordinateur et utilisateur.

- **Moteur de stratégie de groupe**. Recherche d’un composant du Service Client stratégie de groupe qui Récupère les paramètres de stratégie de groupe configuration ordinateur et utilisateur à partir d’Active Directory au démarrage et régulièrement des modifications \(par défaut, toutes les 90 minutes\). Si des modifications sont détectées, le moteur de stratégie de groupe récupère les nouveaux paramètres de stratégie de groupe. Le moteur de stratégie de groupe traite les objets de stratégie de groupe entrantes et informe l’Extension côté Client de qualité de service lorsque les stratégies de QoS sont mis à jour.

- **Extension côté Client de qualité de service**. Un composant du Service Client stratégie de groupe qui attend une indication à partir du moteur de stratégie de groupe qui les stratégies de qualité de service ont été modifiés et informe le Module d’Inspection de qualité de service.

- **Pile TCP/IP**. La pile TCP/IP qui inclut la prise en charge intégrée pour IPv4 et IPv6 et prend en charge de la plateforme de filtrage Windows. 

- **Inspection de la qualité de service**. Composant de module A au sein de la pile TCP/IP qui attend des indications concernant des modifications de stratégie de QoS à partir de l’Extension côté Client de qualité de service, récupère les paramètres de stratégie de QoS et interagit avec la couche de Transport et Pacer.sys pour marquer en interne le trafic qui correspond à la qualité de service stratégies.

- **NDIS 6.x**. Une interface standard entre les pilotes réseau en mode noyau et le système d’exploitation dans les systèmes d’exploitation Windows Server et Client. NDIS 6.x prend en charge légers filtres, qui est un modèle de pilote simplifiée pour les pilotes intermédiaires NDIS et pilotes miniport qui offre de meilleures performances.

- **Interface du fournisseur de QoS réseau \(NPI\)**. Une interface pour les pilotes en mode noyau interagir avec Pacer.sys.

- **Pacer.sys**. Un pilote de filtre léger NDIS 6.x qui contrôle la planification de paquets QoS basée sur des stratégies et pour le trafic des applications qui utilisent les Generic QoS \(GQoS\) et contrôle du trafic \(TC\) API. Pacer.sys remplacé Psched.sys dans Windows Server 2003 et Windows XP. Pacer.sys est installé avec le composant Planificateur de paquets QoS à partir des propriétés d’une connexion réseau ou une carte.

Pour la rubrique suivante de ce guide, consultez [les scénarios de stratégie de QoS](qos-policy-scenarios.md).

Pour la première rubrique de ce guide, consultez [stratégie de qualité de Service (QoS)](qos-policy-top.md).

