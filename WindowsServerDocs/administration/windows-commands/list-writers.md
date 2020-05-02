---
title: répertorier les enregistreurs
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1c30cbc4-f568-4fa7-b564-66c41d3ca82d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ef7fa0bd6e9857e22994178849e49afeb3305a2a
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724487"
---
# <a name="list-writers"></a>répertorier les enregistreurs



Répertorie les enregistreurs qui se trouvent sur le système. S’il est utilisé sans paramètres, **List** affiche la sortie des **métadonnées de liste** par défaut.



## <a name="syntax"></a>Syntaxe

```
list writers [metadata | detailed | status]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|metadata|Répertorie l’identité et l’état des enregistreurs, et affiche les métadonnées telles que les détails du composant et les fichiers exclus. Il s’agit du paramètre par défaut.|
|détaillée|Répertorie les mêmes informations que les **métadonnées**, mais **détaillé** comprend la liste complète des fichiers pour tous les composants.|
|status|Répertorie uniquement l’identité et l’état des enregistreurs inscrits.|

## <a name="examples"></a>Exemples

Pour répertorier uniquement l’identité et l’état des enregistreurs, tapez :
```
list writers status
```
Une sortie semblable à la suivante s’affiche :
```
Listing writer status ...
* WRITER System Writer
        - Status: 5 (VSS_WS_WAITING_FOR_BACKUP_COMPLETE)
        - Writer Failure code: 0x00000000 (S_OK)
        - Writer ID: {e8132975-6f93-4464-a53e-1050253ae220}
        - Instance ID: {7e631031-c695-4229-9da1-a7de057e64cb}
* WRITER Shadow Copy Optimization Writer
        - Status: 1 (VSS_WS_STABLE)
        - Writer Failure code: 0x00000000 (S_OK)
        - Writer ID: {4dc3bdd4-ab48-4d07-adb0-3bee2926fd7f}
        - Instance ID: {9e362607-9794-4dd4-a7cd-b3d5de0aad20}
...
...
...
* WRITER Registry Writer
        - Status: 1 (VSS_WS_STABLE)
        - Writer Failure code: 0x00000000 (S_OK)
        - Writer ID: {afbab4a2-367d-4d15-a586-71dbb18f8485}
        - Instance ID: {e87ba7e3-f8d8-42d8-b2ee-c76ae26b98e8}
8 writers listed. 
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)