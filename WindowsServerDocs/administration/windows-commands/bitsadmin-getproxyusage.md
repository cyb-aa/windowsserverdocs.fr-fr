---
title: bitsadmin getproxyusage
description: Rubrique de référence pour la commande Bitsadmin getproxyusage, qui récupère le paramètre d’utilisation du proxy pour le travail spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f940a70e-3b02-497e-a47f-b37b905c299e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 13a3f216b1ed3c77dbbefee37d73a657525daa36
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717648"
---
# <a name="bitsadmin-getproxyusage"></a>bitsadmin getproxyusage

Récupère le paramètre d’utilisation du proxy pour le travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /getproxyusage <job>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| travail | Nom complet ou GUID du travail. |

#### <a name="output"></a>Output

Les valeurs d’utilisation de proxy retournées peuvent être :

- **Préconfiguration** : utilisez les paramètres par défaut d’Internet Explorer du propriétaire.

- **No_Proxy** -ne pas utiliser de serveur proxy.

- **Override** : utilisez une liste de proxys explicite.

- **Détection** automatique : détecte automatiquement les paramètres du proxy.

## <a name="examples"></a>Exemples

Pour récupérer l’utilisation du proxy pour le travail nommé *myDownloadJob*:

```
bitsadmin /getproxyusage myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin](bitsadmin.md)
