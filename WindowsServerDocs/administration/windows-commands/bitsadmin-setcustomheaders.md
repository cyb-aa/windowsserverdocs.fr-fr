---
title: Bitsadmin setcustomheaders
description: Rubrique relative aux commandes Windows pour **Bitsadmin setcustomheaders**, qui ajoute un en-tête HTTP personnalisé à une requête d’extraction.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ed926410-80d0-46ed-9a90-f752c164bb9a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b5b1a28f03815a22a3f8d10b2c3d1d4a3a2ae635
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/11/2020
ms.locfileid: "81123027"
---
# <a name="bitsadmin-setcustomheaders"></a>Bitsadmin setcustomheaders

Ajoutez un en-tête HTTP personnalisé à une demande d’extraction envoyée à un serveur HTTP.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /setcustomheaders <job> <header1> <header2> <...>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| le travail | Nom complet ou GUID du travail. |
| `<header1> <header2>`, etc. | En-têtes personnalisés pour le travail. |

## <a name="examples"></a>Exemples

L’exemple suivant ajoute un en-tête HTTP personnalisé pour le travail nommé *myDownloadJob*. Pour plus d’informations sur les requêtes d’extraction, consultez définitions de [méthode](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html#sec9.3) et [définitions de champ d’en-tête](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).

```
C:\>bitsadmin /setcustomheaders myDownloadJob accept-encoding:deflate/gzip
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)