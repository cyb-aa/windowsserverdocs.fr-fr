---
title: Prise en charge de la virtualisation MultiPoint Services
description: Décrit comment utiliser MultiPoint services avec Hyper-V
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 3f0864b8-a087-4890-94ef-05efbd3c4241
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: d23034b6a70d36df259fc26829b36ebebeade5ab
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857622"
---
# <a name="multipoint-services-virtualization-support"></a>Prise en charge de la virtualisation MultiPoint Services
MultiPoint Services prend en charge le rôle Hyper-V de deux manières :  
  
-   MultiPoint services peut être déployé en tant que système d’exploitation invité sur un serveur exécutant Hyper-V.  
  
-   MultiPoint services peut être utilisé en tant que serveur de virtualisation.   
  
L’exécution de MultiPoint services sur un ordinateur virtuel permet d’utiliser les outils Hyper-V pour gérer les systèmes d’exploitation. Ces outils incluent des fonctionnalités de point de contrôle et de restauration, et vous permettent d’exporter et d’importer des ordinateurs virtuels. Pour les installations plus volumineuses, vous pouvez consolider les serveurs en exécutant plusieurs ordinateurs virtuels MultiPoint services sur un serveur physique unique. Les scénarios possibles sont :  
  
-   Une seule salle de classe ou un seul atelier compte plus de 20 sièges. Plutôt que de déployer plusieurs ordinateurs physiques exécutant MultiPoint services, vous pouvez déployer plusieurs ordinateurs virtuels sur un même ordinateur physique.  
  
    > [!NOTE]  
    > Vous pouvez gérer plusieurs serveurs MultiPoint, qu’ils soient physiques ou virtuels, par le biais d’une console MultiPoint Manager unique.  
  
-   Le serveur MultiPoint s’exécute sur un ordinateur virtuel avec une autre infrastructure de serveur sur le même ordinateur physique. Dans ce cas, cette infrastructure de serveur centralise le domaine, la sécurité et les données du réseau. Le serveur MultiPoint fournit Services Bureau à distance et centralise les postes de travail.  
  
> [!NOTE]  
> Lors de l’exécution de MultiPoint services sur un ordinateur virtuel, les stations clientes USB-sur-Ethernet et RDP sont prises en charge. Les stations direct Video et USB connectées à zéro ne sont pas prises en charge.  
  
Pour plus d’informations sur le rôle Hyper-V, voir [Hyper-v](../../virtualization/hyper-v/hyper-v-on-windows-server.md).  