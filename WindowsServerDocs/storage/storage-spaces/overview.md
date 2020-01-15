---
title: Vue d’ensemble des espaces de stockage
ms.prod: windows-server
ms.author: jgerend
ms.manager: dougkim
ms.technology: storage-file-systems
ms.topic: article
author: jasongerend
ms.date: 05/22/2018
ms.openlocfilehash: cab92de2a96f1d44c8ad6a33e84aba08cad38837
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75950239"
---
# <a name="storage-spaces-overview"></a>Vue d’ensemble des espaces de stockage

Les espaces de stockage sont une technologie de Windows et de Windows Server qui peut vous aider à protéger vos données contre les défaillances de lecteurs. Elle est conceptuellement similaire à la technologie RAID, implémentée dans le logiciel. Vous pouvez utiliser des espaces de stockage pour regrouper au moins trois lecteurs dans un pool de stockage, puis utiliser la capacité de ce pool pour créer des espaces de stockage. Celles-ci stockent généralement des copies supplémentaires de vos données. ainsi, si l’un de vos lecteurs échoue, vous disposez toujours d’une copie intacte de vos données. Si vous manquez de capacité, ajoutez simplement d’autres lecteurs au pool de stockage.

Il existe quatre façons principales d’utiliser des espaces de stockage :

- **Sur un PC Windows** : pour plus d’informations, consultez [espaces de stockage dans Windows 10](https://windows.microsoft.com/windows-10/storage-spaces-windows-10).
- **Sur un serveur autonome avec tout le stockage sur un seul serveur** : pour plus d’informations, consultez [déployer des espaces de stockage sur un serveur autonome](deploy-standalone-storage-spaces.md).
- **Sur un serveur en cluster à l’aide d’espaces de stockage direct avec un stockage en attachement direct local dans chaque nœud de cluster** -pour plus d’informations, consultez [espaces de stockage direct vue d’ensemble](storage-spaces-direct-overview.md).
- **Sur un serveur en cluster avec un ou plusieurs boîtiers de stockage SAP partagés contenant tous les lecteurs** -pour plus d’informations, consultez [espaces de stockage sur un cluster avec la vue d’ensemble des SAP partagés](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831739(v%3dws.11)).

