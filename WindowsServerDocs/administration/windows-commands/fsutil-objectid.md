---
ms.assetid: 693ab895-9d0c-47c1-9f52-df5cd287842a
title: Fsutil objectid
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: e39b36a6c3126429bc47d5b89d104612cab5db96
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80844282"
---
# <a name="fsutil-objectid"></a>Fsutil objectid
>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Gère les identificateurs d’objets (OID), qui sont des objets internes utilisés par le service client de suivi de lien distribué (DLT) et le service de réplication de fichiers (FRS) pour effectuer le suivi d’autres objets, tels que des fichiers, des répertoires et des liens. Les identificateurs d’objet sont invisibles pour la plupart des programmes et ne doivent jamais être modifiés.

> [!CAUTION]
> Ne supprimez pas, ne définissez pas ou ne modifiez pas un identificateur d’objet. La suppression ou la définition d’un identificateur d’objet peut entraîner la perte de données provenant de portions d’un fichier, jusqu’à et y compris des volumes entiers de données. En outre, vous risquez de provoquer un comportement négatif dans le service client de suivi des liaisons distribuées (DLT) et le service de réplication de fichiers (FRS).

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
fsutil objectid [create] <FileName>
fsutil objectid [delete] <FileName>
fsutil objectid [query] <FileName>
fsutil objectid [set] <ObjectID> <BirthVolumeID> <BirthObjectID> <DomainID> <FileName>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|-------------|---------------|
|créer|Crée un identificateur d’objet si le fichier spécifié n’en a pas déjà un. Si le fichier a déjà un identificateur d’objet, cette sous-commande est équivalente à la sous-commande de **requête** .|
|delete|Supprime un identificateur d’objet.|
|query|Interroge un identificateur d’objet.|
|set|Définit un identificateur d’objet.|
|\<ObjectID >|Définit un identificateur hexadécimal à 16 octets propre au fichier qui est garanti comme étant unique au sein d’un volume. L’identificateur d’objet est utilisé par le service client de suivi de lien distribué (DLT) et le service de réplication de fichiers (FRS) pour identifier les fichiers.|
|\<BirthVolumeID >|Indique le volume sur lequel le fichier a été trouvé lors de la première obtention d’un identificateur d’objet. Cette valeur est un identificateur hexadécimal de 16 octets qui est utilisé par le service client DLT.|
|\<BirthObjectID >|Indique l’identificateur d’objet d’origine du fichier (l' *ObjectID* peut changer quand un fichier est déplacé). Cette valeur est un identificateur hexadécimal de 16 octets qui est utilisé par le service client DLT.|
|\<domaine >|identificateur de domaine hexadécimal de 16 octets. Cette valeur n’est pas utilisée actuellement et doit être définie sur tous les zéros.|
|Nom de fichier \<>|Spécifie le chemin d’accès complet au fichier, y compris le nom de fichier et l’extension, par exemple C:\documents\filename.txt.|

## <a name="remarks"></a>Notes

-   Tout fichier qui a un identificateur d’objet a également un identificateur de volume de naissance, un identificateur d’objet de naissance et un identificateur de domaine. Lorsque vous déplacez un fichier, l’identificateur d’objet peut changer, mais les identificateurs d’objet volume de naissance et anniversaire restent les mêmes. Ce comportement permet au système d’exploitation Windows de toujours trouver un fichier, quel que soit l’endroit où il a été déplacé.

## <a name="examples"></a><a name="BKMK_examples"></a>Illustre
Pour créer un identificateur d’objet, tapez :

`fsutil objectid create c:\temp\sample.txt`

Pour supprimer un identificateur d’objet, tapez :

`fsutil objectid delete c:\temp\sample.txt`

Pour interroger un identificateur d’objet, tapez :

`fsutil objectid query c:\temp\sample.txt`

Pour définir un identificateur d’objet, tapez :

`fsutil objectid set 40dff02fc9b4d4118f120090273fa9fc f86ad6865fe8d21183910008c709d19e 40dff02fc9b4d4118f120090273fa9fc 00000000000000000000000000000000 c:\temp\sample.txt`

## <a name="additional-references"></a>Références supplémentaires
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

[Fsutil](Fsutil.md)


