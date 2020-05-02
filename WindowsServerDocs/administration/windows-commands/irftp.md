---
title: irftp
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e15c60a7-546d-4e9f-9871-43aaa1b569d6 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 34f912878b97bd00fb1c4ea539c7ea4c1423ea59
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724818"
---
# <a name="irftp"></a>irftp

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Envoie des fichiers via une liaison infrarouge.    
## <a name="syntax"></a>Syntaxe  
```  
irftp [<Drive>:\] [[<path>] <FileName>] [/h][/s]  
```  

#### <a name="parameters"></a>Paramètres  
|Paramètre|Description|  
|-------|--------|  
|Lecteur :\|spécifie le lecteur qui contient les fichiers que vous souhaitez envoyer via une liaison infrarouge.|  
|d Extension|Spécifie l’emplacement et le nom du fichier ou de l’ensemble de fichiers que vous souhaitez envoyer via une liaison infrarouge. Si vous spécifiez un ensemble de fichiers, vous devez spécifier le chemin d’accès complet de chaque fichier.|  
|/h|Spécifie le mode masqué. Lorsque le mode masqué est utilisé, les fichiers sont envoyés sans afficher la boîte de dialogue liaison sans fil.|  
|/s|Ouvre la boîte de dialogue liaison sans fil, qui vous permet de sélectionner le fichier ou l’ensemble de fichiers que vous souhaitez envoyer sans utiliser la ligne de commande pour spécifier le lecteur, le chemin d’accès et les noms de fichiers.|  

## <a name="remarks"></a>Notes   
-   Avant d’utiliser cette commande, vérifiez que la fonctionnalité infrarouge est activée et qu’elle fonctionne correctement sur les appareils que vous souhaitez communiquer via une liaison infrarouge, et qu’une liaison infrarouge est établie entre les appareils.  
-   Utilisé sans paramètres ou utilisé avec **/s**, **irftp** ouvre la boîte de dialogue **liaison sans fil** , dans laquelle vous pouvez sélectionner les fichiers que vous souhaitez envoyer sans utiliser la ligne de commande.  

## <a name="examples"></a>Exemples  
Send example. txt sur la liaison infrarouge.  
```  
irftp c:\example.txt  
```  

## <a name="additional-references"></a>Références supplémentaires  
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
