---
ms.assetid: 835fc6f1-cc84-4189-b29a-dde90792469e
title: Fsutil hardlink
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 210196471123e9fc2456a2ee84b1949f9f883d90
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80844292"
---
# <a name="fsutil-hardlink"></a>Fsutil hardlink
>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Crée un lien physique entre un fichier existant et un nouveau fichier.

## <a name="syntax"></a>Syntaxe

```
fsutil hardlink create <NewFileName> <ExistingFileName>
fsutil hardlink list <Filename>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|-------------|---------------|
|créer|Établit un lien physique NTFS entre un fichier existant et un nouveau fichier. (Un lien physique NTFS est semblable à un lien physique POSIX.)|
|\<NewFileName >|Spécifie le fichier vers lequel vous souhaitez créer un lien physique.|
|\<ExistingFileName >|Spécifie le fichier à partir duquel vous souhaitez créer un lien physique.|
|list|Répertorie les liaisons permanentes de *nom de fichier*.<p>Ce paramètre s’applique à : Windows Server 2008 R2 et Windows 7.|

## <a name="remarks"></a>Notes

-   Un lien physique est une entrée d’annuaire pour un fichier. Chaque fichier peut être considéré comme ayant au moins un lien physique. Sur les volumes NTFS, chaque fichier peut avoir plusieurs liens physiques, donc un seul fichier peut apparaître dans de nombreux répertoires (ou même dans le même répertoire avec des noms différents). Étant donné que tous les liens font référence au même fichier, les programmes peuvent ouvrir n’importe quel lien et modifier le fichier. Un fichier est supprimé du système de fichiers uniquement une fois que tous les liens vers celui-ci ont été supprimés. Une fois que vous avez créé un lien physique, les programmes peuvent l’utiliser comme n’importe quel autre nom de fichier.

## <a name="additional-references"></a>Références supplémentaires
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

[Fsutil](Fsutil.md)


