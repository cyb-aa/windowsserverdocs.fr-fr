---
title: Fonctionnement de la stratégie de QoS
description: Cette rubrique fournit une vue d’ensemble de la stratégie de qualité de Service (QoS), qui vous permet d’utiliser la stratégie de groupe pour classer par priorité de la bande passante du trafic réseau des applications spécifiques et des services dans Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 25097cb8-b9b1-41c9-b3c7-3610a032e0d8
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 272272c833bb38924f1daa5561037901f6ff4e25
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59864280"
---
# <a name="how-qos-policy-works"></a>Fonctionnement de la stratégie de QoS

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Lorsque le démarrage ou l’obtention d’utilisateur mis à jour ou les paramètres de stratégie de groupe configuration ordinateur pour la qualité de service, le processus suivant se produit.

1. Le moteur de stratégie de groupe récupère les paramètres de stratégie de groupe configuration utilisateur ou un ordinateur à partir d’Active Directory.

2. Le moteur de stratégie de groupe informe l’Extension côté Client de qualité de service qu’il y avait des modifications dans les stratégies de QoS.

3. L’Extension côté Client de qualité de service envoie une notification d’événement de stratégie QoS pour le Module d’Inspection de qualité de service.

4. Le Module d’Inspection de qualité de service récupère les stratégies de QoS utilisateur ou d’ordinateur et les stocke.

Quand un nouveau point de terminaison de couche de Transport \(TCP connexion ou le trafic UDP\) est créé, le processus suivant se produit.

1. Le composant de couche de Transport de la pile TCP/IP informe le Module d’Inspection de qualité de service.

2. Le Module d’Inspection de QoS compare les paramètres du point de terminaison de couche de Transport pour les stratégies de QoS stockées.

3. Si une correspondance est trouvée, le Module d’Inspection de QoS contacte Pacer.sys pour créer un flux, une structure de données contenant la valeur DSCP et le trafic de la limitation des paramètres de la stratégie de qualité de service correspondante. S’il existe plusieurs stratégies de QoS qui correspondent aux paramètres du point de terminaison de couche de Transport, la stratégie de QoS plus spécifique est utilisée.

4. Pacer.sys stocke le flux et retourne un nombre de flux correspondant au flux pour le Module d’Inspection de qualité de service.

5. Le Module d’Inspection de qualité de service retourne le nombre de flux à la couche de Transport.

6. La couche de Transport stocke le nombre de flux avec le point de terminaison de couche de Transport.

Lorsqu’un paquet correspondant à un point de terminaison de couche de Transport marquée avec un nombre de flux est envoyée, le processus suivant se produit.

1. En interne, la couche de Transport marque le paquet avec le nombre de flux.

2. La couche réseau interroge Pacer.sys pour la valeur DSCP correspondant au nombre de flux du paquet.

3. Pacer.sys retourne la valeur DSCP pour la couche réseau.

4. La couche réseau modifie le champ TOS IPv4 ou le champ classe de trafic IPv6 à la valeur DSCP spécifiée par Pacer.sys et, pour les paquets IPv4, calcule la somme de contrôle en-tête IPv4 finale.

5. La couche réseau transmet le paquet à la couche de tramage.

6. Étant donné que le paquet a été marqué avec un nombre de flux, la couche de tramage transmet le paquet à Pacer.sys via NDIS 6.x.

7. Pacer.sys utilise le nombre de flux de paquet pour déterminer si le paquet doit être limitée et cas dans ce, il planifie le paquet pour l’envoi.

8. Pacer.sys transmet le paquet la soit immédiatement \(si aucun trafic n’est la limitation\) ou comme prévu \(de trafic limitation\) pour NDIS 6.x pour la transmission via la carte réseau appropriée.

Ces processus de QoS basée sur des stratégies offrent les avantages suivants.

- L’inspection du trafic pour déterminer si une stratégie de QoS s’applique s’effectue de point de terminaison de couche Transport, plutôt que par paquet.

- Il n’a aucun impact sur les performances pour le trafic qui ne correspond pas à une stratégie de QoS.

- Les applications n’avez pas besoin être modifié pour tirer parti de service différencié DSCP ou la limitation du trafic.

- Les stratégies QoS peuvent s’appliquent au trafic protégé avec IPsec.

Pour la rubrique suivante de ce guide, consultez [Architecture de stratégie de QoS](qos-policy-architecture.md).

Pour la première rubrique de ce guide, consultez [stratégie de qualité de Service (QoS)](qos-policy-top.md).
