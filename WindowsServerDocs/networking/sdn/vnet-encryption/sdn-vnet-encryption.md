---
title: Chiffrement du réseau virtuel
description: Cette rubrique fournit des informations sur le chiffrement de réseau virtuel pour le logiciel défini de mise en réseau dans Windows Server
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: 7da0f509-7b02-4a0f-90fb-d97c83a2bc4e
ms.author: pashort
author: grcusanz
ms.openlocfilehash: 425cc1cff8c7241aeec7764b60c89b4586fd323b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="virtual-network-encryption"></a>Chiffrement du réseau virtuel

>S’applique à: Windows Server

Chiffrement de réseau virtuel offre la possibilité pour le trafic réseau virtuel à être cryptées entre les ordinateurs virtuels qui communiquent entre eux au sein des sous-réseaux qui sont marqués comme «Chiffrement activé».

Cette fonctionnalité utilise DTLS Datagram Transport Layer Security () sur le sous-réseau virtuel pour chiffrer les paquets.  DTLS offre une protection contre les écoutes clandestines, falsification et falsification par toute personne ayant accès au réseau physique.

Nécessite de chiffrement réseau virtuel un certificat de chiffrement doivent être installées sur chacun du SDN activé les hôtes Hyper-V, un objet d’informations d’identification dans le contrôleur de réseau faisant référence à l’empreinte numérique de ce certificat et la configuration sur chacun des réseaux virtuels qui contiennent des sous-réseaux exigence du chiffrement.

Une fois que le chiffrement est activé sur un sous-réseau, tout le trafic réseau au sein de ce sous-réseau est chiffré automatiquement.  Il s’agit en plus de n’importe quel cryptage au niveau de l’application qui peut également avoir lieu.  Le trafic qui coupe entre les sous-réseaux, même si les deux sous-réseaux sont marqués comme chiffré est automatiquement envoyé non chiffré.  Tout le trafic qui traverse la limite de réseau virtuel est également envoyé non chiffré.

Pour plus d’informations sur la configuration de chiffrement de réseau virtuel, consultez [configurer le chiffrement d’un réseau virtuel](sdn-config-vnet-encryption.md).

Si vous devez limiter les applications de communiquer uniquement sur le sous-réseau chiffré.  Vous pouvez utiliser les listes de contrôle d’accès (ACL) pour autoriser uniquement les communications au sein du sous-réseau en cours.  

Pour plus d’informations sur la configuration des listes de contrôle d’accès, consultez [utilisation Access Control Lists (ACL) pour gérer les centres de données réseau le flux de trafic](../manage/use-acls-for-traffic-flow.md).