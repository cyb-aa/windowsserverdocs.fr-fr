---
ms.assetid: ''
title: Version préliminaire d’Insider pour les fonctionnalités HPN dans Windows Server 2019
description: Découvrez les nouvelles fonctionnalités de mise en réseau hautes performances de Windows Server 2019.
manager: dougkim
author: shortpatti
ms.author: pashort
ms.date: 09/12/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: 7098e81f486a5b0b4974c19b47e2d48c6f98832b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355366"
---
# <a name="insider-preview"></a>Insider Preview


## <a name="dynamic-vrss-and-vmmq"></a>VRSS et VMMQ dynamiques

>S’applique à : Windows Server 2019

Dans le passé, les files d’attente de machines virtuelles et les files d’attente de plusieurs machines virtuelles permettaient un débit nettement plus élevé pour les machines virtuelles individuelles, car les débits du réseau atteignaient d’abord la marque 10 GbE. Malheureusement, la planification, la planification, le paramétrage et la surveillance nécessaires à la réussite sont devenues une entreprise de grande taille ; souvent plus que l’administrateur informatique est censé passer. 

Windows Server 2019 améliore ces optimisations en répartissant et en ajustant de façon dynamique le traitement des charges de travail réseau, selon les besoins. Windows Server 2019 garantit une efficacité optimale et supprime la charge de configuration pour les administrateurs informatiques.

Pour plus d'informations, voir :

-   [Blog d’annonce](https://blogs.technet.microsoft.com/networking/2018/08/22/netperf4vw/)

-   [Guide de validation pour les professionnels de l’informatique](https://aka.ms/DVMMQ-Validation)

## <a name="receive-segment-coalescing-rsc-in-the-vswitch"></a>RSC (Receive Segment Coalescing) dans le commutateur virtuel

>S’applique à : Windows Server 2019 et Windows 10, version 1809

La fusion de segment de réception (RSC) dans le vSwitch est une amélioration qui fusionne plusieurs segments TCP en un segment plus grand avant les données qui traversent le vSwitch. Le segment important améliore les performances de mise en réseau pour les charges de travail virtuelles.

Auparavant, il s’agissait d’un déchargement implémenté par la carte réseau. Malheureusement, cela a été désactivé le moment où vous avez attaché l’adaptateur à un commutateur virtuel. RSC dans la mise à jour de vSwitch sur Windows Server 2019 et Windows 10 octobre 2018 supprime cette limitation.

Par défaut, RSC dans le vSwitch est activé sur les commutateurs virtuels externes.

Pour plus d'informations, voir :

-  [Blog d’annonce](https://blogs.technet.microsoft.com/networking/2018/08/22/netperf4vw/)

-  [Guide de validation pour les professionnels de l’informatique](https://aka.ms/RSC-Validation)
