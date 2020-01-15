---
title: Aide relative au réseau
description: Recommandations en matière de bande passante pour les déploiements Bureau à distance.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 12/12/2019
ms.tgt_pltfrm: na
ms.topic: article
author: Heidilohr
manager: daveba
ms.openlocfilehash: f43d05a22211dcf00e06dc712203d6ecf995cc49
ms.sourcegitcommit: bfe9c5f7141f4f2343a4edf432856f07db1410aa
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75466332"
---
# <a name="network-guidance"></a>Aide relative au réseau

Quand vous utilisez une session Windows à distance, la bande passante disponible de votre réseau a un impact significatif sur la qualité de votre expérience. Différentes applications et résolutions d’affichage appellent différentes configurations réseau. Il est donc important de vérifier que votre réseau est configuré pour répondre à vos besoins.

>[!NOTE]
>Les recommandations suivantes s’appliquent aux réseaux avec moins de 0,1 % de perte. Ces recommandations s’appliquent quel que soit le nombre de sessions que vous hébergez sur vos machines virtuelles.

## <a name="applications"></a>Applications

Le tableau suivant répertorie les bandes passantes minimales recommandées pour une expérience utilisateur fluide. Ces recommandations sont basées sur les lignes directrices des [charges de travail Bureau à distance](remote-desktop-workloads.md).

| Type de charge de travail   | Bande passante recommandée |
|-----------------|-----------------------|
| Léger           | 1,5 Mbits/s              |
| Moyen          | 3 Mbits/s                |
| Intensif           | 5 Mbits/s                |
| Avancé           | 15 Mbits/s               |

Gardez à l’esprit que la contrainte imposée à votre réseau dépend à la fois de la fréquence d’images de sortie de la charge de travail de votre application et de votre résolution d’affichage. Si la fréquence d’images ou la résolution d’affichage augmente, le besoin en bande passante augmente également. Par exemple, une charge de travail légère avec un affichage haute résolution nécessite plus de bande passante qu’une charge de travail légère avec une résolution normale ou basse.

Les besoins en bande passante dans d’autres scénarios peuvent varier en fonction de votre utilisation, par exemple :

- Audioconférence et vidéoconférence
- Communication en temps réel
- Streaming de vidéos 4K

Veillez à tester le chargement de ces scénarios dans votre déploiement à l’aide d’outils de simulation comme Login VSI. Faites varier la taille de la charge, exécutez des tests de contrainte et testez les scénarios utilisateur courants dans des sessions à distance pour mieux comprendre les besoins de votre réseau.

## <a name="display-resolutions"></a>Résolutions d’affichage

Les besoins en bande passante varient en fonction de la résolution d’affichage. Le tableau suivant liste les bandes passantes recommandées pour une expérience utilisateur fluide à des résolutions d’affichage standard avec une fréquence de 30 images par seconde. Ces recommandations s’appliquent à des scénarios comptant un ou plusieurs utilisateurs. Gardez à l’esprit que les scénarios impliquant une fréquence inférieure à 30 images par seconde, comme la lecture de texte statique, nécessitent moins de bande passante disponible.

| Résolutions d’affichage standard à 30 images par seconde    | Bande passante recommandée |
|------------------------------------------|-----------------------|
| Environ 1024 × 768 px                      | 1,5 Mbits/s              |
| Environ 1280 × 720 px                      | 3 Mbits/s                |
| Environ 1920 × 1080 px                     | 5 Mbits/s                |
| Environ 3840 × 2160 px (4K)                | 15 Mbits/s               |

## <a name="additional-resources"></a>Ressources supplémentaires

La région Azure dans laquelle vous vous trouvez peut affecter l’expérience utilisateur autant que les conditions réseau. Pour en savoir plus, consultez l’[estimateur d’expérience Windows Virtual Desktop](https://azure.microsoft.com/services/virtual-desktop/assessment/).
