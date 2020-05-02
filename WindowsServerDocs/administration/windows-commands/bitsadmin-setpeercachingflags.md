---
title: bitsadmin setpeercachingflags
description: Rubrique de référence pour la commande Bitsadmin setpeercachingflags, qui définit des indicateurs qui déterminent si les fichiers du travail peuvent être mis en cache et desservis à des homologues et si le travail peut télécharger du contenu à partir de pairs.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3f54a127-fb68-49a5-b843-664ec833df67
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8b66b169c38ac050ecaaf6546365547148faa9cf
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717266"
---
# <a name="bitsadmin-setpeercachingflags"></a>bitsadmin setpeercachingflags

Définit des indicateurs qui déterminent si les fichiers du travail peuvent être mis en cache et desservis aux homologues et si le travail peut télécharger du contenu à partir de pairs.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /setpeercachingflags <job> <value>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| travail | Nom complet ou GUID du travail. |
| value | Entier non signé, y compris :<ul><li>**1.** le travail peut télécharger du contenu à partir de pairs.</li><li>**2.** les fichiers du travail peuvent être mis en cache et pris en charge par les homologues.</li></ul> |

## <a name="examples"></a>Exemples

Pour autoriser la tâche nommée *myDownloadJob* à télécharger le contenu à partir d’homologues :

```
bitsadmin /setpeercachingflags myDownloadJob 1
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin](bitsadmin.md)
