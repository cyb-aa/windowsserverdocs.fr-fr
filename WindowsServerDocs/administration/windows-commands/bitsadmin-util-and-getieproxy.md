---
title: GETIEPROXY et bitsadmin util
description: Rubrique de commandes de Windows pour **bitsadmin util et getieproxy** -récupère l’utilisation du proxy pour le compte de service donné.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6d50c7e3-f4eb-4ca5-9f0c-4ed396087db6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: de4f86340b1163c4d8e3286d9c86c9df794a21c5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876870"
---
# <a name="bitsadmin-util-and-getieproxy"></a>GETIEPROXY et bitsadmin util

> S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Récupère l’utilisation du proxy pour le compte de service donné.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /Util /GetIEProxy <Account> [/Conn <ConnectionName>]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|-------|--------|
|Compte|Spécifie le compte de service dont vous souhaitez récupérer les paramètres de proxy. Les valeurs possibles sont :<br /><br />-LOCALSYSTEM<br />-NETWORKSERVICE<br />-LOCALSERVICE|
|ConnectionName|Facultatif utilisé avec le **/Conn** paramètre pour spécifier la connexion modem à utiliser. Si vous ne spécifiez pas le **/Conn** paramètre, BITS utilise la connexion de réseau local. Spécifiez le nom de connexion modem qui suit immédiatement la **/Conn** paramètre.|

## <a name="remarks"></a>Notes

Ce commutateur affiche la valeur pour chaque utilisation de proxy, pas seulement l’utilisation du proxy vous avez spécifié pour le compte de service. Pour plus d’informations sur la définition de l’utilisation de proxy pour les comptes de service, consultez le [bitsadmin util et setieproxy](bitsadmin-util-and-setieproxy.md) basculer.

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant affiche l’utilisation du proxy pour le compte SERVICE réseau.

```
C:\>bitsadmin /Util /GetIEProxy NETWORKSERVICE
```

## <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
