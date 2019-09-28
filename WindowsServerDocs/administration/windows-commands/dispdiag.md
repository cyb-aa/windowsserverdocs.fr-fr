---
title: dispdiag
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 9b640883a207648d2ef6c9a7d6e5366cd0bb384c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377758"
---
# <a name="dispdiag"></a>dispdiag



Journalise les informations d’affichage dans un fichier.

## <a name="syntax"></a>Syntaxe

```
dispdiag [-testacpi] [-d] [-delay <Seconds>] [-out <FilePath>]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|- testacpi|Exécute le test de diagnostics du raccourci. Affiche le nom de la clé, le code et le code d’analyse pour toute touche enfoncée pendant le test.|
|-d|Génère un fichier dump avec les résultats des tests.|
|-Delay \<Seconds >|Retarde la collecte de données selon la durée spécifiée en *secondes*.|
|-out \<FilePath >|Spécifie le chemin d’accès et le nom de fichier pour enregistrer les données collectées. Il doit s’agir du dernier paramètre.|
|-?|Affiche les paramètres de commande disponibles et fournit de l’aide pour les utiliser.|