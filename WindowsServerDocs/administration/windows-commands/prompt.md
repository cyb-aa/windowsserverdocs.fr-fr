---
title: prompt
description: Découvrez comment personnaliser votre invite de commandes.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3d98e965-02eb-46ad-9d0a-5dc44830373e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 4b3aedc341dfcee094e20f43e39f34dc6fd08109
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722792"
---
# <a name="prompt"></a>prompt



Modifie l’invite de commandes de cmd. exe. En cas d’utilisation sans paramètres, **prompt** rétablit la valeur par défaut de l’invite de commandes, qui est la lettre de lecteur et le répertoire actuels**>** suivis du signe supérieur à ().



## <a name="syntax"></a>Syntaxe

```
prompt [<Text>]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<Texte>|Spécifie le texte et les informations que vous souhaitez inclure dans l’invite de commandes.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes 

Vous pouvez personnaliser l’invite de commandes pour afficher le texte de votre choix, y compris des informations telles que le nom du répertoire actif, la date et l’heure, ainsi que le numéro de version de Microsoft Windows.

Le tableau suivant répertorie les combinaisons de caractères que vous pouvez inclure au lieu de, ou en plus de, une ou plusieurs chaînes de caractères dans le paramètre *Text* . La liste inclut une brève description du texte ou des informations que chaque combinaison de caractères ajoute à votre invite de commandes.  

| Caractère |                                 Description                                 |
|-----------|-----------------------------------------------------------------------------|
|    $q     |                               = (signe égal)                                |
|    $$     |                               $ (signe $)                               |
|    $t     |                                Heure actuelle                                 |
|    $d     |                                Date actuelle                                 |
|    $p     |                           Lecteur et chemin d’accès actuels                            |
|    $v     |                           Numéro de version de Windows                            |
|    $n     |                                Lecteur actuel                                |
|    $g     |                            > (signe supérieur à)                            |
|    $l     |                             < (signe inférieur à)                              |
|    $b     |                              \|(symbole de barre verticale)                               |
|    $_     |                               ENTRÉE-SAUT DE LA CASSE                                |
|    $e     |                         Code d’échappement ANSI (code 27)                          |
|    $h     | Retour arrière (pour supprimer un caractère qui a été écrit sur la ligne de commande) |
|    $a     |                                & (esperluette)                                |
|    $c     |                            ((parenthèse gauche)                             |
|    $f     |                            ) (parenthèse fermante)                            |
|    $s     |                                    espace                                    |

Lorsque les extensions de commande sont activées (autrement dit, la valeur par défaut), la commande **prompt** prend en charge les caractères de mise en forme suivants :  

|Caractère|Description|
|---------|-----------|
|$+|Zéro, un ou plusieurs caractères**+** signe plus (), en fonction de la profondeur de la pile de répertoires **pushd** (un caractère pour chaque niveau poussé).|
|$m|Nom distant associé à la lettre de lecteur en cours ou à la chaîne vide si le lecteur actuel n’est pas un lecteur réseau.|

Si vous incluez le caractère **$p** dans le paramètre text, votre disque est lu après l’entrée de chaque commande (pour déterminer le lecteur et le chemin d’accès actuels). Cela peut prendre du temps supplémentaire, en particulier pour les lecteurs de disquette.

## <a name="examples"></a><a name="BKMK_examples"></a>Illustre

Pour définir une invite de commandes sur deux lignes avec l’heure et la date actuelles sur la première ligne et le signe supérieur à sur la ligne suivante, tapez :
```
prompt $d$s$s$t$_$g 
```
L’invite est modifiée comme suit, où la date et l’heure sont actuelles :
```
Fri 06/01/2007  13:53:28.91
>
```
Pour définir l’invite de commandes pour qu’elle s’affiche`-->`sous forme de flèche (), tapez :
```
prompt --$g
```
Pour remplacer manuellement l’invite de commandes par le paramètre par défaut (le lecteur et le chemin d’accès actuels suivis du signe supérieur à), tapez :
```
prompt $p$g
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
