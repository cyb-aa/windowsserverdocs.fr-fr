---
title: Fonctionnement de la stratégie QoS
description: Cette rubrique fournit une vue d’ensemble de la stratégie de qualité de service (QoS), qui vous permet d’utiliser stratégie de groupe pour hiérarchiser la bande passante du trafic réseau d’applications et de services spécifiques dans Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 25097cb8-b9b1-41c9-b3c7-3610a032e0d8
manager: brianlic
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 65d2635744e7d49e45d8f878be9a5e734dea9e6d
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80315404"
---
# <a name="how-qos-policy-works"></a>Fonctionnement de la stratégie QoS

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Lors du démarrage ou de l’obtention de la configuration de l’utilisateur ou de l’ordinateur mis à jour stratégie de groupe paramètres pour QoS, le processus suivant se produit.

1. Le moteur de stratégie de groupe récupère les paramètres de la configuration de l’utilisateur ou de l’ordinateur stratégie de groupe à partir de Active Directory.

2. Le moteur de stratégie de groupe informe l’extension de QoS côté client qu’il y a eu des modifications dans les stratégies de QoS.

3. L’extension côté client QoS envoie une notification d’événement de stratégie de qualité de service au module d’inspection QoS.

4. Le module d’inspection QoS récupère les stratégies QoS utilisateur ou ordinateur et les stocke.

Lorsqu’un nouveau point de terminaison de couche de transport \(la connexion TCP ou le\) de trafic UDP est créé, le processus suivant se produit.

1. Le composant couche transport de la pile TCP/IP informe le module d’inspection QoS.

2. Le module d’inspection de la qualité de service compare les paramètres du point de terminaison de la couche de transport aux stratégies de QoS stockées.

3. Si une correspondance est trouvée, le module d’inspection QoS contacte Pacer. sys pour créer un flux, une structure de données contenant la valeur DSCP et les paramètres de limitation du trafic de la stratégie QoS correspondante. S’il existe plusieurs stratégies de QoS qui correspondent aux paramètres du point de terminaison de la couche de transport, la stratégie QoS la plus spécifique est utilisée.

4. Pacer. sys stocke le Flow et retourne un numéro de canal correspondant au Flow dans le module d’inspection de la qualité de service (QoS).

5. Le module d’inspection de la qualité de service renvoie le numéro de Flow à la couche de transport.

6. La couche de transport stocke le numéro de Flow avec le point de terminaison de la couche de transport.

Lorsqu’un paquet correspondant à un point de terminaison de couche de transport marqué avec un numéro de workflow est envoyé, le processus suivant se produit.

1. La couche de transport marque en interne le paquet avec le numéro de Flow.

2. La couche réseau interroge Pacer. sys pour obtenir la valeur DSCP correspondant au numéro de flow du paquet.

3. Pacer. sys renvoie la valeur DSCP à la couche réseau.

4. La couche réseau remplace le champ IPv4 TOS ou la classe de trafic IPv6 par la valeur DSCP spécifiée par Pacer. sys et, pour les paquets IPv4, calcule la somme de contrôle de l’en-tête IPv4 finale.

5. La couche réseau transmet le paquet à la couche de tramage.

6. Étant donné que le paquet a été marqué avec un numéro de Flow, la couche de tramage transmet le paquet à Pacer. sys via NDIS 6. x.

7. Pacer. sys utilise le numéro de flow du paquet pour déterminer si le paquet doit être limité et, le cas échéant, planifie le paquet pour l’envoi.

8. Pacer. sys transmet le paquet immédiatement \(s’il n’y a pas de limitation du trafic\) ou planifiée \(s’il existe une limitation du trafic\) à NDIS 6. x pour la transmission sur la carte réseau appropriée.

Ces processus de QoS basée sur la stratégie offrent les avantages suivants.

- L’inspection du trafic pour déterminer si une stratégie de QoS s’applique est effectuée par point de terminaison de la couche transport, plutôt que par paquet.

- Il n’y a aucun impact sur les performances pour le trafic qui ne correspond pas à une stratégie QoS.

- Les applications n’ont pas besoin d’être modifiées pour tirer parti de la limitation du trafic ou du service différencié basé sur DSCP.

- Les stratégies de QoS peuvent s’appliquer au trafic protégé par IPsec.

Pour la rubrique suivante de ce guide, consultez [architecture de la stratégie QoS](qos-policy-architecture.md).

Pour obtenir la première rubrique de ce guide, consultez [stratégie de qualité de service (QoS)](qos-policy-top.md).
