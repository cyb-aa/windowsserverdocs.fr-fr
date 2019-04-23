---
title: irftp
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e15c60a7-546d-4e9f-9871-43aaa1b569d6 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d469270b744a9de881efd9b0cfa6f1105f70519a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848420"
---
# <a name="irftp"></a>irftp

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Envoie des fichiers via une liaison infrarouge.    
## <a name="syntax"></a>Syntaxe  
```  
irftp [<Drive>:\] [[<path>] <FileName>] [/h][/s]  
```  

### <a name="parameters"></a>Paramètres  
|Paramètre|Description|  
|-------|--------|  
|Lecteur :\|Spécifie le lecteur qui contient les fichiers que vous souhaitez envoyer via une liaison infrarouge.|  
|[path]FileName|Spécifie l’emplacement et le nom du fichier ou un ensemble de fichiers que vous souhaitez envoyer via une liaison infrarouge. Si vous spécifiez un ensemble de fichiers, vous devez spécifier le chemin d’accès complet pour chaque fichier.|  
|/h|Spécifie le mode masqué. Lorsque le mode masqué est utilisé, les fichiers sont envoyés sans afficher la boîte de dialogue Liaison sans fil.|  
|/s|Ouvre la boîte de dialogue Liaison sans fil, afin que vous puissiez sélectionner le fichier ou un ensemble de fichiers que vous souhaitez envoyer sans utiliser la ligne de commande pour spécifier le lecteur, chemin d’accès et les noms de fichiers.|  

## <a name="remarks"></a>Notes  
-   Avant d’utiliser cette commande, vérifiez que les appareils que vous souhaitez communiquer via une liaison infrarouge ont activé la fonctionnalité infrarouge et fonctionne correctement, et qu’une liaison infrarouge est établie entre les appareils.  
-   Utilisé sans paramètres ou avec **/s**, **irftp** ouvre le **liaison sans fil** boîte de dialogue, dans laquelle vous pouvez sélectionner les fichiers que vous souhaitez envoyer sans utiliser la ligne de commande.  

## <a name="BKMK_Examples"></a>Exemples  
Envoyer Example.txt sur la liaison infrarouge.  
```  
irftp c:\example.txt  
```  

## <a name="additional-references"></a>Références supplémentaires  
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)  
