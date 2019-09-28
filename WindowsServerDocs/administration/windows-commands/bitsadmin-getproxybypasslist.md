---
title: bitsadmin getproxybypasslist
description: La rubrique commandes Windows pour **Bitsadmin getproxybypasslist** -récupère la liste de contournement du proxy pour le travail spécifié.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 50959be3-7014-4bc9-9a7b-68f1ff94a94a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 87cc131402707eac40329750e98218ec52083b94
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381416"
---
# <a name="bitsadmin-getproxybypasslist"></a>bitsadmin getproxybypasslist

Récupère la liste de contournement du proxy pour le travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /GetProxyBypassList <Job>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|

## <a name="remarks"></a>Notes

La liste de contournement contient les noms d’hôte ou les adresses IP, ou les deux, qui ne doivent pas être routés via un proxy. La liste peut contenir « \<local > » pour faire référence à tous les serveurs sur le même réseau local. La liste peut être délimitée par des points-virgules ou délimités par des espaces.

## <a name="BKMK_examples"></a>Illustre

L’exemple suivant récupère la liste de contournement du proxy pour le travail nommé *myDownloadJob*.
```
C:\>bitsadmin /GetProxyBypassList myDownloadJob
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)