---
ms.assetid: 77545920-2d13-4f35-a4d1-14dbec8340dc
title: fsutil sparse
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: b1bc4e45ed2a2b06c72318e0999988ed8f016c40
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438968"
---
# <a name="fsutil-sparse"></a>fsutil sparse
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
|     queryflag     |                                                  Fragmentation.                                                  |
|    queryrange     |                        Analyse un fichier et recherche de plages qui contiennent des données différent de zéro.                        |
|      setflag      |                                        Marque le fichier indiqué comme étant fragmenté.                                        |
|     SetRange      |                                   Remplit une plage spécifiée d’un fichier avec des zéros.                                   |
|    <FileName>     | Spécifie le chemin d’accès complet au fichier, y compris le nom de fichier et l’extension, par exemple C:\documents\filename.txt. |
| <BeginningOffset> |                              Spécifie l’offset dans le fichier à marquer comme étant incomplet.                              |
|     <Length>      |                 Spécifie la longueur de la région dans le fichier à marquer comme partiellement alloué (en octets).                 |

## <a name="remarks"></a>Notes

-   Un fichier partiellement alloué est un fichier avec une ou plusieurs régions de données non allouées. Un programme verra ces régions non allouées comme contenant des octets avec la valeur zéro, mais aucun espace disque n’est utilisé pour représenter ces zéros non significatifs. Toutes les données significatives ou différente de zéro est alloué, tandis que toutes les données nonmeaningful (chaînes de grande taille de données sont composées de zéros) n’est pas allouée. Quand un fichier partiellement alloué est lue, données allouées sont retournées comme stockées et les données non allouées sont retournées, par défaut, sous forme de zéros, conformément à la spécification d’exigence de sécurité C2. Prise en charge des fichiers partiellement alloués permet aux données à désallouer dans n’importe où dans le fichier.

-   Dans un fichier partiellement alloué, les grandes plages de zéros ne nécessitent pas de l’allocation de disque. Espace de données différent de zéro sera alloué en fonction des besoins lorsque le fichier est écrit.

-   Uniquement les fichiers compressés ou incomplets peuvent avoir remis à zéro plages par le système d’exploitation.

-   Si le fichier est partiellement alloués ou compressés, NTFS peut libérer l’espace disque sur le fichier. Cela définit la plage d’octets en zéros sans étendre la taille du fichier.

## <a name="BKMK_examples"></a>Exemples
Pour marquer un fichier nommé exemple.txt dans le répertoire C:\Temp comme partiellement alloué, tapez :

```
fsutil sparse setflag c:\temp\sample.txt 
```

#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


