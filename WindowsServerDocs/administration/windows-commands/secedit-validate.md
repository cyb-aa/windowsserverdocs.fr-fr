---
title: 'secedit : valider'
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9fb06354-f55a-4ca4-9fbc-9a872eb9b9cf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3043a4af6c2ac4a6c58b973cca5abd066109eac5
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722037"
---
# <a name="seceditvalidate"></a>secedit : valider



Valide les paramètres de sécurité stockés dans un modèle de sécurité (fichier. inf).

## <a name="syntax"></a>Syntaxe

```
Secedit /validate <configuration file name>  

```

#### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Nom du fichier de configuration|Obligatoire.</br>Spécifie le chemin d’accès et le nom de fichier pour le modèle de sécurité qui sera validé.|

## <a name="remarks"></a>Notes 

La validation des modèles de sécurité peut vous aider si l’une d’elles est endommagée ou définie de manière inappropriée.

Un modèle de sécurité non valide ne sera pas appliqué.

Le fichier journal ne sera pas mis à jour.

Dans Windows Server 2008, `Secedit /refreshpolicy` a été remplacé par `gpupdate`. Pour plus d’informations sur la façon d’actualiser les paramètres de sécurité, consultez [gpupdate](gpupdate.md).

## <a name="examples"></a>Exemples

Une fois la restauration effectuée sur un modèle de sécurité, vous devez vérifier que le fichier INF de restauration, secRBKcontoso. inf, est valide.
```
Secedit /validate secRBKcontoso.inf
```

## <a name="additional-references"></a>Références supplémentaires

-   [Secedit:generaterollback](secedit-generaterollback.md)
-   [Secedit](secedit.md)
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)