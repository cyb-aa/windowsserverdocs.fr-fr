---
title: bitsadmin addfilewithranges
description: Rubrique de commandes de Windows pour **bitsadmin addfilewithranges** -ajoute un fichier pour le travail spécifié. BITS télécharge les plages spécifiées à partir du fichier distant.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 69b402195f90977aa63299c1a2a550ba310a4513
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832340"
---
# <a name="bitsadmin-addfilewithranges"></a>bitsadmin addfilewithranges

Ajoute un fichier pour le travail spécifié. BITS télécharge les plages spécifiées à partir du fichier distant. Ce commutateur est valide uniquement pour les travaux de téléchargement.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /AddFileWithRanges <Job> <RemoteURL> <LocalName> <RangeList>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom d’affichage ou le GUID du travail|
|RemoteURL|*RemoteURL* est l’URL du fichier sur le serveur.|
|LocalName|*LocalName* est le nom du fichier sur l’ordinateur local. *LocalName* doit contenir un chemin d’accès absolu au fichier.|
|RangeList|*RangeList* est une liste délimitée par des virgules de paires de décalage : longueur. Utilisez un signe deux-points pour séparer la valeur de décalage à partir de la valeur de longueur. Par exemple, une valeur de `0:100,2000:100,5000:eof` indique les BITS pour transférer des 100 octets à partir de l’offset 0, 100 octets à partir de l’offset 2000 et les octets restants à partir de décalage 5000 à la fin du fichier.|

## <a name="more-information"></a>Plus d’informations

-   Le jeton **eof** est une valeur de longueur valide dans les paires de décalage et la longueur dans le  *\<RangeList >*. Il indique au service pour lire à la fin du fichier spécifié.
-   Notez que AddFileWithRanges échoue avec le code d’erreur 0x8020002c lors d’une plage de longueur nulle est spécifiée, ainsi que d’une autre plage avec le même décalage, telles que : C:\bits > bitsadmin /addfilewithranges j2 http://bitsdc/dload/1k.zip c:\1k.zip 100:0, 100:5

    Message d’erreur : Impossible d’ajouter le fichier au travail - 0x8020002c. La liste de plages d’octets contient des plages qui se chevauchent, qui ne sont pas pris en charge.

    Solution de contournement : ne spécifiez pas du tout d’abord la plage de longueur nulle. Par exemple : bitsadmin /addfilewithranges j2 http://bitsdc/dload/1k.zip c:\1k.zip 100:5, 100:0.

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant indique les BITS pour transférer des 100 octets à partir de l’offset 0, 100 octets de décalage 2000 et les octets restants à partir d’un décalage 5000 à la fin du fichier.
```
C:\>bitsadmin /addfilewithranges http://downloadsrv/10mb.zip c:\10mb.zip "0:100,2000:100,5000:eof"
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)