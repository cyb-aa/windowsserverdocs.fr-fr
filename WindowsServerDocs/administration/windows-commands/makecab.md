---
title: makecab
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4da95297-c593-427b-9f76-2f389c46cbf4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 46efbae1d59a85071622df51fc018d0bf73dc504
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80840262"
---
# <a name="makecab"></a>makecab

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Empaquetez les fichiers existants dans un fichier cabinet (. cab).
## <a name="syntax"></a>Syntaxe
```
makecab [/v[n]] [/d var=<value> ...] [/l <dir>] <source> [<destination>]
makecab [/v[<n>]] [/d var=<value> ...] /f <directives_file> [...]
```
#### <a name="parameters"></a>Paramètres

|      Paramètre       |                                                                        Description                                                                        |
|----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
|       <source>       |                                                                     Fichier à compresser.                                                                     |
|    <destination>     | Nom de fichier pour fournir un fichier compressé. En cas d’omission, le dernier caractère du nom du fichier source est remplacé par un trait de soulignement (_) et utilisé comme destination. |
| /f < directives_file > |                                                   Un fichier avec des directives **makecab** (peut être répété).                                                   |
|    /d var =<value>    |                                                          Définit la variable avec la valeur spécifiée.                                                           |
|       /l <dir>       |                                               Emplacement de destination de la destination (le répertoire par défaut est le répertoire actif).                                               |
|       /v [<n>]        |                                                    Définissez le niveau de détail du débogage (0 = aucun,..., 3 = complet).                                                     |
|          /?          |                                                           Affiche l'aide à l'invite de commandes.                                                            |

## <a name="remarks"></a>Notes
-   Reportez-vous au [format Microsoft Cabinet](https://go.microsoft.com/fwlink/?LinkId=226852) sur MSDN pour plus d’informations sur directive_file.

## <a name="additional-references"></a>Références supplémentaires
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

