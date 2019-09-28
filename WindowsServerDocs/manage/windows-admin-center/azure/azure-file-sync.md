---
title: Synchronisez votre serveur de fichiers avec le Cloud à l’aide de Azure File Sync
description: Utilisez Azure File Sync pour centraliser les partages de fichiers de votre organisation dans Azure, tout en conservant la flexibilité, les performances et la compatibilité d’un serveur de fichiers local. Azure File Sync transforme Windows Server en un cache rapide de votre partage de fichiers Azure avec la fonctionnalité de hiérarchisation Cloud facultative.
ms.technology: manage
ms.topic: article
author: fauhse
ms.author: fauhse
ms.date: 04/12/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: fe0e3337962b7d9c2f025a9d4eba826f3349c1f9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357378"
---
# <a name="sync-your-file-server-with-the-cloud-by-using-azure-file-sync"></a>Synchronisez votre serveur de fichiers avec le Cloud à l’aide de Azure File Sync

>S’applique à : Windows Admin Center, Windows Admin Center Preview

Utilisez Azure File Sync pour centraliser les partages de fichiers de votre organisation dans Azure, tout en conservant la flexibilité, les performances et la compatibilité d’un serveur de fichiers local. Azure File Sync transforme Windows Server en un cache rapide de votre partage de fichiers Azure avec la fonctionnalité de hiérarchisation Cloud facultative. Vous pouvez utiliser n’importe quel protocole disponible sur Windows Server pour accéder à vos données localement, notamment SMB, NFS et FTPS.

Une fois vos fichiers synchronisés dans le Cloud, vous pouvez connecter plusieurs serveurs au même partage de fichiers Azure pour synchroniser et mettre en cache le contenu localement (les autorisations (ACL) sont toujours transportées). Azure Files offre une fonctionnalité de capture instantanée qui peut générer des instantanés différentiels de votre partage de fichiers Azure. Ces instantanés peuvent même être montés en tant que lecteurs réseau en lecture seule via SMB pour faciliter la navigation et la restauration. Combiné à la hiérarchisation Cloud, l’exécution d’un serveur de fichiers local n’a jamais été aussi simple.

Pour plus d’informations, consultez [planification d’un déploiement de Azure file Sync](https://aka.ms/afs).