---
title: expand
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 5df078af23c77f54ccb2da83b1057c5d7042593a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825960"
---
# <a name="expand"></a>expand

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
|Paramètre|Description|  
|-------|--------|  
|/r|Renomme les fichiers décompressés.|  
|source|Spécifie les fichiers à développer. *Source* peut se composer d’une lettre de lecteur et deux-points, un nom de répertoire, un nom de fichier ou une combinaison de ces éléments. Vous pouvez utiliser des caractères génériques (**\*** ou **?**).|  
|destination|Spécifie où les fichiers doivent être développé.<br /><br />Si *source* se compose de plusieurs fichiers et vous ne spécifiez pas **/r**, *destination* doit être un répertoire.<br /><br />*Destination* peut se composer d’une lettre de lecteur et deux-points, un nom de répertoire, un nom de fichier ou une combinaison de ces éléments.<br /><br />Fichier de destination &#124; spécification de chemin d’accès.|  
|/i|Renomme les fichiers développés, mais ignore la structure de répertoires.<br /><br />Ce paramètre s’applique à :  Windows Server 2008 R2 et Windows 7.|  
|/d|Affiche une liste de fichiers dans l’emplacement source. Ne pas développer ou extrayez les fichiers.|  
|/f:|Spécifie les fichiers dans un fichier CAB (.cab) que vous souhaitez développer. Vous pouvez utiliser des caractères génériques (**\*** ou **?**).|  
|/?|Affiche l'aide à l'invite de commandes.|  
## <a name="remarks"></a>Notes  
-   À l’aide de **développez** à la Console de récupération  
    Le **développez** commande, avec des paramètres différents, est disponible à partir de la Console de récupération. Pour plus d’informations sur la Console de récupération, consultez [article 314058](https://support.microsoft.com/kb/314058) dans la Base de connaissances Microsoft.  
## <a name="additional-references"></a>Références supplémentaires  
[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)  
