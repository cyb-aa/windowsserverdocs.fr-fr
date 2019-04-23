---
title: 'Notes de publication : problèmes importants dans Windows Server, version 1803'
description: En savoir plus sur les problèmes connus, les limitations ou les autres informations que vous avez besoin avant d’installer Windows Server, version 1803
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868500"
---
# <a name="release-notes-important-issues-in-windows-server-version-1803"></a>Notes de publication : Problèmes importants dans Windows Server, version 1803

>S'applique à : Canal semi-annuel Windows Server

Ces notes de publication résument les problèmes les plus critiques dans le système d’exploitation Windows Server, y compris les façons d’éviter ou résoudre les problèmes connus. Pour en savoir plus sur les nouvelles fonctionnalités dans cette version, consultez [What ' s New in Windows Server version 1803](whats-new-in-windows-server-1803.md). Découvrez [les conteneurs sur Windows](https://docs.microsoft.com/virtualization/windowscontainers/about/) si vous êtes intéressé par un serveur Windows, version 1803, le conteneur en cours d’exécution. 

Sauf indication contraire, chaque problème signalé s’applique à toutes les éditions et options d’installation de Windows Server, version 1803.  

En permanence, nous mettre à jour cet article. Si les problèmes connus sont identifiés, nous allons ici les documenter. 


## <a name="software-defined-datacenter"></a>Centre de données défini par logiciel

Fonctionnalités de centre de données défini par logiciel telles que les espaces de stockage Direct, défini par logiciel de mise en réseau, protégées et d’ordinateurs virtuels ne sont pas inclus dans Windows Server, version 1803. Comme décrit dans [mise à jour de Windows Server semi-annuel canal](https://cloudblogs.microsoft.com/windowsserver/2018/03/29/windows-server-semi-annual-channel-update/), le canal semi-annuel de Windows Server se concentre sur les conteneurs et les scénarios d’application qui bénéficient des dernières innovations plus rapidement. 

Si vous avez besoin de rôles d’infrastructure, utilisez une version de canal de maintenance à long terme : Windows Server 2016 (désormais disponible) ou [Windows Server 2019](https://cloudblogs.microsoft.com/windowsserver/2018/03/20/introducing-windows-server-2019-now-available-in-preview) (prochainement).

Nous nous engageons à la création de la plateforme idéale pour l’infrastructure Hyper-convergée, et nous continuons à développer de nouvelles fonctionnalités et améliorer les existants en fonction de vos commentaires. Merci de nous aider à obtenir pour [plus de 10 000 clusters d’espaces de stockage Direct](https://blogs.technet.microsoft.com/filecab/2018/03/27/storage-spaces-direct-momentum)! Nous sommes impatients de partage plus plus tard cette année.