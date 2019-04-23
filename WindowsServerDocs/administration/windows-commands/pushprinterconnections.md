---
title: pushprinterconnections
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c30afb97-b149-478f-a4b9-2cbc25361818 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c571d3adffd0e6a28f63f6d821b2524dc055aa9a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873720"
---
# <a name="pushprinterconnections"></a>pushprinterconnections



Lit les paramètres de connexion d’imprimante déployées à partir de la stratégie de groupe et déploie/supprime des connexions d’imprimante en fonction des besoins.

## <a name="syntax"></a>Syntaxe

```
pushprinterconnections <-log> <-?>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|<-log>|Écrit une par fichier journal de débogage utilisateur à % Temp%, ou des écritures un par un journal de débogage de machine à % windir%\temp.|
|<-?>|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

Cet utilitaire est pour une utilisation dans les scripts de connexion utilisateur ou de démarrage de machine et ne doit pas être exécuté à partir de la ligne de commande.

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Déploiement d’imprimantes à l’aide de stratégie de groupe](https://go.microsoft.com/fwlink/?LinkId=230627)