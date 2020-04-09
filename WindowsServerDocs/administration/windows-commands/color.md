---
title: color
description: La rubrique de commandes Windows pour Color, qui modifie les couleurs de premier plan et d’arrière-plan dans la fenêtre d’invite de commandes pour la session active.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f5b67131-d196-45ec-a3f9-b5d9f091fd86
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0e89c20d90a3b812fa67b597c4c205d34e725785
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847482"
---
# <a name="color"></a>color

Modifie les couleurs de premier plan et d’arrière-plan dans la fenêtre d’invite de commandes pour la session active. En cas d’utilisation sans paramètre, **Color** restaure les couleurs de premier plan et d’arrière-plan de la fenêtre d’invite de commandes par défaut.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
color [[<B>]<F>]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<B >|Spécifie la couleur d'arrière-plan.|
|\<F >|Spécifie la couleur de premier plan.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Le tableau suivant répertorie les chiffres hexadécimaux valides que vous pouvez utiliser comme valeurs pour *B* et *F*.

|Valeur|Couleur|
|-----|-----|
|0|Black|
|1|Bleu|
|2|Vert|
|3|Aqua|
|4|Rouge|
|5|Violet|
|6|Jaune|
|7|Blanc|
|8|Gris|
|9|Bleu clair|
|A|Vert clair|
|B|Cyan clair|
|C|Rouge clair|
|D|Violet clair|
|E|Jaune clair|
|F|Blanc brillant|
    
-   N’utilisez pas de caractères d’espace entre *B* et *F*.
-   Si vous spécifiez un seul chiffre hexadécimal, la couleur correspondante est utilisée comme couleur de premier plan et la couleur d’arrière-plan est définie sur la couleur par défaut.
-   Pour définir la couleur par défaut de la fenêtre d’invite de commandes, cliquez sur l’angle supérieur gauche de la fenêtre d’invite de commandes, cliquez sur **valeurs par défaut**, cliquez sur l’onglet **couleurs** , puis cliquez sur les couleurs que vous souhaitez utiliser pour le **texte** et l' **arrière-plan**de l’écran.
-   Si *B* et *F* sont identiques, la commande **Color** affecte à ErrorLevel la valeur 1 et aucune modification n’est apportée à la couleur de premier plan ou d’arrière-plan.

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

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
