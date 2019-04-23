---
title: goto
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e0de1458-1f78-48ff-a746-c285a945a510
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f1ad0190519d58bd879ae391f378d800760c204f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857530"
---
# <a name="goto"></a>goto



Dirige cmd.exe sur une ligne étiquetée dans un programme de traitement par lots. Au sein d’un programme de traitement par lots, **goto** dirige le traitement des commandes à une ligne qui est identifiée par une étiquette. Lorsque l’étiquette est trouvée, le traitement continue en commençant par les commandes qui commencent sur la ligne suivante.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
goto <Label> 
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<Label>|Spécifie une chaîne de texte qui est utilisée en tant qu’étiquette dans le programme.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Utilisation des extensions de commande

    Si les extensions de commande sont activée (valeur par défaut) et que vous utilisez le **goto** avec une étiquette cible de **: EOF**, transférer le contrôle à la fin du fichier de script de traitement par lots actuel et de quitter le fichier de script de commandes sans définir une étiquette. Lorsque vous utilisez **goto** avec la **: EOF** étiquette, vous devez insérer un signe deux-points avant l’étiquette. Exemple :  
    ```
    goto:EOF
    ```  
-   À l’aide de valide *étiquette* valeurs

    Vous pouvez utiliser des espaces dans le *étiquette* paramètre, mais vous ne pouvez pas inclure d’autres séparateurs (par exemple, des points-virgules ou le signe égal).
-   Mise en correspondance *étiquette* portant l’étiquette dans le programme de traitement par lots

    Le *étiquette* valeur que vous spécifiez doit correspondre à une étiquette dans le programme. L’étiquette dans le programme de traitement par lots doit commencer par un signe deux-points ( :)). Si une ligne commence par un signe deux-points, il est traité comme une étiquette et toutes les commandes de cette ligne sont ignorés. Si votre programme de traitement par lots ne contient pas l’étiquette que vous spécifiez dans *étiquette*, le programme s’arrête et affiche le message suivant :  
    ```
    Label not found
    ```  
-   À l’aide de **goto** pour des opérations conditionnelles

    Vous pouvez utiliser **goto** avec d’autres commandes pour effectuer des opérations conditionnelles. Pour plus d’informations sur l’utilisation de **goto** pour des opérations conditionnelles, consultez la [si](if.md) référence de la commande.

## <a name="BKMK_examples"></a>Exemples

Le programme de traitement par lots suivant met en forme un disque dans le lecteur A comme un disque de système. Si l’opération a réussi, le **goto** commande dirige l’exécution sur le **: fin** étiquette :
```
echo off
format a: /s
if not errorlevel 1 goto end
echo An error occurred during formatting.
:end
echo End of batch program. 
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)

[Cmd](cmd.md)

[If](if.md)