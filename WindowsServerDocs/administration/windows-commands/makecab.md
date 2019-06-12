---
title: makecab
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4da95297-c593-427b-9f76-2f389c46cbf4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7b120cf990abe2024fd6c96ca2f1ef11fa2350ae
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437531"
---
# <a name="makecab"></a>makecab

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Empaqueter les fichiers existants dans un fichier CAB (.cab).
## <a name="syntax"></a>Syntaxe
```
makecab [/v[n]] [/d var=<value> ...] [/l <dir>] <source> [<destination>]
makecab [/v[<n>]] [/d var=<value> ...] /f <directives_file> [...]
```
### <a name="parameters"></a>Paramètres

|      Paramètre       |                                                                        Description                                                                        |
|----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
|       <source>       |                                                                     Fichier à compresser.                                                                     |
|    <destination>     | Nom de fichier à donner de fichier compressé. Si omis, le dernier caractère du nom de fichier source est remplacé par un trait de soulignement (_) et utilisé comme destination. |
| /f <directives_file> |                                                   Un fichier avec **makecab** directives (peut être répété).                                                   |
|    /d var=<value>    |                                                          Définit la variable avec la valeur spécifiée.                                                           |
|       /l <dir>       |                                               Emplacement des destination (valeur par défaut est le répertoire actif).                                               |
|       /v[<n>]        |                                                    Définir le niveau de commentaires de débogage (0 = none,..., 3 = full).                                                     |
|          /?          |                                                           Affiche l'aide à l'invite de commandes.                                                            |

## <a name="remarks"></a>Notes
-   Reportez-vous à [format de Microsoft Cabinet](https://go.microsoft.com/fwlink/?LinkId=226852) sur MSDN pour plus d’informations sur directive_file.

## <a name="additional-references"></a>Références supplémentaires
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

