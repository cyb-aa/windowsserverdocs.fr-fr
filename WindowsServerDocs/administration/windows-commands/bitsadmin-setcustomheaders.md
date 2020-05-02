---
title: bitsadmin setcustomheaders
description: Rubrique de référence pour la commande Bitsadmin setcustomheaders, qui ajoute un en-tête HTTP personnalisé à une requête d’extraction.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ed926410-80d0-46ed-9a90-f752c164bb9a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 92728f8d63a22cf9d13d6c02a69359583a9fc5cc
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719322"
---
# <a name="bitsadmin-setcustomheaders"></a>bitsadmin setcustomheaders

Ajoutez un en-tête HTTP personnalisé à une demande d’extraction envoyée à un serveur HTTP. Pour plus d’informations sur les requêtes d’extraction, consultez définitions de [méthode](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html#sec9.3) et [définitions de champ d’en-tête](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).

## <a name="syntax"></a>Syntaxe

```
bitsadmin /setcustomheaders <job> <header1> <header2> <...>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| travail | Nom complet ou GUID du travail. |
| `<header1> <header2>`etc. | En-têtes personnalisés pour le travail. |

## <a name="examples"></a>Exemples

Pour ajouter un en-tête HTTP personnalisé pour le travail nommé *myDownloadJob*:

```
bitsadmin /setcustomheaders myDownloadJob accept-encoding:deflate/gzip
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin](bitsadmin.md)
