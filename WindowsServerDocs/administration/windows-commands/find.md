---
title: find
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2ca66b22-3b7c-4166-8503-eb75fc53ab46
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4b13a2fe573ffc81fa5c85d8fd28e9ab13ca4342
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439342"
---
# <a name="find"></a>find



Recherche une chaîne de texte dans un ou plusieurs fichiers et affiche les lignes de texte qui contiennent la chaîne spécifiée.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
find [/v] [/c] [/n] [/i] [/off[line]] "<String>" [[<Drive>:][<Path>]<FileName>[...]]
```

## <a name="parameters"></a>Paramètres

|           Paramètre           |                                              Description                                               |
|-------------------------------|--------------------------------------------------------------------------------------------------------|
|              /v               |                    Affiche toutes les lignes qui ne contiennent pas le texte spécifié \<chaîne >.                     |
|              /c               |              Compte les lignes qui contiennent le texte spécifié \<chaîne > et affiche le total.              |
|              /n               |                            Fait précéder chaque ligne avec le numéro de ligne du fichier.                             |
|              /i               |                            Spécifie que la recherche ne respecte pas la casse.                            |
|         [/off[line]]          |                        N’ignore pas les fichiers qui ont l’attribut hors connexion.                        |
|          «\<Chaîne > »          | Obligatoire. Spécifie le groupe de caractères (entre guillemets) que vous souhaitez rechercher. |
| [\<Drive>:][<Path>]<FileName> |        Spécifie l’emplacement et le nom du fichier dans laquelle rechercher la chaîne spécifiée.        |
|              /?               |                                  Affiche l'aide à l'invite de commandes.                                  |

## <a name="remarks"></a>Notes

-   Spécification d’une chaîne

    Si vous n’utilisez pas **/i**, **trouver** recherche exactement ce que vous spécifiez pour *chaîne*. Par exemple, le **trouver** commande traite les caractères « a » et « A » différemment. Si vous utilisez **/i**, toutefois, **trouver** n’est pas en respectant la casse, et il traite « a » et « A » en tant que le même caractère.

    Si la chaîne que vous souhaitez rechercher contient des guillemets, vous devez utiliser des guillemets doubles pour chaque paire contenue dans la chaîne (par exemple, « « « string » » contient entre guillemets »).
-   À l’aide de **trouver** en tant que filtre

    Si vous omettez un nom de fichier **trouver** agit comme un filtre, prendre des entrées de la source d’entrée standard (généralement le clavier, une barre verticale (|) ou un fichier de redirection) et en affichant toutes les lignes qui contiennent des *chaîne*.
-   Syntaxe de commande de classement

    Vous pouvez taper les paramètres et les options de ligne de commande pour le **trouver** dans n’importe quel ordre.
-   À l’aide de caractères génériques

    Vous ne pouvez pas utiliser des caractères génériques ( **&#42;** et **?** ) dans les noms de fichiers ou les extensions que vous spécifiez avec la **trouver** commande. Pour rechercher une chaîne dans un ensemble de fichiers que vous spécifiez avec des caractères génériques, vous pouvez utiliser la **trouver** commande au sein d’un **pour** commande.
-   À l’aide de **/v** ou **/n** avec **/c**

    Si vous utilisez **/c** et **/v** dans la même ligne de commande **trouver** affiche le nombre de lignes qui ne contiennent pas la chaîne spécifiée. Si vous spécifiez **/c** et **/n** dans la même ligne de commande **trouver** ignore **/n**.
-   À l’aide de **trouver** avec chariot retourne

    Le **trouver** commande ne reconnaît pas les retours chariot. Lorsque vous utilisez **trouver** pour rechercher du texte dans un fichier qui inclut des retours-chariot imbriqués, vous devez limiter la chaîne de recherche en texte qui se trouve entre les retours-chariot imbriqués (autrement dit, une chaîne qui n’est pas susceptible d’être interrompu par un retour chariot). Par exemple, **trouver** ne signale pas une correspondance pour la chaîne « fichier taxe » si un retour chariot se produit entre les mots « vos » et « fichier ».

## <a name="BKMK_examples"></a>Exemples

Pour afficher toutes les lignes du fichier Crayon.pub qui contiennent la chaîne « Taille-crayon », tapez :
```
find "Pencil Sharpener" pencil.ad
```
Pour rechercher une chaîne qui contient le texte entre guillemets, vous devez placer la chaîne entière entre guillemets. Vous devez utiliser deux guillemets pour chaque paire contenue dans la chaîne. Pour rechercher « Les scientifiques étiquetés leur livre « pour discussion uniquement. » Il n’est pas un rapport final. » dans Report.doc, tapez :
```
find "The scientists labeled their paper ""for discussion only."" It is not a final report." report.doc
```
Si vous souhaitez rechercher un ensemble de fichiers, vous pouvez utiliser la **trouver** commande au sein de la **pour** commande. Pour rechercher le répertoire actif pour les fichiers ayant l’extension .bat et qui contiennent la chaîne « invite », tapez :
```
for %f in (*.bat) do find "PROMPT" %f 
```
Pour rechercher votre disque dur pour rechercher et afficher les noms de fichiers sur le lecteur C qui contiennent la chaîne « UC », utilisez la barre verticale (|) pour diriger la sortie de la **dir** commande le **trouver** commande comme suit :
```
dir c:\ /s /b | find "CPU" 
```
Étant donné que **trouver** recherches respectent la casse et **dir** génère la sortie en majuscules, vous devez taper la chaîne « UC » en lettres majuscules ou utiliser le **/i** de ligne de commande l’option avec **trouver**.

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)