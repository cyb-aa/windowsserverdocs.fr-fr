---
title: doskey
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4874fd43-d5ea-45f3-ae24-388ae925ed76
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fca89bd2e99b6139b13a5bd45481ae0ec2248574
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720812"
---
# <a name="doskey"></a>doskey



Appelle Doskey. exe (qui rappelle les commandes de ligne de commande entrées précédemment), modifie les lignes de commande et crée des macros.



## <a name="syntax"></a>Syntaxe

```
doskey [/reinstall] [/listsize=<Size>] [/macros:[all | <ExeName>] [/history] [/insert | /overstrike] [/exename=<ExeName>] [/macrofile=<FileName>] [<MacroName>=[<Text>]]
```

### <a name="parameters"></a>Paramètres

|       Paramètre        |                                                                                                                          Description                                                                                                                           |
|------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       /REINSTALL       |                                                                                            Installe une nouvelle copie de Doskey. exe et efface la mémoire tampon de l’historique des commandes.                                                                                            |
|   /LISTSIZE =\<taille>    |                                                                                                Spécifie le nombre maximal de commandes dans la mémoire tampon de l’historique.                                                                                                 |
|        /macros         |                                        Affiche la liste de toutes les macros **doskey** . Vous pouvez utiliser le symbole de redirection (**>**) avec **/macros** pour rediriger la liste vers un fichier. Vous pouvez abréger **/macros** à **/m**.                                         |
|      /macros : tout       |                                                                                                        Affiche les macros **doskey** pour tous les exécutables.                                                                                                         |
|   /macros :\<EXEName>   |                                                                                             Affiche les macros **doskey** pour l’exécutable spécifié par *exeName*.                                                                                              |
|        /History        |                                    Affiche toutes les commandes qui sont stockées en mémoire. Vous pouvez utiliser le symbole de redirection (**>**) avec **/History** pour rediriger la liste vers un fichier. Vous pouvez abréger **/History** comme **/h**.                                    |
| /Insert | Spécifie que le nouveau texte que vous tapez est inséré dans l’ancien texte. |
| /overstrike | Spécifie que le nouveau texte remplace l’ancien texte. |
|  /EXEName =\<EXEName>   |                                                                                        Spécifie le programme (c’est-à-dire, exécutable) dans lequel la macro **doskey** s’exécute.                                                                                         |
| /MACROFILE =\<nom de fichier> |                                                                                              Spécifie un fichier qui contient les macros que vous souhaitez installer.                                                                                               |
| \<Nommacro>= [\<texte>]  | Crée une macro qui exécute les commandes spécifiées par le *texte*. *Nommacro* spécifie le nom que vous souhaitez assigner à la macro. *Texte* spécifie les commandes que vous souhaitez enregistrer. Si le *texte* n’est pas renseigné, la commande *nommacro* est désactivée pour toutes les commandes attribuées. |
|           /?           |                                                                                                              Affiche l'aide à l'invite de commandes.                                                                                                              |

## <a name="remarks"></a>Notes 

- Utilisation de Doskey. exe

  Doskey. exe est toujours disponible pour tous les programmes interactifs basés sur des caractères (tels que les débogueurs de programme ou les programmes de transfert de fichiers) et il gère un tampon d’historique de commande et des macros pour chaque programme qu’il démarre. Vous ne pouvez pas utiliser les options de ligne de commande **doskey** d’un programme. Vous devez exécuter les options de ligne de commande **doskey** avant de démarrer un programme. Les attributions de clés de programme remplacent les attributions de clé **doskey** .
- Rappel d’une commande

  Pour rappeler une commande, vous pouvez utiliser l’une des clés suivantes après avoir démarré Doskey. exe. Si vous utilisez Doskey. exe dans un programme, les attributions de clés de ce programme sont prioritaires.  

  |    Clé     |                              Description                              |
  |------------|-----------------------------------------------------------------------|
  |  Flèche haut  |  Rappelle la commande que vous avez utilisée avant celle qui est affichée.  |
  | Bas |  Rappelle la commande que vous avez utilisée après celle qui est affichée.   |
  |  Pg. préc   |    Rappelle la première commande que vous avez utilisée dans la session active.    |
  | Pg. suiv  | Rappelle la commande la plus récente que vous avez utilisée dans la session active. |


