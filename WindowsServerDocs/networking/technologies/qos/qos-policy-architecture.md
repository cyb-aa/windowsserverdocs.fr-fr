---
title: Architecture de la stratégie de QoS
description: Cette rubrique fournit une vue d’ensemble de la stratégie de qualité de Service (QoS), qui vous permet d’utiliser la stratégie de groupe pour hiérarchiser le trafic bande passante du réseau des applications spécifiques et des services dans Windows Server2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 25097cb8-b9b1-41c9-b3c7-3610a032e0d8
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 00d36604c57add6bf9f45b0166b08c1fb15be467
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="qos-policy-architecture"></a>Architecture de la stratégie de QoS

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette rubrique pour en savoir plus sur l’architecture de la stratégie de QoS.

La figure suivante illustre l’architecture de QoS basée sur la stratégie.

![Architecture de la stratégie de QoS](../../media/QoS/QoS-Policy-Architecture.jpg)

L’architecture de QoS basée sur la stratégie comprend les composants suivants:

- **Service Client stratégie de groupe**. Un service Windows qui gère les paramètres de stratégie de groupe configuration ordinateur et utilisateur.

- **Moteur de stratégie de groupe**. Un composant du Service Client stratégie de groupe qui Récupère les paramètres de stratégie de groupe configuration ordinateur et utilisateur à partir d’ActiveDirectory lors du démarrage et régulièrement vérifie les modifications apportées \ (par défaut, chaque minutes\ 90). Si les modifications sont détectées, le moteur de stratégie de groupe récupère les nouveaux paramètres de stratégie de groupe. Le moteur de stratégie de groupe traite les objets de stratégie de groupe entrantes et informe l’Extension côté Client de la qualité de service lorsque les stratégies de qualité de service sont mis à jour.

- **Extension côté Client de la qualité de service**. Un composant du Service Client stratégie de groupe qui attend une indication à partir du moteur de stratégie de groupe qui les stratégies de qualité de service ont été modifiés et informe le Module d’Inspection de la qualité de service.

- **Pile TCP/IP**. La pile TCP/IP qui inclut la prise en charge intégrée pour IPv4 et IPv6 et prend en charge de la plateforme de filtrage Windows. 

- **Qualité de service Inspection**. Composant d’un module dans la pile TCP/IP qui attend des indications concernant des modifications de stratégie de QoS à partir de l’Extension côté Client de la qualité de service, récupère les paramètres de stratégie de QoS et interagit avec la couche de Transport et Pacer.sys en interne marquer le trafic qui correspond aux stratégies de qualité de service.

- **NDIS 6.x**. Une interface standard entre les pilotes réseau en mode noyau et le système d’exploitation dans les systèmes d’exploitation serveur et Client Windows. NDIS 6.x prend en charge allégées filtres, qui est un modèle de pilote simplifiée pour les pilotes intermédiaires NDIS et les pilotes miniport qui offre de meilleures performances.

- **Fournisseur de qualité de service réseau Interface \(NPI\)**. Une interface pour les pilotes en mode noyau interagir avec Pacer.sys.

- **Pacer.sys**. Un pilote de filtre léger de 6.x NDIS qui contrôle la planification de paquets QoS basée sur la stratégie et pour le trafic d’applications qui utilisent la qualité de service générique \(GQoS\) et contrôle du trafic \(TC\) API. Pacer.sys remplacé Psched.sys dans Windows Server2003 et WindowsXP. Pacer.sys est installé avec le composant Planificateur de paquets QoS à partir des propriétés d’une connexion réseau ou une carte.

Pour la rubrique suivante dans ce guide, voir [scénarios de stratégie de QoS](qos-policy-scenarios.md).

Pour la première rubrique de ce guide, voir [stratégie de qualité de Service (QoS)](qos-policy-top.md).

