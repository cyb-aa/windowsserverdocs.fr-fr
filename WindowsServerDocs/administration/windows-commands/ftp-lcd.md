---
title: écran LCD FTP
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 60a25808-6abb-408b-8373-0bbdcd0994b4 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 646cbfe3feadb63388694218758dae165ffb49c4
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725261"
---
# <a name="ftp-lcd"></a>FTP : LCD

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

modifie le répertoire de travail sur l’ordinateur local. Par défaut, le répertoire de travail est le répertoire dans lequel **FTP** a été démarré.   
## <a name="syntax"></a>Syntaxe  
```  
lcd [<directory>]  
```  
#### <a name="parameters"></a>Paramètres  
|Paramètre|Description|  
|-------|--------|  
|[<directory>]|Spécifie le répertoire sur l’ordinateur local à modifier. Si le *répertoire* n’est pas spécifié, le répertoire de travail actuel est remplacé par le répertoire par défaut.|  
## <a name="examples"></a>Exemples  
Remplacez le répertoire de travail de l’ordinateur local par **C:\Dir1**  
```  
lcd C:\dir1  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
