---
title: find
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 25cd99f3a6411c637a07b7231729cbf529a5d52e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377200"
---
# <a name="find"></a>find



Recherche une chaîne de texte dans un ou des fichiers et affiche des lignes de texte qui contiennent la chaîne spécifiée.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
find [/v] [/c] [/n] [/i] [/off[line]] "<String>" [[<Drive>:][<Path>]<FileName>[...]]
```

## <a name="parameters"></a>Paramètres

|           Paramètre           |                                              Description                                               |
|-------------------------------|--------------------------------------------------------------------------------------------------------|
|              /v               |                    Affiche toutes les lignes qui ne contiennent pas la chaîne de \<spécifiée >.                     |
|              /c               |              Compte les lignes qui contiennent la chaîne de \<spécifiée > et affiche le total.              |
|              /n               |                            Précède chaque ligne avec le numéro de ligne du fichier.                             |
|              /i               |                            Spécifie que la recherche ne respecte pas la casse.                            |
|         [/OFF [Line]]          |                        N’ignore pas les fichiers dont l’attribut offline est défini.                        |
|          «\<chaîne > »          | Obligatoire. Spécifie le groupe de caractères (entre guillemets) que vous souhaitez rechercher. |
| [\<> de lecteur :] [<Path>]<FileName> |        Spécifie l’emplacement et le nom du fichier dans lequel rechercher la chaîne spécifiée.        |
|              /?               |                                  Affiche l'aide à l'invite de commandes.                                  |

## <a name="remarks"></a>Notes

-   Spécification d’une chaîne

    Si vous n’utilisez pas **/i**, **Rechercher** recherche exactement ce que vous spécifiez pour la *chaîne*. Par exemple, la commande **Find** traite différemment les caractères « a » et « a ». Toutefois, si vous utilisez **/i**, **Find** ne respecte pas la casse et traite « A » et « a » comme étant le même caractère.

    Si la chaîne que vous souhaitez rechercher contient des guillemets, vous devez utiliser des guillemets doubles pour chaque guillemet contenu dans la chaîne (par exemple, "This" "String" "Contains guillemets").
-   Utilisation de la fonction **Rechercher** comme filtre

    Si vous omettez un nom de fichier, **Find** agit comme un filtre, en prenant l’entrée de la source d’entrée standard (généralement le clavier, un canal (|) ou un fichier Redirigé), puis en affichant toutes les lignes qui contiennent une *chaîne*.
-   Classement de la syntaxe de commande

    Vous pouvez taper des paramètres et des options de ligne de commande pour la commande **Find** dans n’importe quel ordre.
-   Utilisation de caractères génériques

    Vous ne pouvez pas utiliser de **&#42;** caractères génériques (et **?** ) dans les noms de fichiers ou les extensions que vous spécifiez avec la commande **Find** . Pour rechercher une chaîne dans un ensemble de fichiers que vous spécifiez à l’aide de caractères génériques, vous pouvez utiliser la commande **Find** dans une commande **for** .
-   Utilisation de **/v** ou **/n** avec **/c**

    Si vous utilisez **/c** et **/v** dans la même ligne de commande, **Find** affiche le nombre de lignes qui ne contiennent pas la chaîne spécifiée. Si vous spécifiez **/c** et **/n** dans la même ligne de commande, l’option **Rechercher** ignore **/n**.
-   Utilisation de **Find** avec retours chariot

    La commande **Find** ne reconnaît pas les retours chariot. Lorsque vous utilisez **Rechercher** pour rechercher du texte dans un fichier qui comprend des retours chariot, vous devez limiter la chaîne de recherche au texte qui se trouve entre les retours chariot (c’est-à-dire une chaîne qui n’est pas susceptible d’être interrompue par un retour chariot). Par exemple, **Find** ne signale pas de correspondance pour la chaîne « fichier Tax » si un retour chariot se produit entre les mots « Tax » et « file ».

## <a name="BKMK_examples"></a>Illustre

Pour afficher toutes les lignes de Pencil.ad qui contiennent la chaîne « crayon de crayon », tapez :
```
find "Pencil Sharpener" pencil.ad
```
Pour rechercher une chaîne qui contient du texte entre guillemets, vous devez placer la totalité de la chaîne entre guillemets. Ensuite, vous devez utiliser deux guillemets pour chaque guillemet contenu dans la chaîne. Pour trouver « les scientifiques étiquetés » en matière de discussion uniquement». Il ne s’agit pas d’un rapport final.» dans Report. doc, tapez :
```
find "The scientists labeled their paper ""for discussion only."" It is not a final report." report.doc
```
Si vous souhaitez rechercher un ensemble de fichiers, vous pouvez utiliser la commande **Find** dans la commande **for** . Pour rechercher dans le répertoire actif les fichiers dont l’extension est. bat et qui contiennent la chaîne « PROMPT », tapez :
```
for %f in (*.bat) do find "PROMPT" %f 
```
Pour rechercher et afficher les noms de fichiers sur le lecteur C qui contiennent la chaîne « UC » dans votre disque dur, utilisez le canal (|) pour diriger la sortie de la commande **dir** vers la commande **Find** comme suit :
```
dir c:\ /s /b | find "CPU" 
```
Étant **donné que la recherche de** recherche respecte la casse et que **dir** produit une sortie en majuscules, vous devez taper la chaîne « CPU » en majuscules ou utiliser l’option de ligne de commande **/i** avec **Find**.

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)