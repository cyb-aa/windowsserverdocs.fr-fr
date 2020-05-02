---
title: color
description: Rubrique de référence pour la commande Color, qui modifie les couleurs de premier plan et d’arrière-plan dans la fenêtre d’invite de commandes pour la session active.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f5b67131-d196-45ec-a3f9-b5d9f091fd86
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 94b5a1e4ca4d4a01ea714adc45e64a6efaa32aa6
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82711936"
---
# <a name="color"></a>color

Modifie les couleurs de premier plan et d’arrière-plan dans la fenêtre d’invite de commandes pour la session active. En cas d’utilisation sans paramètre, **Color** restaure les couleurs de premier plan et d’arrière-plan de la fenêtre d’invite de commandes par défaut.

## <a name="syntax"></a>Syntaxe

```
color [[<b>]<f>]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| `<b>` | Spécifie la couleur d'arrière-plan. |
| `<f>` | Spécifie la couleur de premier plan. |
| /? | Affiche l'aide à l'invite de commandes. |

Où :

Le tableau suivant répertorie les chiffres hexadécimaux valides que vous pouvez utiliser comme `<b>` valeurs `<f>`pour et :

| Valeur | Couleur |
| ----- | ----- |
| 0 | Noir |
| 1 | Bleu |
| 2 | Vert |
| 3 | Aqua |
| 4 | Rouge |
| 5 | Violet |
| 6 | Jaune |
| 7 | White |
| 8 | Gris |
| 9 | Bleu clair |
| a | Vert clair |
| b | Cyan clair |
| c | Rouge clair |
| d | Violet clair |
| e | Jaune clair |
| f | Blanc brillant |

#### <a name="remarks"></a>Notes 

- N’utilisez pas de caractères `<b>` d' `<f>`espace entre et.

- Si vous spécifiez un seul chiffre hexadécimal, la couleur correspondante est utilisée comme couleur de premier plan et la couleur d’arrière-plan est définie sur la couleur par défaut.

- Pour définir la couleur de la fenêtre d’invite de commandes par défaut, sélectionnez l’angle supérieur gauche de la fenêtre d' **invite de commandes** , sélectionnez **valeurs par défaut**, sélectionnez l’onglet **couleurs** , puis sélectionnez les couleurs que vous souhaitez utiliser pour le texte de l' **écran** et l' **arrière-plan**de l’écran.

- Si `<b>` et `<f>` ont la même valeur de couleur, ERRORLEVEL a la valeur `1`et aucune modification n’est apportée à la couleur de premier plan ou d’arrière-plan.

## <a name="examples"></a>Exemples

Pour modifier la couleur d’arrière-plan de la fenêtre d’invite de commandes en gris et la couleur de premier plan en rouge, tapez :

```
color 84
```

Pour modifier la couleur de premier plan de la fenêtre d’invite de commandes en jaune clair, tapez :

```
color e
```

> [!NOTE]
> Dans cet exemple, l’arrière-plan est défini sur la couleur par défaut, car un seul chiffre hexadécimal est spécifié.

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
