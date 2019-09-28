---
title: bitsadmin addfilewithranges
description: 'Rubrique relative aux commandes Windows pour **Bitsadmin ADDFILEWITHRANGES** : ajoute un fichier au travail spécifié. Le service BITS télécharge les plages spécifiées à partir du fichier distant.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: df0ce0bf-dff1-4a48-a16f-fd2f4d5f7189
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 557f19f6e106e5fb73b3a229090eecf0fc048758
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382020"
---
# <a name="bitsadmin-addfilewithranges"></a>bitsadmin addfilewithranges

Ajoute un fichier au travail spécifié. Le service BITS télécharge les plages spécifiées à partir du fichier distant. Ce commutateur est valide uniquement pour les travaux de téléchargement.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /AddFileWithRanges <Job> <RemoteURL> <LocalName> <RangeList>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|
|RemoteURL|*RemoteUrl* est l’URL du fichier sur le serveur.|
|LocalName|*LocalName* est le nom du fichier sur l’ordinateur local. *LocalName* doit contenir un chemin d’accès absolu au fichier.|
|RangeList|*RangeList* est une liste délimitée par des virgules de paires offset : length. Utilisez un signe deux-points pour séparer la valeur de décalage de la valeur de longueur. Par exemple, la valeur `0:100,2000:100,5000:eof` indique à BITS de transférer 100 octets à partir du décalage 0, 100 octets du décalage 2000, et les octets restants de l’offset 5000 à la fin du fichier.|

## <a name="more-information"></a>Plus d’informations

-   Le **EOF** du jeton est une valeur de longueur valide dans les paires décalage/longueur de la *> \<RangeList*. Elle indique au service de lire jusqu’à la fin du fichier spécifié.
-   Notez que AddFileWithRanges échouera avec le code d’erreur 0x8020002c quand une plage de longueur nulle est spécifiée avec une autre plage du même décalage, par exemple : C:\bits > Bitsadmin/ADDFILEWITHRANGES J2 http://bitsdc/dload/1k.zip c:\1k.zip 100:0100:5

    Message d’erreur : Impossible d’ajouter le fichier au travail-0x8020002c. La liste de plages d’octets contient des plages qui se chevauchent, ce qui n’est pas pris en charge.

    Solution de contournement : ne spécifiez pas d’abord la plage de longueur nulle. Par exemple : Bitsadmin/ADDFILEWITHRANGES J2 http://bitsdc/dload/1k.zip c:\1k.zip 100:5100:0.

## <a name="examples"></a>Exemples

L’exemple suivant indique à BITS de transférer 100 octets à partir du décalage 0, 100 octets du décalage 2000, et les octets restants de l’offset 5000 à la fin du fichier.

```
C:\>bitsadmin /addfilewithranges http://downloadsrv/10mb.zip c:\10mb.zip "0:100,2000:100,5000:eof"
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)