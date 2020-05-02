---
title: dispdiag
description: Rubrique de référence pour dispdiag, qui enregistre des informations dans un fichier.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5079e1dd-b57c-44ed-970f-e6b409369e03
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f9f44e261b9c46157fb3e6bb7f9105af2480a60b
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719408"
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
|-délai \<en secondes>|Retarde la collecte de données selon la durée spécifiée en *secondes*.|
|-out \<FilePath>|Spécifie le chemin d’accès et le nom de fichier pour enregistrer les données collectées. Il doit s’agir du dernier paramètre.|
|-?|Affiche les paramètres de commande disponibles et fournit de l’aide pour les utiliser.|