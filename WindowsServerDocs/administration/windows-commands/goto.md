---
title: goto
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e0de1458-1f78-48ff-a746-c285a945a510
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 928a9031a7f86261789676257afe95ffc3be8a99
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80842562"
---
# <a name="goto"></a>goto



Dirige cmd. exe vers une ligne étiquetée dans un programme de traitement par lots. Dans un programme de traitement par lots, **goto** dirige le traitement des commandes vers une ligne identifiée par une étiquette. Lorsque l’étiquette est trouvée, le traitement continue à partir des commandes qui commencent sur la ligne suivante.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
goto <Label> 
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Étiquette \<>|Spécifie une chaîne de texte utilisée comme étiquette dans le programme de traitement par lots.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Utilisation des extensions de commande

    Si les extensions de commande sont activées (valeur par défaut) et que vous utilisez la commande **goto** avec une étiquette cible **: EOF**, vous transférez le contrôle à la fin du fichier de script de commandes en cours et quittez le fichier de script de commandes sans définir une étiquette. Quand vous utilisez **goto** avec l’étiquette **: EOF** , vous devez insérer un signe deux-points avant l’étiquette. Par exemple :  
    ```
    goto:EOF
    ```  
-   Utilisation de valeurs d' *étiquette* valides

    Vous pouvez utiliser des espaces dans le paramètre *label* , mais vous ne pouvez pas inclure d’autres séparateurs (par exemple, des points-virgules ou des signes égal).
-   *Étiquette* correspondante avec l’étiquette dans le programme de traitement par lots

    La valeur d' *étiquette* que vous spécifiez doit correspondre à une étiquette dans le programme de traitement par lots. L’étiquette dans le programme batch doit commencer par un signe deux-points ( :). Si une ligne commence par un signe deux-points, elle est traitée comme une étiquette et toute commande sur cette ligne est ignorée. Si votre programme de traitement par lots ne contient pas l’étiquette que vous spécifiez dans *étiquette*, le programme de traitement par lots s’arrête et affiche le message suivant :  
    ```
    Label not found
    ```  
-   Utilisation de **goto** pour les opérations conditionnelles

    Vous pouvez utiliser **goto** avec d’autres commandes pour effectuer des opérations conditionnelles. Pour plus d’informations sur l’utilisation de **goto** pour les opérations conditionnelles, consultez la référence de commande [If](if.md) .

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

Le programme batch suivant met en forme un disque dans le lecteur A en tant que disque système. Si l’opération réussit, la commande **goto** dirige le traitement vers l’étiquette de **fin** :
```
echo off
format a: /s
if not errorlevel 1 goto end
echo An error occurred during formatting.
:end
echo End of batch program. 
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

[Cmd](cmd.md)

[Que](if.md)