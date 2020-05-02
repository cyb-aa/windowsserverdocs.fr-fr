---
title: bitsadmin util and getieproxy
description: Rubrique de référence pour la commande Bitsadmin util et GETIEPROXY, qui récupère l’utilisation du proxy pour le compte de service donné.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6d50c7e3-f4eb-4ca5-9f0c-4ed396087db6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 576f308bc7fb9a4e448638d06621f95eebef0cd0
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82707658"
---
# <a name="bitsadmin-util-and-getieproxy"></a>bitsadmin util and getieproxy

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Récupère l’utilisation du proxy pour le compte de service donné. Cette commande affiche la valeur de chaque utilisation du proxy, pas seulement l’utilisation du proxy que vous avez spécifiée pour le compte de service. Pour plus d’informations sur la définition de l’utilisation du proxy pour des comptes de service spécifiques, consultez la commande [Bitsadmin util et SETIEPROXY](bitsadmin-util-and-setieproxy.md) .

## <a name="syntax"></a>Syntaxe

```
bitsadmin /util /getieproxy <account> [/conn <connectionname>]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ---------- |
| account | Spécifie le compte de service dont vous souhaitez récupérer les paramètres de proxy. Les valeurs possibles incluent :<ul><li>LOCALSYSTEM</li><li>   NETWORKSERVICE</li><li>Local.</li></ul> |
| ConnectionName | facultatif. Utilisé avec le paramètre **/conn** pour spécifier la connexion par modem à utiliser. Si vous ne spécifiez pas le paramètre **/conn** , bits utilise la connexion LAN. |

## <a name="examples"></a>Exemples

Pour afficher l’utilisation du proxy pour le compte de SERVICE réseau :

```
bitsadmin /util /getieproxy NETWORKSERVICE
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin util](bitsadmin-util.md)

- [commande Bitsadmin](bitsadmin.md)
