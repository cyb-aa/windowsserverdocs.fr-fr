---
author: eross-msft
ms.author: lizross
ms.date: 10/02/2018
ms.prod: windows-server
ms:topic: include
ms.openlocfilehash: 47a91c86ac75aedf532289055c94b34899fa01df
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80316482"
---
Avec le port Hyper-V, les équipes de cartes réseau configurées sur des hôtes Hyper-V accordent des adresses MAC indépendantes aux machines virtuelles.  L’adresse MAC des machines virtuelles ou la machine virtuelle portée connectée au commutateur Hyper-V peut être utilisée pour répartir le trafic réseau entre les membres de l’Association de cartes réseau. Vous ne pouvez pas configurer les associations de cartes réseau que vous créez dans des machines virtuelles avec le mode d’équilibrage de charge de port Hyper-V. Utilisez plutôt le mode de hachage d’adresse. 