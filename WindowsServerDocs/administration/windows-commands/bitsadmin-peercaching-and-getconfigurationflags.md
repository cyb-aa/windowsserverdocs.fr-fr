---
title: getconfigurationflags et mise en cache bitsadmin
description: Rubrique de commandes de Windows pour **mise en cache bitsadmin et getconfigurationflags** - Obtient les indicateurs de configuration qui déterminent si l’ordinateur fait Office de contenu des homologues et peut télécharger du contenu à partir d’homologues.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 124ddc15-3444-4bd5-96e5-c6bfabe4f9c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6afa39993cf90b2d71b6b681680c3b4e1fd9b56b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826350"
---
# <a name="bitsadmin-peercaching-and-getconfigurationflags"></a>getconfigurationflags et mise en cache bitsadmin



Obtient les indicateurs de configuration qui déterminent si l’ordinateur traite le contenu des homologues et qu’il peut télécharger du contenu à partir d’homologues.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /PeerCaching /GetConfigurationFlags <Job> 
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom d’affichage ou le GUID du travail|

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant obtient les indicateurs de configuration pour le travail nommé *myJob*.
```
C:\> Bitsadmin /PeerCaching /GetConfigurationFlags myJob
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)