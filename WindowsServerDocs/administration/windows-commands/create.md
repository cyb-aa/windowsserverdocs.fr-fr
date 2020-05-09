---
title: create
description: Rubrique de référence pour la commande Create, qui crée une partition ou une partition fictive sur un disque, un volume sur un ou plusieurs disques ou un disque dur virtuel (VHD).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b45acde1-8f4f-4ec3-b905-d8188f884af8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f371bee591e29a45b1488b2c36112b55ed54d3f8
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2020
ms.locfileid: "82993191"
---
# <a name="create"></a>create

Crée une partition ou une ombre sur un disque, un volume sur un ou plusieurs disques ou un disque dur virtuel (VHD). Si vous utilisez cette commande pour créer un volume sur le disque miroir, vous devez déjà avoir au moins un volume dans le jeu de clichés instantanés.

## <a name="syntax"></a>Syntaxe

```
create partition
create volume
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| [commande CREATE partition Primary](create-partition-primary.md) | Crée une partition principale sur le disque de base avec le focus. |
| [commande CREATE partition EFI](create-partition-efi.md) | Crée une partition système Extensible Firmware Interface (EFI) sur un disque GPT (GUID partition table) sur des ordinateurs Itanium. |
| [créer une partition étendue, commande](create-partition-extended.md) | Crée une partition étendue sur le disque qui a le focus. |
| [créer une partition logique, commande](create-partition-logical.md) | Crée une partition logique dans une partition étendue existante. |
| [commande CREATE partition MSR](create-partition-msr.md) | Crée une partition MSR (Microsoft Reserved) sur un disque GPT (GUID partition table). |
| [commande CREATE volume simple](create-volume-simple.md) | Crée un volume simple sur le disque dynamique spécifié. |
| [commande Create Volume Mirror](create-volume-mirror.md) | Crée un miroir de volume à l’aide des deux disques dynamiques spécifiés. |
| [commande CREATE volume RAID](create-volume-raid.md) | Crée un volume RAID-5 à l’aide de trois disques dynamiques spécifiés ou plus. |
| [commande Create Volume Stripe](create-volume-stripe.md) | Crée un volume agrégé par bandes à l’aide de deux disques dynamiques spécifiés ou plus. |

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
