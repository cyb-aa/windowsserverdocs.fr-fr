---
title: bitsadmin getnotifycmdline
description: Rubrique de commandes de Windows pour **bitsadmin getnotifycmdline** -récupère la ligne de commande qui est exécuté quand le travail terminé le transfert de données.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 90fa33e6-aca5-4a23-82bd-19a9f13f8416
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3ca7b2e67c0b5672733a25465fba89d1bd69d07a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817290"
---
# <a name="bitsadmin-getnotifycmdline"></a>bitsadmin getnotifycmdline

Récupère la ligne de commande à exécuter lorsque le travail terminé le transfert de données.

**BITS 1.2 et versions antérieures**: Non pris en charge.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /GetNotifyCmdLine <Job>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom d’affichage ou le GUID du travail|

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant récupère la ligne de commande utilisé par le service lorsque le travail nommé *myDownloadJob* se termine.
```
C:\>bitsadmin /GetNotifyCmdLine myDownloadJob
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)