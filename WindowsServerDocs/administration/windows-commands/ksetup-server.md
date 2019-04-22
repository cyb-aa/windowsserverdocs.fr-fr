---
title: ksetup:server
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e3407111-ac92-457f-aa1f-a04fe9109d59
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f370d4dede278e1facdda829503ea3793502b9e6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814570"
---
# <a name="ksetupserver"></a>ksetup:server



Vous permet de spécifier un nom pour un ordinateur exécutant le système d’exploitation Windows afin que les modifications apportées à l’aide de **ksetup** met à jour l’ordinateur cible. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
ksetup /server <ServerName>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<ServerName>|Le nom complet de l’ordinateur sur lequel la configuration entreront en vigueur, telles que IPops897.corp.contoso.com.</br>Si complets incomplet nom d’ordinateur de domaine est spécifié, la commande échoue.|

## <a name="remarks"></a>Notes

Il n’existe aucun moyen pour supprimer le nom de serveur ciblé ; Vous pouvez ne modifier qu’au nom de l’ordinateur local, ce qui est la valeur par défaut.

Le nom du serveur cible est stocké dans le Registre **HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Control\LSA\Kerberos**. Il n’est pas signalé à l’aide de **ksetup**.

## <a name="BKMK_Examples"></a>Exemples

Vérifiez votre **ksetup** configurations effectives sur l’ordinateur IPops897 qui est connecté sur le domaine Contoso :
```
ksetup /server IPops897.corp.contoso.com
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Ksetup](ksetup.md)
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)