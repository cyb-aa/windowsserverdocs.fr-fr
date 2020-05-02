---
title: pushprinterconnections
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c30afb97-b149-478f-a4b9-2cbc25361818 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 94ba2d09a3e67248a393350e46e971aa8b24b00d
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722762"
---
# <a name="pushprinterconnections"></a>pushprinterconnections



Lit les paramètres de connexion d’imprimante déployés à partir de stratégie de groupe et déploie/supprime les connexions d’imprimante en fonction des besoins.

## <a name="syntax"></a>Syntaxe

```
pushprinterconnections <-log> <-?>
```

#### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|<-log>|Écrit un fichier journal de débogage par utilisateur dans% Temp, ou écrit un journal de débogage par ordinateur dans répertoire%windir%\temp.|
|<- ? >|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes 

Cet utilitaire est destiné à être utilisé dans les scripts de démarrage de l’ordinateur ou d’ouverture de session de l’utilisateur, et ne doit pas être exécuté à partir de la ligne de commande.

## <a name="additional-references"></a>Références supplémentaires

-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Déployer des imprimantes à l’aide de stratégie de groupe](https://go.microsoft.com/fwlink/?LinkId=230627)