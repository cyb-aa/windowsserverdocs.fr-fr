---
title: pause
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: cab3afc3-d046-432f-a0bf-6282f0099032
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 135d074a71c7153cc1665ad7b543bdba56ed66e8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80837682"
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

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

- Quand vous exécutez la commande **Pause** , le message suivant s’affiche :  
  ```
  Press any key to continue . . .
  ```  
- Si vous appuyez sur CTRL + C pour arrêter un programme de traitement par lots, le message suivant s’affiche :  
  ```
  Terminate batch job (Y/N)?
  ```  
  Si vous appuyez sur o (pour Oui) en réponse à ce message, le programme de traitement par lots se termine et le contrôle retourne au système d’exploitation.
- Vous pouvez insérer la commande **Pause** avant une section du fichier de commandes que vous ne souhaitez peut-être pas traiter. Lorsque l' **interruption** interrompt le traitement du programme batch, vous pouvez appuyer sur Ctrl + C, puis sur Y pour arrêter le programme batch.

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

Pour créer un programme batch qui invite l’utilisateur à modifier les disques de l’un des lecteurs, tapez :
```
@echo off 
:Begin 
copy a:*.* 
echo Put a new disk into drive A 
pause 
goto begin
```
Dans cet exemple, tous les fichiers sur le disque du lecteur A sont copiés dans le répertoire actif. Une fois que le message vous invite à placer un nouveau disque dans le lecteur A, la commande **Pause** interrompt le traitement afin que vous puissiez modifier les disques, puis appuyer sur une touche pour reprendre le traitement. Ce programme batch s’exécute dans une boucle sans fin — la commande **goto Begin** envoie l’interpréteur de commandes à l’étiquette Begin du fichier de commandes. Pour arrêter ce programme batch, appuyez sur CTRL + C, puis sur o.

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)