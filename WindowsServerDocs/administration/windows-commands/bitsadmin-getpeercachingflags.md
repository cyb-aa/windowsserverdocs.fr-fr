---
title: Bitsadmin getpeercachingflags
description: La rubrique commandes Windows pour **Bitsadmin getpeercachingflags**, qui récupère les indicateurs qui déterminent si les fichiers du travail peuvent être mis en cache et servis aux homologues, et si bits peut télécharger du contenu pour le travail à partir d’homologues.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3c3c9f28-4c04-4c49-a23a-dee5bbcc8981
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cdf9683d1a65400286b4604bd9420a5ab863d4af
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850552"
---
# <a name="bitsadmin-getpeercachingflags"></a>Bitsadmin getpeercachingflags

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Récupère les indicateurs qui déterminent si les fichiers du travail peuvent être mis en cache et desservis aux homologues, et si BITS peut télécharger du contenu pour le travail à partir de pairs.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /getpeercachingflags <job> 
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| le travail | Nom complet ou GUID du travail. |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant récupère les indicateurs du travail nommé *myDownloadJob*.

```
C:\>bitsadmin /getpeercachingflags myJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)