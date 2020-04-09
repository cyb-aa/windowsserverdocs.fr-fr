---
title: Wbadmin désactiver la sauvegarde
description: La rubrique commandes Windows pour Wbadmin Disable Backup, qui arrête l’exécution des sauvegardes quotidiennes planifiées existantes.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5176cbd9-0696-4b3f-9c35-272dd84f7898
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e2fb3d22fc3857cce191ee11381ae6e7e6ac1175
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80829852"
---
# <a name="wbadmin-disable-backup"></a>Wbadmin désactiver la sauvegarde



Arrête l’exécution des sauvegardes quotidiennes planifiées existantes.

Pour désactiver une sauvegarde quotidienne planifiée, vous devez être membre du groupe **administrateurs** ou les autorisations appropriées doivent vous avoir été déléguées. En outre, vous devez exécuter **Wbadmin** à partir d’une invite de commandes avec élévation de privilèges. (Pour ouvrir une invite de commandes avec élévation de privilèges, cliquez avec le bouton droit sur **invite de commandes** , puis cliquez sur **exécuter en tant qu’administrateur**.)

## <a name="syntax"></a>Syntaxe

```
wbadmin disable backup
[-quiet]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|-quiet|Exécute la sous-commande sans invite à l’utilisateur.|

## <a name="additional-references"></a>Références supplémentaires

-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)