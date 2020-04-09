---
title: bitsadmin list
description: Rubrique relative aux commandes Windows pour la **liste Bitsadmin**, qui répertorie les tâches de transfert détenues par l’utilisateur actuel.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1416965e-e0e6-49cf-b1d4-b286d3cf8716
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1883da7bfa71a41952f6f67e25eca4dbbdd3353c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850322"
---
# <a name="bitsadmin-list"></a>bitsadmin list

Répertorie les tâches de transfert détenues par l’utilisateur actuel.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /list [/allusers][/verbose]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| /ALLUSERS | Ce paramètre est facultatif. Répertorie les travaux de tous les utilisateurs. Vous devez disposer de privilèges d’administrateur pour utiliser ce paramètre. |
| /verbose | Ce paramètre est facultatif. Fournit des informations détaillées sur chaque travail. |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant récupère des informations sur les travaux détenus par l’utilisateur actuel.

```
C:\>bitsadmin /list
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)