- Modification de la ligne de commande

  Avec Doskey. exe, vous pouvez modifier la ligne de commande actuelle. Si vous utilisez Doskey. exe dans un programme, les attributions de clés de ce programme sont prioritaires et certaines clés d’édition Doskey. exe peuvent ne pas fonctionner.

  Le tableau suivant répertorie les clés d’édition **doskey** et leurs fonctions.  

  | Touche ou combinaison de touches |                                                                                                                                                       Description                                                                                                                                                       |
  |------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
  |       Gauche       |                                                                                                                                      Déplace le point d’insertion d’un caractère vers l’arrière.                                                                                                                                      |
  |      Flèche droite       |                                                                                                                                    Déplace le point d’insertion d’un caractère vers l’avant.                                                                                                                                     |
  |    CTRL+FLECHE GAUCHE     |                                                                                                                                        Déplace le point d’insertion d’un mot vers l’arrière.                                                                                                                                         |
  |    CTRL+FLECHE DROITE    |                                                                                                                                       Déplace le point d’insertion d’un mot vers l’avant.                                                                                                                                       |
  |          Origine          |                                                                                                                                 Déplace le point d’insertion au début de la ligne.                                                                                                                                 |
  |          END           |                                                                                                                                    Déplace le point d’insertion jusqu’à la fin de la ligne.                                                                                                                                    |
  |          Échap           |                                                                                                                                          Efface la commande de l’affichage.                                                                                                                                           |
  |           F1           |                                                                      Copie un caractère d’une colonne du modèle vers la même colonne dans la fenêtre d’invite de commandes. (Le modèle est une mémoire tampon qui contient la dernière commande que vous avez tapée.)                                                                       |
  |           F2           |                                                                 Effectue une recherche vers le bas du modèle pour la touche suivante que vous tapez après avoir appuyé sur F2. Doskey. exe insère le texte à partir du modèle (jusqu’au caractère que vous spécifiez, mais sans l’inclure).                                                                  |
  |           F3           |                                                 Copie le reste du modèle sur la ligne de commande. Doskey. exe commence à copier les caractères à partir de la position dans le modèle qui correspond à la position indiquée par le point d’insertion sur la ligne de commande.                                                 |
  |           F4           |                                                                            Supprime tous les caractères de la position actuelle du point d’insertion jusqu’à l’occurrence suivante du caractère que vous tapez après avoir appuyé sur F4.                                                                            |
  |           F5           |                                                                                                                                   Copie le modèle dans la ligne de commande actuelle.                                                                                                                                    |
  |           F6           |                                                                                                                    Place un caractère de fin de fichier (CTRL + Z) à la position actuelle du point d’insertion.                                                                                                                    |
  |           F7           | Affiche (dans une boîte de dialogue) toutes les commandes de ce programme qui sont stockées en mémoire. Utilisez la touche haut et la touche de direction bas pour sélectionner la commande souhaitée, puis appuyez sur entrée pour exécuter la commande. Vous pouvez également noter le nombre séquentiel devant la commande et utiliser ce nombre conjointement avec la touche F9. |
  |         ALT+F7         |                                                                                                                          Supprime toutes les commandes stockées en mémoire pour le tampon d’historique actuel.                                                                                                                          |
  |           F8           |                                                                                                           Affiche toutes les commandes dans la mémoire tampon de l’historique qui commencent par les caractères de la commande actuelle.                                                                                                            |
  |           F9           |                                             Vous invite à entrer un numéro de commande de tampon d’historique, puis affiche la commande associée au nombre que vous spécifiez. Appuyez sur entrée pour exécuter la commande. Pour afficher tous les nombres et leurs commandes associées, appuyez sur F7.                                             |
  |        ALT+F10         |                                                                                                                                             Supprime toutes les définitions de macros.                                                                                                                                              |


