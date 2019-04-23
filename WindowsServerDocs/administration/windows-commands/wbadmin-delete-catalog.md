---
title: WBADMIN delete catalogue
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 2d9f60cf79fcd856fa972d8f43f195c5823e5d81
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841410"
---
# <a name="wbadmin-delete-catalog"></a>WBADMIN delete catalogue



Supprime le catalogue de sauvegarde est stocké sur l’ordinateur local. Utilisez cette commande lorsque le catalogue de sauvegarde a été endommagé et que vous ne pouvez pas restaurer à l’aide de **wbadmin restauration catalogue**.

Pour supprimer un catalogue de sauvegarde avec la sous-commande, vous devez être membre du **opérateurs de sauvegarde** groupe ou le **administrateurs** groupe, ou vous devez vous avoir été délégué des autorisations appropriées. En outre, vous devez exécuter **wbadmin** à partir d’une invite de commandes avec élévation de privilèges. (Pour ouvrir un invite de commandes avec élévation de privilèges de droit **invite de commandes**, puis cliquez sur **exécuter en tant qu’administrateur**.)

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

Si vous supprimez le catalogue de sauvegarde pour un ordinateur, vous ne serez pas en mesure d’accéder aux sauvegardes de cet ordinateur à l’aide du composant logiciel enfichable Sauvegarde Windows Server créées. Dans ce cas, si vous pouvez accéder à un autre emplacement de sauvegarde, utilisez **wbadmin restauration catalogue** pour restaurer le catalogue de sauvegarde à partir de cet emplacement. Vous devez créer une nouvelle sauvegarde une fois que votre catalogue de sauvegarde est supprimé.

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [Remove-WBCatalog](https://technet.microsoft.com/library/jj902445.aspx)