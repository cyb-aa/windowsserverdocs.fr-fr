---
title: FTP binaire
description: Rubrique de commandes de Windows pour le binaire de ftp
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ee925b4d-85d2-47b1-b7d6-3832b7ec5505 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cadd59bff3bd2acf5c6d700caef66ca5c871b523
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821920"
---
# <a name="ftp-binary"></a>ftp: binary

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Définit le type de transfert de fichier binaire.   
## <a name="syntax"></a>Syntaxe  
```  
binary  
```  
### <a name="parameters"></a>Paramètres  
aucune  
## <a name="remarks-optional-section"></a>Remarques <optional section>  
**FTP** prend en charge ASCII et types de transfert de fichiers image binaires. Utilisez le fichier binaire lors du transfert de fichiers exécutables. En mode binaire, les fichiers sont transférés en unités de 1 octet. Pour plus d’informations sur le transfert de fichiers ASCII, consultez **ftp : ascii** dans des références supplémentaires.  
## <a name="BKMK_Examples"></a>Exemples  
Définir le type de transfert de fichier binaire.  
```  
binary  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   [ftp: ascii](ftp-ascii.md)  
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)  
