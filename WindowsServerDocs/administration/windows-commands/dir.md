---
title: dir
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: edcbf69b-eaa4-466e-b210-3dd8892f4d93
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b073b0557cd011f6742a8a8e532165f53b0a6974
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377877"
---
# <a name="dir"></a>dir



Affiche la liste des fichiers et sous-répertoires d’un répertoire. En cas d’utilisation sans paramètre, **dir** affiche le nom de volume et le numéro de série du disque, suivis d’une liste de répertoires et de fichiers sur le disque (y compris leur nom et la date et l’heure de leur dernière modification). Pour les fichiers, **dir** affiche l’extension de nom et la taille en octets. **Dir** affiche également le nombre total de fichiers et de répertoires figurant dans la liste, leur taille cumulée et l’espace libre (en octets) restant sur le disque.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#examples).

## <a name="syntax"></a>Syntaxe

```
dir [<Drive>:][<Path>][<FileName>] [...] [/p] [/q] [/w] [/d] [/a[[:]<Attributes>]][/o[[:]<SortOrder>]] [/t[[:]<TimeField>]] [/s] [/b] [/l] [/n] [/x] [/c] [/4]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|[\<> de lecteur :] [<Path>]|Spécifie le lecteur et le répertoire dont vous souhaitez afficher la liste.|
|[\<FileName >]|Spécifie un fichier ou un groupe de fichiers particulier pour lequel vous souhaitez afficher une liste.|
|/p|Affiche un écran de la liste à la fois. Pour afficher l’écran suivant, appuyez sur une touche du clavier.|
|/q|Affiche les informations de propriété du fichier.|
|/w|Affiche la liste dans un format étendu, avec un maximum de cinq noms de fichiers ou de répertoires sur chaque ligne.|
|/d|Affiche la liste dans le même format que **/w**, mais les fichiers sont triés par colonne.|
|/a [[ :]\<les attributs >]|Affiche uniquement les noms de ces répertoires et fichiers avec les attributs que vous spécifiez. Si vous omettez **/a**, **dir** affiche les noms de tous les fichiers, à l’exception des fichiers système et cachés. Si vous utilisez **/a** sans spécifier d' *attributs*, **dir** affiche les noms de tous les fichiers, y compris les fichiers système et les fichiers cachés.</br>La liste suivante décrit chacune des valeurs que vous pouvez utiliser pour les *attributs*. Utilisation d’un signe deux-points ( :) est facultatif. Utilisez n’importe quelle combinaison de ces valeurs et ne séparez pas les valeurs par des espaces.</br>**d** répertoires</br>fichiers masqués **h**</br>fichiers système **s**</br>points d’analyse **l**</br>fichiers en lecture seule **r**</br>**fichiers prêts** pour l’archivage</br>**je** ne trouve pas les fichiers indexés</br>**-** Signification du préfixe « not »|
|/o [[ :]\<SortOrder >]|Trie la sortie en fonction de *SortOrder*, qui peut être n’importe quelle combinaison des valeurs suivantes :</br>**n** par nom (alphabétique)</br>**e** par extension (par ordre alphabétique)</br>premiers répertoires du groupe **g**</br>**s** par taille (le plus petit en premier)</br>**d** par date/heure (le plus ancien en premier)</br>**-** Préfixe de l’ordre inverse</br>Remarque : l’utilisation d’un signe deux-points est facultative. Les valeurs multiples sont traitées dans l’ordre dans lequel vous les répertoriez. Ne séparez pas plusieurs valeurs par des espaces.</br>Si *SortOrder* n’est pas spécifié, **dir/o** répertorie les répertoires par ordre alphabétique, suivis des fichiers, qui sont également triés par ordre alphabétique.|
|/t [[ :]\<TimeField >]|Spécifie le champ d’heure à afficher ou à utiliser pour le tri. La liste suivante décrit chacune des valeurs que vous pouvez utiliser pour *TimeField*:</br>création **c**</br>**un** dernier accès</br>en **dernier écrit**|
|/s|Répertorie toutes les occurrences du nom de fichier spécifié dans le répertoire spécifié et dans tous les sous-répertoires.|
|/b|Affiche une liste complète des répertoires et des fichiers, sans informations supplémentaires. **/b** remplace **/w**.|
|/l|Affiche les noms de répertoires et les noms de fichiers non triés en minuscules.|
|/n|Affiche un format de liste longue avec les noms de fichiers à l’extrême droite de l’écran.|
|/x|Affiche les noms courts générés pour les noms de fichiers non-8dot3. L’affichage est le même que l’affichage pour **/n**, mais le nom abrégé est inséré avant le nom long.|
|/c|Affiche le séparateur de milliers dans la taille des fichiers. Il s’agit du comportement par défaut. Utilisez **/-c** pour masquer les séparateurs.|
|/4|Affiche les années au format à quatre chiffres.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

- Pour utiliser plusieurs paramètres de *nom* de fichier, séparez chaque nom de fichier par un espace, une virgule ou un point-virgule.
- Vous pouvez utiliser des caractères génériques **&#42;** (ou **?** ) pour représenter un ou plusieurs caractères d’un nom de fichier et pour afficher un sous-ensemble de fichiers ou de sous-répertoires.

  **Astérisque (\*) :** Utilisez l’astérisque comme substitut pour toute chaîne de caractères, par exemple :  
  - **dir \*. txt** répertorie tous les fichiers du répertoire actif avec les extensions qui commencent par. txt, par exemple. txt,. txt1,. txt_old.
  - **dir read\*. txt** répertorie tous les fichiers du répertoire actif qui commencent par « Read » et les extensions qui commencent par. txt, telles que. txt,. txt1 ou. txt_old.
  - **dir read\*.\\** * répertorie tous les fichiers du répertoire actif qui commencent par « Read », avec n’importe quelle extension.

  Le caractère générique astérisque utilise toujours le mappage de noms de fichiers courts. vous risquez donc d’obtenir des résultats inattendus. Par exemple, le répertoire suivant contient deux fichiers (t. txt2 et T97. txt) : 
 
  ```
  C:\test>dir /x
  Volume in drive C has no label.
  Volume Serial Number is B86A-EF32
    
  Directory of C:\test
    
  11/30/2004  01:40 PM <DIR>  .
  11/30/2004  01:40 PM <DIR> ..
  11/30/2004  11:05 AM 0 T97B4~1.TXT t.txt2
  11/30/2004  01:16 PM 0 t97.txt
  ```  

  Vous pouvez vous attendre à ce que la saisie de **dir t97\\** * retourne le fichier T97. txt. Toutefois, le fait de taper **dir t97\\** * retourne les deux fichiers, car le caractère générique astérisque correspond au fichier t. txt2 T97. txt en utilisant son mappage de noms courts T97B4 ~ 1. txt. De même, si vous tapez **del t97\\** *, les deux fichiers sont supprimés.

  **Point d’interrogation ( ?) :** Utilisez le point d’interrogation comme substitut d’un caractère unique dans un nom. Par exemple, **en tapant dir read ???. txt** répertorie tous les fichiers du répertoire actif portant l’extension. txt qui commencent par « Read » et qui sont suivis d’un maximum de trois caractères. Cela comprend Read. txt, Read1. txt, Read12. txt, Read123. txt et Readme1. txt, mais pas Readme12. txt.
- Spécification des attributs d’affichage de fichier

  Si vous utilisez **/a** avec plusieurs valeurs dans les *attributs*, **dir** affiche uniquement les noms de ces fichiers avec tous les attributs spécifiés. Par exemple, si vous utilisez **/a** avec **r** et **-h** comme attributs (à l’aide de **/a : r-h** ou **/AR-h**), **dir** n’affiche que les noms des fichiers en lecture seule qui ne sont pas masqués.
- Spécification du tri des noms de fichiers

  Si vous spécifiez plusieurs valeurs *SortOrder* , **dir** trie les noms de fichiers selon le premier critère, puis selon le deuxième critère, et ainsi de suite. Par exemple, si vous utilisez **/o** avec les valeurs **e** et **-s** pour *SortOrder* (en utilisant **/o : e-s** ou **/OE-s**), **dir** trie les noms des répertoires et des fichiers par extension, avec le plus grand en premier, puis affiche le résultat final. Le tri par ordre alphabétique par extension entraîne l’affichage en premier des noms de fichiers sans extensions, puis des noms de répertoire, puis des noms de fichiers avec des extensions.
- Utilisation de symboles et de canaux de redirection

  Lorsque vous utilisez le symbole de redirection ( **>** ) pour envoyer la sortie de l' **Annuaire** vers un fichier ou un canal ( **|** ) pour envoyer la sortie du **répertoire** à une autre commande, utilisez **/a :-d** et **/b** pour répertorier uniquement les noms de fichiers. Vous pouvez utiliser *filename* avec **/b** et **/s** pour spécifier que **dir** consiste à rechercher dans le répertoire actif et ses sous-répertoires tous les noms de fichiers qui correspondent au *nom*de fichier. **Dir** répertorie uniquement la lettre de lecteur, le nom de répertoire, le nom de fichier et l’extension de nom de fichier (un chemin par ligne), pour chaque nom de fichier trouvé. Avant d’utiliser un canal pour envoyer une sortie de **répertoire** à une autre commande, vous devez définir la variable d’environnement TEMP dans votre fichier Autoexec. NT.
- La commande **dir** , avec des paramètres différents, est disponible à partir de la console de récupération.

## <a name="examples"></a>Exemples

Pour afficher tous les répertoires l’un après l’autre, dans l’ordre alphabétique, dans un format étendu et en suspendant après chaque écran, assurez-vous que le répertoire racine est le répertoire actif, puis tapez :

```
dir /s/w/o/p
```

**Dir** répertorie le répertoire racine, les sous-répertoires et les fichiers dans le répertoire racine, y compris les extensions. Ensuite, **dir** répertorie les noms de sous-répertoires et les noms de fichiers dans chaque sous-répertoire de l’arborescence.

Pour modifier l’exemple précédent de manière à ce que **dir** affiche les noms et les extensions de fichiers, mais omet les noms de répertoire, tapez :

```
dir /s/w/o/p/a:-d
```

Pour imprimer une liste de répertoires, tapez :

```
dir > prn
```

Lorsque vous spécifiez **PRN**, la liste de répertoires est envoyée à l’imprimante attachée au port LPT1. Si votre imprimante est attachée à un autre port, vous devez remplacer **PRN** par le nom du port approprié.

Vous pouvez également rediriger la sortie de la commande **dir** vers un fichier en remplaçant **PRN** par un nom de fichier. Vous pouvez également taper un chemin d’accès. Par exemple, pour diriger la sortie vers le **fichier dir.** doc dans le répertoire Records, tapez :

```
dir > \records\dir.doc
```

Si dir. doc n’existe pas, **dir** le crée, sauf si le répertoire Records n’existe pas. Dans ce cas, le message suivant s’affiche :

`File creation error`

Pour afficher la liste de tous les noms de fichiers avec l’extension. txt dans tous les répertoires sur le lecteur C, tapez :

```
dir c:\*.txt /w/o/s/p
```

**Dir** affiche, dans un format étendu, une liste alphabétique des noms de fichiers correspondants dans chaque répertoire, et s’interrompt chaque fois que l’écran se remplit jusqu’à ce que vous appuyiez sur une touche pour continuer.

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)