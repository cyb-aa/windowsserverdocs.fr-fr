---
title: Bitsadmin setpeercachingflags
description: Rubrique de commandes de Windows pour **bitsadmin setpeercachingflags** -définit les indicateurs qui déterminent si les fichiers de la tâche peuvent être mis en cache et prise en charge pour les homologues et si le travail peut télécharger du contenu à partir d’homologues.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3f54a127-fb68-49a5-b843-664ec833df67
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2d50a6ccd83a6251808ca3d66437e52f641c60a7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814250"
---
# <a name="bitsadmin-setpeercachingflags"></a>Bitsadmin setpeercachingflags



Définit les indicateurs qui déterminent si les fichiers de la tâche peuvent être mis en cache et prise en charge pour les homologues et si le travail peut télécharger contenu à partir d’homologues.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /SetPeerCachingFlags <Job> <value> 
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom d’affichage ou le GUID du travail|
|Value|La valeur est un entier non signé avec l’interprétation suivante des bits dans la représentation binaire.</br>1 - la tâche peut télécharger du contenu à partir d’homologues.</br>2 : les fichiers de la tâche peuvent être mis en cache et prise en charge pour les homologues.|

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant définit des indicateurs pour le travail nommé *myJob* qui lui permet de télécharger le contenu à partir d’homologues.
```
C:\>bitsadmin / SetPeerCachingFlags myJob 1 
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)