---
title: choice
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c65a9119-410b-4dcf-9fa7-4e07d2a7238b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 78d2816bff754ef04558cf37eaada7c7fafba823
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841550"
---
# <a name="choice"></a>choice



Invite l’utilisateur à sélectionner un élément dans une liste de choix de caractère unique dans un programme de traitement par lots, puis retourne l’index du choix sélectionné. Si utilisée sans paramètres, **choix** affiche les choix par défaut **Y** et **N**.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
choice [/c [<Choice1><Choice2><…>]] [/n] [/cs] [/t <Timeout> /d <Choice>] [/m <"Text">]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/c \<Choice1><Choice2><…>|Spécifie la liste de choix doit être créé. Les choix valides incluent a-z, A-Z, 0-9 et les caractères ASCII étendus (128-254). La liste par défaut est « YN », qui s’affiche en tant que `[Y,N]?`.|
|/n|Masque la liste de choix, bien que les choix sont toujours activées et le texte du message (si spécifié par **/m**) est toujours affichée.|
|/cs|Spécifie que les choix respectent la casse. Par défaut, les choix ne respectent pas la casse.|
|/t \<délai d’attente >|Spécifie le nombre de secondes en pause avant d’utiliser le choix par défaut spécifié par **/d**. Les valeurs acceptables sont comprises entre **0** à **9999**. Si **/t** a la valeur **0**, **choix** ne pas en pause avant de retourner le choix par défaut.|
|/d \<Choice>|Spécifie le choix par défaut à utiliser après avoir attendu le nombre de secondes spécifié par **/t**. Le choix par défaut doit être dans la liste de choix spécifié par **/c**.|
|/m <"Text">|Spécifie un message à afficher avant que la liste de choix. Si **/m** n’est pas spécifié, seulement l’invite de choix s’affiche.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   La variable d’environnement ERRORLEVEL est définie à l’index de la clé de l’utilisateur sélectionne dans la liste de choix. Le premier choix dans la liste retourne une valeur de 1, la seconde une valeur de 2 et ainsi de suite. Si l’utilisateur appuie sur une clé qui n’est pas un choix valid, **choix** émet un signal sonore d’avertissement. Si **choix** détecte une condition d’erreur, elle retourne une valeur ERRORLEVEL de 255. Si l’utilisateur appuie sur CTRL + ATTN ou CTRL + C, **choix** retourne une valeur ERRORLEVEL de 0.

> [!NOTE]
> Lorsque vous utilisez des valeurs ERRORLEVEL dans un programme de traitement par lots, les répertorier dans l’ordre décroissant.

## <a name="BKMK_examples"></a>Exemples

Pour présenter les paramètres Y, N, C, tapez la ligne suivante dans un fichier de commandes :
```
choice /c ync
```
L’invite suivante s’affiche lorsque le fichier de commandes exécute le **choix** commande :
```
[Y,N,C]?
```
Pour masquer les paramètres Y, N, C, mais affiche le texte « Oui, non, ou continuer », tapez la ligne suivante dans un fichier de commandes :
```
choice /c ync /n /m "Yes, No, or Continue?"
```
L’invite suivante s’affiche lorsque le fichier de commandes exécute le **choix** commande :
```
Yes, No, or Continue?
```

> [!NOTE]
> Si vous utilisez le **/n** paramètre, mais n’utilisez pas **/m**, l’utilisateur n’est pas averti lorsque **choix** est en attente d’entrée.

Pour afficher le texte et les options utilisées dans les exemples précédents, tapez la ligne suivante dans un fichier de commandes :
```
choice /c ync /m "Yes, No, or Continue"
```
L’invite suivante s’affiche lorsque le fichier de commandes exécute le **choix** commande :
```
Yes, No, or Continue [Y,N,C]?
```
Pour définir un délai de cinq secondes et **N** comme valeur par défaut, tapez la ligne suivante dans un fichier de commandes :
```
choice /c ync /t 5 /d n
```
L’invite suivante s’affiche lorsque le fichier de commandes exécute le **choix** commande :
```
[Y,N,C]?
```

> [!NOTE]
> Dans cet exemple, si l’utilisateur n’appuyez pas sur une clé dans les cinq secondes, **choix** sélectionne **N** par défaut et retourne une valeur d’erreur 2. Sinon, **choix** retourne la valeur correspondant au choix de l’utilisateur.

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)