- Utilisation de **doskey** dans un programme

  Certains programmes interactifs basés sur des caractères, tels que les débogueurs de programme ou les programmes de transfert de fichiers (FTP), utilisent automatiquement Doskey. exe. Pour utiliser DOSKEY. exe, un programme doit être un processus de console et utiliser une entrée mise en mémoire tampon. Les attributions de clés de programme remplacent les attributions de clé **doskey** . Par exemple, si le programme utilise la touche F7 pour une fonction, vous ne pouvez pas obtenir un historique de commande **doskey** dans une fenêtre indépendante.

  Avec Doskey. exe, vous pouvez conserver un historique de commande pour chaque programme que vous démarrez ou recommencez. Vous pouvez modifier les commandes précédentes à l’invite du programme et démarrer les macros **doskey** créées pour le programme. Si vous quittez, puis redémarrez un programme à partir de la même fenêtre d’invite de commandes, l’historique de commande de la session de programme précédente est disponible.

  Vous devez exécuter Doskey. exe avant de démarrer un programme. Vous ne pouvez pas utiliser les options de ligne de commande **doskey** à partir de l’invite de commandes d’un programme, même si le programme a une commande d’interpréteur de commandes.

  Si vous souhaitez personnaliser la façon dont Doskey. exe fonctionne avec un programme et créer des macros **doskey** pour ce programme, vous pouvez créer un programme de traitement par lots qui modifie Doskey. exe et démarre le programme.
- Spécification d’un mode d’insertion par défaut

  Si vous appuyez sur la touche Inser, vous pouvez taper du texte sur la ligne de commande **doskey** au milieu du texte existant sans remplacer le texte. Toutefois, une fois que vous avez appuyé sur entrée, Doskey. exe retourne votre clavier en mode de remplacement. Vous devez appuyer à nouveau sur Insérer pour revenir au mode insertion.

  Utilisez **/Insert** pour basculer le clavier en mode insertion chaque fois que vous appuyez sur entrée. Votre clavier reste en mode insertion jusqu’à ce que vous utilisiez **/overstrike**. Vous pouvez revenir temporairement au mode de remplacement en appuyant sur la touche Inser, mais après avoir appuyé sur entrée, Doskey. exe retourne votre clavier en mode insertion.

  Le point d’insertion change de forme lorsque vous utilisez la touche Inser pour passer d’un mode à l’autre.
