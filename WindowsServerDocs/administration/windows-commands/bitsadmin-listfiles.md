---
title: bitsadmin listfiles
description: La rubrique commandes Windows pour **Bitsadmin listfiles**, qui répertorie les fichiers dans le travail spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ad0d1eaa-3bd8-45e5-8f72-4da7366f0d59
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1af11f7876a3d1cd36aa38c7ac26563c01e81ab5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850312"
---
# <a name="bitsadmin-listfiles"></a>bitsadmin listfiles

Répertorie les fichiers du travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /listfiles <job>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| le travail | Nom complet ou GUID du travail. |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant récupère la liste des fichiers pour le travail nommé *myDownloadJob*.

```
C:\>bitsadmin /listfiles myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)