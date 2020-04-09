---
title: bitsadmin setdisplayname
description: La rubrique commandes Windows pour Bitsadmin SetDisplayName, qui définit le nom complet du travail spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 13706c53-fb5f-4879-b5ca-82531361d6e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 601c5b406132e70fb7d4facb97329f7456002bb4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849542"
---
# <a name="bitsadmin-setdisplayname"></a>bitsadmin setdisplayname

Définit le nom d’affichage du travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /SetDisplayName <Job> <DisplayName>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|
|DisplayName|Texte utilisé pour le nom complet du travail spécifié.|

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant définit le nom complet de la tâche nommée *myDownloadJob* sur *myDownloadJob2*.
```
C:\>bitsadmin /SetDisplayName myDownloadJob Download Music Job
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)