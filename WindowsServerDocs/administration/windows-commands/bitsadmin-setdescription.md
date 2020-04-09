---
title: bitsadmin setdescription
description: La rubrique commandes Windows pour Bitsadmin SetDescription, qui définit la description du travail spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1e46a5dd-4637-4a2e-b88f-d3f85b177db8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4a17f864e3bc3b3cdc8ba0d76d553bcfcef27d29
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849562"
---
# <a name="bitsadmin-setdescription"></a>bitsadmin setdescription

Définit la description du travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /SetDescription <Job> <Description>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|
|Description|Texte utilisé pour décrire le travail.|

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant récupère la description de la tâche nommée *myDownloadJob*.
```
C:\>bitsadmin /SetDescription myDownloadJob Music Downloads
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)