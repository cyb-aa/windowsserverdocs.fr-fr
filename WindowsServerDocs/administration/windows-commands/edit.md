---
title: edit
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4e0ff2e8-3518-47c1-8c69-5e93f895fa0e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 096632005b3e42dd941ccc7c72c08ead1d291b53
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873150"
---
# <a name="edit"></a>edit



Démarre l’Éditeur MS-DOS, qui crée et modifie des fichiers texte ASCII.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
edit [/b] [/h] [/r] [/s] [/<NNN>] [[<Drive>:][<Path>]<FileName> [<FileName2> [...]]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|[\<Drive>:][<Path>]<FileName> [<FileName2> [...]]|Spécifie l’emplacement et le nom d’un ou plusieurs fichiers texte ASCII. Si le fichier n’existe pas, l’Éditeur MS-DOS le crée. Si le fichier existe, l’Éditeur MS-DOS s’ouvre et affiche son contenu à l’écran. *Nom de fichier* peut contenir des caractères génériques (**&#42;** et **?**). Séparez plusieurs noms de fichiers avec des espaces.|
|/b|Force le mode monochrome, pour que cet Éditeur MS-DOS s’affiche en noir et blanc.|
|/h|Affiche le nombre maximal de lignes possible pour le moniteur actuel.|
|/r|Charge ou les fichiers dans le mode lecture seule.|
|/s|Force l’utilisation de noms de fichiers courts.|
|\<NNN>|Charge une ou plusieurs fichiers binaires, renvoi à la ligne à *NNN* caractères larges.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Pour obtenir une aide supplémentaire, ouvrez l’Éditeur MS-DOS, puis appuyez sur la touche F1.
-   Certaines analyses ne prennent pas en charge l’affichage des touches de raccourci par défaut. Si votre moniteur n’affiche pas les touches de raccourci, utilisez **/b**.

## <a name="BKMK_examples"></a>Exemples

Pour ouvrir l’Éditeur MS-DOS, tapez :
```
edit
```
Pour créer et modifier un fichier nommé newtextfile.txt dans le répertoire actif, tapez :
```
edit newtextfile.txt
```