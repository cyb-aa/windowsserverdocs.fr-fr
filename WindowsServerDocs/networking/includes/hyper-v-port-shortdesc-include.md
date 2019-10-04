---
author: shortpatti
ms.author: pashort
ms.date: 10/02/2018
ms.prod: windows-server
ms:topic: include
ms.openlocfilehash: 01f231ca730a19ac0e7e868bcb7180377830afe1
ms.sourcegitcommit: 73898afec450fb3c2f429ca373f6b48a74b19390
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/03/2019
ms.locfileid: "71935078"
---
Avec le port Hyper-V, les équipes de cartes réseau configurées sur des hôtes Hyper-V accordent des adresses MAC indépendantes aux machines virtuelles.  L’adresse MAC des machines virtuelles ou la machine virtuelle portée connectée au commutateur Hyper-V peut être utilisée pour répartir le trafic réseau entre les membres de l’Association de cartes réseau. Vous ne pouvez pas configurer les associations de cartes réseau que vous créez dans des machines virtuelles avec le mode d’équilibrage de charge de port Hyper-V. Utilisez plutôt le mode de hachage d’adresse. 