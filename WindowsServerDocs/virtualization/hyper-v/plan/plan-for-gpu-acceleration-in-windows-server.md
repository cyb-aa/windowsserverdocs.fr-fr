---
title: Planifier l’accélération GPU dans Windows Server
description: En savoir plus sur les différentes technologies Hyper-V pour l’accélération GPU, notamment DDA et le processeur graphique virtuel RemoteFX
ms.prod: windows-server
ms.reviewer: rickman
author: rick-man
ms.author: rickman
manager: stevelee
ms.topic: article
ms.date: 08/21/2019
ms.openlocfilehash: 7ca8d29b58dc8682575d9cb8b0f26aa49b257335
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80307857"
---
# <a name="plan-for-gpu-acceleration-in-windows-server"></a>Planifier l’accélération GPU dans Windows Server

> S’applique à : Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

Cet article présente les fonctionnalités de virtualisation de graphiques disponibles dans Windows Server.

## <a name="when-to-use-gpu-acceleration"></a>Quand utiliser l’accélération GPU

En fonction de votre charge de travail, vous souhaiterez peut-être envisager l’accélération GPU. Voici ce que vous devez prendre en compte avant de choisir l’accélération GPU :

- **Charges de travail d’application et de bureau à distance (VDI/daas)** : Si vous créez une application ou un service de bureau à distance avec Windows Server, prenez en compte le catalogue des applications que vos utilisateurs sont censés exécuter. Certains types d’applications, telles que les applications de CAO/FAO, les applications de simulation, les jeux et les applications de rendu/visualisation, s’appuient principalement sur le rendu 3D pour fournir une interactivité fluide et fluide. La plupart des clients considèrent les GPU comme une nécessité pour une expérience utilisateur raisonnable avec ces types d’applications.
- **Charges de travail de rendu, d’encodage et de visualisation à distance**: ces charges de travail orientées graphique ont tendance à s’appuyer sur les fonctionnalités spécialisées d’un GPU, telles que le rendu 3D efficace et l’encodage/décodage de trames, afin d’atteindre des objectifs de débit et de rentabilité. Pour ce type de charge de travail, une seule machine virtuelle compatible GPU peut être en mesure de correspondre au débit de nombreuses machines virtuelles à processeur unique.
- **Charges de travail HPC et ml**: pour les charges de travail de calcul haute performance, telles que le calcul hautes performances machine learning et l’inférence ou l’inférence, les GPU peuvent réduire considérablement le temps de résultat, le temps d’inférence et la durée de formation. Elles peuvent également offrir une meilleure rentabilité qu’une architecture de processeur uniquement à un niveau de performances comparable. De nombreux frameworks HPC et Machine Learning ont la possibilité d’activer l’accélération GPU. Déterminez si cela peut tirer parti de votre charge de travail spécifique.

## <a name="gpu-virtualization-in-windows-server"></a>Virtualisation GPU dans Windows Server

Les technologies de virtualisation GPU activent l’accélération GPU dans un environnement virtualisé, généralement au sein des machines virtuelles. Si votre charge de travail est virtualisée à l’aide d’Hyper-V, vous devez utiliser la virtualisation de graphiques pour assurer l’accélération GPU du GPU physique vers vos applications ou services virtualisés. Toutefois, si votre charge de travail s’exécute directement sur des ordinateurs hôtes Windows Server physiques, vous n’avez pas besoin de la virtualisation graphique. vos applications et services ont déjà accès aux fonctionnalités et API du GPU prises en charge en mode natif dans Windows Server.

Les technologies de virtualisation de graphiques suivantes sont disponibles pour les machines virtuelles Hyper-V dans Windows Server :

