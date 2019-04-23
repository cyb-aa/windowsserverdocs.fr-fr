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
ms.openlocfilehash: eabcc5cacdcc5f7f4de7178b5afeff2acc89d7a8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862810"
---
#<a name="bitsadmin-getpeercachingflags"></a>Bitsadmin getpeercachingflags

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
[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)


