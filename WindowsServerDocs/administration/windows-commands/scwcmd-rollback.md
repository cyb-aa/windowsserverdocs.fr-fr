---
title: Scwcmd rollback
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4fd9f89b-0420-420a-ad20-4a328636b1e7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6d6cd79c7068d86915141a37b5a4510bddefc94c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852200"
---
# <a name="scwcmd-rollback"></a>Scwcmd: rollback

> S'applique à : Windows Server 2012 R2, Windows Server 2012

Applique la stratégie de restauration le plus récent et supprime cette stratégie de restauration.

## <a name="syntax"></a>Syntaxe

```
scwcmd rollback /m:<ComputerName> [/u:<UserName>] [/pw:<Password>]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/ m:\<nom_ordinateur >|Spécifie le nom NetBIOS, le nom DNS ou l’adresse IP d’un ordinateur où l’opération de restauration doit être effectuée.|
|/ u:\<nom d’utilisateur >|Spécifie un autre compte d’utilisateur à utiliser lorsque vous effectuez une restauration à distance. La valeur par défaut est connecté sur l’utilisateur.|
|/ pw :\<mot de passe >|Spécifie les informations d’identification utilisateur à utiliser lorsque vous effectuez une restauration à distance. La valeur par défaut est connecté sur l’utilisateur.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

Scwcmd.exe est uniquement disponible sur les ordinateurs exécutant Windows Server 2008 R2, Windows Server 2008 ou Windows Server 2003.

## <a name="BKMK_Examples"></a>Exemples

Pour restaurer la stratégie de sécurité sur un ordinateur à l’adresse IP 172.16.0.0, tapez :
```
scwcmd rollback /m:172.16.0.0
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)