---
title: secedit:validate
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: cca64f6b2904ed11f6b45e316c8e4da0093c373e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877910"
---
# <a name="seceditvalidate"></a>secedit:validate



Valide les paramètres de sécurité stockées dans un modèle de sécurité (fichier .inf). Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
Secedit /validate <configuration file name>  

```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Nom de fichier de configuration|Obligatoire.</br>Spécifie le chemin d’accès et le nom du modèle de sécurité qui sera validé.|

## <a name="remarks"></a>Notes

Validation des modèles de sécurité peut vous aider à si une est endommagée ou définir de façon inappropriée.

Un modèle de sécurité non valide ne sera pas appliqué.

Le fichier journal ne sera pas mis à jour.

Dans Windows Server 2008, `Secedit /refreshpolicy` a été remplacé par `gpupdate`. Pour plus d’informations sur l’actualisation des paramètres de sécurité, consultez [Gpupdate](gpupdate.md).

## <a name="BKMK_Examples"></a>Exemples

Après une restauration est effectuée sur un modèle de sécurité, vous souhaitez vérifier que le fichier inf de restauration, secRBKcontoso.inf, est valide.
```
Secedit /validate secRBKcontoso.inf
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Secedit:generaterollback](secedit-generaterollback.md)
-   [Secedit](secedit.md)
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)