---
title: sort
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 77116469-4790-4442-8a21-9fa73b65ef9f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1d38beef74156d9d57b947883c542c2c7e971e00
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854750"
---
# <a name="sort"></a>sort



Lit l’entrée, trie les données et écrit les résultats à l’écran, dans un fichier ou vers un autre appareil.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
sort [/r] [/+<N>] [/m <Kilobytes>] [/l <Locale>] [/rec <Characters>] [[<Drive1>:][<Path1>]<FileName1>] [/t [<Drive2>:][<Path2>]] [/o [<Drive3>:][<Path3>]<FileName3>]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/r|Inverse l’ordre de tri (autrement dit, les tris de Z à A et de 9 à 0).|
|/+\<N>|Spécifie le numéro de position de caractère où **tri** commence chaque comparaison. *N* peut être un entier valid.|
|/m \<Kilobytes>|Spécifie la quantité de mémoire principale à utiliser pour le tri en kilo-octets (Ko).|
|/l \<Locale>|Remplace l’ordre de tri des caractères qui sont définis par les paramètres régionaux par défaut du système (autrement dit, la langue et le pays/région sélectionné lors de l’installation).|
|/Rec \<caractères >|Spécifie le nombre maximal de caractères dans un enregistrement ou d’une ligne du fichier d’entrée (la valeur par défaut est 4 096 et la valeur maximale est de 65 535).|
|[\<Drive1>:][\<Path1>]\<FileName1>|Spécifie le fichier à trier. Si aucun nom de fichier n’est spécifié, l’entrée standard est triée. En spécifiant le fichier d’entrée est plus rapide que de rediriger le même fichier en tant qu’entrée standard.|
|/t [\<Drive2>:][\<Path2>]|Spécifie le chemin d’accès du répertoire pour contenir le **tri** commande de travail de stockage si les données ne tient pas dans la mémoire principale. Par défaut, le répertoire temporaire du système est utilisé.|
|/o [\<Drive3>:][\<Path3>]\<FileName3>|Spécifie le fichier dans lequel l’entrée triée doit être stocké. Si non spécifié, les données sont écrites dans la sortie standard. En spécifiant le fichier de sortie est plus rapide que la redirection de sortie standard vers le même fichier.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   À l’aide de la **/+** option de ligne de commande

    Par défaut, les comparaisons commencent au premier caractère de chaque ligne. Le **/+** option de ligne de commande démarre les comparaisons au niveau du caractère spécifié par *N*. Par exemple, `/+3` indique que chaque comparaison doit commencer au troisième caractère de chaque ligne. Lignes de moins de *N* caractères sont assemblées avant les autres lignes.
-   À l’aide de la **/m** option de ligne de commande

    La mémoire utilisée est toujours un minimum de 160 Ko. Si la taille de mémoire est spécifiée, la quantité exacte spécifiée est utilisée pour le tri (doit être d’au moins 160 Ko), quelle que soit la quantité de mémoire principale n’est disponible.

    La taille de mémoire maximale par défaut lorsque aucune taille n’est spécifiée est de 90 pour cent de la mémoire principale disponible si l’entrée et la sortie sont des fichiers ou 45 pour cent de la mémoire principale dans le cas contraire. La valeur par défaut définissant généralement offre de meilleures performances.
-   À l’aide de la **/l** option de ligne de commande

    Les paramètres régionaux « C », qui sont plus rapide que le tri de langage naturel (il trie les caractères en fonction de leurs codes binaires) est actuellement, la seule alternative aux paramètres régionaux par défaut.
-   À l’aide de symboles de redirection avec la **tri** commande

    Vous pouvez utiliser le symbole de barre verticale (**|**) pour diriger les données d’entrée pour le **tri** commande à partir d’une autre commande ou pour diriger la sortie triée vers une autre commande. Vous pouvez spécifier des fichiers d’entrée et de sortie à l’aide de symboles de redirection (**<** ou **>**). Il peut être plus rapide et plus efficace (surtout avec des fichiers volumineux) pour spécifier le fichier d’entrée directement (comme défini par *Nom_fichier1* dans la syntaxe de commande), puis spécifiez le fichier de sortie à l’aide de la **/o** paramètre.
-   Respecte la casse

    Le **tri** commande ne fait pas la distinction entre majuscules et minuscules.
-   Limite la taille de fichier

    Le **tri** commande n’a aucune limite sur la taille du fichier.
-   Séquence de classement

    Le programme de tri utilise la table de séquence de classement qui correspond aux paramètres de code et de la page de codes de pays/région. Les caractères supérieurs au code ASCII 127 sont triés en fonction des informations dans le fichier Country.sys ou dans un autre fichier spécifié par le **pays** commande dans votre fichier Config.nt.
-   Utilisation de la mémoire

    Si le tri est adaptée à la taille de mémoire maximale (en tant que jeu par défaut ou comme spécifié par le **/m** paramètre), le tri est effectué en une seule passe. Sinon, le tri est effectué en deux passes de tri et de fusion distincts, et les quantités de mémoire utilisée pour les deux étapes sont égale. Lorsque deux passes sont effectuées, les données triées partiellement sont stockées dans un fichier temporaire sur le disque. Si vous ne comporte pas suffisamment de mémoire pour effectuer le tri en deux passes, une erreur d’exécution est émise. Si le **/m** option de ligne de commande permet de spécifier plus de mémoire qu’il est réellement disponible, une dégradation des performances ou une erreur d’exécution peut se produire.

## <a name="BKMK_examples"></a>Exemples

**Tri d’un fichier**

Pour trier et afficher dans l’ordre inverse les lignes dans un fichier nommé dépenses.txt, tapez :

`sort /r expenses.txt`

**Trier la sortie à partir d’une commande**

Pour rechercher un fichier volumineux nommé Maillist.txt le texte « Jones » et pour trier les résultats de la recherche, utilisez la barre verticale (|) pour diriger la sortie d’un **trouver** commande le **tri** de commande, comme suit :

`find "Jones" maillist.txt | sort`

La commande génère une liste triée des lignes qui contiennent le texte spécifié.

**Tri d’entrée au clavier**

Pour trier les entrées au clavier et afficher les résultats par ordre alphabétique à l’écran, vous pouvez tout d’abord utiliser le **tri** commande sans paramètres, comme suit :

`sort`

Tapez le texte que vous voulez triées, puis appuyez sur entrée à la fin de chaque ligne. Lorsque vous avez fini de taper le texte, appuyez sur CTRL + Z, puis appuyez sur ENTRÉE. Le **tri** commande affiche le texte que vous avez tapé, triées par ordre alphabétique.

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)