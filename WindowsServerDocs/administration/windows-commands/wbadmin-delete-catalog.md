---
title: Wbadmin supprimer le catalogue
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d3041407-4577-4716-a39f-2c8ab48818d1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 58b8bc6043437755675af28c084257ba0d8b176d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362528"
---
# <a name="wbadmin-delete-catalog"></a>Wbadmin supprimer le catalogue



Supprime le catalogue de sauvegarde stocké sur l’ordinateur local. Utilisez cette commande lorsque le catalogue de sauvegarde a été endommagé et que vous ne pouvez pas le restaurer à l’aide de **WBADMIN RESTORE CATALOG**.

Pour supprimer un catalogue de sauvegarde avec cette sous-commande, vous devez être membre du groupe **opérateurs de sauvegarde** ou **administrateurs** , ou l’autorisation appropriée doit vous avoir été déléguée. En outre, vous devez exécuter **Wbadmin** à partir d’une invite de commandes avec élévation de privilèges. (Pour ouvrir une invite de commandes avec élévation de privilèges, cliquez avec le bouton droit sur **invite de commandes**, puis cliquez sur **exécuter en tant qu’administrateur**.)

## <a name="syntax"></a>Syntaxe

```
wbadmin delete catalog
[-quiet]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|-quiet|Exécute la sous-commande sans invite à l’utilisateur.|

## <a name="remarks"></a>Notes

Si vous supprimez le catalogue de sauvegarde d’un ordinateur, vous ne serez pas en mesure d’accéder aux sauvegardes créées sur cet ordinateur à l’aide du composant logiciel enfichable Sauvegarde Windows Server. Dans ce cas, si vous pouvez accéder à un autre emplacement de sauvegarde, utilisez **WBADMIN RESTORE CATALOG** pour restaurer le catalogue de sauvegarde à partir de cet emplacement. Vous devez créer une nouvelle sauvegarde une fois votre catalogue de sauvegarde supprimé.

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [Remove-WBCatalog](https://technet.microsoft.com/library/jj902445.aspx)