---
title: FTP rename
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 977b7c95-6428-4980-80ec-79c3ae7e8c4d vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 80d1a15f038017444c7654a44748bfd22be8e487
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438384"
---
# <a name="ftp-rename"></a>FTP : renommer

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Renomme les fichiers à distance.   
## <a name="syntax"></a>Syntaxe  
```  
rename <FileName> <NewFileName>  
```  
### <a name="parameters"></a>Paramètres  

|   Paramètre   |                 Description                 |
|---------------|---------------------------------------------|
|  <FileName>   | Spécifie le fichier que vous souhaitez renommer. |
| <NewFileName> |        Spécifie le nouveau nom de fichier.         |

## <a name="BKMK_Examples"></a>Exemples  
Renommez le fichier distant **example.txt** à **example1.txt**  
```  
rename example.txt example1.txt  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
