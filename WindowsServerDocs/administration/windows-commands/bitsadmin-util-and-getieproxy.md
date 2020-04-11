---
title: Bitsadmin util et GETIEPROXY
description: La rubrique commandes Windows pour **Bitsadmin util et GETIEPROXY**, qui récupère l’utilisation du proxy pour le compte de service donné.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6d50c7e3-f4eb-4ca5-9f0c-4ed396087db6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 22b24c4f9c0941c88c70b488a82de47c7901bd8b
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122463"
---
# <a name="bitsadmin-util-and-getieproxy"></a>Bitsadmin util et GETIEPROXY

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Récupère l’utilisation du proxy pour le compte de service donné. Cette commande affiche la valeur de chaque utilisation du proxy, pas seulement l’utilisation du proxy que vous avez spécifiée pour le compte de service. Pour plus d’informations sur la définition de l’utilisation du proxy pour des comptes de service spécifiques, consultez la commande [Bitsadmin util et SETIEPROXY](bitsadmin-util-and-setieproxy.md) .

## <a name="syntax"></a>Syntaxe

```
bitsadmin /util /getieproxy <account> [/conn <connectionname>]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ---------- |
| compte | Spécifie le compte de service dont vous souhaitez récupérer les paramètres de proxy. Les valeurs possibles incluent notamment :<ul><li>LOCALSYSTEM</li><li>   NETWORKSERVICE</li><li>Local.</li></ul> |
| ConnectionName | Ce paramètre est facultatif. Utilisé avec le paramètre **/conn** pour spécifier la connexion par modem à utiliser. Si vous ne spécifiez pas le paramètre **/conn** , bits utilise la connexion LAN. |

## <a name="examples"></a>Exemples

L’exemple suivant affiche l’utilisation du proxy pour le compte SERVICE réseau.

```
C:\>bitsadmin /util /getieproxy NETWORKSERVICE
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
