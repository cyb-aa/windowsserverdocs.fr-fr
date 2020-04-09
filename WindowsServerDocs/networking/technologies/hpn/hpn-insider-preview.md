---
title: Version préliminaire d’Insider pour les fonctionnalités HPN dans Windows Server 2019
description: Découvrez les nouvelles fonctionnalités de mise en réseau hautes performances de Windows Server 2019.
manager: dougkim
author: eross-msft
ms.author: lizross
ms.date: 09/12/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: f6403cd9787ccdd4f50eb08ebd7723d2de789ebb
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80819642"
---
# <a name="new-hpn-features-in-windows-server-2019"></a>Nouvelles fonctionnalités HPN dans Windows Server 2019


## <a name="dynamic-vrss-and-vmmq"></a>VRSS et VMMQ dynamiques

>S’applique à : Windows Server 2019

Dans le passé, les files d’attente de machines virtuelles et les files d’attente de plusieurs machines virtuelles permettaient un débit nettement plus élevé pour les machines virtuelles individuelles, car les débits du réseau atteignaient d’abord la marque 10 GbE. Malheureusement, la planification, la planification, le paramétrage et la surveillance nécessaires à la réussite sont devenues une entreprise de grande taille ; souvent plus que l’administrateur informatique est censé passer. 

Windows Server 2019 améliore ces optimisations en répartissant et en ajustant de façon dynamique le traitement des charges de travail réseau, selon les besoins. Windows Server 2019 garantit une efficacité optimale et supprime la charge de configuration pour les administrateurs informatiques.

Pour plus d’informations, consultez :

-   [Blog d’annonce](https://blogs.technet.microsoft.com/networking/2018/08/22/netperf4vw/)

-   [Guide de validation pour les professionnels de l’informatique](https://aka.ms/DVMMQ-Validation)

## <a name="receive-segment-coalescing-rsc-in-the-vswitch"></a>RSC (Receive Segment Coalescing) dans le commutateur virtuel

>S’applique à : Windows Server 2019 et Windows 10, version 1809

La fusion de segment de réception (RSC) dans le vSwitch est une amélioration qui fusionne plusieurs segments TCP en un segment plus grand avant les données qui traversent le vSwitch. Le segment important améliore les performances de mise en réseau pour les charges de travail virtuelles.

Auparavant, il s’agissait d’un déchargement implémenté par la carte réseau. Malheureusement, cela a été désactivé le moment où vous avez attaché l’adaptateur à un commutateur virtuel. RSC dans la mise à jour de vSwitch sur Windows Server 2019 et Windows 10 octobre 2018 supprime cette limitation.

Par défaut, RSC dans le vSwitch est activé sur les commutateurs virtuels externes.

Pour plus d’informations, consultez :

-  [Blog d’annonce](https://blogs.technet.microsoft.com/networking/2018/08/22/netperf4vw/)

-  [Guide de validation pour les professionnels de l’informatique](https://aka.ms/RSC-Validation)
