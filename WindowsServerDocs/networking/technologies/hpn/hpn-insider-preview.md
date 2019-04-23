---
ms.assetid: ''
title: Insider Preview pour les fonctionnalités HPN dans Windows Server 2019
description: En savoir plus sur les nouvelles fonctionnalités de mise en réseau hautes performances dans Windows Server 2019.
manager: dougkim
author: shortpatti
ms.author: pashort
ms.date: 09/12/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: 3d3e974472f28c30d093fbd1094ef3693d984d19
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886270"
---
# <a name="insider-preview"></a>Insider Preview


## <a name="dynamic-vrss-and-vmmq"></a>VRSS dynamique et VMMQ

>S’applique à : Windows Server 2019

Dans le passé, files d’attente de la Machine virtuelle et plusieurs files d’attente de Machine virtuelle activée débit bien plus important pour les machines virtuelles individuelles en tant que le débit réseau tout d’abord atteint la marque de 10 GbE et au-delà. Malheureusement, la planification, planification, paramétrage et surveillance requises pour la réussite est devenu une entreprise de grande taille ; souvent, plus de l’administrateur informatique destiné à passer. 

Windows Server 2019 améliore ces optimisations en répartissant et le traitement des charges de travail réseau en fonction des besoins de paramétrage dynamiquement. Windows Server 2019 garantit l’efficacité de pointe et supprime la charge de configuration pour les administrateurs informatiques.

Pour plus d'informations, consultez :

-   [Blog d’annonce](https://blogs.technet.microsoft.com/networking/2018/08/22/netperf4vw/)

-   [Guide de validation pour l’IT Pro](https://aka.ms/DVMMQ-Validation)

## <a name="receive-segment-coalescing-rsc-in-the-vswitch"></a>RSC (Receive Segment Coalescing) dans le commutateur virtuel

>S’applique à : Windows Server 2019 et Windows 10, version 1809

Réception Segment fusion (RSC) dans le commutateur virtuel est une amélioration qui regroupe plusieurs segments TCP dans un segment plus grand avant les données transitent via le commutateur virtuel. Le segment de grand améliore les performances de mise en réseau pour les charges de travail virtuelles.

Auparavant, cette opération était un déchargement implémenté par la carte réseau. Malheureusement, cela a été désactivée au moment où que vous avez attaché l’adaptateur à un commutateur virtuel. RSC dans le commutateur virtuel sur Windows Server 2019 et Windows 10 octobre 2018 mise à jour supprime cette limitation.

Par défaut, RSC dans le commutateur virtuel est activé sur les commutateurs virtuels externes.

Pour plus d'informations, consultez :

-  [Blog d’annonce](https://blogs.technet.microsoft.com/networking/2018/08/22/netperf4vw/)

-  [Guide de validation pour l’IT Pro](https://aka.ms/RSC-Validation)
