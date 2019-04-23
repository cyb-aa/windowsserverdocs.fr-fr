---
title: break
description: Rubrique de commandes de Windows pour **break_2** - dissocie un volume de clichés instantanés de VSS et le rend accessible comme un volume normal.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: de2b6c95-1c2e-4a43-bec5-341a9014371b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 49516539ae603e2c93b3fc395c77786be790d663
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886330"
---
# <a name="break"></a>break



Dissocie un volume de clichés instantanés de VSS et le rend accessible comme un volume normal. Le volume est ensuite accessible pour les à l’aide d’une lettre de lecteur (s’il est attribué) ou le nom de volume. Si utilisée sans paramètres, **saut** affiche l’aide à l’invite de commandes.

> [!NOTE]
> Cette commande s’applique uniquement aux clichés instantanés matériels après l’importation.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
break [writable] <SetID>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|[writable]|Active en lecture/écriture sur le volume.|
|\<SetID>|Spécifie l’ID de jeu de clichés instantanés.|

## <a name="remarks"></a>Notes

-   Les volumes exposés, tels que les clichés instantanés ils proviennent, sont en lecture seule par défaut.
-   L’alias de l’ID du cliché instantané, qui est stocké sous la forme d’une variable d’environnement par le **charger les métadonnées** commande, peut être utilisé dans le *SetID* paramètre.

## <a name="BKMK_examples"></a>Exemples

Pour créer une ombre à copier avec le nom d’alias Alias1 accessible comme un volume accessible en écriture dans le système d’exploitation, tapez :
```
break writable %Alias1%
```

> [!NOTE]
> Accès au volume est effectué directement le fournisseur de matériel sans enregistrement du volume ayant été un cliché instantané.

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)