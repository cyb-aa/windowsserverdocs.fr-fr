---
title: Ajouter un alias
description: Rubrique de commandes de Windows pour **ajouter l’alias** -ajoute des alias à l’environnement de l’alias.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5fe12f5d-11e9-4f3d-b7f9-40b26c8685e5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 50de932ea0153546816face61f0852a08707ea85
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862220"
---
# <a name="add-alias"></a>Ajouter un alias



Ajoute les alias à l’environnement de l’alias. Si utilisée sans paramètres, **ajouter l’alias** affiche l’aide à l’invite de commandes.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
add alias <AliasName> <AliasValue>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<AliasName>|Spécifie le nom de l’alias.|
|\<AliasValue>|Spécifie la valeur de l’alias.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Alias sont enregistrés dans le fichier de métadonnées et ne sera chargées avec les **charger les métadonnées** commande.

## <a name="BKMK_examples"></a>Exemples

Pour répertorier toutes les ombres, y compris leurs alias, tapez :
```
list shadows all
```
L’extrait suivant montre un cliché instantané pour lequel l’alias par défaut, VSS_SHADOW_x, a été affectée :
```
* Shadow Copy ID = {ff47165a-1946-4a0c-b7f4-80f46a309278}
%VSS_SHADOW_1%
```
Pour affecter un nouvel alias avec le nom System1 à cette copie de clichés instantanés, tapez :
```
add alias System1 %VSS_SHADOW_1%
```
Vous pouvez également affecter l’alias à l’aide de l’ID du cliché instantané :
```
add alias System1 {ff47165a-1946-4a0c-b7f4-80f46a309278}
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)