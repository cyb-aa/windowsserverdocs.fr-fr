---
title: Dimensionnement des machines virtuelles
description: Recommandations de taille pour chaque type de charge de travail.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 12/02/2019
ms.topic: article
author: Heidilohr
manager: lizross
ms.openlocfilehash: 1d6a7daa3966488c951117b083411587d13be56b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857102"
---
# <a name="virtual-machine-sizing-guidance"></a>Guide de dimensionnement des machines virtuelles

Que vous exécutiez votre machine virtuelle sur les services Bureau à distance ou Windows Virtual Desktop, différents types de charges de travail demandent différentes configurations de machines virtuelles d’hôte de session. Pour offrir la meilleure expérience possible, mettez à l’échelle votre déploiement en fonction des besoins de vos utilisateurs.

## <a name="multi-session-recommendations"></a>Recommandations des scénarios multisessions

Le tableau suivant liste le nombre maximal d’utilisateurs suggérés par processeur virtuel (vCPU) et la configuration minimale de machines virtuelles pour chaque charge de travail. Ces recommandations sont basées sur les [charges de travail Bureau à distance](remote-desktop-workloads.md).

| Type de charge de travail | Nombre maximal d’utilisateurs par vCPU | Stockage minimum pour vCPU/RAM/système d’exploitation | Exemples d’instances Azure | Stockage minimum de conteneur de profil |
| --- | --- | --- | --- | --- |
| Léger | 6 | 2 vCPU, 8 Go de RAM, 16 Go de stockage | D2s_v3, F2s_v2 | 30 Go |
| Moyen | 4 | 4 vCPU, 16 Go de RAM, 32 Go de stockage | D4s_v3, F4s_v2 | 30 Go |
| Intensif | 2 | 4 vCPU, 16 Go de RAM, 32 Go de stockage | D4s_v3, F4s_v2 | 30 Go |
| Avancé | 1 | 6 vCPU, 56 Go de RAM, 340 Go de stockage | D4s_v3, F4s_v2, NV6 | 30 Go |

## <a name="single-session-recommendations"></a>Recommandations pour des scénarios monosessions

À titre de recommandation de dimensionnement de machines virtuelles pour les scénarios monosessions, nous recommandons au moins deux cœurs de processeur physiques par machine virtuelle (en général quatre processeurs virtuels avec l’hyperthreading). Si vous avez besoin de recommandations de dimensionnement de machines virtuelles plus spécifiques pour les scénarios monosessions, demandez aux éditeurs des logiciels spécifiques à votre charge de travail. Le dimensionnement des machines virtuelles monosessions est probablement aligné sur les instructions des appareils physiques.

## <a name="general-virtual-machine-recommendations"></a>Recommandations générales pour les machines virtuelles

Pour connaître les exigences en matière de machines virtuelles pour exécuter le système d’exploitation, consultez [Caractéristiques et configuration système requise d’un ordinateur Windows 10](https://www.microsoft.com/windows/windows-10-specifications).

Nous vous recommandons d’utiliser le stockage SSD Premium sur votre disque de système d’exploitation pour les charges de travail de production qui requièrent un contrat de niveau de service (SLA). Pour plus d’informations, consultez le [contrat SLA pour machines virtuelles](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_8/).

Les GPU (Graphics processing units) sont un bon choix pour les utilisateurs qui utilisent régulièrement des programmes gourmands en graphisme pour le rendu vidéo, la conception 3D et les simulations. Pour en savoir plus sur l’accélération graphique, consultez [Choisissez votre technologie de rendu des éléments graphiques](rds-graphics-virtualization.md). Azure propose plusieurs options de déploiement de l’accélération graphique et plusieurs tailles de machine virtuelle avec GPU. Pour en savoir plus, consultez [Tailles de machine virtuelle à GPU optimisé](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-gpu).

Les [machines virtuelles modulables Série B](https://docs.microsoft.com/azure/virtual-machines/windows/b-series-burstable) sont un bon choix pour les utilisateurs qui n’ont pas toujours besoin de performances de processeur maximales. Pour plus d’informations sur les types et les tailles de machines virtuelles, consultez [Tailles des machines virtuelles Windows dans Azure](https://docs.microsoft.com/azure/virtual-machines/windows/sizes) et les informations de tarification dans notre page [Série de machines virtuelles](https://azure.microsoft.com/pricing/details/virtual-machines/series/).

## <a name="test-your-workload"></a>Tester votre charge de travail

Enfin, nous vous recommandons d’utiliser des outils de simulation pour tester votre déploiement avec des tests de contrainte et des simulations d’utilisation dans la vie réelle. Assurez-vous que votre système est suffisamment réactif et résilient pour répondre aux besoins des utilisateurs, et n’oubliez pas de faire varier la taille de la charge pour éviter les surprises.
