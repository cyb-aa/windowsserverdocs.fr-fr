---
title: color
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f5b67131-d196-45ec-a3f9-b5d9f091fd86
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f8d23cc1bb5739b47c755d90c927cbcf82b8da7f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825020"
---
# <a name="color"></a>color



Modifie le premier plan et arrière-plan les couleurs de la fenêtre d’invite de commandes pour la session active. Si utilisée sans paramètres, **couleur** restaure les couleurs de premier plan et d’arrière-plan de fenêtre d’invite de commandes par défaut.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
color [[<B>]<F>]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<B>|Spécifie la couleur d'arrière-plan.|
|\<F>|Spécifie la couleur de premier plan.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Le tableau suivant répertorie les chiffres hexadécimaux valides que vous pouvez utiliser comme valeurs pour *B* et *F*.  
    |Value|Color|
    |-----|-----|
    |0|Noir|
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
    |B|Bleu-vert clair|
    |C|Rouge clair|
    |D|Violet clair|
    |E|Jaune clair|
    |F|Blanc brillant|
-   N’utilisez pas de caractères d’espace entre *B* et *F*.
-   Si vous spécifiez uniquement un chiffre hexadécimal, la couleur correspondante est utilisée comme la couleur de premier plan et la couleur d’arrière-plan est définie sur la couleur par défaut.
-   Pour définir la couleur de fenêtre d’invite de commandes par défaut, cliquez dans le coin supérieur gauche de la fenêtre d’invite de commandes, cliquez sur **valeurs par défaut**, cliquez sur le **couleurs** onglet, puis cliquez sur les couleurs que vous souhaitez utiliser pour le  **Écran texte** et **écran en arrière-plan**.
-   Si *B* et *F* sont identiques, le **couleur** commande affecte ERRORLEVEL 1 et aucune modification n’est apportée au premier plan ou la couleur d’arrière-plan.

## <a name="BKMK_examples"></a>Exemples

Pour modifier la couleur d’arrière-plan de fenêtre invite de commandes comme grise et la couleur de premier plan rouge, tapez :
```
color 84
```
Pour modifier la couleur de premier plan de fenêtre invite de commandes au jaune clair, tapez :
```
color e
```

> [!NOTE]
> Dans cet exemple, l’arrière-plan est défini sur la couleur par défaut, car seul chiffre hexadécimal est spécifié.

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)