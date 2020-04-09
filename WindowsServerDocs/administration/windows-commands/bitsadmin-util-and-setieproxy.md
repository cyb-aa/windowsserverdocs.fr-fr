---
title: Bitsadmin util et SETIEPROXY
description: La rubrique commandes Windows pour Bitsadmin util et SETIEPROXY, qui définit les paramètres de proxy à utiliser lors du transfert de fichiers à l’aide d’un compte de service.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0e9f31ba-3070-4ffd-a94c-388c8d78f688
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4e7d8a9ff4e2388b61ee5ae00ae7afe421de68e6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848882"
---
# <a name="bitsadmin-util-and-setieproxy"></a>Bitsadmin util et SETIEPROXY

Définissez les paramètres de proxy à utiliser lors du transfert de fichiers à l’aide d’un compte de service.

**BITSAdmin 1,5 et versions antérieures**: non pris en charge.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /Util /SetIEProxy <Account> <Usage>[/Conn <ConnectionName>]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Account|Spécifie le type de compte de service dont vous souhaitez définir les paramètres de proxy. Les valeurs possibles sont :</br>-LOCALSYSTEM</br>-NETWORKSERVICE</br>-LOCALSERVICE|
|Utilisation|Spécifie la forme de détection de proxy à utiliser. Les valeurs possibles sont :</br>-NO_PROXY : n’utilisez pas de serveur proxy.</br>-DÉTECTION automatique : détecte automatiquement les paramètres du proxy.</br>-MANUAL_PROXY : utilisez une liste de proxy explicite et une liste de contournement. Spécifiez la liste de proxys et la liste de contournement immédiatement après la balise d’utilisation. Par exemple, MANUAL_PROXY Proxy1, Proxy2 NULL.</br>    -La liste de proxy est une liste délimitée par des virgules de serveurs proxy à utiliser.</br>    -La liste de contournement est une liste délimitée par des espaces de noms d’hôtes ou d’adresses IP, ou les deux, pour laquelle les transferts ne sont pas acheminés via un proxy. Cela peut être \<> local pour faire référence à tous les serveurs sur le même réseau local. Les valeurs NULL ou peuvent être utilisées pour une liste de contournement vide du proxy.</br>-Autoscript : identique à la détection automatique, sauf qu’il exécute également un script. Spécifiez l’URL de script immédiatement après la balise d’utilisation. Par exemple, Autoscript http://server/proxy.js.</br>-RESET : identique à NO_PROXY, sauf qu’elle supprime les URL de proxy manuelles (si elles sont spécifiées) et les URL découvertes à l’aide de la détection automatique.|
|ConnectionName|Facultatif : utilisé avec le paramètre **/conn** pour spécifier la connexion par modem à utiliser. Si vous ne spécifiez pas le paramètre **/conn** , bits utilise la connexion LAN. Spécifiez le nom de connexion du modem immédiatement après le paramètre **/conn** .|

## <a name="remarks"></a>Notes

Chaque appel successif à l’aide de ce commutateur remplace l’utilisation précédemment spécifiée, mais pas les paramètres de l’utilisation précédemment définie. Par exemple, si vous spécifiez NO_PROXY, détection automatique et MANUAL_PROXY sur des appels distincts, le service BITS utilise la dernière utilisation fournie, mais conserve les paramètres de l’utilisation précédemment définie.

> [!IMPORTANT]
> Vous devez exécuter cette commande à partir d’une invite de commandes avec élévation de privilèges pour qu’elle se termine correctement.

## <a name="examples"></a>Exemples

L’exemple suivant définit l’utilisation du proxy pour le compte SERVICE réseau.

```
C:\>bitsadmin /Util /SetIEProxy localsystem AUTODETECT
```

Voici d’autres exemples.

```
bitsadmin /util /setieproxy localsystem MANUAL_PROXY proxy1,proxy2,proxy3 NULL
bitsadmin /util /setieproxy localsystem MANUAL_PROXY proxy1:80 
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)