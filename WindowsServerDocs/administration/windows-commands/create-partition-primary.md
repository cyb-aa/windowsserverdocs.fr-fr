---
title: create partition primary
description: Rubrique de référence pour la commande CREATE partition Primary, qui crée une partition principale sur le disque de base avec le focus.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6d652d8e-3935-4a91-8ced-b17c0e7937be
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ac1763d52d8caf90112fb371aeac5e6b501bd2ea
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2020
ms.locfileid: "82993242"
---
# <a name="create-partition-primary"></a>create partition primary

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crée une partition principale sur le disque de base avec le focus. Une fois la partition créée, le focus se déplace automatiquement vers la nouvelle partition.

> [!IMPORTANT]
> Pour que cette opération aboutisse, vous devez sélectionner un disque de base. Vous devez utiliser la commande [Sélectionner le disque](select-disk.md) pour sélectionner un disque de base et y déplacer le focus.

## <a name="syntax"></a>Syntaxe

```
create partition primary [size=<n>] [offset=<n>] [id={ <byte> | <guid> }] [align=<n>] [noerr]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| taille =`<n>` | Spécifie la taille de la partition en mégaoctets (Mo). Si aucune taille n’est indiquée, la partition se poursuit jusqu’à ce qu’il n’y ait plus d’espace non alloué dans la région actuelle. |
| décalage =`<n>` | Décalage, en kilo-octets (Ko), auquel la partition est créée. Si aucun décalage n’est spécifié, la partition commence au début de la plus grande étendue de disque qui est suffisamment grande pour la conserver. |
| aligner =`<n>` | Aligne toutes les étendues de partition sur la limite d’alignement la plus proche. Généralement utilisé avec les tableaux d’unités logiques RAID matériels pour améliorer les performances. `<n>`nombre de kilo-octets (Ko) entre le début du disque et la limite d’alignement la plus proche. |
| ID = { `<byte>  | <guid>` } | Spécifie le type de partition. Ce paramètre est destiné uniquement aux fabricants OEM. Tous les octets ou GUID de type de partition peuvent être spécifiés avec ce paramètre. DiskPart ne vérifie pas la validité du type de partition, sauf pour s’assurer qu’il s’agit d’un octet au format hexadécimal ou d’un GUID. **Attention :** La création de partitions à l’aide de ce paramètre peut entraîner l’échec de votre ordinateur ou l’impossibilité de démarrer. À moins que vous ne soyez un fabricant d’ordinateurs OEM ou un professionnel de l’informatique expérimenté avec les disques GPT, ne créez pas de partitions sur des disques GPT à l’aide de ce paramètre. Au lieu de cela, utilisez toujours la commande [Create partition EFI](create-partition-efi.md) pour créer des partitions système EFI, la commande [Create partition MSR](create-partition-msr.md) pour créer des partitions réservées Microsoft et la commande [Create partition Primary](create-partition-primary.md)) (sans le `id={ <byte>  | <guid>` paramètre) pour créer des partitions principales sur des disques GPT.<p>**Pour les disques d’enregistrement de démarrage principal (MBR)**, vous devez spécifier un octet de type de partition, au format hexadécimal, pour la partition. Si ce paramètre n’est pas spécifié, la commande crée une partition `0x06`de type, qui spécifie qu’aucun système de fichiers n’est installé. Voici quelques exemples :<ul><li>**Partition de données LDM :** 0x42</li><li>**Partition de récupération :** 0x27</li><li>**Partition OEM reconnue :** 0x12, 0X84, 0XDE, 0XFE, 0xa0</li></ul><p>**Pour les disques de table de partition GUID (GPT)**, vous pouvez spécifier un GUID de type de partition pour la partition que vous souhaitez créer. Les GUID reconnus sont les suivants :<ul><li>**Partition système EFI :** C12A7328-F81F-11D2-BA4B-00A0C93EC93B</li><li>**Partition réservée Microsoft :** e3c9e316-0b5c-4db8-817d-f92df00215ae</li><li>**Partition de données de base :** ebd0a0a2-b9e5-4433-87C0-68b6b72699c7</li><li>**Partition de métadonnées LDM (disque dynamique) :** 5808c8aa-7e8f-42e0-85d2-e1e90434cfb3</li><li>**Partition de données LDM (disque dynamique) :** af9b60a0-1431-4f62-bc68-3311714a69ad</li><li>**Partition de récupération :** de94bba4-06d1-4d40-A16A-bfd50179d6ac<p>Si ce paramètre n’est pas spécifié pour un disque GPT, la commande crée une partition de données de base.</li></ul> |
| noerr | À des fins de script uniquement. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans le paramètre noerr, une erreur provoque la fermeture de DiskPart avec un code d’erreur. |

## <a name="examples"></a>Exemples

Pour créer une partition principale d’une taille de 1000 mégaoctets, tapez :

```
create partition primary size=1000
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [Assign, commande](assign.md)

- [créer une commande](create.md)

- [select disk](select-disk.md)
