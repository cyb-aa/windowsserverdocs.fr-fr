---
title: bitsadmin getnotifycmdline
description: La rubrique commandes Windows pour **Bitsadmin getnotifycmdline**, qui récupère la commande de ligne de commande qui est exécutée lorsque le travail a terminé le transfert des données.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 90fa33e6-aca5-4a23-82bd-19a9f13f8416
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 24b49b3fa0c2dafb999d8cb9c6e0c13ae68bf6f4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850592"
---
# <a name="bitsadmin-getnotifycmdline"></a>bitsadmin getnotifycmdline

Récupère la commande de ligne de commande à exécuter lorsque le travail a terminé le transfert des données.

> [!NOTE]
> Cette commande n’est pas prise en charge par BITS 1,2 et versions antérieures.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /getnotifycmdline <job>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| le travail | Nom complet ou GUID du travail. |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant récupère la commande de ligne de commande utilisée par le service quand la tâche nommée *myDownloadJob* se termine.

```
C:\>bitsadmin /getnotifycmdline myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)