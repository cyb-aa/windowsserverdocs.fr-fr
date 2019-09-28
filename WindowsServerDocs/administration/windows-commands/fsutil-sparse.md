---
ms.assetid: 77545920-2d13-4f35-a4d1-14dbec8340dc
title: Fsutil sparse
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: f9fb3cf46afb7e96c13fb623bc8f4fe67c1f3694
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376806"
---
# <a name="fsutil-sparse"></a>Fsutil sparse
>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Gère les fichiers partiellement alloués.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
fsutil sparse [queryflag] <FileName>
fsutil sparse [queryrange] <FileName>
fsutil sparse [setflag] <FileName>
fsutil sparse [setrange] <FileName> <BeginningOffset> <Length>
```

## <a name="parameters"></a>Paramètres

|     Paramètre     |                                                    Description                                                    |
|-------------------|-------------------------------------------------------------------------------------------------------------------|
|     queryflag     |                                                  Requêtes éparses.                                                  |
|    queryrange     |                        Analyse un fichier et recherche les plages qui peuvent contenir des données de valeur différente de zéro.                        |
|      setflag      |                                        Marque le fichier indiqué comme fragmenté.                                        |
|     SetRange      |                                   Remplit une plage spécifiée d’un fichier avec des zéros.                                   |
|    <FileName>     | Spécifie le chemin d’accès complet au fichier, y compris le nom de fichier et l’extension, par exemple C:\documents\filename.txt. |
| <BeginningOffset> |                              Spécifie le décalage dans le fichier à marquer comme étant fragmenté.                              |
|     <Length>      |                 Spécifie la longueur de la région dans le fichier à marquer comme étant partiellement allouée (en octets).                 |

## <a name="remarks"></a>Notes

-   Un fichier partiellement alloué est un fichier contenant une ou plusieurs régions de données non allouées. Un programme voit ces régions non allouées comme contenant des octets ayant la valeur zéro, mais aucun espace disque n’est utilisé pour représenter ces zéros. Toutes les données significatives ou non nulles sont allouées, alors que toutes les données sans signification (grandes chaînes de données composées de zéros) ne sont pas allouées. Lorsqu’un fichier partiellement alloué est lu, les données allouées sont retournées comme stockées et les données non allouées sont retournées, par défaut, comme des zéros, conformément à la spécification de la spécification de sécurité C2. La prise en charge des fichiers partiellement alloués permet de libérer les données de n’importe où dans le fichier.

-   Dans un fichier partiellement alloué, les plages de zéros volumineuses ne peuvent pas nécessiter l’allocation de disque. L’espace pour les données autres que zéro sera alloué si nécessaire lors de l’écriture du fichier.

-   Seuls les fichiers compressés ou partiellement alloués peuvent avoir des plages de zéro connues du système d’exploitation.

-   Si le fichier est fragmenté ou compressé, NTFS peut libérer de l’espace disque dans le fichier. Cela définit la plage d’octets à zéros sans étendre la taille du fichier.

## <a name="BKMK_examples"></a>Illustre
Pour marquer un fichier nommé Sample. txt dans le répertoire C:\Temp comme fragmenté, tapez :

```
fsutil sparse setflag c:\temp\sample.txt 
```

#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


