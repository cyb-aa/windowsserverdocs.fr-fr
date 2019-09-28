---
title: break
description: La rubrique commandes Windows pour **break_2** -dissocie un volume de clichés instantanés de VSS et le rend accessible comme un volume normal.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: a5789e3442152c705b3197bf1ce5e63dc782a15c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379776"
---
# <a name="break"></a>break



Dissocie un volume de clichés instantanés de VSS et le rend accessible comme un volume normal. Le volume est ensuite accessible à l’aide d’une lettre de lecteur (si affectée) ou d’un nom de volume. En cas d’utilisation sans paramètre, **break** affiche l’aide à l’invite de commandes.

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
|inscriptible|Active l’accès en lecture/écriture sur le volume.|
|@no__t 0SetID >|Spécifie l’ID du jeu de clichés instantanés.|

## <a name="remarks"></a>Notes

-   Les volumes exposés, comme les clichés instantanés à partir desquels ils proviennent, sont en lecture seule par défaut.
-   L’alias de l’ID de cliché instantané, qui est stocké en tant que variable d’environnement par la commande de **chargement des métadonnées** , peut être utilisé dans le paramètre *SetID* .

## <a name="BKMK_examples"></a>Illustre

Pour créer un cliché instantané avec le nom d’alias Alias1 accessible en tant que volume accessible en écriture dans le système d’exploitation, tapez :
```
break writable %Alias1%
```

> [!NOTE]
> L’accès au volume est effectué directement par le fournisseur de matériel sans enregistrement du volume qui a été un cliché instantané.

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)