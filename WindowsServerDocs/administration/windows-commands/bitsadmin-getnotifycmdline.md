---
title: bitsadmin getnotifycmdline
description: Rubrique de référence pour la commande Bitsadmin getnotifycmdline, qui récupère la commande de ligne de commande qui est exécutée lorsque le travail a terminé le transfert des données.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 90fa33e6-aca5-4a23-82bd-19a9f13f8416
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4d4c47c4a1b9ea06fd804c8f2c48e9ac0ce1b319
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717797"
---
# <a name="bitsadmin-getnotifycmdline"></a>bitsadmin getnotifycmdline

Récupère la commande de ligne de commande à exécuter une fois que le travail spécifié a terminé le transfert des données.

> [!NOTE]
> Cette commande n’est pas prise en charge par BITS 1,2 et versions antérieures.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /getnotifycmdline <job>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| travail | Nom complet ou GUID du travail. |

## <a name="examples"></a>Exemples

Pour récupérer la commande de ligne de commande utilisée par le service quand la tâche nommée *myDownloadJob* se termine.

```
bitsadmin /getnotifycmdline myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin](bitsadmin.md)
