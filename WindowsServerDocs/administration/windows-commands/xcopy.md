---
title: xcopy
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 6448c4c5940d286931f6d64ad51970bf577a28fd
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811026"
---
# <a name="xcopy"></a>xcopy

Copie les fichiers et répertoires, y compris les sous-répertoires.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#examples).

## <a name="syntax"></a>Syntaxe

```
Xcopy <Source> [<Destination>] [/w] [/p] [/c] [/v] [/q] [/f] [/l] [/g] [/d [:MM-DD-YYYY]] [/u] [/i] [/s [/e]] [/t] [/k] [/r] [/h] [{/a | /m}] [/n] [/o] [/x] [/exclude:FileName1[+[FileName2]][+[FileName3]] [{/y | /-y}] [/z] [/b] [/j]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<Source>|Obligatoire. Spécifie l’emplacement et les noms des fichiers à copier. Ce paramètre doit inclure un lecteur ou un chemin d’accès.|
|[\<Destination>]|Spécifie la destination des fichiers à copier. Ce paramètre peut inclure une lettre de lecteur et deux-points, un nom de répertoire, un nom de fichier ou une combinaison de ces éléments.|
|/w|Affiche le message suivant et attend une réponse avant de commencer à copier les fichiers :</br>**Appuyez sur n’importe quelle touche pour commencer la copie des fichiers.**|
|/p|Vous invite à confirmer si vous souhaitez créer chaque fichier de destination.|
|/c|Ignore les erreurs.|
|/v|Vérifie chaque fichier tel qu’il est écrit dans le fichier de destination pour vous assurer que les fichiers de destination sont identiques aux fichiers sources.|
|/q|Supprime l’affichage de **xcopy** messages.|
|/f|Affiche les noms de fichier source et de destination lors de la copie.|
|/l|Affiche une liste des fichiers qui doivent être copiés.|
|/g|Crée déchiffrée *Destination* lorsque la destination ne prend pas en charge le chiffrement des fichiers.|
|/d [ : MM-jj-aaaa]|Copie les fichiers modifiés sur ou après la date spécifiée uniquement de sources. Si vous n’incluez pas une *MM-jj-aaaa* valeur, **xcopy** copie tous les *Source* les fichiers qui sont plus récents qu’existant *Destination* fichiers. Cette option de ligne de commande permet de mettre à jour les fichiers qui ont été modifiés.|
|/u|Copie les fichiers à partir de *Source* qui existent sur *Destination* uniquement.|
|/i|Si *Source* est un répertoire ou contient des caractères génériques et *Destination* n’existe pas, **xcopy** suppose *Destination* spécifie un nom de répertoire et crée un nouveau répertoire. Ensuite, **xcopy** copie tous les fichiers spécifiés dans le nouveau répertoire. Par défaut, **xcopy** vous invite à spécifier si *Destination* est un fichier ou un répertoire.|
|/s|Copie les répertoires et sous-répertoires, sauf si elles sont vides. Si vous omettez **/s**, **xcopy** fonctionne au sein d’un seul répertoire.|
|/e|Copie tous les sous-répertoires, même si elles sont vides. Utilisez **/e** avec la **/s** et **/t** les options de ligne de commande.|
|/t|Copie la structure de sous-répertoire (autrement dit, l’arborescence) uniquement, pas les fichiers. Pour copier des répertoires vides, vous devez inclure le **/e** option de ligne de commande.|
|/k|Copie les fichiers et conserve l’attribut en lecture seule sur *Destination* fichiers s’il est présent sur le *Source* fichiers. Par défaut, **xcopy** supprime l’attribut en lecture seule.|
|/r|Copie les fichiers en lecture seule.|
|/h|Copie les fichiers cachés et les attributs de fichier système. Par défaut, **xcopy** ne pas copie masquée ou fichiers système|
|/a|Copie uniquement *Source* ensemble d’attributs de fichiers de fichiers qui ont leur archivage. **/a** ne modifie pas l’attribut de fichier d’archive du fichier source. Pour plus d’informations sur la définition de l’attribut de fichier d’archive à l’aide de **attrib**, consultez [références supplémentaires](#additional-references).|
|/m|Copies *Source* ensemble d’attributs de fichiers de fichiers qui ont leur archivage. Contrairement aux **/a**, **/m** désactive l’attribut archive dans les fichiers qui sont spécifiés dans la source. Pour plus d’informations sur la définition de l’attribut de fichier d’archive à l’aide de **attrib**, consultez [références supplémentaires](#additional-references).|
|/n|Crée des copies en utilisant le fichier court NTFS ou les noms de répertoires. **/n** est requis lorsque vous copiez des fichiers ou répertoires à partir d’un volume NTFS vers un volume FAT ou lors de la matière grasse fichier système d’affectation de noms 8.3 (autrement dit,) est requis sur le *Destination* système de fichiers. Le *Destination* système de fichiers peut être FAT ou NTFS.|
|/o|Copie les fichiers la propriété et accès discrétionnaire (DACL) informations.|
|/x|Copie les fichiers des paramètres d’audit et informations de liste (SACL) de contrôle d’accès système (implique **/o**).|
|/exclude:FileName1[+[FileName2][+[FileName3]( \)]|Spécifie une liste de fichiers. Au moins un fichier doit être spécifié. Chaque fichier contient des chaînes de recherche avec chaque chaîne sur une ligne distincte dans le fichier.</br>Lors d’une des chaînes correspond à n’importe quelle partie du chemin d’accès absolu du fichier à copier, ce fichier ne sont copiées. Par exemple, en spécifiant la chaîne **obj** exclut tous les fichiers sous le répertoire **obj** ou tous les fichiers portant le **.obj** extension.|
|/y|Supprime l’invite pour confirmer que vous souhaitez remplacer un fichier de destination existant.|
|/-y|Vous invite à confirmer que vous souhaitez remplacer un fichier de destination existant.|
|/z|Copie sur un réseau en mode de redémarrage.|
|/b|Copie le lien symbolique au lieu des fichiers. Ce paramètre a été introduit dans Windows Vista®.|
|/j|Copie les fichiers sans mise en mémoire tampon. Recommandé pour les fichiers très volumineux. Ce paramètre a été ajouté dans Windows Server 2008 R2.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

- À l’aide de **/z**

  Si vous perdez la connexion pendant la phase de copie (par exemple, si le serveur de mise hors connexion interrompt la connexion), il reprend une fois que vous rétablissez la connexion. **/z** affiche également le pourcentage de l’opération de copie pour chaque fichier.

- À l’aide de **/y** dans la variable d’environnement COPYCMD.

  Vous pouvez utiliser **/y** dans la variable d’environnement COPYCMD. Vous pouvez remplacer cette commande à l’aide de **/-y** sur la ligne de commande. Par défaut, vous êtes invité à remplacer.

- Copie de fichiers cryptés

  Copie de fichiers cryptés vers un volume qui ne prend pas en charge les résultats d’EFS dans une erreur. Tout d’abord de déchiffrer les fichiers ou copiez les fichiers sur un volume qui ne prend pas en charge le système EFS.

- Ajout de fichiers

  Pour ajouter des fichiers, spécifiez un seul fichier de destination, mais plusieurs fichiers sources (autrement dit, en utilisant des caractères génériques ou fichier1 + fichier2 + fichier3).

- Valeur par défaut pour *Destination*

  Si vous omettez *Destination*, le **xcopy** commande copie les fichiers dans le répertoire actif.

- Spécifiant si *Destination* est un fichier ou répertoire

  Si *Destination* ne contient pas un répertoire existant et ne se termine pas par une barre oblique inverse (\), le message suivant apparaît :
  
  ```
  Does <Destination> specify a file name or directory name on the target(F = file, D = directory)?
  ```  
  
Si vous souhaitez que l’ou les fichiers à copier dans un fichier, appuyez sur F. Appuyez sur D si vous souhaitez que l’ou les fichiers à copier dans un répertoire.

  Vous pouvez supprimer ce message à l’aide de la **/i** option de ligne de commande, ce qui entraîne **xcopy** de supposer que la destination est un répertoire, si la source est plus d’un fichier ou un répertoire.
- À l’aide de la **xcopy** commande pour définir l’attribut d’archivage pour *Destination* fichiers

  Le **xcopy** commande crée des fichiers avec l’attribut archive, si cet attribut n’a pas dans le fichier source. Pour plus d’informations sur les attributs de fichier et **attrib**, consultez [références supplémentaires](#additional-references).

- Comparaison **xcopy** et **diskcopy**

  Si vous avez un disque qui contient les fichiers dans les sous-répertoires et que vous souhaitez copier sur un disque qui a un format différent, utilisez la **xcopy** commande au lieu de **diskcopy**. Étant donné que le **diskcopy** commande copie les disques piste par piste, vos disques source et de destination doivent avoir le même format. Le **xcopy** commande n’a pas cette exigence. Utilisez **xcopy** , sauf si vous avez besoin d’une copie d’image de disque complet.

- Codes de sortie pour **xcopy**

  Pour traiter les codes de sortie retournés par **xcopy**, utilisez le **ErrorLevel** paramètre sur le **si** ligne de commande dans un programme de traitement par lots. Pour obtenir un exemple d’un programme de traitement par lots qui traite de sortie à l’aide de codes **si**, consultez [références supplémentaires](#additional-references). Le tableau suivant répertorie chaque code de sortie et une description.  

  |Code de sortie|Description|
  |---------|-----------|
  |0|Fichiers ont été copiés sans erreur.|
  |1|Il n’existe aucun fichier à copier.|
  |2|L’utilisateur a appuyé sur CTRL + C pour arrêter **xcopy**.|
  |4|Erreur d’initialisation s’est produite. Il n’est pas suffisamment de mémoire ou l’espace disque, ou que vous avez entré un nom de lecteur ou une syntaxe non valide sur la ligne de commande.|
  |5|Erreur d’écriture disque s’est produite.|

## <a name="examples"></a>Exemples

**1.** Pour copier tous les fichiers et sous-répertoires (y compris tous les sous-répertoires vides) à partir du lecteur A lecteur B, tapez :

```
xcopy a: b: /s /e 
```

**2.** Pour inclure n’importe quel système ou les fichiers masqués dans l’exemple précédent, ajoutez le<strong>/h</strong> une option de ligne de commande comme suit :

```
xcopy a: b: /s /e /h
```

**3.** Pour mettre à jour des fichiers dans le répertoire \Reports avec les fichiers dans le répertoire \Rawdata qui ont été modifiés depuis le 29 décembre 1993, tapez :

```
xcopy \rawdata \reports /d:12-29-1993
```

**4.** Pour mettre à jour tous les fichiers qui existent dans \Reports dans l’exemple précédent, quelle que soit la date, tapez :

```
xcopy \rawdata \reports /u
```

**5.** Pour obtenir une liste des fichiers doit être copié par la commande précédente (autrement dit, sans copier réellement les fichiers), type :

```
xcopy \rawdata \reports /d:12-29-1993 /l > xcopy.out
```

Le fichier xcopy.out répertorie chaque fichier doit être copié.

**6.** Pour copier le répertoire \Customer et tous les sous-répertoires dans le répertoire \\ \\Public\Address sur réseau lecteur H:, conserver l’attribut en lecture seule et être invité lorsqu’un nouveau fichier est créé sur le lecteur H:, tapez :

```
xcopy \customer h:\public\address /s /e /k /p
```

**7.** Pour émettre la commande précédente, vérifiez que **xcopy** crée le répertoire de \Address s’il n’existe pas et supprimer le message qui s’affiche lorsque vous créez un nouveau répertoire, ajoutez le **/i** de ligne de commande option comme suit :

```
xcopy \customer h:\public\address /s /e /k /p /i
```

**8.** Vous pouvez créer un programme de traitement par lots pour effectuer **xcopy** opérations et l’utilisation du lot **si** commande pour traiter le code de sortie si une erreur se produit. Par exemple, le programme de commandes suivant utilise des paramètres remplaçables pour la **xcopy** paramètres source et destination :

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

Pour utiliser le programme de traitement par lots précédent pour copier tous les fichiers dans le répertoire C:\Prgmcode et ses sous-répertoires lecteur B, tapez :

```
copyit c:\prgmcode b:
```

Substituts d’interpréteur de commande **C:\Prgmcode** pour *%1* et **b :** pour *%2*, puis utilise **xcopy**avec la **/e** et **/s** des options de ligne de commande. Si **xcopy** rencontre une erreur, le programme lit le code de sortie et passe à l’étiquette indiquée dans approprié **IF ERRORLEVEL** instruction, puis affiche le message approprié et quitte le programme de traitement par lots.

**9.** Cet exemple montre comment tous le non vide répertoires, ainsi que les fichiers dont le nom correspondent au modèle donné avec le symbole astérisque.

```
xcopy .\toc*.yml ..\..\Copy-To\ /S /Y

rem Output example.
rem  .\d1\toc.yml
rem  .\d1\d12\toc.yml
rem  .\d2\toc.yml
rem  3 File(s) copied
```

Dans l’exemple précédent, cette valeur de paramètre source particulier **.\\ table des matières\*.yml** copier le 3 fichiers même si son chemin de deux accès caractères **.\\**  ont été supprimés. Toutefois, aucun fichier ne sont copiés si le caractère générique astérisque a peut-être été retiré le paramètre source, rendant simplement **.\\ TOC.yml**.

#### <a name="additional-references"></a>Références supplémentaires

-   [Copier](copy.md)
-   [Déplacer](move.md)
-   [Dir](dir.md)
-   [Attrib](attrib.md)
-   [Diskcopy](diskcopy.md)
-   [If](if.md)
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
