---
title: bcdboot
description: Rubrique de référence pour la commande BCDboot, qui permet de configurer rapidement une partition système, ou de réparer l’environnement de démarrage situé sur la partition système.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e62a250e-08e9-47f6-89d1-e6002560ab57
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cde91738f524350f72f0278495e4bd46a3960e6f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718709"
---
# <a name="bcdboot"></a>bcdboot

Vous permet de configurer rapidement une partition système ou de réparer l’environnement de démarrage situé sur la partition système. La partition système est configurée en copiant un ensemble simple de fichiers Données de configuration de démarrage (BCD) (BCD) dans une partition vide existante.

## <a name="syntax"></a>Syntaxe

```
bcdboot <source> [/l] [/s]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| source | Spécifie l’emplacement du répertoire Windows à utiliser comme source pour la copie des fichiers de l’environnement de démarrage. |
| /l | Spécifie les paramètres régionaux. Les paramètres régionaux par défaut sont anglais (États-Unis). |
| /s | Spécifie la lettre de volume de la partition système. La valeur par défaut est la partition système identifiée par le microprogramme. |

## <a name="examples"></a>Exemples

Pour plus d’informations sur l’emplacement de BCDboot et des exemples d’utilisation de cette commande, consultez la rubrique [options de ligne de commande BCDboot](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh824874(v=win.10)x) .

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
