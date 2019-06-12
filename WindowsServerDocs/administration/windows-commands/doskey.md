---
title: doskey
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4874fd43-d5ea-45f3-ae24-388ae925ed76
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3e7a2fd2aa6a608a8857b325d3d6b0608eb4e59e
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439501"
---
# <a name="doskey"></a>doskey



Appelle Doskey.exe (qui rappelle les commandes de ligne de commande entrées précédemment), les modifications des lignes de commande et crée des macros.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
doskey [/reinstall] [/listsize=<Size>] [/macros:[all | <ExeName>] [/history] [/insert | /overstrike] [/exename=<ExeName>] [/macrofile=<FileName>] [<MacroName>=[<Text>]]
```

## <a name="parameters"></a>Paramètres

|       Paramètre        |                                                                                                                          Description                                                                                                                           |
|------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       / réinstaller       |                                                                                            Installe une nouvelle copie de Doskey.exe et efface le tampon de l’historique de commande.                                                                                            |
|   / LISTSIZE =\<taille >    |                                                                                                Spécifie le nombre maximal de commandes dans la mémoire tampon de l’historique.                                                                                                 |
|        /macros         |                                        Affiche une liste de tous les **doskey** macros. Vous pouvez utiliser le symbole de redirection ( **>** ) avec **/macros** pour rediriger la liste vers un fichier. Vous pouvez abréger **/macros** à **/m**.                                         |
|      nom_d       |                                                                                                        Affiche **doskey** macros pour tous les exécutables.                                                                                                         |
|   /macros :\<nom-exe >   |                                                                                             Affiche **doskey** macros pour le fichier exécutable spécifié par *nom-exe*.                                                                                              |
|        /History        |                                    Affiche toutes les commandes qui sont stockés en mémoire. Vous pouvez utiliser le symbole de redirection ( **>** ) avec **/history** pour rediriger la liste vers un fichier. Vous pouvez abréger **/history** comme **/h**.                                    |
|        [/insert        |                                                                                                                          /overstrike]                                                                                                                          |
|  /exename=\<ExeName>   |                                                                                        Spécifie le programme (autrement dit, exécutable) dans lequel le **doskey** macro s’exécute.                                                                                         |
| /macrofile=\<FileName> |                                                                                              Spécifie un fichier qui contient les macros que vous souhaitez installer.                                                                                               |
| \<MacroName>=[<Text>]  | Crée une macro qui exécute les commandes spécifiées par *texte*. *Nom_macro* Spécifie le nom que vous souhaitez affecter à la macro. *Texte* spécifie les commandes que vous souhaitez enregistrer. Si *texte* est laissé vide, *Nom_macro* est exempte de toutes les commandes affectées. |
|           /?           |                                                                                                              Affiche l'aide à l'invite de commandes.                                                                                                              |

## <a name="remarks"></a>Notes

- À l’aide de Doskey.exe

  Doskey.exe est toujours disponible pour les programmes de tous les caractères et interactifs (par exemple, les débogueurs de programme ou des programmes de transfert de fichier), et il maintient un tampon de l’historique des commandes et des macros pour chaque programme qu’il démarre. Vous ne pouvez pas utiliser **doskey** des options de ligne de commande à partir d’un programme. Vous devez exécuter **doskey** des options de ligne de commande avant de démarrer un programme. Programme annulent **doskey** affectations de clé.
- Rappel d’une commande

  Pour rappeler une commande, vous pouvez utiliser toutes les clés suivantes après avoir démarré Doskey.exe. Si vous utilisez Doskey.exe au sein d’un programme, les affectations de touches de ce programme sont prioritaires.  

  |    Touche     |                              Description                              |
  |------------|-----------------------------------------------------------------------|
  |  Flèche haut  |  Rappelle la commande que vous avez utilisé avant celle qui s’affiche.  |
  | Bas |  Rappelle la commande que vous avez utilisé après celle qui s’affiche.   |
  |  Pg. préc   |    Rappelle la première commande que vous avez utilisé dans la session active.    |
  | Pg. suiv  | Rappelle la dernière commande que vous avez utilisé dans la session active. |


- Modification de la ligne de commande

  Avec Doskey.exe, vous pouvez modifier la ligne de commande actuelle. Si vous utilisez Doskey.exe au sein d’un programme, les affectations de touches de ce programme sont prioritaires et certaines touches de modification Doskey.exe peut ne pas fonctionner.

  Le tableau suivant répertorie **doskey** touches de modification et leurs fonctions.  

  | Touche ou combinaison de touches |                                                                                                                                                       Description                                                                                                                                                       |
  |------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
  |       Gauche       |                                                                                                                                      Déplace le précédent caractère du point d’insertion.                                                                                                                                      |
  |      Flèche droite       |                                                                                                                                    Déplace le point d’insertion d’un caractère.                                                                                                                                     |
  |    CTRL + FLÈCHE GAUCHE     |                                                                                                                                        Déplace le précédent mot du point d’insertion.                                                                                                                                         |
  |    CTRL + FLÈCHE DROITE    |                                                                                                                                       Déplace le point d’insertion d’un mot.                                                                                                                                       |
  |          Origine          |                                                                                                                                 Déplace le point d’insertion au début de la ligne.                                                                                                                                 |
  |          END           |                                                                                                                                    Déplace le point d’insertion à la fin de la ligne.                                                                                                                                    |
  |          Échap           |                                                                                                                                          Efface la commande à partir de l’affichage.                                                                                                                                           |
  |           F1           |                                                                      Copie un seul caractère à partir d’une colonne dans le modèle à la même colonne dans la fenêtre d’invite de commandes. (Le modèle est une mémoire tampon qui contient la dernière commande que vous avez tapé.)                                                                       |
  |           F2           |                                                                 Recherche vers l’avant dans le modèle pour la clé suivante que vous tapez après que vous appuyez sur F2. Doskey.exe insère le texte à partir du modèle, inscrire, mais sans inclure, le caractère que vous spécifiez.                                                                  |
  |           F3           |                                                 Copie le reste du modèle dans la ligne de commande. Doskey.exe commence à copier les caractères à partir de la position dans le modèle qui correspond à la position indiquée par le point d’insertion sur la ligne de commande.                                                 |
  |           F4           |                                                                            Supprime que tous les caractères à partir de l’insertion actuelle point position jusqu'à mais sans l’inclure, l’occurrence suivante du caractère que vous tapez après vous appuyez sur F4.                                                                            |
  |           F5           |                                                                                                                                   Copie le modèle dans la ligne de commande actuelle.                                                                                                                                    |
  |           F6           |                                                                                                                    Place un caractère de fin de fichier (CTRL + Z) à la position du point d’insertion actuel.                                                                                                                    |
  |           F7           | Affiche toutes les commandes de ce programme sont stockées en mémoire (dans une boîte de dialogue). Utiliser la touche de direction haut et la touche flèche bas pour sélectionner la commande souhaitée, puis appuyez sur ENTRÉE pour exécuter la commande. Vous pouvez également noter le numéro séquentiel devant la commande et utiliser ce numéro avec la touche F9. |
  |         ALT+F7         |                                                                                                                          Supprime toutes les commandes stockées en mémoire pour la mémoire tampon de l’historique en cours.                                                                                                                          |
  |           F8           |                                                                                                           Affiche toutes les commandes dans la mémoire tampon de l’historique qui commencent par les caractères dans la commande actuelle.                                                                                                            |
  |           F9           |                                             Vous invitent à entrer un numéro de commande de mémoire tampon de l’historique et affiche ensuite la commande associée au numéro que vous spécifiez. Appuyez sur ENTRÉE pour exécuter la commande. Pour afficher tous les nombres et leurs commandes associées, appuyez sur F7.                                             |
  |        ALT + F10         |                                                                                                                                             Supprime toutes les définitions de macros.                                                                                                                                              |


- À l’aide de **doskey** au sein d’un programme

  Certains programmes interactifs, en fonction du caractère, telles que les débogueurs de programme ou les programmes de transfert de fichiers (FTP) utilisent automatiquement Doskey.exe. Pour utiliser Doskey.exe, un programme doit être un processus de console et utiliser la mise en mémoire tampon d’entrée. Programme annulent **doskey** affectations de clé. Par exemple, si le programme utilise la touche F7 pour une fonction, vous ne pouvez pas obtenir un **doskey** l’historique dans une fenêtre indépendante.

  Avec Doskey.exe, vous pouvez conserver un historique de commande pour chaque programme que vous démarrerez ou répétez. Vous pouvez modifier les commandes précédentes à l’invite du programme et démarrer **doskey** macros créées pour le programme. Si vous quittez puis redémarrez un programme à partir de la même fenêtre d’invite de commandes, l’historique des commandes de la session de programme précédente sont disponible.

  Vous devez exécuter Doskey.exe avant de démarrer un programme. Vous ne pouvez pas utiliser **doskey** des options de ligne de commande à partir d’une commande de programme invitent, même si le programme a une invite de commandes.

  Si vous souhaitez personnaliser comment Doskey.exe fonctionne avec un programme et créer **doskey** macros pour ce programme, vous pouvez créer un programme de traitement par lots qui modifie Doskey.exe et démarre le programme.
- En spécifiant un mode d’insertion par défaut

  Si vous appuyez sur la touche Insertion, vous pouvez taper le texte sur le **doskey** ligne de commande au milieu du texte existant sans remplacer le texte. Toutefois, une fois que vous appuyez sur entrée, Doskey.exe retourne votre clavier pour le mode de remplacement. Vous devez appuyer sur Insérer à nouveau pour revenir au mode insertion.

  Utilisez **/insertion** pour passer de votre clavier en mode insertion chaque fois que vous appuyez sur ENTRÉE. Le clavier demeure en mode insertion jusqu'à ce que vous utilisez **/overstrike**. Vous pouvez temporairement retourne au mode de remplacement en appuyant sur la touche INSER, mais une fois que vous appuyez sur entrée, Doskey.exe votre clavier en mode insertion.

  L’insertion point change de forme lorsque vous utilisez la touche Insertion pour passer d’un mode à l’autre.
- Création d’une macro

  Vous pouvez utiliser Doskey.exe pour créer des macros qui effectuent une ou plusieurs commandes. Le tableau suivant répertorie les caractères spéciaux que vous pouvez utiliser pour contrôler les opérations de commande lorsque vous définissez une macro.  

  |   Caractère   |                                                                                                                                                                               Description                                                                                                                                                                               |
  |---------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
  |   $G ou $g    |                                                                                   Redirige la sortie. Utiliser une de ces caractères spéciaux pour envoyer la sortie vers un fichier ou un périphérique à la place de l’écran. Ce caractère est équivalent au symbole de redirection pour la sortie ( **>** ).                                                                                    |
  | $G$ G ou $g$ g  |                                                         Ajoute la sortie à la fin d’un fichier. Pour ajouter la sortie vers un fichier existant au lieu de remplacer les données dans le fichier, utilisez un de ces caractères doubles. Ces caractères double sont équivalents au symbole de redirection d’ajout pour la sortie ( **>>** ).                                                         |
  |   $L ou $l    |                                                                                  Effectue une redirection d’entrée. Utiliser une de ces caractères spéciaux pour lire d’entrée à partir d’un fichier ou un périphérique à la place d’à partir du clavier. Ce caractère est équivalent au symbole de redirection pour l’entrée ( **<** ).                                                                                  |
  |   $B ou $b    |                                                                                                                                    Envoie la sortie de la macro à une commande. Ces caractères spéciaux sont équivalentes à l’aide de la barre verticale)\*\*                                                                                                                                     |
  |   $T ou $t    |                                                            Sépare les commandes. Utiliser une de ces caractères spéciaux pour séparer les commandes lorsque vous créez des macros ou que vous tapez des commandes sur le **doskey** ligne de commande. Ces caractères spéciaux sont équivalentes à l’aide de l’esperluette ( **&** ) sur une ligne de commande.                                                            |
  |      $$       |                                                                                                                                                              Spécifie le signe dollar ( **$** ).                                                                                                                                                               |
  | $1 à $9 |             Représente toute information de ligne de commande que vous voulez spécifier lorsque vous exécutez la macro. Les caractères spéciaux **$1** via **$9** sont des paramètres de traitement par lots qui vous permettent d’utiliser des données différentes sur la ligne de commande chaque fois que vous exécutez la macro. Le **$1** caractère dans un **doskey** commande est similaire à la **%1** caractère dans un programme de traitement par lots.             |
  |      $\*      | Représente toutes les informations de ligne de commande que vous souhaitez spécifier lorsque vous tapez le nom de macro. Le caractère spécial **$ \\** \* est un paramètre remplaçable semblable aux paramètres de lot **$1** via **$9**, avec une différence importante : tout ce que vous tapez sur la ligne de commande une fois que le nom de macro est remplacé par le **$ \\** \* dans la macro. |


- En cours d’exécution un **doskey** (macro)

  Pour exécuter une macro, tapez le nom de la macro à l’invite de commandes, en commençant à la première position. Si la macro a été définie avec **$ \\** * ou l’un des paramètres de lot **$1** via **$9**, utilisez un espace pour séparer les paramètres. Vous ne pouvez pas exécuter un **doskey** macro à partir d’un programme de traitement par lots.
- Création d’une macro avec le même nom qu’une commande de la famille Windows Server 2003

  Si vous utilisez toujours une commande particulière avec les options de ligne de commande spécifiques, vous pouvez créer une macro qui porte le même nom que la commande. Pour spécifier si vous souhaitez exécuter la macro ou la commande, suivez ces instructions :  
  -   Pour exécuter la macro, tapez le nom de la macro à l’invite de commandes. N’ajoutez pas d’espace avant le nom de macro.
  -   Pour exécuter la commande, insérer un ou plusieurs espaces à l’invite de commandes et tapez le nom de commande.
- La suppression d’une macro

  Pour supprimer une macro, tapez :  
  ```
  doskey <MacroName> =
  ```

## <a name="BKMK_examples"></a>Exemples

Le **/macros** et **/history** des options de ligne de commande sont utiles pour créer des programmes de traitement par lots pour enregistrer des macros et des commandes. Par exemple, pour stocker tous les cours **doskey** macros, tapez :
```
doskey /macros > macinit 
```
Pour utiliser les macros stockées dans Macinit, tapez :
```
doskey /macrofile=macinit 
```
Pour créer un lot programme nommé Tmp.bat contenant récemment utilisé des commandes, tapez :
```
doskey /history> tmp.bat 
```
Pour définir une macro avec plusieurs commandes, utilisez **$t** pour séparer les commandes, comme suit :
```
doskey tx=cd temp$tdir/w $*
```
Dans l’exemple précédent, la macro TX change le répertoire actif à Temp, puis affiche une liste en format large des répertoires. Vous pouvez utiliser **$ \\** * à la fin de la macro à ajouter d’autres options de ligne de commande pour **dir** lorsque vous exécutez TX.

La macro suivante utilise un paramètre de lot pour un nouveau nom de répertoire :
```
doskey mc=md $1$tcd $1
```
La macro crée un répertoire, puis passe au nouveau répertoire à partir du répertoire actuel.

Pour utiliser la macro ci-dessus pour créer et modifier dans un répertoire nommé livres, tapez :
```
mc books
```
Pour créer un **doskey** macro pour un programme appelé Ftp.exe, incluez **/nom_exe** comme suit :
```
doskey /exename=ftp.exe go=open 172.27.1.100$tmget *.TXT c:\reports$tbye 
```
Pour utiliser la macro ci-dessus, lancez FTP. À l’invite FTP, tapez :
```
go
```
Exécutions FTP le **ouvrir**, **mget**, et **bye** commandes.

Pour créer une macro qui met en forme un disque rapidement et sans condition, tapez :
```
doskey qf=format $1 /q /u
```
Pour rapidement et sans condition formater un disque dans le lecteur A, tapez :
```
qf a:
```
Pour supprimer une macro appelée vlist, tapez :
```
doskey vlist =
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)