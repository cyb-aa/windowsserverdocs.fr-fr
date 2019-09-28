---
title: Protection du volume du système avec la Protection des disques
description: Fournit des informations sur la protection des disques pour MultiPoint services
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 18694665-ed65-4d84-8505-f460cf3df907
author: evaseydl
manager: scotman
ms.author: evas
ms.openlocfilehash: 27bcce68cfe5856e987f5ad93460abd12217c26b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71389509"
---
# <a name="protecting-the-system-volume-with-disk-protection"></a>Protection du volume du système avec la Protection des disques
MultiPoint services offre la possibilité d’effacer instantanément les modifications apportées au volume système à chaque démarrage de l’ordinateur. Si vous activez la fonctionnalité de protection des disques, toutes les modifications apportées au lecteur, telles que la configuration endommagée ou l’introduction de logiciels malveillants, sont annulées lors du redémarrage suivant de l’ordinateur. Il s’agit d’une fonctionnalité pratique pour les administrateurs qui souhaitent s’assurer qu’une image de logiciel « bonne » ou « Golden » connue est chargée à chaque fois. Les mises à jour automatiques ou les correctifs logiciels peuvent être planifiés, par exemple, au milieu de la nuit. La prise en compte de la planification consiste à indiquer si vous souhaitez que les utilisateurs finaux puissent apporter des modifications, telles que l’installation de logiciels, à partir d’Internet. Lorsque cette fonctionnalité est activée, si vous souhaitez que les utilisateurs puissent stocker des fichiers, le partage de fichiers doit être en dehors du volume système.  
  
