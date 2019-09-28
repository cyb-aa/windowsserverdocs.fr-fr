---
title: Bitsadmin getsecurityflags
description: La rubrique commandes Windows pour **Bitsadmin getsecurityflags** -signale les indicateurs de sécurité http pour la redirection d’URL et les vérifications effectuées sur le certificat de serveur pendant le transfert.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: fb53664a6366b411ae1eb9b0fe7c93392d60b542
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381459"
---
# <a name="bitsadmin-getsecurityflags"></a>Bitsadmin getsecurityflags

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Signale les indicateurs de sécurité HTTP pour la redirection d’URL et les vérifications effectuées sur le certificat de serveur pendant le transfert.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /GetSecurityFlags <Job> 
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|-------|--------|
|Tâche|Nom complet ou GUID du travail|

## <a name="BKMK_examples"></a>Illustre
L’exemple suivant récupère les indicateurs securitly à partir d’un travail nommé *myJob*.

```
C:\>bitsadmin /GetSecurityFlags myJob 
```

## <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)


