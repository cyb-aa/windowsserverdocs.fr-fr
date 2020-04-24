---
title: Notes de publication - Points importants sur Windows Server, version 1803
description: Découvrez les problèmes connus, les limitations ou autres informations dont vous avez besoin avant d’installer Windows Server, version 1803.
ms.prod: windows-server
ms.date: 05/07/2018
ms.technology: server-general
ms.topic: article
author: lizap
ms.author: elizapo
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: a511c9888f27fe97fdeaf65cde022e5c2aa999d4
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2020
ms.locfileid: "80826082"
---
# <a name="release-notes-important-issues-in-windows-server-version-1803"></a>Notes de publication : Points importants dans Windows Server, version 1803

>S'applique à : Canal semi-annuel Windows Server

Ces notes de publication résument les problèmes les plus critiques du système d’exploitation Windows Server et expliquent comment éviter ou contourner les problèmes connus. Pour en savoir plus sur les nouvelles fonctionnalités dans cette version, consultez [Nouveautés de Windows Server, version 1803](whats-new-in-windows-server-1803.md). Consultez [À propos des conteneurs Windows](https://docs.microsoft.com/virtualization/windowscontainers/about/) si vous souhaitez exécuter un conteneur Windows Server, version 1803. 

Sauf mention contraire, chaque problème signalé s’applique à toutes les éditions et options d’installation de Windows Server, version 1803.  

Nous mettons à jour cet article de manière continue. Si des problèmes connus sont identifiés, nous les documenterons ici. 


## <a name="software-defined-datacenter"></a>Centre de données à définition logicielle

Les fonctionnalités de centre de données à définition logicielle telles que les espaces de stockage direct, SDN (Software-Defined Networking) et les machines virtuelles dotées d’une protection maximale ne sont pas incluses dans Windows Server, version 1803. Comme décrit dans [Mise à jour du canal semi-annuel Windows Server](https://cloudblogs.microsoft.com/windowsserver/2018/03/29/windows-server-semi-annual-channel-update/), le canal semi-annuel de Windows Server est axé sur les conteneurs et les scénarios d’application bénéficiant d’une innovation plus rapide. 

Si vous avez besoin de rôles d’infrastructure, utilisez une version de canal de maintenance à long terme : Windows Server 2016 (disponible dès maintenant) ou [Windows Server 2019](https://cloudblogs.microsoft.com/windowsserver/2018/03/20/introducing-windows-server-2019-now-available-in-preview) (disponible dans le courant de l’année).

Nous nous engageons à créer la plateforme la plus performante possible pour l’infrastructure hyperconvergée, et nous continuons à développer de nouvelles fonctionnalités et à améliorer les fonctionnalités existantes en fonction de vos commentaires. Merci de nous aider à obtenir [plus de 10 000 clusters d’espaces de stockage direct](https://blogs.technet.microsoft.com/filecab/2018/03/27/storage-spaces-direct-momentum) ! Nous sommes impatients de pouvoir partager plus dans le courant de l’année.