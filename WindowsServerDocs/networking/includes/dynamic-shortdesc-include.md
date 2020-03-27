---
author: eross-msft
ms.author: lizross
ms.date: 10/02/2018
ms.prod: windows-server
ms:topic: include
ms.openlocfilehash: f2065acb89af4bed4dc525453bb5a294a4e2c3ef
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80316509"
---
Avec les charges dynamiques, les charges sortantes sont distribuées en fonction d’un hachage des ports TCP et des adresses IP. Le mode dynamique rééquilibre également les charges en temps réel afin qu’un workflow sortant donné puisse se déplacer entre les membres de l’équipe. Les charges entrantes, en revanche, sont distribuées de la même façon que le port Hyper-V. En résumé, le mode dynamique utilise les meilleurs aspects de hachage d’adresse et de port Hyper-V. il s’agit du mode d’équilibrage de charge le plus performant. 

