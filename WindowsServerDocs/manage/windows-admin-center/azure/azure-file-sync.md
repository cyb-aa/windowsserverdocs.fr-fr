---
title: Synchroniser votre serveur de fichiers avec le service cloud à l’aide de la synchronisation de fichier Azure
description: Synchronisation de fichier Azure permet de centraliser les partages de fichiers de votre organisation dans Azure, tout en conservant la flexibilité, performances et la compatibilité d’un serveur de fichiers local. Synchronisation de fichier Azure transforme Windows Server dans un cache rapide de votre partage de fichiers Azure avec la fonctionnalité de hiérarchisation cloud facultatif.
ms.technology: manage
ms.topic: article
author: fauhse
ms.author: fauhse
ms.date: 04/12/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 93cfa469a3c14410d95b0b224cbf59b913ec9f7e
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296903"
---
# Synchroniser votre serveur de fichiers avec le service cloud à l’aide de la synchronisation de fichier Azure

>S’applique à: Windows Admin Center, Windows Admin Center Preview

Synchronisation de fichier Azure permet de centraliser les partages de fichiers de votre organisation dans Azure, tout en conservant la flexibilité, performances et la compatibilité d’un serveur de fichiers local. Synchronisation de fichier Azure transforme Windows Server dans un cache rapide de votre partage de fichiers Azure avec la fonctionnalité de hiérarchisation cloud facultatif. Vous pouvez utiliser n’importe quel protocole qui est disponible sur Windows Server pour accéder à vos données en local, notamment SMB, NFS et FTPS.

Une fois que vos fichiers ont été synchronisées sur le cloud, vous pouvez vous connecter plusieurs serveurs dans le même partage de fichiers Azure pour la synchronisation et mettre en cache le contenu localement, les autorisations (ACL) sont acheminées toujours également. Fichiers Azure offre une fonctionnalité de capture instantanée qui peut de générer des captures instantanées différentielles de votre partage de fichier Azure. Ces instantanés peuvent même être montés en tant que lecteurs réseau en lecture seule via SMB à faciliter la navigation et restauration. Combiné à cloud hiérarchisation, un serveur de fichiers local en cours d’exécution n’a jamais été plus facile.

Pour plus d’informations, consultez [planification d’un déploiement de synchronisation de fichier Azure](https://aka.ms/afs).