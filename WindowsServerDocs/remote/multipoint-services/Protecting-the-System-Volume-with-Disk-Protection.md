---
title: Protection du volume du système avec la Protection des disques
description: Fournit des informations sur la protection des disques pour MultiPoint Services
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 18694665-ed65-4d84-8505-f460cf3df907
author: evaseydl
manager: scotman
ms.author: evas
ms.openlocfilehash: a663bb27a7480066c997844d68b33774f9032e92
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855210"
---
# <a name="protecting-the-system-volume-with-disk-protection"></a>Protection du volume du système avec la Protection des disques
MultiPoint Services fournit l’option d’effacement instantanément toutes les modifications sur le volume système chaque fois que l’ordinateur est démarré. Si vous activez la fonctionnalité de Protection des disques, toutes les modifications sur le disque, telles que l’altération de la configuration ou l’ajout de programmes malveillants, sont annulées lors du prochain redémarrage de l’ordinateur. Il s’agit d’une fonctionnalité pratique pour les administrateurs qui souhaitent s’assurer qu’une image de logiciels connus de « bonnes » ou « golden » est chargée chaque fois. Mises à jour automatiques ou mise à jour corrective peut être planifié, par exemple, au milieu de la nuit. La planification considération est que vous souhaitez que les utilisateurs finaux puissent apporter des modifications, telles que l’installation des logiciels, à partir d’Internet. Avec cette fonctionnalité activée, si vous souhaitez que les utilisateurs pour être en mesure de stocker des fichiers, le partage de fichiers doit être en dehors du volume système.  
  
