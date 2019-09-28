---
title: bcdboot
description: 'Rubrique relative aux commandes Windows pour **BCDboot** : configurer rapidement une partition système ou réparer l’environnement de démarrage situé sur la partition système.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e62a250e-08e9-47f6-89d1-e6002560ab57
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a1c0f505180a503617335cc9575fea3d346bbe02
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383441"
---
# <a name="bcdboot"></a>bcdboot



Vous permet de configurer rapidement une partition système ou de réparer l’environnement de démarrage situé sur la partition système. La partition système est configurée en copiant un ensemble simple de fichiers Données de configuration de démarrage (BCD) (BCD) dans une partition vide existante.

Pour plus d’informations sur BCDboot, y compris sur l’emplacement de BCDboot et des exemples d’utilisation de cette commande, consultez la rubrique [options de ligne de commande BCDboot](https://technet.microsoft.com/library/hh824874.aspx) .

## <a name="syntax"></a>Syntaxe

```
bcdboot <source> [/l] [/s]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|source|Spécifie l’emplacement du répertoire Windows à utiliser comme source pour la copie des fichiers de l’environnement de démarrage.|
|/l|Spécifie les paramètres régionaux. Les paramètres régionaux par défaut sont anglais (États-Unis).|
|/s|Spécifie la lettre de volume de la partition système. La valeur par défaut est la partition système identifiée par le microprogramme.|

## <a name="BKMK_examples"></a>Illustre

Pour obtenir plus d’exemples sur l’utilisation de cette commande, consultez la rubrique [options de ligne de commande BCDboot](https://technet.microsoft.com/library/hh824874.aspx) .

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)