- [Attribution d’appareil discrète (DDA)](#discrete-device-assignment-dda)
- [RemoteFX vGPU](#remotefx-vgpu)

En plus des charges de travail de machine virtuelle, Windows Server prend également en charge l’accélération GPU des charges de travail en conteneur dans les conteneurs Windows. Pour plus d’informations, consultez [accélération du GPU dans les conteneurs Windows](https://docs.microsoft.com/virtualization/windowscontainers/deploy-containers/gpu-acceleration).

## <a name="discrete-device-assignment-dda"></a>Attribution d’appareil discrète (DDA)

L’attribution d’appareil discrète (DDA), également appelée transmission de GPU, vous permet de dédier un ou plusieurs GPU physiques à un ordinateur virtuel. Dans un déploiement DDA, les charges de travail virtualisées s’exécutent sur le pilote natif et ont généralement un accès complet à la fonctionnalité du GPU. DDA offre le niveau le plus élevé de compatibilité des applications et des performances potentielles. DDA peut également fournir une accélération GPU pour les machines virtuelles Linux, sous réserve de prise en charge.

Un déploiement DDA peut accélérer uniquement un nombre limité d’ordinateurs virtuels, car chaque GPU physique peut fournir une accélération à une seule machine virtuelle. Si vous développez un service dont l’architecture prend en charge les machines virtuelles partagées, envisagez d’héberger plusieurs charges de travail accélérées par machine virtuelle. Par exemple, si vous créez un service de communication à distance pour ordinateur de bureau avec RDS, vous pouvez améliorer la mise à l’échelle des utilisateurs en tirant parti des fonctionnalités de plusieurs sessions de Windows Server pour héberger plusieurs bureaux d’utilisateurs sur chaque machine virtuelle. Ces utilisateurs vont partager les avantages de l’accélération GPU.

Pour plus d’informations, consultez les rubriques suivantes :

- [Planifier le déploiement de l’affectation discrète des appareils](plan-for-deploying-devices-using-discrete-device-assignment.md)
- [Déployer des appareils graphiques à l’aide de l’affectation discrète des appareils](../deploy/Deploying-graphics-devices-using-dda.md)

## <a name="remotefx-vgpu"></a>RemoteFX vGPU

> [!NOTE]
> Le processeur graphique virtuel RemoteFX est entièrement pris en charge dans Windows Server 2016, mais n’est pas pris en charge dans Windows Server 2019.

Le processeur graphique virtuel RemoteFX est une technologie de virtualisation graphique qui permet de partager un seul GPU physique entre plusieurs machines virtuelles. Dans un déploiement de processeur graphique virtuel RemoteFX, les charges de travail virtualisées s’exécutent sur l’adaptateur 3D RemoteFX de Microsoft, qui coordonne les demandes de traitement GPU entre l’hôte et les invités. Le processeur graphique virtuel RemoteFX est plus approprié pour les charges de travail et les charges de travail intenses, dans lesquelles les ressources GPU dédiées ne sont pas requises. Le processeur graphique virtuel RemoteFX peut uniquement fournir une accélération GPU aux machines virtuelles Windows.

Pour plus d’informations, consultez les rubriques suivantes :

- [Déployer des périphériques graphiques avec vGPU RemoteFX](../deploy/deploy-graphics-devices-using-remotefx-vgpu.md)
- [Prise en charge de la carte vidéo 3D RemoteFX](../../../remote/remote-desktop-services/rds-supported-config.md#remotefx-3d-video-adapter-vgpu-support)

## <a name="comparing-dda-and-remotefx-vgpu"></a>Comparaison de DDA et du processeur graphique virtuel RemoteFX

Tenez compte des différences entre les fonctionnalités suivantes et la prise en charge des technologies de virtualisation de graphiques lors de la planification de votre déploiement :

|                       | RemoteFX vGPU                                                                       | Attribution d’appareils en mode discret                                                          |
|-----------------------|-------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------|
| Modèle de ressource GPU    | Dédié ou partagé                                                                 | Dédié uniquement                                                                      |
| Densité de machines virtuelles            | Élevé (un ou plusieurs GPU sur plusieurs machines virtuelles)                                                 | Faible (un ou plusieurs GPU sur une seule machine virtuelle)                                                    |
| Compatibilité des applications     | DX 11.1, OpenGL 4.4, OpenCL 1.1                                                     | Toutes les fonctionnalités GPU fournies par le fournisseur (DX 12, OpenGL, CUDA)                       |
| AVC444                | Activée par défaut                                                                  | Disponible via stratégie de groupe                                                      |
| VRAM de GPU              | Jusqu’à 1 Go de RAM vidéo dédiée                                                           | Jusqu’à la quantité de RAM vidéo prise en charge par le GPU                                                     |
| Fréquence d’images            | Jusqu’à 30fps                                                                         | Jusqu’à 60fps                                                                         |
| Pilote GPU dans l’invité   | Pilote d’affichage de carte 3D RemoteFX (Microsoft)                                      | Pilote du fournisseur GPU (NVIDIA, AMD, Intel)                                              |
| Prise en charge du système d’exploitation hôte       | Windows Server 2016                                                                 | Windows Server 2016 ; Windows Server 2019                                            |
| Prise en charge du système d’exploitation invité      | Windows Server 2012 R2 ; Windows Server 2016 ; Windows 7 SP1 ; Windows 8.1 ; Windows 10 | Windows Server 2012 R2 ; Windows Server 2016 ; Windows Server 2019 ; Windows 10 ; Linux |
| Hyperviseur            | Microsoft Hyper-V                                                                   | Microsoft Hyper-V                                                                   |
| Matériel GPU          | GPU Entreprise (comme Nvidia Quadro/GRID ou AMD FirePro)                         | GPU Entreprise (comme Nvidia Quadro/GRID ou AMD FirePro)                         |
| Matériel serveur       | Aucune exigence                                                             | Serveur moderne, expose IOMMU au système d’exploitation (généralement un matériel compatible SR-IOV)              |
