---
title: Services Bureau à distance - Quelle technologie de virtualisation graphique vous convient ?
description: Informations de planification pour vous aider à choisir l’option de virtualisation graphique appropriée pour votre déploiement des services Bureau à distance.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 03/16/2017
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d6ff5b22-7695-4fee-b1bd-6c9dce5bd0e8
author: lizap
manager: scottman
ms.openlocfilehash: ce10575d38bccc0b22dadf55bd89156c6ce5ea7b
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871046"
---
# <a name="which-graphics-virtualization-technology-is-right-for-you"></a>Quelle technologie de virtualisation graphique vous convient ?

Vous disposez d’un choix d’options lorsqu’il s’agit d’activer le rendu de graphismes dans les services Bureau à distance. Lorsque vous planifiez votre environnement de virtualisation, les considérations suivantes orientent votre choix sur la technologie de rendu graphique à adopter :

![Considérations sur le rendu graphique : comparaison entre l’échelle des utilisateurs et les besoins en GPU, afin de déterminer la meilleure technologie GPU appropriée à votre environnement](media/rds-gpu.png)

Dans Windows Server 2016, vous disposez de deux technologies de virtualisation graphique, utilisables avec Hyper-V, pour vous permettre de tirer pleinement parti des composants matériels du processeur graphique (GPU) :

