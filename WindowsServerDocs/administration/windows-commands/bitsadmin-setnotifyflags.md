---
title: bitsadmin setnotifyflags
description: Rubrique de référence pour la commande Bitsadmin setnotifyflags, qui définit les indicateurs de notification d’événement pour le travail spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d5763d95-94a6-45ca-9e03-891c20047e06
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 00b704bf0943790ef01bbfbdbcbbde4dfd1845c6
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717279"
---
# <a name="bitsadmin-setnotifyflags"></a>bitsadmin setnotifyflags

Définit les indicateurs de notification d’événement pour le travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /setnotifyflags <job> <notifyflags>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| travail | Nom complet ou GUID du travail. |
| notifyflags | Peut inclure un ou plusieurs des indicateurs de notification suivants, notamment :<ul><li>**1.** génère un événement lorsque tous les fichiers du travail ont été transférés.</li><li>**2.** génère un événement lorsqu’une erreur se produit.</li><li>**3.** génère un événement lorsque tous les fichiers ont été transférés ou lorsqu’une erreur se produit.</li><li>**4.** désactive les notifications.</li></ul> |

## <a name="examples"></a>Exemples

Pour définir les indicateurs de notification afin de générer un événement lorsqu’une erreur se produit, pour un travail nommé *myDownloadJob*:

```
bitsadmin /setnotifyflags myDownloadJob 2
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin](bitsadmin.md)
