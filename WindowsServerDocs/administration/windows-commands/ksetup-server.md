---
title: 'Ksetup : serveur'
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e3407111-ac92-457f-aa1f-a04fe9109d59
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7889e1a03d3c0eec1958bf1d6356c67e9371a80f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841442"
---
# <a name="ksetupserver"></a>Ksetup : serveur



Vous permet de spécifier un nom pour un ordinateur exécutant le système d’exploitation Windows afin que les modifications apportées à l’aide de **Ksetup** mettent à jour l’ordinateur cible. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
ksetup /server <ServerName>
```

#### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<ServerName >|Nom complet de l’ordinateur sur lequel la configuration sera effective, par exemple IPops897.corp.contoso.com.</br>Si un nom d’ordinateur de domaine complet incomplet est spécifié, la commande échoue.|

## <a name="remarks"></a>Notes

Il n’existe aucun moyen de supprimer le nom du serveur ciblé ; vous pouvez uniquement revenir au nom de l’ordinateur local, qui est la valeur par défaut.

Le nom du serveur cible est stocké dans le registre dans **HKEY_LOCAL_MACHINE \system\controlset001\control\lsa\kerberos**. Elle n’est pas signalée à l’aide de **Ksetup**.

## <a name="examples"></a><a name=BKMK_Examples></a>Illustre

Faites en sorte que vos configurations **Ksetup** soient effectives sur l’ordinateur IPops897 connecté au domaine contoso :
```
ksetup /server IPops897.corp.contoso.com
```

## <a name="additional-references"></a>Références supplémentaires

-   [Ksetup](ksetup.md)
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)