---
title: Bitsadmin et setconfigurationflags
description: La rubrique commandes Windows pour Bitsadmin met en **cache et setconfigurationflags** -définit les indicateurs de configuration qui déterminent si l’ordinateur peut fournir du contenu à des pairs et peut télécharger du contenu à partir de pairs.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: a65d54bcaa2bce26eb2b7c98250837ab09c7a423
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381112"
---
# <a name="bitsadmin-peercaching-and-setconfigurationflags"></a>Bitsadmin et setconfigurationflags



Définit les indicateurs de configuration qui déterminent si l’ordinateur peut fournir du contenu à des pairs et peut télécharger du contenu à partir d’homologues.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /PeerCaching /SetConfigurationFlags <Job> <Value>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|
|Value|La valeur est un entier non signé avec l’interprétation suivante pour les bits dans la représentation binaire :</br>-Autoriser le téléchargement des données du travail à partir d’un homologue : Définir le bit le moins significatif</br>-Autoriser le traitement des données du travail aux homologues : Définissez le 2e bit à partir de la droite.|

## <a name="BKMK_examples"></a>Illustre

L’exemple suivant spécifie les données du travail à télécharger à partir des homologues pour le travail nommé *myJob*.
```
C:\> Bitsadmin /PeerCaching /SetConfigurationFlags myJob 1
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)