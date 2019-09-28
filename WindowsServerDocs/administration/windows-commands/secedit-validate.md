---
title: 'secedit : valider'
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9fb06354-f55a-4ca4-9fbc-9a872eb9b9cf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ece0a0324b77eb4226b679bc29f7bd599f15a120
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371100"
---
# <a name="seceditvalidate"></a>secedit : valider



Valide les paramètres de sécurité stockés dans un modèle de sécurité (fichier. inf). Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
Secedit /validate <configuration file name>  

```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Nom du fichier de configuration|Obligatoire.</br>Spécifie le chemin d’accès et le nom de fichier pour le modèle de sécurité qui sera validé.|

## <a name="remarks"></a>Notes

La validation des modèles de sécurité peut vous aider si l’une d’elles est endommagée ou définie de manière inappropriée.

Un modèle de sécurité non valide ne sera pas appliqué.

Le fichier journal ne sera pas mis à jour.

Dans Windows Server 2008, `Secedit /refreshpolicy` a été remplacé par `gpupdate`. Pour plus d’informations sur la façon d’actualiser les paramètres de sécurité, consultez [gpupdate](gpupdate.md).

## <a name="BKMK_Examples"></a>Illustre

Une fois la restauration effectuée sur un modèle de sécurité, vous devez vérifier que le fichier INF de restauration, secRBKcontoso. inf, est valide.
```
Secedit /validate secRBKcontoso.inf
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Secedit:generaterollback](secedit-generaterollback.md)
-   [Secedit](secedit.md)
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)