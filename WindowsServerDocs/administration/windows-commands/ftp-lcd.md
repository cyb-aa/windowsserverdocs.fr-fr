---
title: FTP lcd
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 60a25808-6abb-408b-8373-0bbdcd0994b4 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: eac028c8aa675e680dedefcfe9f0b8da18ce7179
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826890"
---
# <a name="ftp-lcd"></a>ftp: lcd

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

change le répertoire de travail sur l’ordinateur local. Par défaut, le répertoire de travail est le répertoire dans lequel **ftp** a été démarré.   
## <a name="syntax"></a>Syntaxe  
```  
lcd [<directory>]  
```  
### <a name="parameters"></a>Paramètres  
|Paramètre|Description|  
|-------|--------|  
|[<directory>]|Spécifie le répertoire sur l’ordinateur local vers lequel basculer. Si *directory* n’est pas spécifiée, le répertoire de travail actuel est modifié dans le répertoire par défaut.|  
## <a name="BKMK_Examples"></a>Exemples  
modifier le répertoire de travail sur l’ordinateur local à **C:\dir1**  
```  
lcd C:\dir1  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)  
