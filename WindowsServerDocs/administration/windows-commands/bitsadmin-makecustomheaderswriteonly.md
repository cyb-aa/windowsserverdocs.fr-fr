---
title: Bitsadmin makecustomheaderswriteonly
description: Rubrique relative aux commandes Windows pour **Bitsadmin makecustomheaderswriteonly**, qui rend les en-têtes HTTP personnalisés d’un travail en écriture seule.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 9183b1b5de51020c5c6d2efad2c0a788d158a183
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850242"
---
# <a name="bitsadmin-makecustomheaderswriteonly"></a>Bitsadmin makecustomheaderswriteonly

Rendre les en-têtes HTTP personnalisés d’un travail en écriture seule.

> [!Important]
> Cette action ne peut pas être annulée.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /makecustomheaderswriteonly <job>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| le travail | Nom complet ou GUID du travail. |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant rend les en-têtes HTTP personnalisés en écriture seule pour le travail nommé *myDownloadJob*.

```
C:\>bitsadmin /makecustomheaderswriteonly myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)