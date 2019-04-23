---
author: shortpatti
ms.author: pashort
ms.date: 10/02/2018
ms.prod: windows-server-threshold
ms:topic: include
ms.openlocfilehash: 761deb136ebd4ec22dfeebc47b4eeb7650594d89
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863710"
---
Avec Port Hyper-V, les associations de cartes réseau configurées sur les ordinateurs hôtes Hyper-V offrent des adresses MAC indépendantes de machines virtuelles.  L’adresse MAC des machines virtuelles ou la machine virtuelle porté connectées au commutateur Hyper-V, permettre servir à diviser le trafic réseau entre les membres de l’association de cartes réseau. Vous ne pouvez pas configurer les associations de cartes réseau que vous créez au sein de machines virtuelles avec la mode d’équilibrage de charge de Port Hyper-V. Au lieu de cela, utilisez le mode de hachage d’adresse. 