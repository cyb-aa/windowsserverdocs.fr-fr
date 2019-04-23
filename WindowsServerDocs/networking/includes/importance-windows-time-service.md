---
author: shortpatti
ms.author: pashort
ms.date: 10/02/2018
ms.prod: windows-server-threshold
ms:topic: include
ms.openlocfilehash: 4e8e3234b89630bf16148eef644f0c6607ad38bd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852340"
---
## <a name="importance-of-time-protocols"></a>Importance des protocoles de temps
Protocoles de temps la communication entre deux ordinateurs pour échanger des informations de temps, puis utiliser ces informations pour synchroniser leur horloge. Avec le protocole de temps de service de temps de Windows, un client demande des informations de temps à partir d’un serveur et synchronise son horloge en fonction des informations qui sont reçues.
  
Le service de temps de Windows utilise le protocole NTP afin de synchroniser l’heure sur un réseau. NTP est un protocole de temps Internet qui inclut les algorithmes de discipline nécessaires pour la synchronisation des horloges. NTP est un protocole de temps plus précis que le temps protocole SNTP (Simple Network) qui est utilisé dans certaines versions de Windows ; Toutefois, W32Time continue à prendre en charge SNTP pour permettre la compatibilité descendante avec les ordinateurs exécutant les services de temps basées sur SNTP tels que Windows 2000.

Il existe de nombreuses raisons que vous devrez peut-être heure précise.  Le cas par défaut pour Windows est Kerberos, ce qui oblige les 5 minutes de précision entre le client et le serveur.  Toutefois, il existe beaucoup d’autres domaines qui peut être affectés par la précision du temps, y compris :


- Réglementations gouvernementales telles que :
    - précision de 50 ms pour la FINRA dans le fuseau horaire
    - 1 ESMA ms (MiFID II) dans l’Union européenne.
- Algorithmes de chiffrement
- Systèmes distribués tels que le Cluster/SQL/Exchange et les bases de données de Document
- Framework de Blockchain pour les transactions de bitcoin
- Les journaux distribués et analyse des menaces 
- Réplication AD
- Norme PCI (Payment Card Industry), précision deuxième actuellement 1