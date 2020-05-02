---
title: tâche d’arrêt Wbadmin
description: Rubrique de référence pour Wbadmin Stop Job, qui annule la sauvegarde ou l’opération de récupération en cours d’exécution. Les opérations annulées ne peuvent pas être redémarrées ; vous devez réexécuter une opération de sauvegarde ou de récupération annulée à partir du début.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3b83b398-39c7-4410-bf17-5c1fb1a4f46d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3133688ac0d60d97d80192611c9b561c53a74c35
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725847"
---
# <a name="wbadmin-stop-job"></a>tâche d’arrêt Wbadmin



Annule la sauvegarde ou l’opération de récupération en cours d’exécution. Les opérations annulées ne peuvent pas être redémarrées ; vous devez réexécuter une opération de sauvegarde ou de récupération annulée à partir du début.

Pour arrêter une sauvegarde ou une opération de récupération avec cette sous-commande, vous devez être membre du groupe **opérateurs de sauvegarde** ou **administrateurs** , ou l’autorité appropriée doit vous avoir été déléguée. En outre, vous devez exécuter **Wbadmin** à partir d’une invite de commandes avec élévation de privilèges. (Pour ouvrir une invite de commandes avec élévation de privilèges, cliquez avec le bouton droit sur **invite de commandes** , puis cliquez sur **exécuter en tant qu’administrateur**.)

## <a name="syntax"></a>Syntaxe

```
wbadmin stop job
[-quiet]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|-quiet|Exécute la sous-commande sans invite à l’utilisateur.|

## <a name="additional-references"></a>Références supplémentaires

-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)