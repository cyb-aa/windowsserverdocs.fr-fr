---
title: Bitsadmin setpeercachingflags
description: La rubrique commandes Windows pour **Bitsadmin setpeercachingflags**, qui définit des indicateurs qui déterminent si les fichiers du travail peuvent être mis en cache et servis aux homologues et si le travail peut télécharger du contenu à partir de pairs.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3f54a127-fb68-49a5-b843-664ec833df67
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1b4a7807975fb46440301e30b1fdbd01784d7c85
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122777"
---
# <a name="bitsadmin-setpeercachingflags"></a>Bitsadmin setpeercachingflags

Définit des indicateurs qui déterminent si les fichiers du travail peuvent être mis en cache et desservis aux homologues et si le travail peut télécharger du contenu à partir de pairs.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /setpeercachingflags <job> <value>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| le travail | Nom complet ou GUID du travail. |
| valeur | Entier non signé, y compris :<ul><li>**1.** le travail peut télécharger du contenu à partir de pairs.</li><li>**2.** les fichiers du travail peuvent être mis en cache et pris en charge par les homologues.</li></ul> |

## <a name="examples"></a>Exemples

L’exemple suivant définit des indicateurs pour le travail nommé *myDownloadJob*, ce qui lui permet de télécharger du contenu à partir de pairs.

```
C:\>bitsadmin /setpeercachingflags myDownloadJob 1
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)