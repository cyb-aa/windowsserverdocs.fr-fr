---
title: Architecture de la stratégie QoS
description: Cette rubrique fournit une vue d’ensemble de la stratégie de qualité de service (QoS), qui vous permet d’utiliser stratégie de groupe pour hiérarchiser la bande passante du trafic réseau d’applications et de services spécifiques dans Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 25097cb8-b9b1-41c9-b3c7-3610a032e0d8
manager: brianlic
ms.author: lizross
author: eross-msft
ms.openlocfilehash: ba64887dd52e614f3b6279042bc4f803bd9a836a
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80315583"
---
# <a name="qos-policy-architecture"></a>Architecture de la stratégie QoS

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour en savoir plus sur l’architecture de la stratégie QoS.

La figure suivante illustre l’architecture de la qualité de service (QoS) basée sur la stratégie.

![Architecture de la stratégie QoS](../../media/QoS/QoS-Policy-Architecture.jpg)

L’architecture de la QoS basée sur la stratégie est constituée des composants suivants :

- **Service Client stratégie de groupe**. Service Windows qui gère les paramètres de configuration des utilisateurs et des ordinateurs stratégie de groupe.

- **Moteur de stratégie de groupe**. Composant du service client stratégie de groupe qui récupère les paramètres de stratégie de groupe de configuration utilisateur et ordinateur à partir d’Active Directory au démarrage et vérifie périodiquement les modifications \(par défaut, toutes les 90 minutes\). Si des modifications sont détectées, le moteur de stratégie de groupe récupère les nouveaux paramètres de stratégie de groupe. Le moteur de stratégie de groupe traite les objets de stratégie de groupe entrants et indique l’extension côté client QoS lorsque les stratégies de QoS sont mises à jour.

- **Extension côté client QoS**. Composant du service client stratégie de groupe qui attend une indication du moteur de stratégie de groupe que les stratégies de QoS ont changé et qui informe le module d’inspection QoS.

- **Pile TCP/IP**. Pile TCP/IP qui comprend la prise en charge intégrée de IPv4 et IPv6 et prend en charge la plateforme de filtrage Windows. 

- **Inspection QoS**. Module un composant dans la pile TCP/IP qui attend les indications des modifications de la stratégie QoS de l’extension côté client QoS, récupère les paramètres de stratégie QoS et interagit avec la couche de transport et Pacer. sys pour marquer en interne le trafic qui correspond aux stratégies de QoS.

- **NDIS 6. x**. Interface standard entre les pilotes réseau en mode noyau et le système d’exploitation dans Windows Server et les systèmes d’exploitation clients. NDIS 6. x prend en charge les filtres légers, qui est un modèle de pilote simplifié pour les pilotes intermédiaires NDIS et les pilotes de miniport qui offrent de meilleures performances.

- **Interface du fournisseur de réseau QoS \(\)NPI** . Interface pour que les pilotes en mode noyau interagissent avec Pacer. sys.

- **Pacer. sys**. Un pilote de filtre léger NDIS 6. x qui contrôle la planification des paquets pour la QoS basée sur la stratégie et pour le trafic des applications qui utilisent la QoS générique \(les\) GQoS et le contrôle du trafic \(les API TC\). Pacer. sys a remplacé Psched. sys dans Windows Server 2003 et Windows XP. Pacer. sys est installé avec le composant planificateur de paquets QoS à partir des propriétés d’une connexion réseau ou d’un adaptateur.

Pour la rubrique suivante de ce guide, consultez [scénarios de stratégie de QoS](qos-policy-scenarios.md).

Pour obtenir la première rubrique de ce guide, consultez [stratégie de qualité de service (QoS)](qos-policy-top.md).

