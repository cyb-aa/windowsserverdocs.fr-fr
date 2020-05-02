---
title: graftabl
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b08351d4-3d24-490c-86f6-1252da11d923
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b3815b85da163c03dea7bd2619d4647454d3ebd7
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724926"
---
# <a name="graftabl"></a>graftabl



Permet aux systèmes d’exploitation Windows d’afficher un jeu de caractères étendu en mode graphique. En cas d’utilisation sans paramètre, **GRAFTABL** affiche la page de codes précédente et actuelle.



## <a name="syntax"></a>Syntaxe

```
graftabl <CodePage>
graftabl /status
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<Page de codes>|Spécifie une page de codes pour définir l’apparence des caractères étendus en mode graphique.</br>Les numéros d’identification de page de codes valides sont :</br>437 : États-Unis</br>850 : multilingue (I latin)</br>852 : Slave (latin II)</br>855 : cyrillique (russe)</br>857 : turc</br>860 : Portugais</br>861 : islandais</br>863 : Canada-français</br>865 : nordique</br>866 : russe</br>869 : grec moderne|
|/status|Affiche la page de codes actuelle que **GRAFTABL** utilise.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes 

-   **GRAFTABL** affecte uniquement l’affichage de l’analyse des caractères étendus de la page de codes que vous spécifiez. Elle ne modifie pas la page de codes d’entrée de la console. Pour modifier la page de codes d’entrée de la console, utilisez la commande **mode** ou **chcp** .
-   Le tableau suivant répertorie chaque code de sortie et fournit une brève description.  
    |Code de sortie|Description|
    |---------|-----------|
    |0|Le jeu de caractères a été chargé avec succès. Aucune page de codes précédente n’a été chargée.|
    |1|Un paramètre incorrect a été spécifié. Aucune action n'a été menée.|
    |2|Une erreur de fichier s’est produite.|
-   Vous pouvez utiliser la variable d’environnement ERRORLEVEL dans un programme de traitement par lots pour traiter les codes de sortie retournés par **GRAFTABL**.

## <a name="examples"></a>Exemples

Pour afficher la page de codes actuelle utilisée par **GRAFTABL**, tapez :
```
graftabl /status
```
Pour charger le jeu de caractères graphiques pour la page de codes 437 (États-Unis) en mémoire, tapez :
```
graftabl 437
```
Pour charger le jeu de caractères graphiques pour la page de codes 850 (multilingue) en mémoire, tapez :
```
graftabl 850
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

[Freedisk](freedisk.md)

[Modifie](chcp.md)