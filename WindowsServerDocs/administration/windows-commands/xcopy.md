---
title: xcopy
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 76a310d7-9925-4571-a252-0e28960d5f89
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 01/05/2019
ms.openlocfilehash: 885729f2bca100d7ac89a3463135d56f48c8b75a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361791"
---
# <a name="xcopy"></a>xcopy

Copie les fichiers et les répertoires, y compris les sous-répertoires.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#examples).

## <a name="syntax"></a>Syntaxe

```
Xcopy <Source> [<Destination>] [/w] [/p] [/c] [/v] [/q] [/f] [/l] [/g] [/d [:MM-DD-YYYY]] [/u] [/i] [/s [/e]] [/t] [/k] [/r] [/h] [{/a | /m}] [/n] [/o] [/x] [/exclude:FileName1[+[FileName2]][+[FileName3]] [{/y | /-y}] [/z] [/b] [/j]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<> source|Obligatoire. Spécifie l’emplacement et les noms des fichiers que vous souhaitez copier. Ce paramètre doit inclure un lecteur ou un chemin d’accès.|
|[\<> de destination]|Spécifie la destination des fichiers que vous souhaitez copier. Ce paramètre peut inclure une lettre de lecteur et un signe deux-points, un nom de répertoire, un nom de fichier ou une combinaison de ceux-ci.|
|/w|Affiche le message suivant et attend votre réponse avant de commencer à copier les fichiers :</br>**Appuyez sur n’importe quelle touche pour commencer à copier le ou les fichiers**|
|/p|Vous invite à confirmer si vous souhaitez créer chaque fichier de destination.|
|/c|Ignore les erreurs.|
|/v|Vérifie chaque fichier tel qu’il est écrit dans le fichier de destination pour s’assurer que les fichiers de destination sont identiques aux fichiers sources.|
|/q|Supprime l’affichage des messages **xcopy** .|
|/f|Affiche les noms des fichiers source et de destination lors de la copie.|
|/l|Affiche la liste des fichiers à copier.|
|/g|Crée des fichiers de *destination* déchiffrés lorsque la destination ne prend pas en charge le chiffrement.|
|/d [ : MM-JJ-AAAA]|Copie les fichiers sources modifiés à la date spécifiée uniquement ou après. Si vous n’incluez pas de valeur *mm-jj-aaaa* , **xcopy** copie tous les fichiers *sources* plus récents que les fichiers de *destination* existants. Cette option de ligne de commande vous permet de mettre à jour les fichiers qui ont été modifiés.|
|/u.|Copie les fichiers de la *source* qui existent sur la *destination* uniquement.|
|/i|Si la *source* est un répertoire ou contient des caractères génériques et que la *destination* n’existe pas, **xcopy** suppose que la *destination* spécifie un nom de répertoire et crée un répertoire. **Xcopy** copie ensuite tous les fichiers spécifiés dans le nouveau répertoire. Par défaut, **xcopy** vous invite à spécifier si la *destination* est un fichier ou un répertoire.|
|/s|Copie les répertoires et les sous-répertoires, sauf s’ils sont vides. Si vous omettez **/s**, **xcopy** fonctionne dans un répertoire unique.|
|/e|Copie tous les sous-répertoires, même s’ils sont vides. Utilisez **/e** avec les options de ligne de commande **/s** et **/t** .|
|commutateur|Copie la structure du sous-répertoire (autrement dit, l’arborescence) uniquement, pas les fichiers. Pour copier des répertoires vides, vous devez inclure l’option de ligne de commande **/e** .|
|/k|Copie les fichiers et conserve l’attribut de lecture seule sur les fichiers de *destination* , s’ils sont présents dans les fichiers *sources* . Par défaut, **xcopy** supprime l’attribut de lecture seule.|
|/r|Copie les fichiers en lecture seule.|
|/h|Copie les fichiers avec des attributs de fichiers système et cachés. Par défaut, **xcopy** ne copie pas les fichiers cachés ou système|
|/a|Copie uniquement les fichiers *sources* dont les attributs du fichier d’archive sont définis. **/a** ne modifie pas l’attribut fichier d’archive du fichier source. Pour plus d’informations sur la définition de l’attribut de fichier d’archive à l’aide d' **Attrib**, consultez [Références supplémentaires](#additional-references).|
|/m|Copie les fichiers *sources* dont les attributs du fichier d’archive sont définis. Contrairement à **/a**, **/m** désactive les attributs du fichier d’archive dans les fichiers spécifiés dans la source. Pour plus d’informations sur la définition de l’attribut de fichier d’archive à l’aide d' **Attrib**, consultez [Références supplémentaires](#additional-references).|
|/n|Crée des copies en utilisant les noms de fichiers ou de répertoires courts NTFS. **/n** est requis lorsque vous copiez des fichiers ou des répertoires d’un volume NTFS vers un volume FAT ou lorsque la Convention d’affectation de noms du système de fichiers FAT (c’est-à-dire 8,3 caractères) est requise sur le système de fichiers de *destination* . Le système de fichiers de *destination* peut être FAT ou NTFS.|
|/o|Copie la propriété du fichier et les informations de liste de contrôle d’accès discrétionnaire (DACL).|
|/x|Copie les paramètres d’audit de fichier et les informations de liste de contrôle d’accès système (SACL) (implique **/o**).|
|/Exclude : Nomfichier1 [+ [Nomfichier2] [+ [FileName3] (\)]|Spécifie une liste de fichiers. Au moins un fichier doit être spécifié. Chaque fichier contient des chaînes de recherche avec chaque chaîne sur une ligne distincte dans le fichier.</br>Lorsque l’une des chaînes correspond à une partie du chemin d’accès absolu du fichier à copier, ce fichier est exclu de la copie. Par exemple, la spécification de la chaîne **obj** exclut tous les fichiers sous le répertoire **obj** ou tous les fichiers avec l’extension **. obj** .|
|/y|Supprime l’invite pour confirmer que vous souhaitez remplacer un fichier de destination existant.|
|/-y|Invite à confirmer que vous souhaitez remplacer un fichier de destination existant.|
|z|Copie sur un réseau en mode redémarrable.|
|/b|Copie le lien symbolique à la place des fichiers. Ce paramètre a été introduit dans Windows Vista®.|
|/j|Copie les fichiers sans mise en mémoire tampon. Recommandé pour les fichiers de très grande taille. Ce paramètre a été ajouté dans Windows Server 2008 R2.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

- Utilisation de **/z**

  Si vous perdez votre connexion au cours de la phase de copie (par exemple, si le serveur est connecté à la connexion), il reprend une fois la connexion rétablie. **/z** affiche également le pourcentage de l’opération de copie effectuée pour chaque fichier.

- Utilisation de **/y** dans la variable d’environnement COPYCMD.

  Vous pouvez utiliser **/y** dans la variable d’environnement COPYCMD. Vous pouvez remplacer cette commande à l’aide de **/-y** sur la ligne de commande. Par défaut, vous êtes invité à remplacer.

- Copie de fichiers chiffrés

  La copie de fichiers chiffrés sur un volume qui ne prend pas en charge le système de fichiers EFS génère une erreur. Déchiffrez les fichiers en premier ou copiez les fichiers sur un volume qui prend en charge EFS.

- Ajout de fichiers

  Pour ajouter des fichiers, spécifiez un seul fichier à destination, mais plusieurs fichiers pour la source (autrement dit, en utilisant des caractères génériques ou fichier1 + fichier2 + fichier3 format).

- Valeur par défaut pour la *destination*

  Si vous omettez la *destination*, la commande **xcopy** copie les fichiers dans le répertoire actif.

- Spécification de la *destination* d’un fichier ou d’un répertoire

  Si la *destination* ne contient pas de répertoire existant et ne se termine pas par une barre oblique inverse (\), le message suivant s’affiche :
  
  ```
  Does <Destination> specify a file name or directory name on the target(F = file, D = directory)?
  ```  
  
Appuyez sur F Si vous souhaitez copier le ou les fichiers dans un fichier. Appuyez sur D si vous souhaitez que les fichiers soient copiés dans un répertoire.

  Vous pouvez supprimer ce message à l’aide de l’option de ligne de commande **/i** , qui oblige **xcopy** à supposer que la destination est un répertoire si la source est plus d’un fichier ou d’un répertoire.
- Utilisation de la commande **xcopy** pour définir l’attribut archive des fichiers de *destination*

  La commande **xcopy** crée des fichiers avec l’attribut archive définie, que cet attribut ait été défini ou non dans le fichier source. Pour plus d’informations sur les attributs de fichier et sur **Attrib**, consultez [Références supplémentaires](#additional-references).

- Comparaison de **xcopy** et de **diskcopy**

  Si vous avez un disque qui contient des fichiers dans des sous-répertoires et que vous souhaitez le copier sur un disque ayant un format différent, utilisez la commande **xcopy** au lieu de **diskcopy**. Étant donné que la commande **diskcopy** copie les disques par piste, vos disques source et de destination doivent avoir le même format. La commande **xcopy** n’a pas cette exigence. Utilisez **xcopy** sauf si vous avez besoin d’une copie complète de l’image de disque.

- Codes de sortie pour **xcopy**

  Pour traiter les codes de sortie retournés par **xcopy**, utilisez le paramètre **ERRORLEVEL** sur la ligne de commande **If** dans un programme de traitement par lots. Pour obtenir un exemple de programme de traitement par lots qui traite les codes de sortie à l’aide de **If**, consultez [Références supplémentaires](#additional-references). Le tableau suivant répertorie chaque code de sortie et une description.  

  |Code de sortie|Description|
  |---------|-----------|
  |0|Les fichiers ont été copiés sans erreur.|
  |1|Aucun fichier à copier n’a été trouvé.|
  |2|L’utilisateur a appuyé sur CTRL + C pour terminer **xcopy**.|
  |4|Une erreur d’initialisation s’est produite. Il n’y a pas assez de mémoire ou d’espace disque, ou vous avez entré un nom de lecteur non valide ou une syntaxe non valide sur la ligne de commande.|
  |5|Une erreur d’écriture sur le disque s’est produite.|

## <a name="examples"></a>Exemples

**1.** pour copier tous les fichiers et sous-répertoires (y compris les sous-répertoires vides) du lecteur A vers le lecteur B, tapez :

```
xcopy a: b: /s /e 
```

**2.** pour inclure tous les fichiers système ou masqués dans l’exemple précédent, ajoutez l’option de ligne de commande<strong>/h</strong> comme suit :

```
xcopy a: b: /s /e /h
```

**3.** pour mettre à jour les fichiers du répertoire \Rapports avec les fichiers du répertoire \Rawdata qui ont été modifiés depuis le 29 décembre 1993, tapez :

```
xcopy \rawdata \reports /d:12-29-1993
```

**4.** pour mettre à jour tous les fichiers qui existent dans le fichier \Rapports dans l’exemple précédent, quelle que soit la date, tapez :

```
xcopy \rawdata \reports /u
```

**5.** pour obtenir la liste des fichiers à copier par la commande précédente (autrement dit, sans copier réellement les fichiers), tapez :

```
xcopy \rawdata \reports /d:12-29-1993 /l > xcopy.out
```

Le fichier xcopy. out répertorie tous les fichiers qui doivent être copiés.

**6.** pour copier le répertoire \Customer et tous les sous-répertoires dans le répertoire \\\\Public\Address sur le lecteur réseau h :, conservez l’attribut lecture seule et soyez invité à entrer un nouveau fichier sur h :, tapez :

```
xcopy \customer h:\public\address /s /e /k /p
```

**7.** pour émettre la commande précédente, vérifiez que **xcopy** crée le répertoire \Address s’il n’existe pas et supprimez le message qui s’affiche lorsque vous créez un nouveau répertoire, ajoutez l’option de ligne de commande **/i** comme suit :

```
xcopy \customer h:\public\address /s /e /k /p /i
```

**8.** vous pouvez créer un programme batch pour effectuer des opérations **xcopy** et utiliser la commande Batch **If** pour traiter le code de sortie si une erreur se produit. Par exemple, le programme de traitement par lots suivant utilise des paramètres remplaçables pour les paramètres source et destination de **xcopy** :

```
@echo off
rem COPYIT.BAT transfers all files in all subdirectories of
rem the source drive or directory (%1) to the destination
rem drive or directory (%2)
xcopy %1 %2 /s /e
if errorlevel 4 goto lowmemory
if errorlevel 2 goto abort
if errorlevel 0 goto exit
:lowmemory
echo Insufficient memory to copy files or
echo invalid drive or command-line syntax.
goto exit
:abort
echo You pressed CTRL+C to end the copy operation.
goto exit
:exit 
```

Pour utiliser le programme batch précédent pour copier tous les fichiers dans le répertoire C:\Prgmcode et ses sous-répertoires vers le lecteur B, tapez :

```
copyit c:\prgmcode b:
```

L’interpréteur de commandes substitue **C:\Prgmcode** pour *%1* et **B :** pour *%2*, **utilise xcopy** avec les options de ligne de commande **/e** et **/s** . Si **xcopy** rencontre une erreur, le programme de traitement par lots lit le code de sortie et accède à l’étiquette indiquée dans l’instruction **if errorlevel** appropriée, puis affiche le message approprié et quitte le programme de traitement par lots.

**9.** cet exemple tous les répertoires non vides, ainsi que les fichiers dont le nom correspond au modèle donné avec le symbole de l’astérisque.

```
xcopy .\toc*.yml ..\..\Copy-To\ /S /Y

rem Output example.
rem  .\d1\toc.yml
rem  .\d1\d12\toc.yml
rem  .\d2\toc.yml
rem  3 File(s) copied
```

Dans l’exemple précédent, cette valeur de paramètre source particulière **.\\toc\*. yml** copiez les mêmes 3 fichiers, même si ses deux caractères de chemin d’accès **.\\** ont été supprimés. Toutefois, aucun fichier n’est copié si le caractère générique astérisque a été supprimé du paramètre source, ce qui le rend juste **.\\toc. yml**.

#### <a name="additional-references"></a>Références supplémentaires

-   [Copier](copy.md)
-   [Déplacer](move.md)
-   [Public](dir.md)
-   [Attrib](attrib.md)
-   [Convient](diskcopy.md)
-   [Que](if.md)
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
