---
title: développer
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 66de0488-a0c4-40c2-9b03-e40c107ba343
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0ab4b9bcb5c9da2aeace71a8e535fa762d6d6e95
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80844872"
---
# <a name="expand"></a>développer

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

développe un ou plusieurs fichiers compressés. Vous pouvez utiliser cette commande pour récupérer des fichiers compressés à partir de disques de distribution.  
## <a name="syntax"></a>Syntaxe  
```  
expand [/r] <source> <destination>  
expand /r <source> [<destination>]  
expand /i <source> [<destination>]  
expand /d <source>.cab [/f:<files>]  
expand <source>.cab /f:<files> <destination>  
```  
#### <a name="parameters"></a>Paramètres  

|  Paramètre  |                                                                                                                                                                   Description                                                                                                                                                                    |
|-------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     /r      |                                                                                                                                                             renomme les fichiers développés.                                                                                                                                                              |
|   source    |                                                                              Spécifie les fichiers à développer. La *source* peut se composer d’une lettre de lecteur et du signe deux-points, d’un nom de répertoire, d’un nom de fichier ou d’une combinaison de ces deux. Vous pouvez utiliser des caractères génériques ( **\\** \* ou **?** ).                                                                               |
| destination | Spécifie l’emplacement où les fichiers doivent être développés.<p>Si la *source* est composée de plusieurs fichiers et que vous ne spécifiez pas **/r**, la *destination* doit être un répertoire.<p>La *destination* peut se composer d’une lettre de lecteur et du signe deux-points, d’un nom de répertoire, d’un nom de fichier ou d’une combinaison de ces deux.<p>Spécification du &#124; chemin d’accès au fichier de destination. |
|     /i      |                                                                                                   renomme les fichiers développés, mais ignore la structure de répertoires.<p>Ce paramètre s’applique à : Windows Server 2008 R2 et Windows 7.                                                                                                    |
|     /d      |                                                                                                                              Affiche la liste des fichiers dans l’emplacement source. Ne développe pas ou n’extrait pas les fichiers.                                                                                                                              |
|     /f:     |                                                                                                                 Spécifie les fichiers dans un fichier cabinet (. cab) que vous souhaitez développer. Vous pouvez utiliser des caractères génériques ( **\\** \* ou **?** ).                                                                                                                 |
|     /?      |                                                                                                                                                       Affiche l'aide à l'invite de commandes.                                                                                                                                                       |

## <a name="remarks"></a>Notes  
- Utilisation de l’option **développer** sur la console de récupération  
  La commande de **développement** , avec des paramètres différents, est disponible à partir de la console de récupération. Pour plus d’informations sur la console de récupération, voir l' [article 314058](https://support.microsoft.com/kb/314058) de la base de connaissances Microsoft.  
  ## <a name="additional-references"></a>Références supplémentaires  
  - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
