---
title: dir
description: Rubrique de référence pour la commande dir, qui affiche la liste des fichiers et sous-répertoires d’un répertoire.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: edcbf69b-eaa4-466e-b210-3dd8892f4d93
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 067b4961aecc6149d6ff68872123acb2cfe99305
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2020
ms.locfileid: "82992539"
---
# <a name="dir"></a>dir

Affiche la liste des fichiers et sous-répertoires d’un répertoire. En cas d’utilisation sans paramètre, cette commande affiche le nom de volume et le numéro de série du disque, suivis d’une liste de répertoires et de fichiers sur le disque (y compris leur nom et la date et l’heure de leur dernière modification). Pour les fichiers, cette commande affiche l’extension de nom et la taille en octets. Cette commande affiche également le nombre total de fichiers et de répertoires figurant dans la liste, leur taille cumulée et l’espace libre (en octets) restant sur le disque.

La commande **dir** peut également être exécutée à partir de la console de récupération Windows, à l’aide de différents paramètres. Pour plus d’informations, consultez [environnement de récupération Windows (WinRE)](https://docs.microsoft.com/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference).

## <a name="syntax"></a>Syntaxe

```
dir [<drive>:][<path>][<filename>] [...] [/p] [/q] [/w] [/d] [/a[[:]<attributes>]][/o[[:]<sortorder>]] [/t[[:]<timefield>]] [/s] [/b] [/l] [/n] [/x] [/c] [/4]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| `[<drive>:][<path>]` | Spécifie le lecteur et le répertoire dont vous souhaitez afficher la liste. |
| `[<filename>]` | Spécifie un fichier ou un groupe de fichiers particulier pour lequel vous souhaitez afficher une liste. |
| /p | Affiche un écran de la liste à la fois. Pour afficher l’écran suivant, appuyez sur une touche. |
| /q | Affiche les informations de propriété du fichier. |
| /w | Affiche la liste dans un format étendu, avec un maximum de cinq noms de fichiers ou de répertoires sur chaque ligne. |
| /d | Affiche la liste dans le même format que **/w**, mais les fichiers sont triés par colonne. |
| /a [[ :]`<attributes>`] | Affiche uniquement les noms de ces répertoires et fichiers avec les attributs spécifiés. Si vous n’utilisez pas ce paramètre, la commande affiche les noms de tous les fichiers, à l’exception des fichiers système et cachés. Si vous utilisez ce paramètre sans spécifier d' *attributs*, la commande affiche les noms de tous les fichiers, y compris les fichiers cachés et système. La liste des valeurs d' *attributs* possibles est :<ul><li>**d** -répertoires</li><li>fichiers masqués **h**</li><li>**s** -fichiers système</li><li>**l** -points d’analyse</li><li>**r** -fichiers en lecture seule</li><li>**a** -fichiers prêts pour l’archivage</li><li>**i** -non-fichiers indexés de contenu</li></ul>Vous pouvez utiliser n’importe quelle combinaison de ces valeurs, mais ne séparez pas vos valeurs à l’aide d’espaces. Vous pouvez éventuellement utiliser un signe deux-points ( :) ou vous pouvez utiliser un trait d’Union (-) comme préfixe pour signifier « not ». Par exemple, l’utilisation de l’attribut **-s** n’affiche pas les fichiers système. |
| /o [[ :]`<sortorder>`] | Trie la sortie en fonction de *SortOrder*, qui peut être n’importe quelle combinaison des valeurs suivantes :<ul><li>**n** -alphabétiquement par nom</li><li>**e** -par ordre alphabétique par extension</li><li>**g** -grouper les annuaires en premier</li><li>**s** -par taille, le plus petit en premier</li><li>**d** -par date/heure, en commençant par le plus ancien</li><li>Utiliser le **-** préfixe pour inverser l’ordre de tri</li></ul>Les valeurs multiples sont traitées dans l’ordre dans lequel vous les répertoriez. Ne séparez pas plusieurs valeurs par des espaces, mais vous pouvez éventuellement utiliser un signe deux-points ( :).<p>Si *SortOrder* n’est pas spécifié, **dir/o** répertorie les répertoires par ordre alphabétique, suivis des fichiers, qui sont également triés par ordre alphabétique. |
| /t [[ :]`<timefield>`] | Spécifie le champ d’heure à afficher ou à utiliser pour le tri. Les valeurs *TimeField* disponibles sont les suivantes :<ul><li>**c** -création</li><li>**a** -dernier accès</li><li>a **-dernier** écrit</li></ul> |
| /s | Répertorie toutes les occurrences du nom de fichier spécifié dans le répertoire spécifié et dans tous les sous-répertoires. |
| /b | Affiche une liste complète des répertoires et des fichiers, sans informations supplémentaires. Le paramètre **/b** remplace **/w**. |
| /l | Affiche les noms de fichiers et de répertoires non triés, en utilisant des minuscules. |
| /n | Affiche un format de liste longue avec les noms de fichiers à l’extrême droite de l’écran. |
| /x | Affiche les noms courts générés pour les noms de fichiers non-8dot3. L’affichage est le même que l’affichage pour **/n**, mais le nom abrégé est inséré avant le nom long. |
| /C | Affiche le séparateur de milliers dans la taille des fichiers. Il s'agit du comportement par défaut. Utilisez **/c** pour masquer les séparateurs. |
| /4 | Affiche les années au format à quatre chiffres. |
| /? | Affiche l'aide à l'invite de commandes. |

#### <a name="remarks"></a>Notes 

- Pour utiliser plusieurs paramètres de *nom* de fichier, séparez chaque nom de fichier par un espace, une virgule ou un point-virgule.

- Vous pouvez utiliser des caractères génériques (**&#42;** ou **?**) pour représenter un ou plusieurs caractères d’un nom de fichier et pour afficher un sous-ensemble de fichiers ou de sous-répertoires.

- Vous pouvez utiliser le caractère générique, **&#42;**, pour remplacer toute chaîne de caractères, par exemple :

  - `dir *.txt`répertorie tous les fichiers du répertoire actif avec les extensions qui commencent par. txt, par exemple. txt,. txt1,. txt_old.

  - `dir read *.txt`répertorie tous les fichiers du répertoire actif qui commencent par Read et les extensions qui commencent par. txt, telles que. txt,. txt1 ou. txt_old.

  - `dir read *.*`répertorie tous les fichiers du répertoire actif qui commencent par lecture avec n’importe quelle extension.

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

  Vous pouvez vous attendre à `dir t97\*` ce que la saisie retourne le fichier T97. txt. Toutefois, la `dir t97\*` saisie retourne les deux fichiers, car le caractère générique astérisque correspond au fichier t. txt2 en T97. txt en utilisant son mappage de noms courts *T97B4 ~ 1. txt*. De même, la `del t97\*` saisie supprime les deux fichiers.

- Vous pouvez utiliser le point d’interrogation ( ?) comme substitut d’un caractère unique dans un nom. Par exemple, l' `dir read???.txt` entrée répertorie tous les fichiers du répertoire actif avec l’extension. txt qui commence par Read et qui sont suivis d’un maximum de trois caractères. Cela comprend Read. txt, Read1. txt, Read12. txt, Read123. txt et Readme1. txt, mais pas Readme12. txt.

- Si vous utilisez **/a** avec plusieurs valeurs dans les *attributs*, cette commande affiche uniquement les noms des fichiers avec tous les attributs spécifiés. Par exemple, si vous utilisez **/a** avec **r** et **-h** comme attributs (à l’aide `/a:r-h` de `/ar-h`ou de), cette commande affiche uniquement les noms des fichiers en lecture seule qui ne sont pas masqués.

- Si vous spécifiez plusieurs valeurs *SortOrder* , cette commande trie les noms de fichiers selon le premier critère, puis le deuxième critère, et ainsi de suite. Par exemple, si vous utilisez **/o** avec les paramètres **e** et **-s** pour *SortOrder* (à l’aide `/o:e-s` de `/oe-s`ou de), cette commande trie les noms des répertoires et des fichiers par extension, avec la plus grande priorité, puis affiche le résultat final. Le tri par ordre alphabétique par extension entraîne l’affichage en premier des noms de fichiers sans extensions, puis des noms de répertoire, puis des noms de fichiers avec des extensions.

- Si vous utilisez le symbole de redirection (`>`) pour envoyer la sortie de cette commande vers un fichier, ou si vous utilisez un canal`|`() pour envoyer la sortie de cette commande à une autre commande, `/a:-d` vous devez utiliser et **/b** pour répertorier uniquement les noms de fichiers. Vous pouvez utiliser *filename* avec **/b** et **/s** pour spécifier que cette commande consiste à rechercher dans le répertoire actif et ses sous-répertoires tous les noms de fichiers qui correspondent au *nom*de fichier. Cette commande répertorie uniquement la lettre de lecteur, le nom de répertoire, le nom de fichier et l’extension de nom de fichier (un chemin par ligne), pour chaque nom de fichier trouvé. Avant d’utiliser un canal pour envoyer la sortie de cette commande vers une autre commande, vous devez définir la variable d’environnement *temp* dans votre fichier Autoexec. NT.

## <a name="examples"></a>Exemples

Pour afficher tous les répertoires l’un après l’autre, dans l’ordre alphabétique, dans un format étendu et en suspendant après chaque écran, assurez-vous que le répertoire racine est le répertoire actif, puis tapez :

```
dir /s/w/o/p
```

La sortie répertorie le répertoire racine, les sous-répertoires et les fichiers dans le répertoire racine, y compris les extensions. Cette commande répertorie également les noms de sous-répertoires et les noms de fichiers dans chaque sous-répertoire de l’arborescence.

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

Si dir. doc n’existe pas, **dir** le crée, sauf si le répertoire **Records** n’existe pas. Dans ce cas, le message suivant s’affiche :

```
File creation error
```

Pour afficher la liste de tous les noms de fichiers avec l’extension. txt dans tous les répertoires sur le lecteur C, tapez :

```
dir c:\*.txt /w/o/s/p
```

La commande **dir** affiche, dans un format étendu, une liste alphabétique des noms de fichiers correspondants dans chaque répertoire, et s’interrompt chaque fois que l’écran se remplit jusqu’à ce que vous appuyiez sur une touche pour continuer.

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
