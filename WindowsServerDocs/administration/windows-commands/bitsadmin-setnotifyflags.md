---
title: bitsadmin setnotifyflags
description: La rubrique commandes Windows pour **Bitsadmin setnotifyflags**, qui définit les indicateurs de notification d’événement pour le travail spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d5763d95-94a6-45ca-9e03-891c20047e06
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 73c088ce2bae8d2ad99b313417c14449ddd822b5
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122799"
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
| le travail | Nom complet ou GUID du travail. |
| notifyflags | Peut inclure un ou plusieurs des indicateurs de notification suivants, notamment :<ul><li>**1.** génère un événement lorsque tous les fichiers du travail ont été transférés.</li><li>**2.** génère un événement lorsqu’une erreur se produit.</li><li>**3.** génère un événement lorsque tous les fichiers ont été transférés ou lorsqu’une erreur se produit.</li><li>**4.** désactive les notifications.</li></ul> |

## <a name="examples"></a>Exemples

L’exemple suivant définit les indicateurs de notification pour générer un événement lorsqu’une erreur se produit, pour un travail nommé *myDownloadJob*.

```
C:\>bitsadmin /setnotifyflags myDownloadJob 2
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)