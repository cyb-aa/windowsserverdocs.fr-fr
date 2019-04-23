---
title: FTP literal_1
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fb81aa2d-07fa-4e79-bf44-1fb5526fdf14 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d50c6356dfa56a4a1c22c09b08dffc6b3a514c4a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879260"
---
# <a name="ftp-literal1"></a>ftp: literal_1

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 envoie textuellement des arguments au serveur ftp distant. Un code de réponse ftp unique est renvoyé.   

## <a name="syntax"></a>Syntaxe  
```  
literal <Argument> [ ]  
```  
### <a name="parameters"></a>Paramètres  
|Paramètre|Description|  
|-------|--------|  
|<Argument>|Spécifie l’argument à envoyer au serveur ftp.|  
## <a name="remarks"></a>Notes  
Le **littéral** commande est identique à la **devis** commande.  
## <a name="BKMK_Examples"></a>Exemples  
Envoyer un **quitter** commande au serveur ftp distant.  
```  
literal quit  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   [ftp: quote](ftp-quote.md)  
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)  
