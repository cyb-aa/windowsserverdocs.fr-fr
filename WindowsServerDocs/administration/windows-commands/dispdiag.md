---
title: dispdiag
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5079e1dd-b57c-44ed-970f-e6b409369e03
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9c96c70aac1b3329e050fa8b02743e61fed44d15
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831460"
---
# <a name="dispdiag"></a>dispdiag



Journaux affichent des informations dans un fichier.

## <a name="syntax"></a>Syntaxe

```
dispdiag [-testacpi] [-d] [-delay <Seconds>] [-out <FilePath>]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|- testacpi|Exécute le test de diagnostic de touche d’accès rapide. Affiche le nom de clé, code et analyse de code pour n’importe quelle touche enfoncée pendant le test.|
|-d|Génère un fichier dump avec les résultats des tests.|
|-délai \<secondes >|Retarde la collecte de données par heure spécifiée dans *secondes*.|
|-out \<FilePath >|Spécifie le chemin d’accès et nom de fichier pour enregistrer les données collectées. Cela doit être le dernier paramètre.|
|-?|Affiche les paramètres de commande disponibles et fournit une aide pour leur utilisation.|