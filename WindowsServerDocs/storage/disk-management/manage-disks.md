---
title: Gérer les disques
description: Cet article explique comment gérer les disques
ms.date: 06/07/2019
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 344dd363e970b195abe20fcb69e741c450fc7a21
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812411"
---
# <a name="manage-disks"></a>Gérer les disques

> **S’applique à :** Windows 10, Windows 8.1, Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette rubrique et ses sous-rubriques traite de l’utilisation de la gestion des disques pour gérer les disques sur un ordinateur et inclut des informations sur l’initialisation des nouveaux disques, conversion de disques entre les styles de partition différente, et la façon dont Windows gère l’état en ligne de nouveaux disques.

## <a name="online-and-offline-status"></a>État en ligne et hors ligne

Gestion des disques affiche si un disque est en ligne (disponible), ou en mode hors connexion.

Dans Windows, par défaut, tous les disques nouvellement détectés sont mis en ligne avec un accès en lecture et écriture. Dans Windows Server, par défaut, les disques nouvellement détectés sont mis en ligne avec un accès en lecture et écriture, sauf s’ils se trouvent sur un bus partagé (par exemple, SCSI, iSCSI, Serial Attached SCSI ou Fibre Channel). Disques sur un bus partagé sont hors connexion, la première fois qu’elles sont détectées.

Si un disque est hors connexion, vous devez le mettre en ligne avant de l’initialiser ou de créer des volumes sur celui-ci.

Pour mettre un disque en ligne ou hors connexion, cliquez sur le nom du disque, puis en choisissant l’action appropriée.

## <a name="see-also"></a>Voir aussi

-   [Initialiser les nouveaux disques](initialize-new-disks.md)
-   [Déplacez les disques vers un autre ordinateur](move-disks-to-another-computer.md)
-   [Convertir un disque dynamique en disque de base](change-a-dynamic-disk-back-to-a-basic-disk.md)
-   [Modifier un disque MBR en un disque de Table de Partition GUID](change-an-mbr-disk-into-a-gpt-disk.md)
-   [Modifier un disque de Table de Partition GUID en disque Master Boot Record](change-a-gpt-disk-into-an-mbr-disk.md)
-   [Gérer les disques durs virtuels](manage-virtual-hard-disks.md)