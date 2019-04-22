---
title: bcdboot
description: Rubrique de commandes de Windows pour **bcdboot** - rapidement configurer une partition système, ou réparer l’environnement de démarrage situé sur la partition système.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 78838dd6567ad886948df8ac21425a8f9b596d5f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825880"
---
# <a name="bcdboot"></a>bcdboot



Vous permet de configurer rapidement une partition système, ou pour réparer l’environnement de démarrage situé sur la partition système. La partition système est configurée en copiant un ensemble simple de fichiers de données de Configuration de démarrage (BCD) sur une partition vide existante.

Pour plus d’informations sur BCDboot, y compris des informations sur où trouver BCDboot et des exemples montrant comment utiliser cette commande, consultez le [les Options de ligne de commande BCDboot](https://technet.microsoft.com/library/hh824874.aspx) rubrique.

## <a name="syntax"></a>Syntaxe

```
bcdboot <source> [/l] [/s]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|source|Spécifie l’emplacement du répertoire Windows à utiliser comme source pour la copie des fichiers d’environnement de démarrage.|
|/l|Spécifie les paramètres régionaux. Les paramètres régionaux par défaut sont anglais (États-Unis).|
|/s|Spécifie la lettre de volume de la partition système. La valeur par défaut est la partition système identifiée par le microprogramme.|

## <a name="BKMK_examples"></a>Exemples

Pour plus d’exemples montrant comment utiliser cette commande, consultez le [les Options de ligne de commande BCDboot](https://technet.microsoft.com/library/hh824874.aspx) rubrique.

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)