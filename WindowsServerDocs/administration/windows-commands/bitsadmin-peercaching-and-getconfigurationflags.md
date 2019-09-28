---
title: Bitsadmin et getconfigurationflags
description: Rubrique relative aux commandes Windows pour **Bitsadmin en cache et getconfigurationflags** -obtient les indicateurs de configuration qui déterminent si l’ordinateur fournit du contenu aux pairs et peut télécharger du contenu à partir de pairs.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 94c7eb1a115fe9152b149b8cf65765b179080cc3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381089"
---
# <a name="bitsadmin-peercaching-and-getconfigurationflags"></a>Bitsadmin et getconfigurationflags



Obtient les indicateurs de configuration qui déterminent si l’ordinateur fournit du contenu aux pairs et peut télécharger du contenu à partir d’homologues.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /PeerCaching /GetConfigurationFlags <Job> 
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|

## <a name="BKMK_examples"></a>Illustre

L’exemple suivant obtient les indicateurs de configuration pour le travail nommé *myJob*.
```
C:\> Bitsadmin /PeerCaching /GetConfigurationFlags myJob
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)