- [Attribution d’appareils en mode discret](#discrete-device-assignment) - Pour des performances maximales, utilisation d’un ou de plusieurs GPU dédiés à une machine virtuelle fournissant la prise en charge du pilote GPU natif à l’intérieur de la machine virtuelle. La densité est faible, car elle est limitée par le nombre de GPU physiques disponibles dans le serveur. 
- [RemoteFX vGPU](#remotefx-vgpu) - Pour des scénarios de travailleur de la connaissance et de GPU à pics d’activité élevés, dans lesquels plusieurs machines virtuelles utilisent le potentiel d’un ou de plusieurs GPU par le biais de la paravirtualisation. Cette solution fournit une densité utilisateur supérieure par serveur.

L’illustration suivante montre les options de la virtualisation graphique dans Windows Server 2016.

![Options de la virtualisation graphique dans Windows Server 2016 avec les services Bureau à distance : montre les trois technologies disponibles et leurs différences en fonction de l’échelle et des performances](media/rds-graphics-virtualization.png)

## <a name="discrete-device-assignment"></a>Attribution d’appareils en mode discret
L’attribution d’appareils en mode discret est une solution matérielle directe qui offre des performances optimales, car la machine virtuelle bénéficie d’un accès complet au GPU avec le pilote natif. Votre utilisateur de machine virtuelle peut accéder à toutes les fonctionnalités de son appareil ainsi qu’au pilote natif de celui-ci. Autrement dit, aux fonctionnalités et capacités d’exécution de l’appareil dans une mise en miroir de machine virtuelle exécutant ce même appareil sur un système nu.

Pour plus d’informations sur l’attribution d’appareils en mode discret, consultez [Plan de déploiement de l’attribution discrète d’appareils](../../virtualization/hyper-v/plan/plan-for-deploying-devices-using-discrete-device-assignment.md).

## <a name="remotefx-vgpu"></a>RemoteFX vGPU 
RemoteFX vGPU est une technologie de virtualisation graphique qui permet à la puissance de traitement d’un processeur graphique (GPU) d’être répartie sur différents systèmes d’exploitation invités pour activer des scénarios de travailleur de la connaissance (reportez-vous à la première illustration ci-dessus). Des améliorations dans Windows Server 2016 ouvrent le champ à d’autres avancées dans les scénarios à pics d’activité GPU, par exemple pour les applications de concepteur et la visualisation de données. D’autres améliorations de fonctionnalités sont les suivantes :

- Prise en charge des machines virtuelles invitées de génération 2, des machines virtuelles invitées Windows Server 2016 et de l’hôte Hyper-V client de Windows.
  >[!NOTE] 
  > Le service Hôte de session Bureau à distance n’est pas pris en charge sur une machine virtuelle invitée Windows Server 2016 ; une seule session peut être hébergée par machine virtuelle invitée Windows Server 2016.

- Compatibilité plus importante et stabilité supérieure des applications.
- Mode de session étendue VM Connect, permettant la redirection d’appareils USB et du Presse-papiers via VM Connect vers une machine virtuelle qui est activée pour RemoteFX vGPU.

Pour plus d’informations, consultez [Installer et configurer RemoteFX vGPU pour les services Bureau à distance](rds-remotefx-vgpu.md).

## <a name="which-should-you-use"></a>Laquelle devez-vous utiliser ?

Considérations essentielles portant sur le choix de la technologie de virtualisation qui peut être subordonné à des spécifications matérielles ou à des configurations exigées pour les applications dans votre environnement. Voici un tableau succinct sur les fonctionnalités de l’attribution d’appareils en mode discret et de RemoteFX vGPU :

| Fonctionnalité               | RemoteFX vGPU                                                                       | Attribution d’appareils en mode discret                                             |
|-----------------------|-------------------------------------------------------------------------------------|------------------------------------------------------------------------|
| Affectation de GPU d’appareil | Paravirtualisée (nombreuses machines virtuelles pour un ou plusieurs GPU)                                     | 1 ou plusieurs GPU pour 1 machine virtuelle                                                  |
| Scale                 | Meilleure échelle / 1 GPU pour de nombreuses machines virtuelles                                                      | Échelle basse / 1 ou plusieurs GPU pour 1 machine virtuelle                                     |
| Compatibilité des applications     | DX 11.1, OpenGL 4.4, OpenCL 1.1                                                     | Toutes les fonctionnalités GPU fournies par le fournisseur (DX 12, OpenGL, CUDA)          |
| AVC444                | Activée par défaut (Windows 10 et Windows Server 2016)                             | Disponible via la stratégie de groupe (Windows 10 et Windows Server 2016)    |
| VRAM de GPU              | Jusqu’à 1 Go de RAM vidéo dédiée                                                           | Jusqu’à la quantité de RAM vidéo prise en charge par le GPU                                        |
| Fréquence d’images            | Jusqu’à 30fps                                                                         | Jusqu’à 60fps                                                            |
| Pilote GPU dans l’invité   | Pilote d’affichage de carte 3D RemoteFX (Microsoft)                                      | Pilote de fournisseur GPU (Nvidia, AMD, Intel)                                 |
| Prise en charge du système d’exploitation invité      |  Windows 2012 R2, Windows Server 2016, Windows 7 SP1, Windows 8.1, Windows 10 |  Windows Server 2012 R2, Windows Server 2016, Windows 10 Linux         |
| Hyperviseur            | Microsoft Hyper-V                                                                   | Microsoft Hyper-V                                                      |
| Disponibilité du système d’exploitation hôte  |  Windows Server 2012 R2, Windows Server 2016, Windows 10                             | Windows Server 2016                                                    |
| Matériel GPU          | GPU Entreprise (comme Nvidia Quadro/GRID ou AMD FirePro)                         | GPU Entreprise (comme Nvidia Quadro/GRID ou AMD FirePro)            |
| Matériel serveur       | Aucune exigence                                                             | Serveur moderne, expose IOMMU au système d’exploitation (généralement un matériel compatible SR-IOV) |

La règle générale prescrit d’utiliser l’affectation d’appareils en mode discret (DDA) pour bénéficier d’une compatibilité optimale des applications, car la machine virtuelle profite d’un accès direct au processeur graphique. Si vos applications ou charges de travail ne présentent pas d’exigences aussi strictes en matière de GPU, et que vous souhaitez servir une base d’utilisateurs plus importante, RemoteFX vGPU semble être la solution qui vous convient le mieux.