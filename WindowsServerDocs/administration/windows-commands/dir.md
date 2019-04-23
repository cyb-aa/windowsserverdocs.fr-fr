---
title: dir
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 5d11a2d149ec1d83facd4aea64019bbb963ec70e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840680"
---
# <a name="dir"></a>dir



Affiche une liste de fichiers et les sous-répertoires d’un répertoire. Si utilisée sans paramètres, **dir** affiche le nom de volume et le numéro de série, suivi d’une liste de répertoires et fichiers sur le disque (y compris leurs noms et la date et l’heure de dernière modification chacun) le disque. Pour les fichiers, **dir** affiche l’extension de nom et la taille en octets. **Dir** affiche également le nombre total de fichiers et répertoires répertoriés, leur taille cumulée et l’espace libre (en octets) restant sur le disque.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
dir [<Drive>:][<Path>][<FileName>] [...] [/p] [/q] [/w] [/d] [/a[[:]<Attributes>]][/o[[:]<SortOrder>]] [/t[[:]<TimeField>]] [/s] [/b] [/l] [/n] [/x] [/c] [/4]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|[\<Drive>:][<Path>]|Spécifie le lecteur et le répertoire pour lequel vous souhaitez afficher une liste.|
|[\<FileName>]|Spécifie un fichier particulier ou un groupe de fichiers pour lequel vous souhaitez afficher une liste.|
|/p|Affiche un écran de la liste à la fois. Pour afficher l’écran suivant, appuyez sur n’importe quelle touche du clavier.|
|/q|Affiche des informations de la propriété de fichier.|
|/w|Affiche la liste en format large, avec jusqu'à cinq noms de fichiers ou des noms de répertoire sur chaque ligne.|
|/d|Affiche la liste dans le même format que **/w**, mais les fichiers sont triés par colonne.|
|/ a [[ ::]\<attributs >]|Affiche uniquement les noms des répertoires et fichiers avec les attributs que vous spécifiez. Si vous omettez **/a**, **dir** affiche les noms de tous les fichiers à l’exception masqués et les fichiers système. Si vous utilisez **/a** sans spécifier *attributs*, **dir** affiche les noms de tous les fichiers, y compris les fichiers cachés et système.</br>La liste suivante décrit chacune des valeurs que vous pouvez utiliser pour *attributs*. À l’aide d’un signe deux-points ( :)) est facultatif. Utilisez n’importe quelle combinaison de ces valeurs et ne séparent pas les valeurs avec des espaces.</br>**d** répertoires</br>**h** répertorier les fichiers cachés</br>**s** fichiers système</br>**l** des points d’analyse</br>**r** des fichiers en lecture seule</br>**un** prêt pour l’archivage des fichiers</br>**J’ai** pas les fichiers indexés de contenu</br>**-** Préfixe de ce qui signifie « ne pas »|
|/ o [[ ::]\<SortOrder >]|Trie la sortie en fonction de *SortOrder*, ce qui peut être n’importe quelle combinaison des valeurs suivantes :</br>**n** par nom (par ordre alphabétique)</br>**e** par extension (par ordre alphabétique)</br>**g** groupe tout d’abord les répertoires</br>**s** par taille (plus petit en premier)</br>**d** par date/heure (plus ancienne)</br>**-** Préfixe de l’ordre inverse</br>Remarque: À l’aide d’un signe deux-points est facultative. Plusieurs valeurs sont traités dans l’ordre dans lequel vous les répertorierez. Ne séparez pas plusieurs valeurs avec des espaces.</br>Si *SortOrder* n’est pas spécifié, **/o dir** répertorie les répertoires dans l’ordre alphabétique, suivi par les fichiers qui sont également triés par ordre alphabétique.|
|/t[[:]\<TimeField>]|Spécifie le champ d’heure à afficher ou à utiliser pour le tri. La liste suivante décrit chacune des valeurs que vous pouvez utiliser pour *heure*:</br>**c** la création</br>**un** dernier accès</br>**w** de la dernière écriture|
|/s|Répertorie toutes les occurrences du nom de fichier spécifié dans le répertoire spécifié et tous les sous-répertoires.|
|/b|Affiche une liste de système de répertoires et fichiers, sans aucune information supplémentaire. **/b** substitue **/w**.|
|/l|Affiche les noms de répertoire et en minuscules sans les trier.|
|/n|Affiche un format de la longue liste avec les noms de fichiers à l’extrême droite de l’écran.|
|/x|Affiche les noms courts générés pour les noms de fichier non-8dot3. L’affichage est identique à l’affichage pour **/n**, mais le nom court est inséré avant le nom long.|
|/c|Affiche le séparateur des milliers de taille des fichiers. Il s’agit du comportement par défaut. Utilisez **/c** pour masquer les séparateurs.|
|/4|Affiche les années à quatre chiffres.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Pour utiliser plusieurs *FileName* paramètres, séparez chaque nom de fichier par un espace, une virgule ou un point-virgule.
-   Vous pouvez utiliser des caractères génériques (**&#42;** ou **?**), pour représenter un ou plusieurs caractères d’un nom de fichier et pour afficher un sous-ensemble de fichiers ou sous-répertoires.

    **Astérisque (\*) :** Utiliser l’astérisque comme un remplacement pour n’importe quelle chaîne de caractères, par exemple :  
    -   **dir \*.txt** répertorie tous les fichiers dans le répertoire actif avec des extensions qui commencent avec .txt, par exemple .txt, .txt1, .txt_old.
    -   **dir lire\*.txt** répertorie tous les fichiers qui commencent par « lecture » et avec les extensions qui commencent par .txt, par exemple .txt, .txt1 ou .txt_old dans le répertoire actif.
    -   **dir lire\*.\***  répertorie tous les fichiers dans le répertoire actif qui commencent par « lecture » avec n’importe quelle extension.

    Le caractère générique astérisque utilise toujours mappage de nom de fichier court, vous pouvez obtenir des résultats inattendus. Par exemple, le répertoire suivant contient deux fichiers (t.txt2 et t97.txt) :  
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
    Vous pouvez l’imaginer qu’une pression **dir t97\***  retournerait la t97.txt de fichier. Toutefois, en tapant **dir t97\***  renvoie les deux fichiers, étant donné que le caractère générique astérisque correspond à la t.txt2 de fichier à t97.txt à l’aide de sa carte de nom court T97B4~1.TXT. De même, en tapant **del t97\***  supprime les deux fichiers.

    **Point d’interrogation ( ?) :** Utilisez le point d’interrogation en remplacement d’un seul caractère dans un nom. Par exemple, si vous saisissez **dir lire ???. txt** répertorie tous les fichiers dans le répertoire actif avec l’extension .txt qui commencent par « lecture » et sont suivis par trois caractères. Cela inclut Read.txt, Read1.txt, Read12.txt, Read123.txt et Readme1.txt, mais pas Readme12.txt.
-   Spécification des attributs d’affichage de fichier

    Si vous utilisez **/a** avec plusieurs valeurs dans *attributs*, **dir** affiche les noms des fichiers dotés de tous les attributs spécifiés uniquement. Par exemple, si vous utilisez **/a** avec **r** et **-h** en tant qu’attributs (à l’aide **/ a : r-h** ou **ar** ), **dir** affiche uniquement les noms des fichiers en lecture seule qui ne sont pas masquées.
-   Spécification du tri de nom de fichier

    Si vous spécifiez plusieurs *SortOrder* valeur, **dir** trie les noms de fichiers par le premier critère, puis par le deuxième et ainsi de suite. Par exemple, si vous utilisez **/o** avec la **e** et **-s** valeurs pour *SortOrder* (à l’aide **/o:e-s**ou **OE**), **dir** trie les noms des répertoires et des fichiers par extension, par le plus grand et affiche ensuite le résultat final. Le tri alphabétique par extension entraîne des noms de fichiers sans extensions s’affichent en premier, puis les noms de répertoire et les noms de fichiers avec des extensions.
-   À l’aide de canaux et des symboles de redirection

    Lorsque vous utilisez le symbole de redirection (**>**) pour envoyer **dir** dans un fichier ou un canal de sortie (**|**) pour envoyer **dir**de sortie vers une autre commande, utilisez **/a :-d** et **/b** pour répertorier uniquement les noms de fichiers. Vous pouvez utiliser *FileName* avec **/b** et **/s** pour spécifier que **dir** consiste à rechercher le répertoire actif et ses sous-répertoires pour tous les fichiers noms qui correspondent à *FileName*. **Dir** répertorie une seule recherche nommez-le la lettre de lecteur, répertoire, nom de fichier, nom et extension de fichier (un chemin d’accès par ligne), pour chaque fichier. Avant d’utiliser un canal pour envoyer **dir** la sortie vers une autre commande, vous devez définir le TEMP variable d’environnement dans le fichier Autoexec.nt.
-   Le **dir** commande, avec des paramètres différents, est disponible à partir de la Console de récupération.

## <a name="BKMK_examples"></a>Exemples

Pour afficher tous les répertoires un après l’autre, dans l’ordre alphabétique, en format large et après chaque écran, assurez-vous que le répertoire racine est le répertoire actif et tapez :
```
dir /s/w/o/p
```
**Dir** répertorie le répertoire racine, les sous-répertoires et les fichiers dans le répertoire racine, y compris les extensions. Ensuite, **dir** répertorie le nom et noms de fichiers dans chaque sous-répertoire dans l’arborescence.

Pour modifier l’exemple précédent afin que **dir** affiche les noms de fichiers et les extensions, mais omet les noms de répertoires, type :
```
dir /s/w/o/p/a:-d
```
Pour imprimer une liste de répertoires, tapez :
```
dir > prn
```
Lorsque vous spécifiez **prn**, la liste de répertoires est envoyée à l’imprimante qui est attaché au port LPT1. Si votre imprimante est connectée à un autre port, vous devez remplacer **prn** avec le nom du port.

Vous pouvez également rediriger la sortie de la **dir** commande dans un fichier en remplaçant **prn** avec un nom de fichier. Vous pouvez également taper un chemin d’accès. Par exemple, pour direct **dir** la sortie vers le fichier dir.doc dans le répertoire d’enregistrements, tapez :
```
dir > \records\dir.doc
```
Si dir.doc n’existe pas, **dir** crée, à moins que le répertoire n’existe pas. Dans ce cas, le message suivant apparaît :

`File creation error`

Pour afficher une liste de tous les noms de fichier avec l’extension .txt dans tous les répertoires sur le lecteur C, tapez :
```
dir c:\*.txt /w/o/s/p
```
**Dir** affiche, en format large, une liste classée par ordre alphabétique du fichier correspondant de noms dans chaque répertoire, et il s’interrompt chaque fois que l’écran se remplit jusqu'à ce que vous appuyez sur n’importe quelle touche pour continuer.

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)