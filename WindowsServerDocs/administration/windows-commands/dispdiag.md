---
title: dispdiag
description: La rubrique commandes Windows pour dispdiag, qui enregistre les informations dans un fichier.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5079e1dd-b57c-44ed-970f-e6b409369e03
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 498294aa6678cc4904880128bca55d7900c91db5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80845382"
---
# <a name="dispdiag"></a>dispdiag

Journalise les informations d’affichage dans un fichier.

## <a name="syntax"></a>Syntaxe

```
dispdiag [-testacpi] [-d] [-delay <Seconds>] [-out <FilePath>]
```

#### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|- testacpi|Exécute le test de diagnostics du raccourci. Affiche le nom de la clé, le code et le code d’analyse pour toute touche enfoncée pendant le test.|
|-d|Génère un fichier dump avec les résultats des tests.|
|-délai \<secondes >|Retarde la collecte de données selon la durée spécifiée en *secondes*.|
|-out \<FilePath >|Spécifie le chemin d’accès et le nom de fichier pour enregistrer les données collectées. Il doit s’agir du dernier paramètre.|
|-?|Affiche les paramètres de commande disponibles et fournit de l’aide pour les utiliser.|