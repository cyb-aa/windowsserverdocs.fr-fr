---
title: appel
description: Rubrique de référence pour la commande Call, qui appelle un programme de traitement par lots à partir d’un autre sans arrêter le programme de traitement par lots parent.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d34a41dc-e6c7-4467-bf6a-15cec704833e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 06/05/2018
ms.openlocfilehash: 64c4b89d18ab869a7e6c8b1ee8537c4f808bce8f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719666"
---
# <a name="call"></a>appel

Appelle un programme de traitement par lots à partir d’un autre sans arrêter le programme de traitement par lots parent. La commande **Call** accepte des étiquettes en tant que cible de l’appel.

> [!NOTE]
> L’appel n’a aucun effet à l’invite de commandes lorsqu’il est utilisé en dehors d’un script ou d’un fichier de commandes.

## <a name="syntax"></a>Syntaxe

```
call [drive:][path]<filename> [<batchparameters>] [:<label> [<arguments>]]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| `[<drive>:][<path>]<filename>` | Spécifie l’emplacement et le nom du programme de traitement par lots que vous souhaitez appeler. Le `<filename>` paramètre est obligatoire et doit avoir une extension. bat ou. cmd. |
| `<batchparameters>` | Spécifie les informations de ligne de commande requises par le programme de traitement par lots. |
| `:<label>` | Spécifie l’étiquette à laquelle vous souhaitez qu’un contrôle de programme de traitement par lots accède. |
| `<arguments>` | Spécifie les informations de ligne de commande à passer à la nouvelle instance du programme de traitement par lots `:<label>`, en commençant à.|
| /? | Affiche l'aide à l'invite de commandes. |

## <a name="batch-parameters"></a>Paramètres du lot

Les références de l’argument de script batch (**%0**, **%1**,...) sont répertoriées dans les tableaux suivants.

L’utilisation de la valeur **% &#42;** dans un script de commandes fait référence à tous les arguments (par exemple, **%1**, **%2**, **%3**...).

Vous pouvez utiliser les syntaxes facultatives suivantes comme substitutions pour les paramètres de lot (**% n**) :

| Paramètre batch | Description |
| --------------- | ----------- |
| % ~ 1 | Développe **%1** et supprime les guillemets autour. |
| % ~ F1 | Développe **%1** vers un chemin d’accès complet. |
| % ~ D1 | Développe **%1** vers une lettre de lecteur uniquement. |
| % ~ P1 | Développe **%1** vers un chemin d’accès uniquement. |
| % ~ N1 | Développe **%1** à un nom de fichier uniquement. |
| % ~ x1 | Développe **%1** vers une extension de nom de fichier uniquement. |
| % ~ S1 | Développe **%1** vers un chemin d’accès complet qui contient uniquement des noms courts. |
| % ~ a1 | Développe **%1** vers les attributs du fichier. |
| % ~ T1 | Développe **%1** jusqu’à la date et à l’heure du fichier. |
| % ~ Z1 | Développe **%1** jusqu’à la taille du fichier. |
| % ~ $PATH : 1 | Recherche dans les répertoires figurant dans la variable d’environnement PATH et développe **%1** avec le nom complet du premier répertoire trouvé. Si le nom de la variable d’environnement n’est pas défini ou si le fichier est introuvable par la recherche, ce modificateur se développe en une chaîne vide. |

Le tableau suivant montre comment combiner des modificateurs avec les paramètres de lot pour les résultats composés :

| Paramètre batch avec modificateur | Description |
| ----------------------------- | ----------- |
| % ~ DP1 | Développe **%1** en une lettre de lecteur et un chemin d’accès uniquement. |
| % ~ NX1 | Développe **%1** vers un nom de fichier et une extension uniquement. |
| % ~ DP $ chemin : 1 | Recherche dans les répertoires figurant dans la variable d’environnement PATH pour **%1**, puis développe la lettre de lecteur et le chemin d’accès du premier répertoire trouvé. |
| % ~ ftza1 | Développe **%1** pour afficher une sortie similaire à la commande **dir** . |

Dans les exemples ci-dessus, **%1** et Path peuvent être remplacés par d’autres valeurs valides. La **%~** syntaxe se termine par un numéro d’argument valide. Les **%~** modificateurs ne peuvent pas être utilisés avec **% &#42;**.

### <a name="remarks"></a>Notes 

- Utilisation des paramètres de lot :

    Les paramètres de lot peuvent contenir toutes les informations que vous pouvez transmettre à un programme de traitement par lots, y compris les options de ligne de commande, les noms de fichiers, les paramètres de lot **%0** à **%9**et les variables (par exemple, **% Baud%**).

- Utilisation du `<label>` paramètre :

    En utilisant **Call** avec le `<label>` paramètre, vous créez un nouveau contexte de fichier de commandes et vous transmettez le contrôle à l’instruction après l’étiquette spécifiée. La première fois que la fin du fichier de commandes est rencontrée (autrement dit, après avoir atteint l’étiquette), le contrôle retourne à l’instruction après l’instruction **Call** . La deuxième fois que la fin du fichier de commandes est rencontrée, le script de traitement est fermé.

- Utilisation de canaux et de symboles de redirection :

    N’utilisez pas de `(|)` canaux ou de symboles de`<` redirection `>`(ou) avec l' **appel**.

- Exécution d’un appel récursif

    Vous pouvez créer un programme de traitement par lots qui s’appelle lui-même. Toutefois, vous devez fournir une condition de sortie. Dans le cas contraire, les programmes de traitement par lots parent et enfant peuvent être en boucle infinis.

- Utilisation des extensions de commande

    Si les extensions de commande sont **call** activées `<label>` , Call accepte en tant que cible de l’appel. La syntaxe correcte est`call :<label> <arguments>`

## <a name="examples"></a>Exemples

Pour exécuter le programme checknew. bat à partir d’un autre programme de traitement par lots, tapez la commande suivante dans le programme de traitement par lots parent :

```
call checknew
```

Si le programme de traitement par lots parent accepte deux paramètres de lot et que vous souhaitez qu’il transmette ces paramètres à checknew. bat, tapez la commande suivante dans le programme de traitement par lots parent :

```
call checknew %1 %2
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
