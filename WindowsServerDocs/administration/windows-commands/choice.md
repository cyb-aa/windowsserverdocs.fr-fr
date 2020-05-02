---
title: choice
description: Rubrique de référence pour la commande Choice, qui invite l’utilisateur à sélectionner un élément dans une liste de choix à un seul caractère dans un programme de traitement par lots, puis retourne l’index du choix sélectionné.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c65a9119-410b-4dcf-9fa7-4e07d2a7238b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 32c0daa680178c1952015c62c6c6749acf5f6143
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82713535"
---
# <a name="choice"></a>choice

Invite l’utilisateur à sélectionner un élément dans une liste de choix à un seul caractère dans un programme de traitement par lots, puis retourne l’index du choix sélectionné. S’il est utilisé sans paramètres, **Choice** affiche les choix par défaut **Y** et **N**.

## <a name="syntax"></a>Syntaxe

```
choice [/c [<choice1><choice2><…>]] [/n] [/cs] [/t <timeout> /d <choice>] [/m <text>]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| commutateur`<choice1><choice2><…>` | Spécifie la liste des choix à créer. Les choix valides sont les suivants : a-z, A-Z, 0-9 et les caractères ASCII étendus (128-254). La liste par défaut est YN, qui est affichée `[Y,N]?`sous la forme. |
| /n | Masque la liste de choix, bien que les choix soient toujours activés et que le texte du message (s’il est spécifié par **/m**) s’affiche toujours. |
| /CS | Spécifie que les choix respectent la casse. Par défaut, les choix ne respectent pas la casse. |
| commutateur`<timeout>` | Spécifie le nombre de secondes à suspendre avant d’utiliser le choix par défaut spécifié par **/d**. Les valeurs acceptables sont comprises entre **0** et **9999**. Si **/t** est défini sur **0**, **Choice** ne s’interrompt pas avant de retourner le choix par défaut. |
| /d`<choice>` | Spécifie le choix par défaut à utiliser après avoir attendu le nombre de secondes spécifié par **/t**. Le choix par défaut doit se trouver dans la liste de choix spécifiée par **/c**. |
| /m`<text>` | Spécifie un message à afficher avant la liste de choix. Si **/m** n’est pas spécifié, seule l’invite de choix s’affiche. |
| /? | Affiche l'aide à l'invite de commandes. |

## <a name="remarks"></a>Notes 

- La variable d’environnement **ERRORLEVEL** est définie sur l’index de la clé que l’utilisateur sélectionne dans la liste de choix. Le premier choix dans la liste retourne la valeur `1`, la deuxième valeur `2`, et ainsi de suite. Si l’utilisateur appuie sur une touche qui n’est pas un choix valide, le **choix** émet un signal sonore d’avertissement. 

- Si **Choice** détecte une condition d’erreur, il retourne une valeur **ERRORLEVEL** de `255`. Si l’utilisateur appuie sur CTRL + ATTN ou CTRL + C, **Choice** retourne une valeur **ERRORLEVEL** de `0`.

> [!NOTE]
> Lorsque vous utilisez des valeurs **ERRORLEVEL** dans un programme de traitement par lots, vous devez les répertorier dans l’ordre décroissant.

## <a name="examples"></a>Exemples

Pour présenter les choix **Y**, **N**et **C**, tapez la ligne suivante dans un fichier de commandes :

```
choice /c ync
```

L’invite suivante s’affiche lorsque le fichier de commandes exécute la commande **Choice** :

```
[Y,N,C]?
```

Pour masquer les choix **Y**, **n**et **C**, mais afficher le texte **Oui**, **non**ou **Continuer**, tapez la ligne suivante dans un fichier de commandes :

```
choice /c ync /n /m Yes, No, or Continue?
```

> [!NOTE]
> Si vous utilisez le paramètre **/n** , mais que vous n’utilisez pas l' **option** **/m**, l’utilisateur n’est pas invité à entrer une entrée.

Pour afficher à la fois le texte et les options utilisées dans les exemples précédents, tapez la ligne suivante dans un fichier de commandes :

```
choice /c ync /m Yes, No, or Continue
```

Pour définir une limite de durée de cinq secondes et spécifier **N** comme valeur par défaut, tapez la ligne suivante dans un fichier de commandes :

```
choice /c ync /t 5 /d n
```

> [!NOTE]
> Dans cet exemple, si l’utilisateur n’appuie pas sur une touche dans un délai de cinq secondes, **Choice** sélectionne **N** par défaut et `2`retourne une valeur d’erreur. Dans le cas contraire, **Choice** retourne la valeur correspondant au choix de l’utilisateur.

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
