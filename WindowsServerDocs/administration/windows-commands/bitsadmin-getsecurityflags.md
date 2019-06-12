---
title: Bitsadmin getsecurityflags
description: 'Rubrique de commandes de Windows pour **bitsadmin getsecurityflags** : signale les indicateurs de sécurité HTTP pour la redirection d’URL et les vérifications effectuées sur le certificat de serveur pendant le transfert.'
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c2e73519-34f4-487b-af11-97d5d08ef9bb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6e1db167b12d47afccb8842da617f1e9fe72acff
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434964"
---
# <a name="bitsadmin-getsecurityflags"></a>Bitsadmin getsecurityflags

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Rapports HTTP des indicateurs de sécurité pour la redirection d’URL et les vérifications effectuées sur le certificat de serveur pendant le transfert.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /GetSecurityFlags <Job> 
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|-------|--------|
|Tâche|Nom d’affichage ou le GUID du travail|

## <a name="BKMK_examples"></a>Exemples
L’exemple suivant récupère les indicateurs securitly à partir d’une tâche nommée *myJob*.

```
C:\>bitsadmin /GetSecurityFlags myJob 
```

## <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)


