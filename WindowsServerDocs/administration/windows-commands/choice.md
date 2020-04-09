---
title: choice
description: La rubrique commandes Windows pour Choice, qui invite l’utilisateur à sélectionner un élément dans une liste de choix à un seul caractère dans un programme de traitement par lots, puis retourne l’index du choix sélectionné.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c65a9119-410b-4dcf-9fa7-4e07d2a7238b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 26f915bc35b9edf3b206ff65e011b7f4a22988b7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847802"
---
# <a name="choice"></a>choice

Invite l’utilisateur à sélectionner un élément dans une liste de choix à un seul caractère dans un programme de traitement par lots, puis retourne l’index du choix sélectionné. S’il est utilisé sans paramètres, **Choice** affiche les choix par défaut **Y** et **N**.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
choice [/c [<Choice1><Choice2><…>]] [/n] [/cs] [/t <Timeout> /d <Choice>] [/m <Text>]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/c \<Choice1 ><Choice2><... >|Spécifie la liste des choix à créer. Les choix valides sont les suivants : a-z, A-Z, 0-9 et les caractères ASCII étendus (128-254). La liste par défaut est YN, qui est affichée en tant que `[Y,N]?`.|
|/n|Masque la liste de choix, bien que les choix soient toujours activés et que le texte du message (s’il est spécifié par **/m**) s’affiche toujours.|
|/CS|Spécifie que les choix respectent la casse. Par défaut, les choix ne respectent pas la casse.|
|/t \<délai d’expiration >|Spécifie le nombre de secondes à suspendre avant d’utiliser le choix par défaut spécifié par **/d**. Les valeurs acceptables sont comprises entre **0** et **9999**. Si **/t** est défini sur **0**, **Choice** ne s’interrompt pas avant de retourner le choix par défaut.|
|/d \<choix >|Spécifie le choix par défaut à utiliser après avoir attendu le nombre de secondes spécifié par **/t**. Le choix par défaut doit se trouver dans la liste de choix spécifiée par **/c**.|
|/m <Text>|Spécifie un message à afficher avant la liste de choix. Si **/m** n’est pas spécifié, seule l’invite de choix s’affiche.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   La variable d’environnement ERRORLEVEL est définie sur l’index de la clé que l’utilisateur sélectionne dans la liste de choix. Le premier choix dans la liste retourne la valeur 1, la seconde la valeur 2, et ainsi de suite. Si l’utilisateur appuie sur une touche qui n’est pas un choix valide, le **choix** émet un signal sonore d’avertissement. Si **Choice** détecte une condition d’erreur, il renvoie une valeur ERRORLEVEL de 255. Si l’utilisateur appuie sur CTRL + ATTN ou CTRL + C, **Choice** retourne une valeur ERRORLEVEL égale à 0.

> [!NOTE]
> Lorsque vous utilisez des valeurs ERRORLEVEL dans un programme de traitement par lots, répertoriez-les dans l’ordre décroissant.

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

Pour présenter les choix Y, N et C, tapez la ligne suivante dans un fichier de commandes :
```
choice /c ync
```
L’invite suivante s’affiche lorsque le fichier de commandes exécute la commande **Choice** :
```
[Y,N,C]?
```
Pour masquer les choix Y, N et C, mais afficher le texte Oui, non ou continuer, tapez la ligne suivante dans un fichier de commandes :
```
choice /c ync /n /m Yes, No, or Continue?
```
L’invite suivante s’affiche lorsque le fichier de commandes exécute la commande **Choice** :
```
Yes, No, or Continue?
```

> [!NOTE]
> Si vous utilisez le paramètre **/n** , mais que vous n’utilisez pas l' **option** **/m**, l’utilisateur n’est pas invité à entrer une entrée.

Pour afficher à la fois le texte et les options utilisées dans les exemples précédents, tapez la ligne suivante dans un fichier de commandes :
```
choice /c ync /m Yes, No, or Continue
```
L’invite suivante s’affiche lorsque le fichier de commandes exécute la commande **Choice** :
```
Yes, No, or Continue [Y,N,C]?
```
Pour définir une limite de durée de cinq secondes et spécifier **N** comme valeur par défaut, tapez la ligne suivante dans un fichier de commandes :
```
choice /c ync /t 5 /d n
```
L’invite suivante s’affiche lorsque le fichier de commandes exécute la commande **Choice** :
```
[Y,N,C]?
```

> [!NOTE]
> Dans cet exemple, si l’utilisateur n’appuie pas sur une touche dans un délai de cinq secondes, **Choice** sélectionne **N** par défaut et retourne une valeur d’erreur de 2. Dans le cas contraire, **Choice** retourne la valeur correspondant au choix de l’utilisateur.

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)