---
title: bitsadmin replaceremoteprefix
description: La rubrique commandes Windows pour **Bitsadmin REPLACEREMOTEPREFIX**, qui modifie l’URL distante de tous les fichiers du travail de *oldprefix* à *newprefix*, si nécessaire.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d0e0abb1-bdb4-4c74-abbc-16c809f5fd81
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0cea0108a292815e31e893e91dc4079305c1da9a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849812"
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
| le travail | Nom complet ou GUID du travail. |
| oldprefix | Préfixe d’URL existant. |
| newprefix | Nouveau préfixe d’URL. |

## <a name="examples"></a>Exemples

L’exemple suivant modifie l’URL distante de tous les fichiers de la tâche nommée *myDownloadJob*, de *http://stageserver* à *http://prodserver* .

```
C:\>bitsadmin /replaceremoteprefix myDownloadJob http://stageserver http://prodserver
```

## <a name="additional-information"></a>Informations supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)