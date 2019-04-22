---
title: prompt
description: Découvrez comment personnaliser l’invite de commandes.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3d98e965-02eb-46ad-9d0a-5dc44830373e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 320e12fd30deda30ccc0da1ad6e5bea6f9a19d8a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818440"
---
# <a name="prompt"></a>prompt



Modifie l’invite de commandes Cmd.exe. Si utilisée sans paramètres, **invite** réinitialise l’invite de commandes pour le paramètre par défaut, qui est la lettre de lecteur en cours et le répertoire suivi par le symbole supérieur à (**>**).

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
prompt [<Text>]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<Texte >|Spécifie le texte et les informations que vous souhaitez inclure dans l’invite de commandes.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

Vous pouvez personnaliser l’invite de commandes pour afficher le texte de que votre choix, y compris des informations telles que le nom du répertoire actif, l’heure et date et le numéro de version de Microsoft Windows.

Le tableau suivant répertorie les combinaisons de caractères que vous pouvez inclure à la place ou en plus, un ou plusieurs chaînes de caractères dans le *texte* paramètre. La liste comprend une brève description du texte ou des informations de chaque combinaison de caractères ajoute à l’invite de commandes.  
|Caractère|Description|
|---------|-----------|
|$q|= (signe égal)|
|$$|$ (signe dollar)|
|$t|Heure actuelle|
|$d|Date actuelle|
|$p|Chemin d’accès et le lecteur en cours|
|$v|Numéro de version de Windows|
|$n|Lecteur en cours|
|$g|> (signe supérieur à)|
|$l|< (signe inférieur à)|
|$b|| (pipe)|
|$_|ENTREZ-SAUT DE LIGNE|
|$e|Code d’échappement ANSI (code 27)|
|$h|Retour arrière (pour supprimer un caractère qui a été écrite dans la ligne de commande)|
|$un|& (esperluette)|
|$c|((parenthèse de gauche)|
|$f|) (parenthèse droite)|
|$s|espace|

Lorsque les extensions de commande sont activées (autrement dit, la valeur par défaut) le **invite** commande prend en charge les caractères de mise en forme suivants :  

|Caractère|Description|
|---------|-----------|
|$+|Signe de zéro ou plusieurs (**+**) caractères, selon la profondeur de la **pushd** pile de répertoire (un seul caractère pour chaque niveau envoyée).|
|$m|Le nom distant associé à la lettre de lecteur actuelle ou une chaîne vide si le lecteur n’est pas un lecteur réseau.|

Si vous incluez le **$p** caractère dans le paramètre de texte, le disque est lu une fois que vous entrez chaque commande (pour déterminer le lecteur actuel et le chemin d’accès). Cela peut prendre du temps, en particulier pour les lecteurs de disquette.

## <a name="BKMK_examples"></a>Exemples

Pour définir une invite de commandes de deux lignes avec la date et l’heure actuelle sur la première ligne et le signe supérieur à la ligne suivante, tapez :
```
prompt $d$s$s$t$_$g 
```
L’invite est modifiée comme suit, où la date et l’heure sont en cours :
```
Fri 06/01/2007  13:53:28.91
>
```
Pour définir l’invite de commandes à afficher sous forme de flèche (`-->`), type :
```
prompt --$g
```
Pour modifier manuellement l’invite de commandes pour le paramètre par défaut (le lecteur actuel et le chemin suivi par le signe supérieur à), tapez :
```
prompt $p$g
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)