---
title: 'Ksetup : serveur'
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e3407111-ac92-457f-aa1f-a04fe9109d59
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 91549eb78f825264016ec0e03b7035f79132f260
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724593"
---
# <a name="ksetupserver"></a>Ksetup : serveur



Vous permet de spécifier un nom pour un ordinateur exécutant le système d’exploitation Windows afin que les modifications apportées à l’aide de **Ksetup** mettent à jour l’ordinateur cible.

## <a name="syntax"></a>Syntaxe

```
ksetup /server <ServerName>
```

#### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<Nom du serveur>|Nom complet de l’ordinateur sur lequel la configuration sera effective, par exemple IPops897.corp.contoso.com.</br>Si un nom d’ordinateur de domaine complet incomplet est spécifié, la commande échoue.|

## <a name="remarks"></a>Notes 

Il n’existe aucun moyen de supprimer le nom du serveur ciblé ; vous pouvez uniquement revenir au nom de l’ordinateur local, qui est la valeur par défaut.

Le nom du serveur cible est stocké dans le registre dans **HKEY_LOCAL_MACHINE \system\controlset001\control\lsa\kerberos**. Elle n’est pas signalée à l’aide de **Ksetup**.

## <a name="examples"></a>Exemples

Faites en sorte que vos configurations **Ksetup** soient effectives sur l’ordinateur IPops897 connecté au domaine contoso :
```
ksetup /server IPops897.corp.contoso.com
```

## <a name="additional-references"></a>Références supplémentaires

-   [Ksetup](ksetup.md)
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)