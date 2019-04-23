---
title: graftabl
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b08351d4-3d24-490c-86f6-1252da11d923
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 395873cf3dbeb574dd9abc69f45b410bece80c25
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848440"
---
# <a name="graftabl"></a>graftabl



Permet aux systèmes d’exploitation de Windows afficher un caractère étendu défini en mode graphique. Si utilisée sans paramètres, **graftabl** affiche l’ancienne et la page de codes actuelle.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
graftabl <CodePage>
graftabl /status
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<CodePage>|Spécifie une page de codes pour définir l’apparence de caractères étendus en mode graphique.</br>Numéros d’identification de page de code valides sont :</br>437: États-Unis</br>850: Multilingue (Latin I)</br>852: Slave (Latin II)</br>855: Cyrillique (russe)</br>857: Turc</br>860: Portugais</br>861: Islandais</br>863: Français (Canada)</br>865: Europe du Nord</br>866: Russe</br>869: Grec moderne|
|/status|Affiche les cours page de codes **graftabl** est à l’aide.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   **Graftabl** affecte uniquement l’affichage de l’analyse des caractères étendus de la page de codes que vous spécifiez. Il ne modifie pas la page de code d’entrée de console. Pour modifier la page de codes d’entrée de console, utilisez le **mode** ou **chcp** commande.
-   Le tableau suivant répertorie chaque code de sortie et une brève description de celle-ci.  
    |Code de sortie|Description|
    |---------|-----------|
    |0|Jeu de caractères a été chargé avec succès. Aucune page de code précédente a été chargé.|
    |1|Un paramètre incorrect a été spécifié. Aucune action n'a été menée.|
    |2|Une erreur de fichier s’est produite.|
-   Vous pouvez utiliser la variable d’environnement ERRORLEVEL dans un programme de traitement par lots pour traiter des codes de sortie qui sont retournés par **graftabl**.

## <a name="BKMK_examples"></a>Exemples

Pour afficher la page de codes actuelle utilisée par **graftabl**, type :
```
graftabl /status
```
Pour charger le jeu de caractères graphiques pour la page de codes 437 (États-Unis) dans la mémoire, tapez :
```
graftabl 437
```
Pour charger le graphique jeu de caractères pour la page de codes 850 (multilingue en mémoire), tapez :
```
graftabl 850
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)

[freedisk](freedisk.md)

[Chcp](chcp.md)