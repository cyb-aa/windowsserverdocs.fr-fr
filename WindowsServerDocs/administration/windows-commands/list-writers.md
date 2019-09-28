---
title: répertorier les enregistreurs
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1c30cbc4-f568-4fa7-b564-66c41d3ca82d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d00eebe4e562764e97794b3eb1b76ea96c2dc47c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374718"
---
# <a name="list-writers"></a>répertorier les enregistreurs



Répertorie les enregistreurs qui se trouvent sur le système. S’il est utilisé sans paramètres, **List** affiche la sortie des **métadonnées de liste** par défaut.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
list writers [metadata | detailed | status]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|métadonnées|Répertorie l’identité et l’état des enregistreurs, et affiche les métadonnées telles que les détails du composant et les fichiers exclus. Il s’agit du paramètre par défaut.|
|modalités|Répertorie les mêmes informations que les **métadonnées**, mais **détaillé** comprend la liste complète des fichiers pour tous les composants.|
|status|Répertorie uniquement l’identité et l’état des enregistreurs inscrits.|

## <a name="BKMK_examples"></a>Illustre

Pour répertorier uniquement l’identité et l’état des enregistreurs, tapez :
```
list writers status
```
Une sortie semblable à la suivante s’affiche :
```
Listing writer status ...
* WRITER "System Writer"
        - Status: 5 (VSS_WS_WAITING_FOR_BACKUP_COMPLETE)
        - Writer Failure code: 0x00000000 (S_OK)
        - Writer ID: {e8132975-6f93-4464-a53e-1050253ae220}
        - Instance ID: {7e631031-c695-4229-9da1-a7de057e64cb}
* WRITER "Shadow Copy Optimization Writer"
        - Status: 1 (VSS_WS_STABLE)
        - Writer Failure code: 0x00000000 (S_OK)
        - Writer ID: {4dc3bdd4-ab48-4d07-adb0-3bee2926fd7f}
        - Instance ID: {9e362607-9794-4dd4-a7cd-b3d5de0aad20}
...
...
...
* WRITER "Registry Writer"
        - Status: 1 (VSS_WS_STABLE)
        - Writer Failure code: 0x00000000 (S_OK)
        - Writer ID: {afbab4a2-367d-4d15-a586-71dbb18f8485}
        - Instance ID: {e87ba7e3-f8d8-42d8-b2ee-c76ae26b98e8}
8 writers listed. 
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)