---
title: Ajouter un alias
description: Rubrique relative aux commandes Windows pour **Ajouter un alias**, qui ajoute des alias à l’environnement d’alias.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5fe12f5d-11e9-4f3d-b7f9-40b26c8685e5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ebffc1504f502711dab30f6f9b120ad20e64ae9d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851362"
---
# <a name="add-alias"></a>Ajouter un alias

Ajoute des alias à l’environnement d’alias. S’il est utilisé sans paramètres, l’option **Ajouter un alias** affiche l’aide à l’invite de commandes.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
add alias <AliasName> <AliasValue>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|`<AliasName>`|Indique le nom de l'alias.|
|`<AliasValue>`|Spécifie la valeur de l’alias.|
|`/?`|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Les alias sont enregistrés dans le fichier de métadonnées et sont chargés à l’aide de la commande **charger les métadonnées** .

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

Pour répertorier toutes les ombres, y compris leurs alias, tapez :

```
list shadows all
```

L’extrait suivant montre un cliché instantané auquel l’alias par défaut, VSS_SHADOW_x, a été affecté :

```
* Shadow Copy ID = {ff47165a-1946-4a0c-b7f4-80f46a309278}
%VSS_SHADOW_1%
```

Pour affecter un nouvel alias portant le nom system1 à ce cliché instantané, tapez :

```
add alias System1 %VSS_SHADOW_1%
```

Vous pouvez également attribuer l’alias à l’aide de l’ID de cliché instantané :

```
add alias System1 {ff47165a-1946-4a0c-b7f4-80f46a309278}
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)