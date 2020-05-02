---
title: bitsadmin peercaching and setconfigurationflags
description: Rubrique de référence pour la commande Bitsadmin et setconfigurationflags, qui définit les indicateurs de configuration qui déterminent si l’ordinateur peut fournir du contenu à des homologues et s’il peut télécharger du contenu à partir de pairs.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ff0a2b49-66e3-4d40-824c-6a3816055d2e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3c3ce69ce7a372311ce0c30e9b3a391ea33f45ce
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717237"
---
# <a name="bitsadmin-peercaching-and-setconfigurationflags"></a>bitsadmin peercaching and setconfigurationflags

Définit les indicateurs de configuration qui déterminent si l’ordinateur peut fournir du contenu à des pairs et s’il peut télécharger du contenu à partir de pairs.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /peercaching /setconfigurationflags <job> <value>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| travail | Nom complet ou GUID du travail. |
| value | Entier non signé avec l’interprétation suivante pour les bits dans la représentation binaire :<ul><li>Pour autoriser le téléchargement des données du travail à partir d’un homologue, définissez le bit le moins significatif.</li><li>Pour permettre aux données du travail d’être fournies aux homologues, définissez le deuxième bit à partir de la droite.</li></ul>|

## <a name="examples"></a>Exemples

Pour spécifier les données du travail à télécharger à partir des homologues pour le travail nommé *myDownloadJob*:

```
bitsadmin /peercaching /setconfigurationflags myDownloadJob 1
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin](bitsadmin.md)

- [commande Bitsadmin de la surcache](bitsadmin-peercaching.md)
