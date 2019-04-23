---
title: Vue d’ensemble des espaces de stockage
ms.prod: windows-server-threshold
ms.author: jgerend
ms.manager: dougkim
ms.technology: storage-file-systems
ms.topic: article
author: jasongerend
ms.date: 05/22/2018
ms.openlocfilehash: 9977bb35be3676e31cdcab7322b5b5a2cfc67609
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832910"
---
# <a name="storage-spaces-overview"></a>Vue d’ensemble des espaces de stockage

Espaces de stockage est une technologie de Windows et Windows Server qui peuvent aider à protéger vos données contre les défaillances de disque. Il est conceptuellement semblable à RAID, implémenté dans le logiciel. Vous pouvez utiliser des espaces de stockage pour regrouper au moins trois lecteurs ensemble dans un pool de stockage, puis capacité à partir de ce pool pour créer des espaces de stockage. Ces derniers stockent généralement les copies supplémentaires de vos données si un de vos lecteurs échoue, vous donc toujours une copie intacte de vos données. Si vous exécutez une faible capacité, ajoutez simplement des disques au pool de stockage.

Il existe quatre manières principales pour utiliser les espaces de stockage :

- **Sur un PC Windows** : pour plus d’informations, consultez [espaces de stockage dans Windows 10](http://windows.microsoft.com/en-us/windows-10/storage-spaces-windows-10).
- **Sur un serveur autonome avec tout le stockage dans un seul serveur** : pour plus d’informations, consultez [déployer des espaces de stockage sur un serveur autonome](deploy-standalone-storage-spaces.md).
- **Sur un serveur en cluster à l’aide d’espaces de stockage Direct avec stockage en attachement direct local dans chaque nœud du cluster** : pour plus d’informations, consultez [vue d’ensemble des espaces de stockage Direct](storage-spaces-direct-overview.md).
- **Sur un serveur en cluster avec un ou plusieurs partagé de stockage SAS contenant tous les lecteurs** : pour plus d’informations, consultez [espaces de stockage sur un cluster avec une vue d’ensemble SAS partagé](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831739(v%3dws.11)).

