---
title: cache Bitsadmin et DeleteUrl
description: Rubrique relative aux commandes Windows pour le **cache Bitsadmin et DeleteUrl**, qui supprime toutes les entrées de cache pour l’URL donnée.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e108b76b-fae9-4c16-bf4c-d74c9f025953
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 70099e795d0f05d0fcf75fbf6b82f5466d1c0c55
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850932"
---
# <a name="bitsadmin-cache-and-deleteurl"></a>cache Bitsadmin et DeleteUrl

Supprime toutes les entrées de cache pour l’URL donnée.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /deleteURL url
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| url | Uniform Resource Locator qui identifie un fichier distant. |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant supprime toutes les entrées de cache pour `https://www.contoso.com/en/us/default.aspx`

```
C:\>bitsadmin /deleteURL https://www.contoso.com/en/us/default.aspx 
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)