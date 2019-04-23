---
title: attrib
description: Rubrique de commandes de Windows pour **attrib** -affiche, définit ou supprime les attributs assignés aux fichiers ou répertoires.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5e763ca5-21a2-45d2-b26d-a9c44c99091a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0f640af2c7957e43dd3f31dfa732bfc887112651
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841090"
---
# <a name="attrib"></a>attrib



Affiche, définit ou supprime les attributs assignés à des fichiers ou répertoires. Si utilisée sans paramètres, **attrib** affiche les attributs de tous les fichiers dans le répertoire actif.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
attrib [{+|-}r] [{+|-}a] [{+|-}s] [{+|-}h] [{+|-}i] [<Drive>:][<Path>][<FileName>] [/s [/d] [/l]]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|{+\|-}r|Jeux (**+**) ou efface (**-**) l’attribut de fichier en lecture seule.|
|{+\|-}a|Jeux (**+**) ou efface (**-**) l’attribut de fichier d’Archive.|
|{+\|-}s|Jeux (**+**) ou efface (**-**) l’attribut de fichier système.|
|{+\|-}h|Jeux (**+**) ou efface (**-**) l’attribut de fichier masqué.|
|{+\|-}i|Jeux (**+**) ou efface (**-**) l’attribut de fichier non contenu indexé.|
|[\<Drive>:][<Path>][<FileName>]|Spécifie l’emplacement et le nom du répertoire, du fichier ou du groupe de fichiers pour lequel vous souhaitez afficher ou modifier les attributs. Vous pouvez utiliser le **?** et **&#42;** caractères génériques dans le *FileName* paramètre pour afficher ou modifier les attributs d’un groupe de fichiers.|
|/s|S’applique **attrib** et les options de ligne de commande aux fichiers correspondants dans le répertoire actif et tous ses sous-répertoires.|
|/d|S’applique **attrib** et toutes les options de ligne de commande aux répertoires.|
|/l|S’applique **attrib** et toutes les options de ligne de commande pour le lien symbolique, plutôt que la cible du lien symbolique.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Vous pouvez utiliser des caractères génériques (**?** et **&#42;**) avec le *FileName* paramètre pour afficher ou modifier les attributs d’un groupe de fichiers.
-   Si un fichier a le système (**s**) ou masqué (**h**) jeu d’attributs, vous devez effacer l’attribut que vous puissiez modifier tous les autres attributs de ce fichier.
-   L’attribut d’archivage (**un**) marque les fichiers qui ont été modifiés depuis la dernière fois qu’ils ont été sauvegardés. Notez que le **xcopy** commande utilise l’attribut archive.

## <a name="BKMK_examples"></a>Exemples

Pour afficher les attributs d’un fichier nommé Nouv86 qui se trouve dans le répertoire actif, tapez :
```
attrib news86 
```
Pour affecter l’attribut en lecture seule au fichier rapport.txt, tapez :
```
attrib +r report.txt 
```
Pour supprimer l’attribut en lecture seule à partir de fichiers dans le répertoire Public et ses sous-répertoires sur un disque dans le lecteur B, tapez :
```
attrib -r b:\public\*.* /s 
```
Pour définir l’attribut d’Archive pour tous les fichiers sur le lecteur A, puis désactivez l’attribut d’archivage pour les fichiers portant l’extension .bak, tapez :
```
attrib +a a:*.* & attrib -a a:*.bak 
```
