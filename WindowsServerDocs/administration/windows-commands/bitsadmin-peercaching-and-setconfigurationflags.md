---
title: setconfigurationflags et mise en cache bitsadmin
description: Rubrique de commandes de Windows pour **mise en cache bitsadmin et setconfigurationflags** -définit les indicateurs de configuration qui déterminent si l’ordinateur peut servir du contenu des homologues et qu’il peut télécharger du contenu à partir d’homologues.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ff0a2b49-66e3-4d40-824c-6a3816055d2e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 22408d4aab7f5ea374511bc16751d911a84644f2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813330"
---
# <a name="bitsadmin-peercaching-and-setconfigurationflags"></a>setconfigurationflags et mise en cache bitsadmin



Définit les indicateurs de configuration qui déterminent si l’ordinateur peut servir du contenu des homologues et qu’il peut télécharger du contenu à partir d’homologues.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /PeerCaching /SetConfigurationFlags <Job> <Value>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom d’affichage ou le GUID du travail|
|Value|La valeur est un entier non signé avec l’interprétation suivante des bits dans la représentation binaire :</br>-Autoriser les données du travail à télécharger à partir d’un homologue : Définir le bit le moins significatif.</br>-Autoriser les données de la tâche à servir à homologues : Définir le bit 2nd à partir de la droite.|

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant spécifie les données de la tâche à être téléchargé à partir d’homologues pour le travail nommé *myJob*.
```
C:\> Bitsadmin /PeerCaching /SetConfigurationFlags myJob 1
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)