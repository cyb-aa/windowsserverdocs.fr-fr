---
title: Bitsadmin et setconfigurationflags
description: La rubrique commandes Windows pour Bitsadmin met en **cache** et **setconfigurationflags**, qui définit les indicateurs de configuration qui déterminent si l’ordinateur peut fournir du contenu à des pairs et s’il peut télécharger du contenu à partir de pairs.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ff0a2b49-66e3-4d40-824c-6a3816055d2e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ebaa09da2d4594d2762e67dc5884dd15cf4d1da8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850132"
---
# <a name="bitsadmin-peercaching-and-setconfigurationflags"></a>Bitsadmin et setconfigurationflags

Définit les indicateurs de configuration qui déterminent si l’ordinateur peut fournir du contenu à des pairs et s’il peut télécharger du contenu à partir de pairs.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /peercaching /setconfigurationflags <job> <value>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| le travail | Nom complet ou GUID du travail. |
| valeur | Entier non signé avec l’interprétation suivante pour les bits dans la représentation binaire :<ul><li> Pour autoriser le téléchargement des données du travail à partir d’un homologue, définissez le bit le moins significatif.</li><li>Pour permettre aux données du travail d’être fournies aux homologues, définissez le deuxième bit à partir de la droite.</li></ul>|

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant spécifie les données du travail à télécharger à partir des homologues pour le travail nommé *myDownloadJob*.

```
C:\> bitsadmin /peercaching /setconfigurationflags myDownloadJob 1
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)