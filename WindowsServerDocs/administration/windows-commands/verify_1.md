---
title: vérifier
description: La rubrique commandes Windows pour Verify, qui indique à **cmd** s’il faut vérifier que vos fichiers sont correctement écrits sur un disque.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: dfe8bc91-d948-4e47-84ad-a79a60506ffa
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 91a0777999a604a23e2de83eda6b89c926cb241c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830052"
---
# <a name="verify"></a>vérifier



Indique à **cmd** s’il faut vérifier que vos fichiers sont correctement écrits sur un disque. S’il est utilisé sans paramètres, **verify** affiche le paramètre actuel.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
verify [on | off]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|[on \| OFF]|Active ou désactive le paramètre **verify** .|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

Pour afficher le paramètre de **vérification** en cours, tapez :
```
verify
```
Pour activer le paramètre **vérifier** , tapez :
```
Verify on
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)