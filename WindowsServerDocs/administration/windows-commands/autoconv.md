---
title: autoconv
description: La rubrique commandes Windows pour **autoconv**, qui convertit les volumes FAT (File Allocation Table) et FAT32 dans le système de fichiers NTFS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 17281e54-0b18-4e84-94ac-24586c82df4e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fe0e388a1d4fd79567ef0562197e3181bbbc46f4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851112"
---
# <a name="autoconv"></a>autoconv

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Convertit les volumes FAT (File Allocation Table) et FAT32 dans le système de fichiers NTFS, en laissant les fichiers et répertoires existants intacts au démarrage après l’exécution de **Autochk** . les volumes convertis dans le système de fichiers NTFS ne peuvent pas être reconvertis en FAT ou FAT32.

## <a name="remarks"></a>Notes

Vous ne pouvez pas exécuter **autoconv** sur la ligne de commande. Cette opération est exécutée uniquement au démarrage, si elle est définie via **Convert. exe**.

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [autochk](autochk.md)

- [convert](convert.md)

- [Utilisation des systèmes de fichiers](https://go.microsoft.com/fwlink/?LinkId=4509)
