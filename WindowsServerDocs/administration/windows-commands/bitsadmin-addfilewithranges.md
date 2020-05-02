---
title: bitsadmin addfilewithranges
description: Rubrique de référence pour la commande Bitsadmin ADDFILEWITHRANGES, qui ajoute un fichier au travail spécifié. Le service BITS télécharge les plages spécifiées à partir du fichier distant.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: df0ce0bf-dff1-4a48-a16f-fd2f4d5f7189
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4b878b4f48441808bf971c051397d3af9bd975fe
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718466"
---
# <a name="bitsadmin-addfilewithranges"></a>bitsadmin addfilewithranges

Ajoute un fichier au travail spécifié. Le service BITS télécharge les plages spécifiées à partir du fichier distant. Ce commutateur est valide uniquement pour les travaux de téléchargement.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /addfilewithranges <job> <remoteURL> <localname> <rangelist>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| travail | Nom complet ou GUID du travail. |
| remoteURL | URL du fichier sur le serveur. |
| localname | Nom du fichier sur l’ordinateur local. Doit contenir un chemin d’accès absolu au fichier. |
| rangelist | Liste délimitée par des virgules de paires offset : length. Utilisez un signe deux-points pour séparer la valeur de décalage de la valeur de longueur. Par exemple, la valeur `0:100,2000:100,5000:eof` indique à bits de transférer 100 octets à partir du décalage 0, 100 octets du décalage 2000, et les octets restants de l’offset 5000 à la fin du fichier. |

## <a name="remarks"></a>Notes 

- Le **EOF** du jeton est une valeur de longueur valide dans les paires décalage/longueur `<rangelist>`dans le. Elle indique au service de lire jusqu’à la fin du fichier spécifié.

- La `addfilewithranges` commande échoue avec le code d’erreur 0x8020002c, si une plage de longueur nulle est spécifiée avec une autre plage en utilisant le même décalage, par exemple :

    `c:\bits>bitsadmin /addfilewithranges j2 http://bitsdc/dload/1k.zip c:\1k.zip 100:0,100:5`

    **Message d’erreur :** Impossible d’ajouter le fichier au travail-0x8020002c. La liste de plages d’octets contient des plages qui se chevauchent, ce qui n’est pas pris en charge.

    **Solution de contournement :** Ne spécifiez pas d’abord la plage de longueur nulle. Par exemple, utilisez `bitsadmin /addfilewithranges j2 http://bitsdc/dload/1k.zip c:\1k.zip 100:5,100:0`

## <a name="examples"></a>Exemples

Pour transférer 100 octets à partir du décalage 0, 100 octets à partir du décalage 2000, et les octets restants du décalage 5000 à la fin du fichier :

```
bitsadmin /addfilewithranges http://downloadsrv/10mb.zip c:\10mb.zip 0:100,2000:100,5000:eof
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin](bitsadmin.md)
