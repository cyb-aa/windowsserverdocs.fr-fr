---
title: Gérer les disques
description: Cet article explique comment gérer des disques
ms.date: 06/07/2019
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 0aac0b78e79949de94ebd20912b8c2b2db167339
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385870"
---
# <a name="manage-disks"></a>Gérer les disques

> **S’applique à :** Windows 10, Windows 8.1, Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette rubrique et ses sous-rubriques abordent l’utilisation de l’outil Gestion des disques pour gérer les disques d’un ordinateur. Elles incluent également des informations sur l’initialisation de nouveaux disque, la conversion de disques entre différents styles de partition et sur la façon dont Windows gère l’état en ligne de nouveaux disques.

## <a name="online-and-offline-status"></a>État en ligne et hors ligne

La gestion des disques affiche si un disque est en ligne (disponible) ou hors ligne.

Dans Windows, par défaut, tous les disques nouvellement détectés sont mis en ligne avec un accès en lecture et écriture. Dans Windows Server, par défaut, les disques nouvellement détectés sont mis en ligne avec un accès en lecture et écriture, sauf s’ils se trouvent sur un bus partagé (par exemple, SCSI, iSCSI, Serial Attached SCSI ou Fibre Channel). Les disques qui se trouvent sur un bus partagé seront hors connexion la première fois qu’ils sont détectés.

Si un disque est hors connexion, vous devez le mettre en ligne avant de l’initialiser ou de créer des volumes sur celui-ci.

Pour mettre un disque en ligne ou hors connexion, cliquez avec le bouton droit sur son nom, puis choisissez l’action appropriée.

## <a name="see-also"></a>Voir aussi

-   [Initialiser les nouveaux disques](initialize-new-disks.md)
-   [Déplacer des disques vers un autre ordinateur](move-disks-to-another-computer.md)
-   [Convertir un disque dynamique en disque de base](change-a-dynamic-disk-back-to-a-basic-disk.md)
-   [Convertir un disque d’enregistrement de démarrage principal en disque de table de partition GUID](change-an-mbr-disk-into-a-gpt-disk.md)
-   [Convertir un disque de table de partition GUID en disque d’enregistrement de démarrage principal](change-a-gpt-disk-into-an-mbr-disk.md)
-   [Gérer les disques durs virtuels](manage-virtual-hard-disks.md)