---
title: Services Bureau à distance - quelle technologie de virtualisation de graphiques vous convient ?
description: Informations de planification pour vous aider à choisir les graphiques appropriés option de virtualisation pour votre déploiement des services Bureau à distance.
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
ms.openlocfilehash: 7cf7fdf3510fcaaa955bd0031fb3564fe4372472
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875800"
---
# <a name="which-graphics-virtualization-technology-is-right-for-you"></a>Quelle technologie de virtualisation de graphiques vous convient ?

Vous avez un éventail d’options lorsqu’il s’agit de l’activation de rendu des graphiques dans les Services Bureau à distance. Lorsque vous planifiez votre environnement de virtualisation, les considérations suivantes lecteur les choix de la technologie de rendu des graphiques :

![Considérations de rendu graphique - mise à l’échelle de compare utilisateur en matière de GPU pour déterminer la meilleure technologie GPU pour votre environnement](media/rds-gpu.png)

Dans Windows Server 2016, vous disposez de deux technologies de virtualisation graphiques disponibles avec Hyper-V qui vous permettre de tirer parti du matériel GPU :

- [Affectation d’appareil discrètes (DDA)](#discrete-device-assignment) : pour les meilleures performances à l’aide d’une ou plusieurs GPU dédié à une machine virtuelle prise en charge native du pilote GPU à l’intérieur de la machine virtuelle. La densité est faible, car il est limité par le nombre de GPU physiques disponibles sur le serveur. 
- [À distance FX vGPU](#remotefx-vgpu) : pour un travailleur du savoir et les scénarios GPU haute rafale où plusieurs machines virtuelles tirer parti d’une ou plusieurs GPU via paravirtualisation. Cette solution fournit la densité de l’utilisateur par serveur.

L’illustration suivante montre les graphiques d’options de virtualisation dans Windows Server 2016.

![Options de virtualisation des graphiques dans Windows Server 2016 avec les services Bureau à distance - affiche les trois technologies disponibles ainsi que leurs différences sur les performances et évolutivité](media/rds-graphics-virtualization.png)

## <a name="discrete-device-assignment"></a>Affectation de périphérique en mode discret
Affectation d’appareil discrètes (DDA) est une solution de directe de matériel qui fournit les meilleures performances, étant donné que la machine virtuelle a un accès complet vers le GPU de l’utilisation du pilote natif. Votre utilisateur de machine virtuelle peut accéder à toutes les fonctionnalités de leur appareil en tant que bien le pilote de périphérique natif. Cela signifie que les fonctionnalités de l’appareil en cours d’exécution d’une mise en miroir de la machine virtuelle le même appareil en cours d’exécution sur des systèmes nus.

Pour plus d’informations sur DDA, visitez [planifier le déploiement d’attribution discrète d’appareil](../../virtualization/hyper-v/plan/plan-for-deploying-devices-using-discrete-device-assignment.md).

## <a name="remotefx-vgpu"></a>RemoteFX vGPU 
Processeur graphique virtuel RemoteFX est une technologie de virtualisation de graphiques qui permet à la puissance de traitement d’un GPU être réparties sur différents systèmes de d’exploitation invité pour activer les scénarios de travailleur de connaissances (consultez le premier graphique ci-dessus). Des améliorations dans Windows Server 2016 autoriser d’autres améliorations pour les scénarios de rafale GPU, par exemple pour le concepteur des applications et visualisation de données. Autres améliorations des fonctionnalités sont les suivantes :

-   Prise en charge des invités de génération 2 machines virtuelles, Windows Server 2016 invité machines virtuelles et les hôtes Hyper-V de Windows Client.
   >[!NOTE] 
   > Hôte de Session Bureau à distance n’est pas pris en charge sur un invité de Windows Server 2016 machine virtuelle ; 1 seule session peut être hébergée par un ordinateur virtuel invité de Windows Server 2016.

-   Compatibilité des applications améliorée et la stabilité.
-   Machine virtuelle se connecter Mode de Session étendu, ce qui permet la redirection USB et Presse-papiers via la machine virtuelle se connecter à une machine virtuelle prenant en charge pour les vGPU RemoteFX.

Pour plus d’informations, consultez [définir créer et configurer des vGPU RemoteFX pour les Services Bureau à distance](rds-remotefx-vgpu.md).

## <a name="which-should-you-use"></a>Lequel devez-vous utiliser ?

Points clés sur la virtualisation de quelle technologie peut-être dépendre les spécifications matérielles ou les exigences d’application dans votre environnement. Voici une brève table concernant les fonctionnalités de vGPU DDA et RemoteFX :

| Fonctionnalité               | RemoteFX vGPU                                                                       | Affectation de périphérique en mode discret                                             |
|-----------------------|-------------------------------------------------------------------------------------|------------------------------------------------------------------------|
| Affectation de GPU de périphérique | Paravirtualisés (nombre de machines virtuelles à un ou plusieurs GPU)                                     | 1 ou plusieurs GPU à machine 1 virtuelle                                                  |
| Scale                 | Meilleure échelle / 1 GPU pour le nombre de machines virtuelles                                                      | Mise à l’échelle à faible / 1 ou plusieurs GPU à machine 1 virtuelle                                     |
| Compatibilité des applications     | DX 11.1, OpenGL 4.4, OpenCL 1.1                                                     | Toutes les fonctionnalités GPU fournies par le fournisseur (12 DX, OpenGL, CUDA)          |
| AVC444                | Activé par défaut (Windows 10 et Windows Server 2016)                             | Disponible via la stratégie de groupe (Windows 10 et Windows Server 2016)    |
| VRAM DE GPU              | Jusqu'à 1 Go dédiée VRAM                                                           | Jusqu'à VRAM pris en charge par le GPU                                        |
| Fréquence d’images            | Jusqu'à 30 i/s                                                                         | Jusqu'à 60 i/s                                                            |
| Pilote GPU dans l’invité   | Pilote d’affichage de carte 3D RemoteFX (Microsoft)                                      | Pilote de fournisseur GPU (Nvidia, AMD, Intel)                                 |
| Prise en charge du système d’exploitation invité      |  Windows Server 2012 R2 Windows Server 2016 Windows 7 SP1, Windows 8.1, Windows 10 |  Windows Server 2012 R2 Windows Server 2016 Windows 10 Linux         |
| hyperviseur            | Microsoft Hyper-V                                                                   | Microsoft Hyper-V                                                      |
| Disponibilité du système d’exploitation hôte  |  Windows Server 2012 R2 Windows Server 2016 Windows 10                             | Windows Server 2016                                                    |
| Matériel GPU          | GPU Enterprise (telles que Nvidia Quadro/la grille ou AMD FirePro)                         | GPU Enterprise (telles que Nvidia Quadro/la grille ou AMD FirePro)            |
| Matériel de serveur       | Aucune exigence                                                             | Serveur moderne, expose IOMMU au système d’exploitation (généralement un matériel compatible SR-IOV) |

Une règle générale consiste à utiliser les DDA la meilleure compatibilité des applications dans la mesure où la machine virtuelle aura un accès direct au GPU. Si vos applications ou les charges de travail n’ont pas comme des exigences strictes de GPU et souhaitez serveur une base plus large d’utilisateurs, RemoteFX vGPU peut fonctionnent le mieux pour vous.