---
title: autoconv
description: Rubrique de référence pour la commande autoconv, qui convertit les volumes FAT (File Allocation Table) et FAT32 au système de fichiers NTFS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 17281e54-0b18-4e84-94ac-24586c82df4e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ea8f55270435c6632be2e527569b4a4b4ca81136
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718775"
---
# <a name="autoconv"></a>autoconv

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Convertit les volumes FAT (File Allocation Table) et FAT32 dans le système de fichiers NTFS, en laissant les fichiers et répertoires existants intacts au démarrage après l’exécution de **Autochk** . les volumes convertis dans le système de fichiers NTFS ne peuvent pas être reconvertis en FAT ou FAT32.

> [!IMPORTANT]
> Vous ne pouvez pas exécuter **autoconv** à partir de la ligne de commande. Cela peut s’exécuter uniquement au démarrage, si elle est définie via **Convert. exe**.

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Autochk](autochk.md)

- [commande Convert](convert.md)
