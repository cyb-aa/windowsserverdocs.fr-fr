---
title: put FTP
description: Rubrique relative aux commandes Windows pour FTP-put
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 95cc1e3f-523d-4374-98b8-16e6c276b2ca vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/30/2020
ms.openlocfilehash: d86ba8418f4c43c26f3745a9f70e676ca790c640
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725175"
---
# <a name="ftp-put"></a>FTP : put

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copie un fichier local sur l’ordinateur distant à l’aide du type de transfert de fichier actuel.
## <a name="syntax"></a>Syntaxe
```
put <LocalFile> [<remoteFile>]
```
#### <a name="parameters"></a>Paramètres

|    Paramètre     |                    Description                    |
|------------------|---------------------------------------------------|
|   `<LocalFile>`  |         Spécifie le fichier local à copier.         |
| `[<remoteFile>]` | Spécifie le nom à utiliser sur l’ordinateur distant. |

## <a name="remarks"></a>Notes 
- La commande **put** est identique à la commande **Send** .
- Si *remoteFile* n’est pas spécifié, le nom du *fichier_local* est attribué au fichier.
  ## <a name="examples"></a>Exemples
  Copiez le fichier local **test. txt** et nommez-le **Test1. txt** sur l’ordinateur distant.
  ```
  put test.txt test1.txt
  ```
  Copiez le fichier **Program. exe** local sur l’ordinateur distant.
  ```
  put program.exe
  ```
  ## <a name="additional-references"></a>Références supplémentaires
- [FTP : ASCII](ftp-ascii.md)
- [FTP : binaire](ftp-binary.md)
- - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
