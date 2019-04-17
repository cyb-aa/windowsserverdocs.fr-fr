---
title: Fonctionnement de la stratégie de QoS
description: Cette rubrique fournit une vue d’ensemble de la stratégie de qualité de Service (QoS), qui vous permet d’utiliser la stratégie de groupe pour hiérarchiser le trafic bande passante du réseau des applications spécifiques et des services dans Windows Server2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 25097cb8-b9b1-41c9-b3c7-3610a032e0d8
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1073308b5939e648fdcc2006acdce76ecf0331c9
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="how-qos-policy-works"></a>Fonctionnement de la stratégie de QoS

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Lors du démarrage ou obtenir utilisateur mis à jour ou les paramètres de stratégie de groupe configuration ordinateur pour la qualité de service, le processus suivant se produit.

1. Le moteur de stratégie de groupe récupère les paramètres de stratégie de groupe configuration utilisateur ou un ordinateur à partir d’ActiveDirectory.

2. Le moteur de stratégie de groupe informe l’Extension côté Client de la qualité de service qu’il existait des modifications dans les stratégies de qualité de service.

3. L’Extension côté Client de la qualité de service envoie une notification d’événements de stratégie QoS pour le Module d’Inspection de la qualité de service.

4. Le Module d’Inspection QoS récupère les stratégies de QoS utilisateur ou d’ordinateur et les stocke.

Lorsqu’un nouveau point de terminaison de couche de Transport \ (connexion TCP ou UDP traffic\) est créé, le processus suivant se produit.

1. Le composant de la couche de Transport de la pile TCP/IP informe le Module d’Inspection de la qualité de service.

2. Le Module d’Inspection QoS compare les paramètres du point de terminaison couche de Transport dans les stratégies de QoS stockées.

3. Si une correspondance est trouvée, le Module d’Inspection QoS contacte Pacer.sys pour créer un flux, une structure de données contenant la valeur DSCP et le trafic de paramètres de la stratégie de QoS de limitation. S’il existe plusieurs stratégies de QoS qui correspondent aux paramètres du point de terminaison couche de Transport, la stratégie de QoS plus spécifique est utilisée.

4. Pacer.sys stocke le flux et renvoie un nombre de flux correspondant au flux pour le Module d’Inspection de la qualité de service.

5. Le Module d’Inspection QoS renvoie le nombre de flux pour la couche de Transport.

6. La couche de Transport stocke le nombre de flux avec le point de terminaison de couche de Transport.

Quand un paquet correspondant à un point de terminaison de couche de Transport marqués d’un nombre de flux sont envoyé, le processus suivant se produit.

1. En interne, la couche de Transport marque les paquets avec le nombre de flux.

2. La couche réseau interroge Pacer.sys pour la valeur DSCP correspondant au nombre de flux de paquet.

3. Pacer.sys renvoie la valeur DSCP à la couche réseau.

4. La couche réseau remplace la valeur DSCP spécifiée par Pacer.sys champ TOS IPv4 ou classe de trafic IPv6 et, pour les paquets IPv4, calcule la somme de contrôle en-tête IPv4 finale.

5. La couche réseau transmet le paquet à la couche de cadrage.

6. Étant donné que le paquet a été marqué avec un certain nombre de flux, la couche de cadrage transmet le paquet Pacer.sys par le biais de NDIS 6.x.

7. Pacer.sys utilise le nombre de flux de paquet pour déterminer si le paquet doit être limité et si tel est le cas, les calendriers du paquet pour l’envoi.

8. Pacer.sys exercice pratique, le paquet soit immédiatement \ (s’il n’existe aucune throttling\ trafic) ou comme prévu \ (s’il existe throttling\ le trafic) vers NDIS 6.x pour la transmission de la carte réseau appropriés.

Ces processus de QoS basée sur la stratégie offrent les avantages suivants.

- L’inspection du trafic pour déterminer si une stratégie de QoS s’applique s’effectue de point de terminaison de couche Transport, plutôt que par paquet.

- Il n’a aucun impact sur les performances pour le trafic qui ne correspond pas à une stratégie de QoS.

- Applications n’ont pas besoin être modifié pour tirer parti de service différencié DSCP ou la limitation du trafic.

- Stratégies de QoS peuvent appliquer au trafic protégé par IPsec.

Pour la rubrique suivante dans ce guide, voir [Architecture de stratégie de QoS](qos-policy-architecture.md).

Pour la première rubrique de ce guide, voir [stratégie de qualité de Service (QoS)](qos-policy-top.md).
