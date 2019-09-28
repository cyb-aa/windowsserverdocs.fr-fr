---
title: 'Ksetup : serveur'
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: dd05fd294640c63e633b7b866307197ae6770476
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374956"
---
# <a name="ksetupserver"></a>Ksetup : serveur



Vous permet de spécifier un nom pour un ordinateur exécutant le système d’exploitation Windows afin que les modifications apportées à l’aide de **Ksetup** mettent à jour l’ordinateur cible. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
ksetup /server <ServerName>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|@no__t 0ServerName >|Nom complet de l’ordinateur sur lequel la configuration sera effective, par exemple IPops897.corp.contoso.com.</br>Si un nom d’ordinateur de domaine complet incomplet est spécifié, la commande échoue.|

## <a name="remarks"></a>Notes

Il n’existe aucun moyen de supprimer le nom du serveur ciblé ; vous pouvez uniquement revenir au nom de l’ordinateur local, qui est la valeur par défaut.

Le nom du serveur cible est stocké dans le registre dans **HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Control\LSA\Kerberos**. Elle n’est pas signalée à l’aide de **Ksetup**.

## <a name="BKMK_Examples"></a>Illustre

Faites en sorte que vos configurations **Ksetup** soient effectives sur l’ordinateur IPops897 connecté au domaine contoso :
```
ksetup /server IPops897.corp.contoso.com
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Ksetup](ksetup.md)
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)