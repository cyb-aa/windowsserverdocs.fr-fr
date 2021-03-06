---
title: bitsadmin util and setieproxy
description: Rubrique de référence pour la commande Bitsadmin util et SETIEPROXY, qui définit les paramètres de proxy à utiliser lors du transfert de fichiers à l’aide d’un compte de service.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0e9f31ba-3070-4ffd-a94c-388c8d78f688
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f45ca0a1a27aaf41fc55a82bcbcd3019e73ec6c8
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82707672"
---
# <a name="bitsadmin-util-and-setieproxy"></a>bitsadmin util and setieproxy

Définissez les paramètres de proxy à utiliser lors du transfert de fichiers à l’aide d’un compte de service. Vous devez exécuter cette commande à partir d’une invite de commandes avec élévation de privilèges pour qu’elle se termine correctement.

> [!NOTE]
> Cette commande n’est pas prise en charge par BITS 1,5 et versions antérieures.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /util /setieproxy <account> <usage> [/conn <connectionname>]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ---------- |
| account | Spécifie le compte de service dont vous souhaitez définir les paramètres de proxy. Les valeurs possibles incluent :<ul><li>LOCALSYSTEM</li><li>   NETWORKSERVICE</li><li>Local.</li></ul> |
| usage | Spécifie la forme de détection de proxy à utiliser. Les valeurs possibles incluent :<ul><li>**NO_PROXY.** N’utilisez pas de serveur proxy.</li><li>**Détection automatique.** Détectez automatiquement les paramètres du proxy.</li><li>**MANUAL_PROXY.** Utilisez une liste de proxys et une liste de contournement spécifiés. Vous devez spécifier vos listes immédiatement après la balise d’utilisation. Par exemple : `MANUAL_PROXY proxy1,proxy2 NULL`.<ul><li>**Liste de proxys.** Liste délimitée par des virgules de serveurs proxy à utiliser.</li><li>**Liste de contournement.** Liste délimitée par des espaces de noms d’hôtes ou d’adresses IP, ou les deux, pour lesquels les transferts ne doivent pas être routés via un proxy. Il peut s' \<agir d’un> local pour faire référence à tous les serveurs sur le même réseau local. Les valeurs NULL ou peuvent être utilisées pour une liste de contournement vide du proxy.</li></ul><li>**Autoscript.** Identique à la **détection**automatique, sauf qu’elle exécute également un script. Vous devez spécifier l’URL du script immédiatement après la balise d’utilisation. Par exemple : `AUTOSCRIPT http://server/proxy.js`.</li><li>**Initialisation.** Identique à **NO_PROXY**, sauf qu’elle supprime les URL de proxy manuelles (si elles sont spécifiées) et toutes les URL découvertes à l’aide de la détection automatique.</li></ul> |
| ConnectionName | facultatif. Utilisé avec le paramètre **/conn** pour spécifier la connexion par modem à utiliser. Si vous ne spécifiez pas le paramètre **/conn** , bits utilise la connexion LAN. |

### <a name="remarks"></a>Notes 

Chaque appel successif à l’aide de ce commutateur remplace l’utilisation précédemment spécifiée, mais pas les paramètres de l’utilisation précédemment définie. Par exemple, si vous spécifiez **NO_PROXY**, **détection**automatique et **MANUAL_PROXY** sur des appels distincts, bits utilise la dernière utilisation fournie, mais conserve les paramètres de l’utilisation précédemment définie.

## <a name="examples"></a>Exemples

Pour définir l’utilisation du proxy pour le compte LOCALSYSTEM :

```
bitsadmin /util /setieproxy localsystem AUTODETECT
```

```
bitsadmin /util /setieproxy localsystem MANUAL_PROXY proxy1,proxy2,proxy3 NULL
```

```
bitsadmin /util /setieproxy localsystem MANUAL_PROXY proxy1:80
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin util](bitsadmin-util.md)

- [commande Bitsadmin](bitsadmin.md)
