---
title: Bitsadmin util et GETIEPROXY
description: La rubrique commandes Windows pour **Bitsadmin util et GETIEPROXY** -récupère l’utilisation du proxy pour le compte de service donné.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 6936e088ddcf467b5a8f931bc8217ba9da4662c2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380270"
---
# <a name="bitsadmin-util-and-getieproxy"></a>Bitsadmin util et GETIEPROXY

> S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Récupère l’utilisation du proxy pour le compte de service donné.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /Util /GetIEProxy <Account> [/Conn <ConnectionName>]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|-------|--------|
|Compte|Spécifie le compte de service dont vous souhaitez récupérer les paramètres de proxy. Les valeurs possibles sont les suivantes :<br /><br />-LOCALSYSTEM<br />-NETWORKSERVICE<br />-LOCALSERVICE|
|ConnectionName|Facultatif utilisé avec le paramètre **/conn** pour spécifier la connexion par modem à utiliser. Si vous ne spécifiez pas le paramètre **/conn** , bits utilise la connexion LAN. Spécifiez le nom de connexion du modem immédiatement après le paramètre **/conn** .|

## <a name="remarks"></a>Notes

Ce commutateur affiche la valeur de chaque utilisation du proxy, pas seulement l’utilisation du proxy que vous avez spécifiée pour le compte de service. Pour plus d’informations sur la définition de l’utilisation du proxy pour les comptes de service, consultez le commutateur [Bitsadmin util et SETIEPROXY](bitsadmin-util-and-setieproxy.md)

## <a name="BKMK_examples"></a>Illustre

L’exemple suivant affiche l’utilisation du proxy pour le compte SERVICE réseau.

```
C:\>bitsadmin /Util /GetIEProxy NETWORKSERVICE
```

## <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
