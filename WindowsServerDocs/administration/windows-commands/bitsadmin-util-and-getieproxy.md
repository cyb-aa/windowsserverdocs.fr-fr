---
title: Bitsadmin util et GETIEPROXY
description: La rubrique commandes Windows pour Bitsadmin util et GETIEPROXY, qui récupère l’utilisation du proxy pour le compte de service donné.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6d50c7e3-f4eb-4ca5-9f0c-4ed396087db6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d0d2a8634f3b42d655632a280cb9b998111c800b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848972"
---
# <a name="bitsadmin-util-and-getieproxy"></a>Bitsadmin util et GETIEPROXY

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Récupère l’utilisation du proxy pour le compte de service donné.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /Util /GetIEProxy <Account> [/Conn <ConnectionName>]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|-------|--------|
|Account|Spécifie le compte de service dont vous souhaitez récupérer les paramètres de proxy. Les valeurs possibles sont :<p>-LOCALSYSTEM<br />-NETWORKSERVICE<br />-LOCALSERVICE|
|ConnectionName|Facultatif utilisé avec le paramètre **/conn** pour spécifier la connexion par modem à utiliser. Si vous ne spécifiez pas le paramètre **/conn** , bits utilise la connexion LAN. Spécifiez le nom de connexion du modem immédiatement après le paramètre **/conn** .|

## <a name="remarks"></a>Notes

Ce commutateur affiche la valeur de chaque utilisation du proxy, pas seulement l’utilisation du proxy que vous avez spécifiée pour le compte de service. Pour plus d’informations sur la définition de l’utilisation du proxy pour les comptes de service, consultez le commutateur [Bitsadmin util et SETIEPROXY](bitsadmin-util-and-setieproxy.md)

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant affiche l’utilisation du proxy pour le compte SERVICE réseau.

```
C:\>bitsadmin /Util /GetIEProxy NETWORKSERVICE
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
