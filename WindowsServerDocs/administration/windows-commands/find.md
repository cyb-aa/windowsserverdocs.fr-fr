---
title: trouver
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2ca66b22-3b7c-4166-8503-eb75fc53ab46
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3cd731ef64912644965ef6bb96d060a46f0a6067
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725623"
---
# <a name="find"></a>trouver



Recherche une chaîne de texte dans un ou des fichiers et affiche des lignes de texte qui contiennent la chaîne spécifiée.



## <a name="syntax"></a>Syntaxe

```
find [/v] [/c] [/n] [/i] [/off[line]] <String> [[<Drive>:][<Path>]<FileName>[...]]
```

### <a name="parameters"></a>Paramètres

|           Paramètre           |                                              Description                                               |
|-------------------------------|--------------------------------------------------------------------------------------------------------|
|              /v               |                    Affiche toutes les lignes qui ne contiennent pas la \<chaîne spécifiée>.                     |
|              /C               |              Compte les lignes qui contiennent la chaîne \<spécifiée>et affiche le total.              |
|              /n               |                            Précède chaque ligne avec le numéro de ligne du fichier.                             |
|              /i               |                            Spécifie que la recherche ne respecte pas la casse.                            |
|         [/OFF [Line]]          |                        N’ignore pas les fichiers dont l’attribut offline est défini.                        |
|          \<> de chaîne          | Obligatoire. Spécifie le groupe de caractères (entre guillemets) que vous souhaitez rechercher. |
| [\<Lecteur> :] [<Path>]<FileName> |        Spécifie l’emplacement et le nom du fichier dans lequel rechercher la chaîne spécifiée.        |
|              /?               |                                  Affiche l'aide à l'invite de commandes.                                  |

## <a name="remarks"></a>Notes 

-   Spécification d’une chaîne

    Si vous n’utilisez pas **/i**, **Rechercher** recherche exactement ce que vous spécifiez pour la *chaîne*. Par exemple, la commande **Find** traite les caractères A et un différemment. Toutefois, si vous utilisez **/i**, **Find** ne respecte pas la casse et traite un et un comme le même caractère.

    Si la chaîne que vous souhaitez rechercher contient des guillemets, vous devez utiliser des guillemets doubles pour chaque guillemet contenu dans la chaîne (par exemple, cette chaîne contient des guillemets).
-   Utilisation de la fonction **Rechercher** comme filtre

    Si vous omettez un nom de fichier, **Find** agit comme un filtre, en prenant l’entrée de la source d’entrée standard (généralement le clavier, un canal (|) ou un fichier Redirigé), puis en affichant toutes les lignes qui contiennent une *chaîne*.
-   Classement de la syntaxe de commande

    Vous pouvez taper des paramètres et des options de ligne de commande pour la commande **Find** dans n’importe quel ordre.
-   Utilisation de caractères génériques

    Vous ne pouvez pas utiliser de caractères génériques (**&#42;** et **?**) dans les noms de fichiers ou les extensions que vous spécifiez avec la commande **Find** . Pour rechercher une chaîne dans un ensemble de fichiers que vous spécifiez à l’aide de caractères génériques, vous pouvez utiliser la commande **Find** dans une commande **for** .
-   Utilisation de **/v** ou **/n** avec **/c**

    Si vous utilisez **/c** et **/v** dans la même ligne de commande, **Find** affiche le nombre de lignes qui ne contiennent pas la chaîne spécifiée. Si vous spécifiez **/c** et **/n** dans la même ligne de commande, l’option **Rechercher** ignore **/n**.
-   Utilisation de **Find** avec retours chariot

    La commande **Find** ne reconnaît pas les retours chariot. Lorsque vous utilisez **Rechercher** pour rechercher du texte dans un fichier qui comprend des retours chariot, vous devez limiter la chaîne de recherche au texte qui se trouve entre les retours chariot (c’est-à-dire une chaîne qui n’est pas susceptible d’être interrompue par un retour chariot). Par exemple, **Find** ne signale pas de correspondance pour le fichier de taxes de chaîne si un retour chariot se produit entre les mots Tax et file.

## <a name="examples"></a>Exemples

Pour afficher toutes les lignes de Pencil.ad qui contiennent la chaîne de crayon, tapez :
```
find Pencil Sharpener pencil.ad
```
Pour rechercher une chaîne qui contient du texte entre guillemets, vous devez placer la totalité de la chaîne entre guillemets. Ensuite, vous devez utiliser deux guillemets pour chaque guillemet contenu dans la chaîne. Pour rechercher les scientifiques qui ont été étiquetés dans leur document à des fins de discussion uniquement. Il ne s’agit pas d’un rapport final. dans Report. doc, tapez :
```
find The scientists labeled their paper for discussion only. It is not a final report. report.doc
```
Si vous souhaitez rechercher un ensemble de fichiers, vous pouvez utiliser la commande **Find** dans la commande **for** . Pour rechercher dans le répertoire actif les fichiers dont l’extension est. bat et qui contiennent l’invite de chaîne, tapez :
```
for %f in (*.bat) do find PROMPT %f 
```
Pour rechercher et afficher les noms de fichiers sur le lecteur C contenant la chaîne processeur, utilisez le canal (|) pour diriger la sortie de la commande **dir** vers la commande **Find** comme suit :
```
dir c:\ /s /b | find CPU 
```
Étant **donné que** les recherches de recherche respectent la casse et que **dir** produit une sortie en majuscules, vous devez taper la chaîne UC en majuscules ou utiliser l’option de ligne de commande **/i** avec **Find**.

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)