---
title: bitsadmin replaceremoteprefix
description: Rubrique de référence pour la commande Bitsadmin REPLACEREMOTEPREFIX, qui modifie l’URL distante de tous les fichiers du travail de *oldprefix* à *newprefix*, si nécessaire.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d0e0abb1-bdb4-4c74-abbc-16c809f5fd81
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 745d026513413db799e86df3422d5ee19c89274f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717036"
---
# <a name="bitsadmin-replaceremoteprefix"></a>bitsadmin replaceremoteprefix

Modifie l’URL distante de tous les fichiers du travail de *oldprefix* à *newprefix*, si nécessaire.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /replaceremoteprefix <job> <oldprefix> <newprefix>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| travail | Nom complet ou GUID du travail. |
| oldprefix | Préfixe d’URL existant. |
| newprefix | Nouveau préfixe d’URL. |

## <a name="examples"></a>Exemples

Pour modifier l’URL distante de tous les fichiers de la tâche *http://stageserver* nommée *http://prodserver* *myDownloadJob*, de à.

```
bitsadmin /replaceremoteprefix myDownloadJob http://stageserver http://prodserver
```

## <a name="additional-information"></a>Informations supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin](bitsadmin.md)
