---
title: Bitsadmin getpeercachingflags
description: Rubrique de commandes de Windows pour **bitsadmin getpeercachingflags** -récupère les indicateurs qui déterminent si les fichiers de la tâche peuvent être mis en cache et prise en charge pour les homologues, et si les BITS peuvent télécharger du contenu pour le travail à partir d’homologues.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3c3c9f28-4c04-4c49-a23a-dee5bbcc8981
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 28f248bab3e3cc3f5c7dd4f5f878f0b6d776029b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434925"
---
# <a name="bitsadmin-getpeercachingflags"></a>Bitsadmin getpeercachingflags

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Récupère les indicateurs qui déterminent si les fichiers de la tâche peuvent être mis en cache et prise en charge pour les homologues, et si les BITS peuvent télécharger du contenu pour le travail à partir d’homologues.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /GetPeerCachingFlags <Job> 
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|-------|--------|
|Tâche|Nom d’affichage ou le GUID du travail|

## <a name="BKMK_examples"></a>Exemples
L’exemple suivant récupère les indicateurs pour le travail nommé *myJob*.

```
C:\>bitsadmin /GetPeerCachingFlags myJob
```

## <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)


