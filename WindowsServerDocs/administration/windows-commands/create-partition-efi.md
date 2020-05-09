---
title: créer une partition EFI
description: Rubrique de référence pour la commande CREATE partition EFI, qui crée une partition système Extensible Firmware Interface (EFI) sur un disque GPT (GUID partition table) sur des ordinateurs Itanium.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3cfc1fca-6515-4a4d-bfae-615fa8045ea9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8c314d756bebd0d0ec2ed9c844f714f395d04c6b
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2020
ms.locfileid: "82993292"
---
# <a name="create-partition-efi"></a>créer une partition EFI

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crée une partition système Extensible Firmware Interface (EFI) sur un disque GPT (GUID partition table) sur des ordinateurs Itanium. Une fois la partition créée, le focus est donné à la nouvelle partition.

>[!NOTE]
> Pour que cette opération aboutisse, vous devez sélectionner un disque GPT. Utilisez la commande [Sélectionner le disque](select-disk.md) pour sélectionner un disque et lui déplacer le focus.

## <a name="syntax"></a>Syntaxe

```
create partition efi [size=<n>] [offset=<n>] [noerr]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| taille =`<n>` | Taille de la partition en mégaoctets (Mo). Si aucune taille n’est indiquée, la partition se poursuit jusqu’à ce qu’il n’y ait plus d’espace libre dans la région actuelle. |
| décalage =`<n>` | Décalage, en kilo-octets (Ko), auquel la partition est créée. Si aucun décalage n’est spécifié, la partition est placée dans la première étendue de disque qui est suffisamment grande pour la contenir. |
| noerr | À des fins de script uniquement. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur. |

#### <a name="remarks"></a>Notes 

- Vous devez ajouter au moins un volume avec la commande **Add volume** avant de pouvoir utiliser la commande **Create** .

- Après avoir exécuté la commande **Create** , vous pouvez utiliser la commande **Exec** pour exécuter un script de duplication pour la sauvegarde à partir du cliché instantané.

- Vous pouvez utiliser la commande **Démarrer la sauvegarde** pour spécifier une sauvegarde complète plutôt qu’une sauvegarde de copie.

## <a name="examples"></a>Exemples

Pour créer une partition EFI de 1000 mégaoctets sur le disque sélectionné, tapez :

```
create partition efi size=1000
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [créer une commande](create.md)

- [select disk](select-disk.md)
