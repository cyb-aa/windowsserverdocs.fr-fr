---
title: bitsadmin monitor
description: La rubrique commandes Windows pour **Bitsadmin Monitor**, qui surveille les travaux de la file d’attente de transfert qui sont détenus par l’utilisateur actuel.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2c424d27-e011-49c2-b579-a2c235467c39
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bda268afd5fda24bba2afb04b32bac9cda9a05bb
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850212"
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
| /ALLUSERS | Ce paramètre est facultatif. Surveille les travaux de tous les utilisateurs. Vous devez disposer de privilèges d’administrateur pour utiliser ce paramètre. |
| /Refresh | Ce paramètre est facultatif. Actualise les données à un intervalle spécifié par `<seconds>`. L’intervalle d’actualisation par défaut est de cinq secondes. Pour arrêter l’actualisation, appuyez sur CTRL + C. |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant surveille la file d’attente de transfert pour les travaux détenus par l’utilisateur actuel et actualise les informations toutes les 60 secondes.

```
C:\>bitsadmin /monitor /refresh 60
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)