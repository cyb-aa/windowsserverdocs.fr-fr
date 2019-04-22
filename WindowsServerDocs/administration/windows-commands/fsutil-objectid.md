---
ms.assetid: 693ab895-9d0c-47c1-9f52-df5cd287842a
title: fsutil objectid
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 2f5887f20e2c36ec7dcfcd6f4e920b5273c6c60c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813740"
---
# <a name="fsutil-objectid"></a>fsutil objectid
>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Gère les identificateurs d’objet (OID), qui sont des objets internes utilisés par le service Client de suivi de lien distribué (DLT) et le Service de réplication de fichiers (FRS) pour effectuer le suivi d’autres objets tels que des fichiers, répertoires et des liens. Identificateurs d’objet sont invisibles pour la plupart des programmes et ne doivent jamais être modifiés.

> [!CAUTION]
> Ne supprimez, définissez pas ou modifier un identificateur d’objet. La suppression ou de la définition d’un identificateur d’objet peut entraîner la perte de données à partir de parties d’un fichier, y compris des volumes entiers de données. En outre, vous risquez d’endommager sérieusement dans le service Client de suivi de lien distribué (DLT) et le Service de réplication de fichiers (FRS).

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
fsutil objectid [create] <FileName>
fsutil objectid [delete] <FileName>
fsutil objectid [query] <FileName>
fsutil objectid [set] <ObjectID> <BirthVolumeID> <BirthObjectID> <DomainID> <FileName>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|-------------|---------------|
|créer|Crée un identificateur d’objet si le fichier spécifié n’a pas déjà un. Si le fichier a déjà un identificateur d’objet, la sous-commande équivaut à la **requête** sous-commande.|
|supprimer|Supprime un identificateur d’objet.|
|requête|Interroge un identificateur d’objet.|
|jeu|Définit un identificateur d’objet.|
|\<ObjectID>|Définit un identificateur hexadécimal de fichier de 16 octets qui est garanti être unique au sein d’un volume. L’identificateur d’objet est utilisé par le service Client de suivi de lien distribué (DLT) et le Service de réplication de fichiers (FRS) pour identifier les fichiers.|
|\<BirthVolumeID>|Indique le volume sur lequel le fichier se trouvait lorsqu’il obtenu tout d’abord un identificateur d’objet. Cette valeur est un identificateur hexadécimal de 16 octets qui est utilisé par le service Client DLT.|
|\<BirthObjectID>|Indique l’identificateur d’objet d’origine du fichier (le *ObjectID* peut changer lorsqu’un fichier est déplacé). Cette valeur est un identificateur hexadécimal de 16 octets qui est utilisé par le service Client DLT.|
|\<DomainID>|identificateur de domaine hexadécimal de 16 octets. Cette valeur n’est pas actuellement utilisée et doit être définie sur tous les zéros non significatifs.|
|\<FileName>|Spécifie le chemin d’accès complet au fichier, y compris le nom de fichier et l’extension, par exemple C:\documents\filename.txt.|

## <a name="remarks"></a>Notes

-   N’importe quel fichier qui a un identificateur d’objet possède également un identificateur de volume de naissance, un identificateur d’objet de naissance et un identificateur de domaine. Lorsque vous déplacez un fichier, l’identificateur d’objet peut changer, mais le volume de naissance et les identificateurs d’objet naissance restent les mêmes. Ce comportement permet au système d’exploitation Windows de toujours trouver un fichier, quel que soit l’endroit où il a été déplacé.

## <a name="BKMK_examples"></a>Exemples
Pour créer un identificateur d’objet, tapez :

`fsutil objectid create c:\temp\sample.txt`

Pour supprimer un identificateur d’objet, tapez :

`fsutil objectid delete c:\temp\sample.txt`

Pour interroger un identificateur d’objet, tapez :

`fsutil objectid query c:\temp\sample.txt`

Pour définir un identificateur d’objet, tapez :

`fsutil objectid set 40dff02fc9b4d4118f120090273fa9fc f86ad6865fe8d21183910008c709d19e 40dff02fc9b4d4118f120090273fa9fc 00000000000000000000000000000000 c:\temp\sample.txt`

#### <a name="additional-references"></a>Références supplémentaires
[Clé de la syntaxe de ligne de commande](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


