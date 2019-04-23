---
title: pause
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cab3afc3-d046-432f-a0bf-6282f0099032
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 109d162e8d5c4bdd59871a21f16b6f568df4fbd6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861660"
---
# <a name="pause"></a>pause



Interrompt le traitement d’un programme de traitement par lots et affiche l’invite suivante :
```
Press any key to continue . . .
```
Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
pause
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Lorsque vous exécutez le **suspendre** commande, le message suivant apparaît :  
    ```
    Press any key to continue . . .
    ```  
-   Si vous appuyez sur CTRL + C pour arrêter un programme, le message suivant apparaît :  
    ```
    Terminate batch job (Y/N)?
    ```  
    Si vous appuyez sur O (pour Oui) en réponse à ce message, le programme de traitement par lots prend fin et le contrôle retourne au système d’exploitation.
-   Vous pouvez insérer le **suspendre** commande avant une section du fichier de commandes que vous souhaiterez peut-être pas traiter. Lorsque **suspendre** interrompt le traitement du programme de traitement par lots, vous pouvez appuyer sur CTRL + C et appuyez sur Y pour arrêter le programme.

## <a name="BKMK_examples"></a>Exemples

Pour créer un programme de traitement par lots qui invite l’utilisateur à modifier des disques dans un des lecteurs, tapez :
```
@echo off 
:Begin 
copy a:*.* 
echo Put a new disk into drive A 
pause 
goto begin
```
Dans cet exemple, tous les fichiers sur le disque dans le lecteur A sont copiés dans le répertoire actif. Une fois que le message vous invite à insérer un nouveau disque dans le lecteur A, le **suspendre** commande interrompt le traitement afin que vous pouvez modifier des disques et appuyez sur n’importe quelle touche pour reprendre le traitement. Ce programme de traitement par lots s’exécute dans une boucle sans fin, le **goto début** commande envoie l’interpréteur de commandes à l’étiquette de début du fichier de commandes. Pour arrêter ce programme de commandes, appuyez sur CTRL + C et appuyez sur Y.

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)