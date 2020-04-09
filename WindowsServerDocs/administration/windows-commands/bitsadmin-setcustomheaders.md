---
title: Bitsadmin setcustomheaders
description: Rubrique relative aux commandes Windows pour Bitsadmin setcustomheaders, qui ajoute un en-tête HTTP personnalisé à une requête d’extraction.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ed926410-80d0-46ed-9a90-f752c164bb9a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e5d97fae5f84637c80c3d1ef00aa36f09049bb17
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849612"
---
# <a name="bitsadmin-setcustomheaders"></a>Bitsadmin setcustomheaders

Ajoutez un en-tête HTTP personnalisé à une requête d’extraction.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /SetCustomHeaders <Job> <Header1> <Header2> <. . .>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|
|Header1 Header2. . .|En-têtes personnalisés pour le travail|

## <a name="remarks"></a>Notes

-   Ce commutateur est utilisé pour ajouter un en-tête HTTP personnalisé à une demande d’extraction envoyée à un serveur HTTP.

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant ajoute un en-tête HTTP personnalisé pour le travail nommé *myDownloadJob*.
```
C:\>bitsadmin / SetCustomHeaders myDownloadJob Accept-encoding:deflate/gzip
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)