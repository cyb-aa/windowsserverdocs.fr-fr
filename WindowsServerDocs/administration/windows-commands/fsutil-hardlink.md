---
ms.assetid: 835fc6f1-cc84-4189-b29a-dde90792469e
title: fsutil hardlink
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 69474bd1817471176598afba508cd80c8fa1df8a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840050"
---
# <a name="fsutil-hardlink"></a>fsutil hardlink
>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Crée un lien physique entre un fichier existant et un nouveau fichier.

## <a name="syntax"></a>Syntaxe

```
fsutil hardlink create <NewFileName> <ExistingFileName>
fsutil hardlink list <Filename>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|-------------|---------------|
|créer|Établit un lien physique NTFS entre un fichier existant et un nouveau fichier. (Un lien physique NTFS est similaire à un lien physique POSIX.)|
|\<NewFileName>|Spécifie le fichier que vous souhaitez créer un lien physique pour.|
|\<ExistingFileName>|Spécifie le fichier que vous souhaitez créer un lien à partir de.|
|list|Répertorie les liaisons permanentes pour *Filename*.<br /><br />Ce paramètre s’applique à :  Windows Server 2008 R2 et Windows 7.|

## <a name="remarks"></a>Notes

-   Un lien physique est une entrée d’annuaire pour un fichier. Chaque fichier peut être considérée d’avoir au moins un lien physique. Sur les volumes NTFS, chaque fichier peut avoir plusieurs liens physiques, pour un seul fichier peut apparaître dans de nombreux répertoires (ou même dans le même répertoire avec des noms différents). Étant donné que tous les liens de référencent le même fichier, les programmes peuvent ouvrir les liens et modifier le fichier. Un fichier est supprimé du système de fichiers uniquement après la suppression de tous les liens vers elle. Une fois que vous créez un lien physique, les programmes peuvent l’utiliser comme tout autre nom de fichier.

#### <a name="additional-references"></a>Références supplémentaires
[Clé de la syntaxe de ligne de commande](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


