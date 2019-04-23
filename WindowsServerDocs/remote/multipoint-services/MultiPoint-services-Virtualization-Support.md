---
title: Prise en charge de la virtualisation MultiPoint Services
description: Décrit l’utilisation de MultiPoint Services avec Hyper-V
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3f0864b8-a087-4890-94ef-05efbd3c4241
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 06d518dcea154ac2bab49a7d0e83a90f96be6e44
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872520"
---
# <a name="multipoint-services-virtualization-support"></a>Prise en charge de la virtualisation MultiPoint Services
MultiPoint Services prend en charge le rôle Hyper-V de deux manières :  
  
-   MultiPoint Services peut être déployés comme système d’exploitation invité sur un serveur exécutant Hyper-V.  
  
-   MultiPoint Services peut être utilisés comme un serveur de virtualisation.   
  
MultiPoint Services en cours d’exécution sur une machine virtuelle fournit l’utilisation des outils Hyper-V pour gérer les systèmes d’exploitation. Ces outils incluent des fonctionnalités de point de contrôle et de restauration, et ils vous permettent d’exporter et importer des machines virtuelles. Environnements de grande taille, vous pouvez consolider les serveurs en exécutant plusieurs ordinateurs virtuels de MultiPoint Services sur un seul serveur physique. Les scénarios possibles sont :  
  
-   Un laboratoire ou salle de classe unique a plus de 20 sièges. Plutôt que de déployer plusieurs ordinateurs physiques exécutant MultiPoint Services, vous pouvez déployer plusieurs machines virtuelles sur un seul ordinateur physique.  
  
    > [!NOTE]  
    > Vous pouvez gérer plusieurs serveurs MultiPoint, physique ou virtuel, via une seule console du gestionnaire MultiPoint.  
  
-   Le serveur MultiPoint est en cours d’exécution sur une machine virtuelle avec une autre infrastructure de serveur sur le même ordinateur physique. Dans ce cas, cette infrastructure de serveur centralise le domaine, la sécurité et les données pour le réseau. Le serveur MultiPoint fournit des Services Bureau à distance et centralise les postes de travail.  
  
> [!NOTE]  
> Lorsque vous exécutez MultiPoint Services sur une machine virtuelle, over USB-Ethernet et des stations de client RDP sont pris en charge. Vidéo direct et USB zéro client connecté stations ne sont pas pris en charge.  
  
Pour plus d’informations sur le rôle Hyper-V, consultez [Hyper-V](../../virtualization/hyper-v/hyper-v-on-windows-server.md).  