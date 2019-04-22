---
title: appel
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d34a41dc-e6c7-4467-bf6a-15cec704833e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 06/05/2018
ms.openlocfilehash: e2366133c4699f43731c9a4e8a7a8238fc83031d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59815250"
---
# <a name="call"></a>appel



Appelle un lot à partir d’un autre sans arrêter le programme parent. Le **appeler** commande accepte des étiquettes comme cible de l’appel.

> [!NOTE]
> **Appelez** à l’invite de commande n’a aucun effet lorsqu’il est utilisé en dehors d’un fichier de script ou lot.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
call [Drive:][Path]<FileName> [<BatchParameters>] [:<Label> [<Arguments>]]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|[\<Drive>:][<Path>]<FileName>|Spécifie l’emplacement et le nom du programme de traitement par lots que vous souhaitez appeler. Le *FileName* paramètre est obligatoire et doit avoir une extension .bat ou .cmd.|
|\<BatchParameters>|Spécifie les informations de ligne de commande requises par le programme de traitement par lots.|
|:\<Label>|Spécifie l’étiquette que vous souhaitez un contrôle de programme de traitement par lots pour accéder à.|
|\<Arguments>|Spécifie les informations de ligne de commande à passer à la nouvelle instance du programme de traitement par lots, en commençant à *: étiquette.*|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="batch-parameters"></a>Paramètres de lot

Les références d’argument de script batch (**%0**, **%1**,...) sont répertoriés dans les tableaux suivants.

**%*** dans un lot script fait référence à tous les arguments (par exemple, **%1**, **%2**, **%3**...)

Vous pouvez utiliser les syntaxes facultatives suivantes en tant que substitutions pour les paramètres de traitement par lots (**%n**) :

|Paramètre de lot|Description|
|---------------|-----------|
|%~1|Se développe **%1** et supprime entourant de guillemets ( » «).|
|%~f1|Se développe **%1** à un chemin d’accès qualifié complet.|
|%~d1|Se développe **%1** uniquement une lettre de lecteur.|
|%~p1|Se développe **%1** uniquement un chemin d’accès.|
|%~n1|Se développe **%1** à un nom de fichier uniquement.|
|%~x1|Se développe **%1** à une extension de nom de fichier uniquement.|
|%~s1|Se développe **%1** à un chemin d’accès qualifié complet qui contient uniquement des noms courts.|
|%~a1|Se développe **%1** aux attributs du fichier.|
|%~t1|Se développe **%1** à la date et l’heure du fichier.|
|%~z1|Se développe **%1** à la taille du fichier.|
|% ~ $PATH : 1|Recherche les répertoires énumérés dans la variable d’environnement PATH et développe **%1** pour le nom qualifié complet du premier répertoire trouvé. Si le nom de variable d’environnement n’est pas défini ou le fichier n’est pas trouvé par la recherche, ce modificateur se développe en une chaîne vide.|

Le tableau suivant montre comment vous pouvez combiner des modificateurs avec les paramètres de traitement par lots pour les résultats composés :

|Paramètre de lot avec le modificateur|Description|
|-----------------------------|-----------|
|%~dp1|Se développe **%1** à une lettre de lecteur et le chemin d’accès uniquement.|
|%~nx1|Se développe **%1** à un nom de fichier et l’extension uniquement.|
|%~dp$PATH:1|Recherche les répertoires énumérés dans la variable d’environnement PATH pour **%1**, puis développe à la lettre de lecteur et le chemin d’accès du premier répertoire trouvé.|
|%~ftza1|Se développe **%1** pour afficher la sortie similaire à la **dir** commande.|

Dans les exemples ci-dessus, **%1** et chemin d’accès peut être remplacé par d’autres valeurs valides. Le **%~** syntaxe se termine par un numéro d’argument valide. Le **%~** modificateurs ne peut pas être utilisés avec **% \***.

## <a name="remarks"></a>Notes

-   À l’aide des paramètres de lot

    Paramètres de lot peuvent contenir toutes les informations que vous pouvez passer à un programme de commandes, y compris les options de ligne de commande, les noms de fichiers, les paramètres de lot **%0** via **%9**et les variables (par exemple, **(en bauds) %**).
-   À l’aide de la *étiquette* paramètre

    À l’aide de **appeler** avec la *étiquette* paramètre, vous créez un nouveau contexte de fichier de commandes et passer le contrôle à l’instruction après l’étiquette spécifiée. La première fois que la fin du fichier de commandes (autrement dit, après le passage à l’étiquette), contrôle retourne à l’instruction après le **appeler** instruction. La deuxième fois que la fin du fichier de commandes, le script de lot est terminé.
-   À l’aide de canaux et des symboles de redirection

    N’utilisez pas de canaux (**|**) et les symboles de redirection (**<** ou **>**) avec **appeler**.
-   Appel récursif

    Vous pouvez créer un programme de traitement par lots qui s’appelle elle-même. Toutefois, vous devez fournir une condition de sortie. Sinon, les programmes de traitement par lots parent et enfant peuvent exécuter indéfiniment une boucle.
-   Utilisation des extensions de commande

    Si les extensions de commande sont activées, **appeler** accepte *étiquette* comme cible de l’appel. La syntaxe correcte est comme suit :

    `call :\<Label> <Arguments>`

## <a name="BKMK_examples"></a>Exemples

Pour exécuter le programme VeriNouv.bat à partir d’un autre programme de commandes, tapez la commande suivante dans le programme parent :
```
call checknew
```
Si le programme parent accepte deux paramètres de traitement par lots et que vous le souhaitez pour transférer ces paramètres à VeriNouv.bat, tapez la commande suivante dans le programme parent :
```
call checknew %1 %2
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)