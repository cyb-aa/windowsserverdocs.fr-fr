---
title: maintien
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: eeab0aef-2ba5-441a-a10d-bbef6f0d7e3e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 891192c0d6b1687cf6bffea0f57d1853e023b776
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80835762"
---
# <a name="retain"></a>maintien



Prépare un volume simple dynamique existant à utiliser comme volume de démarrage ou de système.

## <a name="syntax"></a>Syntaxe

```
retain
```

## <a name="remarks"></a>Notes

-   Sur un disque dynamique d’enregistrement de démarrage principal (MBR), cette commande crée une entrée de partition dans l’enregistrement de démarrage principal.
-   Sur un disque dynamique de table de partition GUID (GPT), cette commande crée une entrée de partition dans la table de partition GUID.

## <a name="additional-references"></a>Références supplémentaires

