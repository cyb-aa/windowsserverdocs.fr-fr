---
author: shortpatti
ms.author: pashort
ms.date: 10/02/2018
ms.prod: windows-server-threshold
ms:topic: include
ms.openlocfilehash: d1bfc74c4daa751e3081b26c87dd0d2c88f5f095
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879850"
---
Les options de carte de mise en veille sont **None (toutes les cartes actif)** ou votre sélection d’une carte réseau spécifique dans l’association de cartes réseau qui agit comme une carte de mise en veille. Lorsque vous configurez une carte réseau comme une carte de mise en veille, tous les autres membres de l’équipe non sélectionnés sont actifs, et aucun trafic réseau est envoyé vers ou traité par l’adaptateur jusqu'à ce qu’une carte réseau Active échoue. Après l’échec d’une carte réseau Active, la carte réseau secondaire devient active et traite le trafic réseau. En cas de tous les membres de l’équipe sont restaurés au service, le membre de l’équipe de secours retourne à l’état de secours.  