- Création d’une macro

  Vous pouvez utiliser DOSKEY. exe pour créer des macros qui exécutent une ou plusieurs commandes. Le tableau suivant répertorie les caractères spéciaux que vous pouvez utiliser pour contrôler les opérations de commande lorsque vous définissez une macro.  

  |   Caractère   |                                                                                                                                                                               Description                                                                                                                                                                               |
  |---------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
  |   $G ou $g    |                                                                                   Redirige la sortie. Utilisez l’un de ces caractères spéciaux pour envoyer la sortie vers un appareil ou un fichier plutôt que vers l’écran. Ce caractère est équivalent au symbole de redirection pour la sortie (**>**).                                                                                    |
  | $G $ G ou $g $ g  |                                                         Ajoute la sortie à la fin d’un fichier. Utilisez l’un ou l’autre de ces deux caractères pour ajouter la sortie à un fichier existant au lieu de remplacer les données du fichier. Ces caractères doubles sont équivalents au symbole de redirection d’ajout pour la**>>** sortie ().                                                         |
  |   $L ou $l    |                                                                                  Redirige l’entrée. Utilisez l’un ou l’autre de ces caractères spéciaux pour lire l’entrée à partir d’un appareil ou d’un fichier au lieu du clavier. Ce caractère est équivalent au symbole de redirection pour Input (**<**).                                                                                  |
  |   $B ou $b    |                                                                                                                                    Envoie la sortie de la macro à une commande. Ces caractères spéciaux sont équivalents à l’utilisation du canal (\*\*                                                                                                                                     |
  |   $T ou $t    |                                                            Sépare les commandes. Utilisez l’un ou l’autre de ces caractères spéciaux pour séparer les commandes lorsque vous créez des macros ou tapez des commandes sur la ligne de commande **doskey** . Ces caractères spéciaux sont équivalents à l’utilisation de**&** l’esperluette () sur une ligne de commande.                                                            |
  |      $$       |                                                                                                                                                              Spécifie le caractère de signe dollar**$**().                                                                                                                                                               |
  | de $1 à $9 |             Représente les informations de ligne de commande que vous souhaitez spécifier lorsque vous exécutez la macro. Les caractères spéciaux **$1** à **$9** sont des paramètres batch qui vous permettent d’utiliser des données différentes sur la ligne de commande chaque fois que vous exécutez la macro. Le caractère **$1** dans une commande **doskey** est semblable au caractère **%1** dans un programme de traitement par lots.             |
  |      $\*      | Représente toutes les informations de ligne de commande que vous souhaitez spécifier lorsque vous tapez le nom de la macro. ** $ ** Le caractère \* spécial est un paramètre remplaçable qui est similaire aux paramètres de lot **$1** à **$9**, avec une différence importante : tout ce que vous tapez sur la ligne de commande après le remplacement ** $ ** du nom de la macro \* dans la macro. |


- Exécution d’une macro **doskey**

  Pour exécuter une macro, tapez le nom de la macro à l’invite de commandes, en commençant à la première position. Si la macro a été définie ** $ **avec * ou l’un des paramètres de lot **$1** à **$9**, utilisez un espace pour séparer les paramètres. Vous ne pouvez pas exécuter une macro **doskey** à partir d’un programme de traitement par lots.
- Création d’une macro portant le même nom qu’une commande de la famille Windows Server 2003

  Si vous utilisez toujours une commande particulière avec des options de ligne de commande spécifiques, vous pouvez créer une macro qui porte le même nom que la commande. Pour spécifier si vous souhaitez exécuter la macro ou la commande, suivez ces instructions :  
  -   Pour exécuter la macro, tapez le nom de la macro à l’invite de commandes. N’ajoutez pas d’espace avant le nom de la macro.
  -   Pour exécuter la commande, insérez un ou plusieurs espaces à l’invite de commandes, puis tapez le nom de la commande.
- Suppression d’une macro

  Pour supprimer une macro, tapez :  
  ```
  doskey <MacroName> =
  ```

## <a name="examples"></a>Exemples

Les options de ligne de commande **/macros** et **/History** sont utiles pour créer des programmes batch afin d’enregistrer des macros et des commandes. Par exemple, pour stocker toutes les macros **doskey** actuelles, tapez :
```
doskey /macros > macinit 
```
Pour utiliser les macros stockées dans Macinit, tapez :
```
doskey /macrofile=macinit 
```
Pour créer un programme de commandes nommé tmp. bat qui contient les commandes récemment utilisées, tapez :
```
doskey /history> tmp.bat 
```
Pour définir une macro avec plusieurs commandes, utilisez **$t** pour séparer les commandes, comme suit :
```
doskey tx=cd temp$tdir/w $*
```
Dans l’exemple précédent, la macro TX remplace le répertoire actuel par Temp, puis affiche une liste de répertoires dans un format d’affichage étendu. Vous pouvez utiliser ** $ *** à la fin de la macro pour ajouter d’autres options de ligne de commande à **dir** lorsque vous exécutez TX.

La macro suivante utilise un paramètre batch pour un nouveau nom de répertoire :
```
doskey mc=md $1$tcd $1
```
La macro crée un nouveau répertoire, puis passe au nouveau répertoire dans le répertoire actif.

Pour utiliser la macro précédente pour créer un répertoire nommé Books et le modifier, tapez :
```
mc books
```
Pour créer une macro **doskey** pour un programme appelé FTP. exe, incluez **/exeName** comme suit :
```
doskey /exename=ftp.exe go=open 172.27.1.100$tmget *.TXT c:\reports$tbye 
```
Pour utiliser la macro précédente, démarrez FTP. À l’invite FTP, tapez :
```
go
```
FTP exécute les commandes **Open**, **mget**et **Bye** .

Pour créer une macro qui formate rapidement et sans condition un disque, tapez :
```
doskey qf=format $1 /q /u
```
Pour formater rapidement et sans condition un disque dans le lecteur A, tapez :
```
qf a:
```
Pour supprimer une macro appelée Vlist, tapez :
```
doskey vlist =
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
