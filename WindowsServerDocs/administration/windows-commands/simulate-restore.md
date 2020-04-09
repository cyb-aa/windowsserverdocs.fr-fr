---
title: Simuler la restauration
description: Rubrique commandes Windows pour simuler la restauration, qui teste l’implication de l’enregistreur dans les sessions de restauration sur l’ordinateur sans émettre des événements prerestauration ou PostRestore aux rédacteurs.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d883d94c-3cb1-4848-9d74-1b4378044b31
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 024d654864c000e44bccb9ddb167c6147444cc00
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834102"
---
# <a name="simulate-restore"></a>Simuler la restauration

Teste l’implication de l’enregistreur dans les sessions de restauration sur l’ordinateur sans émettre des événements **prerestauration** ou **postRestore** aux rédacteurs.

## <a name="syntax"></a>Syntaxe

```
simulate restore
```

## <a name="remarks"></a>Notes

-   **Simuler Restore** permet de déterminer si la restauration avec des enregistreurs peut aboutir.
-   Avant de pouvoir utiliser **simuler la restauration**, vous devez charger un fichier de métadonnées DiskShadow à l’aide de la commande **charger les métadonnées** . Cela charge les enregistreurs et les composants sélectionnés pour la restauration.

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)