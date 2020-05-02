---
title: remplacer
description: Découvrez comment utiliser la commande Replace pour remplacer des fichiers.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6143661e-d90f-4812-b265-6669b567dd1f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 4ac424154968b4f4c55664d0d20f524345b87986
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722379"
---
# <a name="replace"></a>remplacer



Remplace des fichiers. Si elle est utilisée avec l’option **/a** , **Replace** ajoute de nouveaux fichiers à un répertoire au lieu de remplacer les fichiers existants.



## <a name="syntax"></a>Syntaxe

```
replace [<Drive1>:][<Path1>]<FileName> [<Drive2>:][<Path2>] [/a] [/p] [/r] [/w] 
replace [<Drive1>:][<Path1>]<FileName> [<Drive2>:][<Path2>] [/p] [/r] [/s] [/w] [/u] 
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|[\<Lecteur1> :] [\<Chemin1>] \<Nom de fichier>|Spécifie l’emplacement et le nom du fichier source ou de l’ensemble de fichiers. *Filename* est obligatoire et peut inclure des caractères génériques (**&#42;** et **?**).|
|[\<Lecteur2> :] [\<Chemin2>]|Spécifie l’emplacement du fichier de destination. Vous ne pouvez pas spécifier un nom de fichier pour les fichiers que vous remplacez. Si vous ne spécifiez pas de lecteur ou de chemin d’accès, **Replace** utilise le lecteur et le répertoire actuels comme destination.|
|/a|Ajoute de nouveaux fichiers au répertoire de destination au lieu de remplacer les fichiers existants. Vous ne pouvez pas utiliser cette option de ligne de commande avec l’option de ligne de commande **/s** ou **/u** .|
|/p|Vous invite à confirmer le remplacement d’un fichier de destination ou l’ajout d’un fichier source.|
|/r|Remplace les fichiers en lecture seule et non protégés. Si vous tentez de remplacer un fichier en lecture seule, mais que vous ne spécifiez pas **/r**, une erreur se produit et arrête l’opération de remplacement.|
|/w|Attend l’insertion d’un disque avant le début de la recherche des fichiers sources. Si vous ne spécifiez pas **/w**, **Replace** commence à remplacer ou à ajouter des fichiers immédiatement après avoir appuyé sur entrée.|
|/s|Effectue une recherche dans tous les sous-répertoires du répertoire de destination et remplace les fichiers correspondants. Vous ne pouvez pas utiliser **/s** avec l’option de ligne de commande **/a** . La commande **Replace** n’effectue pas de recherche dans les sous-répertoires spécifiés dans *chemin1*.|
|/U|Remplace uniquement les fichiers du répertoire de destination qui sont antérieurs à ceux du répertoire source. Vous ne pouvez pas utiliser **/u** avec l’option de ligne de commande **/a** .|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes 

- Comme le **remplacement** ajoute ou remplace des fichiers, les noms de fichiers s’affichent à l’écran. Une fois le **remplacement** terminé, une ligne de résumé s’affiche dans l’un des formats suivants :  
  ```
  nnn files added
  nnn files replaced
  no file added
  no file replaced
  ```  
- Si vous utilisez des disquettes et que vous devez changer de disque au cours de l’opération de **remplacement** , vous pouvez spécifier l’option de ligne de commande **/w** afin que le **remplacement** attende que vous basculiez les disques.
- Vous ne pouvez pas utiliser **remplacer** pour mettre à jour les fichiers cachés ou les fichiers système.
- Le tableau suivant présente chaque code de sortie et une brève description de sa signification :  
  |Code de sortie|Description|
  |---------|-----------|
  |0|La commande de **remplacement** a correctement remplacé ou ajouté les fichiers.|
  |1|La commande **Replace** a rencontré une version incorrecte de MS-DOS.|
  |2|La commande **Replace** n’a pas pu trouver les fichiers sources.|
  |3|La commande de **remplacement** n’a pas pu trouver le chemin d’accès source ou de destination.|
  |5|L’utilisateur n’a pas accès aux fichiers que vous souhaitez remplacer.|
  |8|La mémoire système est insuffisante pour exécuter la commande.|
  |11|L’utilisateur a utilisé une syntaxe incorrecte sur la ligne de commande.|

> [!NOTE]
> Vous pouvez utiliser le paramètre ERRORLEVEL sur la ligne de commande **If** dans un programme de traitement par lots pour traiter les codes de sortie retournés par **Replace**.

## <a name="examples"></a><a name="BKMK_examples"></a>Illustre

Pour mettre à jour toutes les versions d’un fichier nommé téléphones. CLI (qui apparaissent dans plusieurs répertoires sur le lecteur C), avec la dernière version du fichier phones. CLI à partir d’une disquette dans le lecteur A, tapez :

`replace a:\phones.cli c:\ /s`

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)