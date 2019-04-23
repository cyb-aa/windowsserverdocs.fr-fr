---
title: autoconv
description: Rubrique de commandes de Windows pour **conversion** - convertit de fichiers Fat (allocation table) et le système de fichiers Fat32 des volumes au format NTFS.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 17281e54-0b18-4e84-94ac-24586c82df4e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1d135da085558f12a51c8febfd72aa805e1d12f1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872490"
---
# <a name="autoconv"></a>autoconv

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

convertit de fichiers Fat (allocation table) et les volumes Fat32 au système de fichiers NTFS, conservant les fichiers existants et des répertoires au démarrage après **autochk** s’exécute. les volumes convertis au système de fichiers NTFS ne peut pas être reconverties en Fat ou Fat32.
## <a name="remarks"></a>Notes
Vous ne pouvez pas exécuter **conversion** sur la ligne de commande. Cela n’est exécutée au démarrage, si défini par le biais **convert.exe**.
## <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[autochk](autochk.md)
[convertir](convert.md)
[fonctionne avec les systèmes de fichiers](https://go.microsoft.com/fwlink/?LinkId=4509)
