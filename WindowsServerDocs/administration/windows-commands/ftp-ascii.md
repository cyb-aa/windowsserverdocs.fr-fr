---
title: ftp ascii
description: 'Rubrique de commandes de Windows pour ftp ascii '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 523be48e-eab0-4237-8fb5-ca222824f0b6 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8e38c57b7a5ffd9afe677c4b49787383412621fe
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438799"
---
# <a name="ftp-ascii"></a>ftp: ascii

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Définit le type de transfert de fichier ASCII.   
## <a name="syntax"></a>Syntaxe  
```  
ascii  
```  
### <a name="parameters"></a>Paramètres  
aucune  
## <a name="remarks"></a>Notes  
- Le type de transfert de fichier par défaut est ASCII.  
- En mode ASCII, les conversions de caractères vers et depuis le jeu de caractères standard de réseau sont effectuées. Par exemple, les caractères de fin de ligne sont convertis en fonction des besoins, selon le système d’exploitation cible.  
- **FTP** prend en charge ASCII et types de transfert de fichiers image binaires. Utilisez ASCII lors du transfert de fichiers texte. Pour plus d’informations sur le transfert de fichiers binaires, consultez **ftp : binaire** dans des références supplémentaires.  
  ## <a name="BKMK_Examples"></a>Exemples  
  La valeur du type de transfert de fichier ASCII.  
  ```  
  ascii  
  ```  
  ## <a name="additional-references"></a>Références supplémentaires  
- [ftp: binary](ftp-binary.md)  
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
