---
title: Bitsadmin removeclientcertificate
description: Rubrique relative aux commandes Windows pour **Bitsadmin removeclientcertificate** -supprime le certificat client du travail.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b417c3e5-aadd-4fcc-968f-45d8b67ca516
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c664ba9b26f3511dedf35477a1cd393db709337e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381042"
---
# <a name="bitsadmin-removeclientcertificate"></a>Bitsadmin removeclientcertificate



Supprime le certificat client du travail.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /RemoveClientCertificate <Job> 
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|

## <a name="BKMK_examples"></a>Illustre

L’exemple suivant supprime le certificat client du travail nommé *myJob*.
```
C:\>Bitsadmin /RemoveClientCertificate myJob 
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)