---
title: sort
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 65b091a6de4f20ce94389ed39f4fe645c72b3560
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383948"
---
# <a name="sort"></a>sort



Lit les données d’entrée, trie les données et écrit les résultats à l’écran, dans un fichier ou sur un autre appareil.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
sort [/r] [/+<N>] [/m <Kilobytes>] [/l <Locale>] [/rec <Characters>] [[<Drive1>:][<Path1>]<FileName1>] [/t [<Drive2>:][<Path2>]] [/o [<Drive3>:][<Path3>]<FileName3>]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/r|Inverse l’ordre de tri (autrement dit, le tri de Z à A et de 9 à 0).|
|/+\<N >|Spécifie le numéro de position de caractère où le **Tri** commence chaque comparaison. *N* peut être n’importe quel entier valide.|
|/m \<kilo-octets >|Spécifie la quantité de mémoire principale à utiliser pour le tri en kilo-octets (Ko).|
|/l \<paramètres régionaux >|Remplace l’ordre de tri des caractères définis par les paramètres régionaux par défaut du système (autrement dit, la langue et le pays/région sélectionnés lors de l’installation).|
|/Rec \<caractères >|Spécifie le nombre maximal de caractères dans un enregistrement ou une ligne du fichier d’entrée (la valeur par défaut est 4 096 et la valeur maximale 65 535).|
|[\<Lecteur1 >:] [\<chemin1 >]\<Nomfichier1 >|Spécifie le fichier à trier. Si aucun nom de fichier n’est spécifié, l’entrée standard est triée. La spécification du fichier d’entrée est plus rapide que la redirection du même fichier en tant qu’entrée standard.|
|/t [\<lecteur2 >:] [\<Chemin2 >]|Spécifie le chemin d’accès du répertoire dans lequel stocker le stockage de travail de la commande de **Tri** si les données ne tiennent pas dans la mémoire principale. Par défaut, le répertoire temporaire du système est utilisé.|
|/o [\<Drive3 >:] [\<path3 >]\<FileName3 >|Spécifie le fichier dans lequel l’entrée triée doit être stockée. S’il n’est pas spécifié, les données sont écrites dans la sortie standard. La spécification du fichier de sortie est plus rapide que la redirection de la sortie standard vers le même fichier.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Utilisation de l’option de ligne de commande **/+**

    Par défaut, les comparaisons commencent au premier caractère de chaque ligne. L’option de ligne de commande **/+** démarre les comparaisons au caractère spécifié par *N*. Par exemple, `/+3` indique que chaque comparaison doit commencer au troisième caractère de chaque ligne. Les lignes contenant moins de *N* caractères sont assemblées avant d’autres lignes.
-   Utilisation de l’option de ligne de commande **/m**

    La mémoire utilisée est toujours au minimum de 160 Ko. Si la taille de la mémoire est spécifiée, la quantité exacte spécifiée est utilisée pour le tri (doit être d’au moins 160 Ko), quelle que soit la quantité de mémoire principale disponible.

    La taille de mémoire maximale par défaut quand aucune taille n’est spécifiée est de 90% de la mémoire principale disponible si l’entrée et la sortie sont des fichiers, ou 45% de la mémoire principale dans le cas contraire. Le paramètre par défaut offre généralement les meilleures performances.
-   Utilisation de l’option de ligne de commande **/l**

    Actuellement, la seule alternative aux paramètres régionaux par défaut est les paramètres régionaux « C », ce qui est plus rapide que le tri en langage naturel (il trie les caractères en fonction de leurs encodages binaires).
-   Utilisation de symboles de redirection avec la commande **sort**

    Vous pouvez utiliser le symbole de barre verticale ( **|** ) pour diriger les données d’entrée vers la commande de **Tri** à partir d’une autre commande ou pour diriger la sortie triée vers une autre commande. Vous pouvez spécifier les fichiers d’entrée et de sortie à l’aide de symboles de redirection ( **<** ou **>** ). Il peut être plus rapide et plus efficace (surtout avec les fichiers volumineux) pour spécifier le fichier d’entrée directement (comme défini par *NomFichier1* dans la syntaxe de commande), puis spécifier le fichier de sortie à l’aide du paramètre **/o** .
-   Respect de la casse

    La commande de **Tri** ne fait pas la distinction entre les majuscules et les minuscules.
-   Limites de taille de fichier

    La commande de **Tri** n’a pas de limite de taille de fichier.
-   Séquence de classement

    Le programme de tri utilise la table de séquence de classement qui correspond aux paramètres de code de pays/région et de page de codes. Les caractères supérieurs au code ASCII 127 sont triés en fonction des informations contenues dans le fichier Country. sys ou dans un autre fichier spécifié par la commande **Country** dans votre fichier config. NT.
-   Utilisation de la mémoire

    Si le tri correspond à la taille de mémoire maximale (définie par défaut ou telle que spécifiée par le paramètre **/m** ), le tri est effectué en une seule passe. Dans le cas contraire, le tri est effectué en deux passes de tri et de fusion distincts, et les quantités de mémoire utilisées pour les deux passes sont égales. Lorsque deux passes sont effectuées, les données partiellement triées sont stockées dans un fichier temporaire sur le disque. Si la mémoire est insuffisante pour effectuer le tri en deux passes, une erreur d’exécution est générée. Si l’option de ligne de commande **/m** est utilisée pour spécifier plus de mémoire que ce qui est réellement disponible, une dégradation des performances ou une erreur d’exécution peut se produire.

## <a name="BKMK_examples"></a>Illustre

**Tri d’un fichier**

Pour trier et afficher dans l’ordre inverse, les lignes d’un fichier nommé dépenses. txt, tapez :

`sort /r expenses.txt`

**Tri de la sortie d’une commande**

Pour effectuer une recherche dans un fichier volumineux nommé maillist. txt pour le texte « Jones » et trier les résultats de la recherche, utilisez la barre verticale (|) pour diriger la sortie d’une commande de **recherche** vers la commande de **Tri** , comme suit :

`find "Jones" maillist.txt | sort`

La commande génère une liste triée des lignes qui contiennent le texte spécifié.

**Tri de l’entrée au clavier**

Pour trier l’entrée au clavier et afficher les résultats par ordre alphabétique à l’écran, vous pouvez tout d’abord utiliser la commande de **Tri** sans paramètres, comme suit :

`sort`

Tapez ensuite le texte que vous souhaitez trier, puis appuyez sur entrée à la fin de chaque ligne. Lorsque vous avez fini de taper du texte, appuyez sur CTRL + Z, puis appuyez sur entrée. La commande de **Tri** affiche le texte que vous avez tapé, trié par ordre alphabétique.

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)