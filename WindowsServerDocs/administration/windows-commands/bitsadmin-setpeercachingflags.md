---
title: Bitsadmin setpeercachingflags
description: La rubrique commandes Windows pour Bitsadmin setpeercachingflags, qui définit des indicateurs qui déterminent si les fichiers du travail peuvent être mis en cache et servis aux homologues et si le travail peut télécharger du contenu à partir de pairs.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3f54a127-fb68-49a5-b843-664ec833df67
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d19e4d14b47e4aa96e9ad9d4367e872350ad4d43
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849242"
---
# <a name="bitsadmin-setpeercachingflags"></a>Bitsadmin setpeercachingflags

Définit des indicateurs qui déterminent si les fichiers du travail peuvent être mis en cache et desservis aux homologues et si le travail peut télécharger du contenu à partir de pairs.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /SetPeerCachingFlags <Job> <value> 
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|
|Valeur|La valeur est un entier non signé avec l’interprétation suivante pour les bits dans la représentation binaire.</br>1-la tâche peut télécharger du contenu à partir de pairs.</br>2-les fichiers du travail peuvent être mis en cache et pris en charge par les homologues.|

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant définit des indicateurs pour le travail nommé *myJob* , ce qui lui permet de télécharger du contenu à partir de pairs.
```
C:\>bitsadmin / SetPeerCachingFlags myJob 1 
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)