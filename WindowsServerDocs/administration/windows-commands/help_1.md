---
title: aide
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 75dbf94f-d79c-45b2-9463-c06648218f4a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 404cefe879e8f63678f2cba90516fad9c216eb23
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80842312"
---
# <a name="help"></a>aide

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche la liste des commandes disponibles ou des informations d’aide détaillées sur une commande spécifiée.  
  
  
  
## <a name="syntax"></a>Syntaxe  
  
```  
help [<command>]  
```  
  
### <a name="parameters"></a>Paramètres  
  
| Paramètre |                              Description                              |
|-----------|-----------------------------------------------------------------------|
| <command> | Spécifie la commande pour laquelle afficher des informations détaillées sur l’aide. |
  
## <a name="remarks"></a>Notes  
  
-   Si aucune commande n’est spécifiée, **l’aide** affiche toutes les commandes possibles.  
  
## <a name="examples"></a><a name=BKMK_examples></a>Illustre  
Pour afficher la liste de toutes les commandes disponibles dans DiskPart, tapez :  
  
```  
help  
```  
  
Pour afficher des informations d’aide détaillées sur l’utilisation de la commande **Create partition Primary** dans DiskPart, tapez :  
  
```  
help create partition primary  
```  
  
## <a name="additional-references"></a>Références supplémentaires  
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
  

  

