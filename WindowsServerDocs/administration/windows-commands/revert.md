---
title: rétablir
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 75ad40e4-502a-401e-b11e-8b31e00424b5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d7dae609e4615868b03e4b5ea9a681f553aa0667
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80835752"
---
# <a name="revert"></a>rétablir



Ramène un volume à un cliché instantané spécifié. Cela est pris en charge uniquement pour les clichés instantanés dans le contexte CLIENTACCESSIBLE. Ces clichés instantanés sont persistants et peuvent être effectués uniquement par le fournisseur système. S’il est utilisé sans paramètres, l’option **rétablir** affiche l’aide à l’invite de commandes.

## <a name="syntax"></a>Syntaxe

```
revert <ShadowCopyID>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<ShadowCopyID >|Spécifie l’ID de cliché instantané pour rétablir le volume.|

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)