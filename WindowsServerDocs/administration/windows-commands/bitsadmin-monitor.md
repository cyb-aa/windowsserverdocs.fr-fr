---
title: bitsadmin monitor
description: Rubrique de référence pour la commande Bitsadmin Monitor, qui surveille les travaux de la file d’attente de transfert qui sont détenus par l’utilisateur actuel.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2c424d27-e011-49c2-b579-a2c235467c39
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4c8fa52f9fcf30a66b41c9cdbf7b7e1fab69f06e
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717374"
---
# <a name="bitsadmin-monitor"></a>bitsadmin monitor

Surveille les travaux de la file d’attente de transfert qui sont détenus par l’utilisateur actuel.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /monitor [/allusers] [/refresh <seconds>]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| /ALLUSERS | facultatif. Surveille les travaux de tous les utilisateurs. Vous devez disposer de privilèges d’administrateur pour utiliser ce paramètre. |
| /Refresh | facultatif. Actualise les données à un intervalle spécifié par `<seconds>`. L’intervalle d’actualisation par défaut est de cinq secondes. Pour arrêter l’actualisation, appuyez sur CTRL + C. |

## <a name="examples"></a>Exemples

Pour surveiller la file d’attente de transfert des travaux appartenant à l’utilisateur actuel et actualiser les informations toutes les 60 secondes.

```
bitsadmin /monitor /refresh 60
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin](bitsadmin.md)
