---
title: Bitsadmin et getconfigurationflags
description: Rubrique relative aux commandes Windows pour **Bitsadmin en cache** et **getconfigurationflags**, qui obtient les indicateurs de configuration qui déterminent si l’ordinateur fournit du contenu aux pairs et s’il peut télécharger du contenu à partir de pairs.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 124ddc15-3444-4bd5-96e5-c6bfabe4f9c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: be8d6a719d63c8e9c6250320560b6ce21274c680
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850172"
---
# <a name="bitsadmin-peercaching-and-getconfigurationflags"></a>Bitsadmin et getconfigurationflags

Obtient les indicateurs de configuration qui déterminent si l’ordinateur fournit du contenu aux pairs et s’il peut télécharger du contenu à partir de pairs.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /peercaching /getconfigurationflags <job>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| le travail | Nom complet ou GUID du travail. |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant obtient les indicateurs de configuration pour le travail nommé *myDownloadJob*.

```
C:\> bitsadmin /peercaching /getconfigurationflags myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)