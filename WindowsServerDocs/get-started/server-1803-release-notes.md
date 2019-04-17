---
title: 'Notes de publication: problèmes importants dans Windows Server, version 1803'
description: En savoir plus sur les problèmes connus, limitations ou d’autres informations que vous avez besoin avant d’installer Windows Server, version 1803
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.date: 05/07/2018
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
ms.author: elizapo
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: e9bd860769ec375a6d89ac452e3430b791fff3ad
ms.sourcegitcommit: 9ed4c9fe04ebf3ef488170503c9a354c992b6fde
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/03/2018
ms.locfileid: "4339317"
---
# Notes de publication: Les problèmes importants dans Windows Server, version 1803

>S’applique à: Windows Server (canal semi-annuel)

Ces notes de publication résument les problèmes les plus critiques du système d’exploitation Windows, y compris les éviter ou résoudre les problèmes connus. Pour en savoir plus sur les nouvelles fonctionnalités dans cette version, reportez-vous à la section [Nouveautés de Windows Server version 1803](whats-new-in-windows-server-1803.md). Regardez [les conteneurs sur Windows](https://docs.microsoft.com/virtualization/windowscontainers/about/) si vous êtes intéressé par un serveur Windows, version 1803, le conteneur en cours d’exécution. 

Sauf indication contraire, chaque problème signalé s’applique à toutes les éditions et options d’installation de Windows Server, version 1803.  

Nous mettons à jour en permanence cet article. Si des problèmes connus sont identifiés, nous allons les documenter ici. 


## Le centre de données défini par logiciel

Fonctionnalités de centre de données défini par logiciel, telles que les espaces de stockage Direct, défini par logiciel mise en réseau et protéger les machines virtuelles ne sont pas incluses dans Windows Server, version 1803. Comme décrit dans [la mise à jour de Windows Server (canal semi-annuel)](https://cloudblogs.microsoft.com/windowsserver/2018/03/29/windows-server-semi-annual-channel-update/), le canal semi-annuel de Windows Server se concentre sur les conteneurs et les scénarios d’application qui bénéficient d’une innovation plus rapide. 

Si vous avez besoin de rôles d’infrastructure, utilisez une version de canal de maintenance à long terme: Windows Server 2016 (désormais disponible) ou [Windows Server 2019](https://cloudblogs.microsoft.com/windowsserver/2018/03/20/introducing-windows-server-2019-now-available-in-preview) (sera disponible plus tard cette année).

Nous nous sommes engagés à la création de la meilleure plate-forme d’une infrastructure hyperconvergée, et nous continuons à développer de nouvelles fonctionnalités et améliorer la fonction de vos commentaires en cours. Nous vous remercions de nous aider à accéder à [plus de 10 000 clusters d’espaces de stockage Direct](https://blogs.technet.microsoft.com/filecab/2018/03/27/storage-spaces-direct-momentum)! Nous sommes impatients de partager plus plus tard cette année.