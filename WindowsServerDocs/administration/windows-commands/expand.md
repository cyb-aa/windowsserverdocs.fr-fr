---
title: expand
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 66de0488-a0c4-40c2-9b03-e40c107ba343
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4a88222ffe9ff374626a6406c330a6660d992f96
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377323"
---
# <a name="expand"></a>expand

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

développe un ou plusieurs fichiers compressés. Vous pouvez utiliser cette commande pour récupérer des fichiers compressés à partir de disques de distribution.  
## <a name="syntax"></a>Syntaxe  
```  
expand [/r] <source> <destination>  
expand /r <source> [<destination>]  
expand /i <source> [<destination>]  
expand /d <source>.cab [/f:<files>]  
expand <source>.cab /f:<files> <destination>  
```  
### <a name="parameters"></a>Paramètres  

|  Paramètre  |                                                                                                                                                                   Description                                                                                                                                                                    |
|-------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     /r      |                                                                                                                                                             renomme les fichiers développés.                                                                                                                                                              |
|   source    |                                                                              Spécifie les fichiers à développer. La *source* peut se composer d’une lettre de lecteur et du signe deux-points, d’un nom de répertoire, d’un nom de fichier ou d’une combinaison de ces deux. Vous pouvez utiliser des caractères génériques ( **\\** \* ou **?** ).                                                                               |
| destination | Spécifie l’emplacement où les fichiers doivent être développés.<br /><br />Si la *source* est composée de plusieurs fichiers et que vous ne spécifiez pas **/r**, la *destination* doit être un répertoire.<br /><br />La *destination* peut se composer d’une lettre de lecteur et du signe deux-points, d’un nom de répertoire, d’un nom de fichier ou d’une combinaison de ces deux.<br /><br />Spécification du &#124; chemin d’accès au fichier de destination. |
|     /i      |                                                                                                   renomme les fichiers développés, mais ignore la structure de répertoires.<br /><br />Ce paramètre s’applique à :  Windows Server 2008 R2 et Windows 7.                                                                                                    |
|     /d      |                                                                                                                              Affiche la liste des fichiers dans l’emplacement source. Ne développe pas ou n’extrait pas les fichiers.                                                                                                                              |
|     /f:     |                                                                                                                 Spécifie les fichiers dans un fichier cabinet (. cab) que vous souhaitez développer. Vous pouvez utiliser des caractères génériques ( **\\** \* ou **?** ).                                                                                                                 |
|     /?      |                                                                                                                                                       Affiche l'aide à l'invite de commandes.                                                                                                                                                       |

## <a name="remarks"></a>Notes  
- Utilisation de l’option **développer** sur la console de récupération  
  La commande de **développement** , avec des paramètres différents, est disponible à partir de la console de récupération. Pour plus d’informations sur la console de récupération, voir l' [article 314058](https://support.microsoft.com/kb/314058) de la base de connaissances Microsoft.  
  ## <a name="additional-references"></a>Références supplémentaires  
  [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
