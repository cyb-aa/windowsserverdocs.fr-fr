---
title: graftabl
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b08351d4-3d24-490c-86f6-1252da11d923
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d55df814cb962e82775a86e154a024c579987cf2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80842412"
---
# <a name="graftabl"></a>graftabl



Permet aux systèmes d’exploitation Windows d’afficher un jeu de caractères étendu en mode graphique. En cas d’utilisation sans paramètre, **GRAFTABL** affiche la page de codes précédente et actuelle.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
graftabl <CodePage>
graftabl /status
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Page de codes \<>|Spécifie une page de codes pour définir l’apparence des caractères étendus en mode graphique.</br>Les numéros d’identification de page de codes valides sont :</br>437 : États-Unis</br>850 : multilingue (I latin)</br>852 : Slave (latin II)</br>855 : cyrillique (russe)</br>857 : turc</br>860 : Portugais</br>861 : islandais</br>863 : Canada-français</br>865 : nordique</br>866 : russe</br>869 : grec moderne|
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

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

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