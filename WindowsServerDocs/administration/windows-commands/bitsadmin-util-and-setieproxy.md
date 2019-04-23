---
title: SETIEPROXY et bitsadmin util
description: Rubrique de commandes de Windows pour **bitsadmin util et setieproxy** -définir les paramètres de proxy à utiliser lors du transfert de fichiers à l’aide d’un compte de service.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0e9f31ba-3070-4ffd-a94c-388c8d78f688
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d4f6ab2e52284895d2e7918364c24bbb69f2b1c9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853510"
---
# <a name="bitsadmin-util-and-setieproxy"></a>SETIEPROXY et bitsadmin util

Définir les paramètres de proxy à utiliser lors du transfert de fichiers à l’aide d’un compte de service.

**BITSAdmin 1.5 et versions antérieur**: Non pris en charge.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /Util /SetIEProxy <Account> <Usage>[/Conn <ConnectionName>]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Compte|Spécifie le type de compte de service dont vous souhaitez définir les paramètres de proxy. Les valeurs possibles sont :</br>-LOCALSYSTEM</br>-NETWORKSERVICE</br>-LOCALSERVICE|
|Utilisation|Spécifie la forme de détection de proxy à utiliser. Les valeurs possibles sont :</br>-NO_PROXY : n’utilisez pas un serveur proxy.</br>-Détection automatique, détecter automatiquement les paramètres de proxy.</br>-MANUAL_PROXY : utiliser une liste de proxy explicite et la liste de contournement. Spécifiez la liste de proxy et ignorer la liste qui suit immédiatement la balise de l’utilisation. Par exemple, MANUAL_PROXY proxy1, proxy2 NULL.</br>    -La liste de proxy est une liste délimitée par des virgules de serveurs proxy à utiliser.</br>    -La liste de contournement est une liste délimitée par l’espace de noms d’hôtes ou adresses IP ou les deux, pour lequel les transferts ne doivent ne pas être acheminé via un proxy. Il peut s’agir \<local > pour faire référence à tous les serveurs sur le même réseau local. Valeurs NULL ou « » peut être utilisé pour obtenir une liste de contournement proxy vide.</br>-AUTOSCRIPT : Identique à détection automatique, à ceci près qu’il exécute également un script. Spécifiez l’URL du script qui suit immédiatement la balise de l’utilisation. Par exemple, AUTOSCRIPT http://server/proxy.js.</br>-RÉINITIALISATION — Identique NO_PROXY, sauf qu’elle supprime les URL manuelle du proxy (si spécifié) et URL détectées à l’aide de la détection automatique.|
|ConnectionName|Facultatif : utilisé avec le **/Conn** paramètre pour spécifier la connexion modem à utiliser. Si vous ne spécifiez pas le **/Conn** paramètre, BITS utilise la connexion de réseau local. Spécifiez le nom de connexion modem qui suit immédiatement la **/Conn** paramètre.|

## <a name="remarks"></a>Notes

Chaque appel à l’aide de ce commutateur remplace l’utilisation spécifiée précédemment, mais pas les paramètres de l’utilisation précédemment définie. Par exemple, si vous spécifiez NO_PROXY, détection automatique et MANUAL_PROXY sur des appels distincts, BITS utilise la dernière utilisation fournie mais conserve les paramètres de l’utilisation précédemment définie.

> [!IMPORTANT]
> Vous devez exécuter cette commande à partir d’une invite de commandes avec élévation de privilèges pour qu’elle se termine avec succès.

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant définit l’utilisation de proxy pour le compte de SERVICE réseau.

```
C:\>bitsadmin /Util /SetIEProxy localsystem AUTODETECT
```

Voici des exemples supplémentaires.

```
bitsadmin /util /setieproxy localsystem MANUAL_PROXY proxy1,proxy2,proxy3 NULL
bitsadmin /util /setieproxy localsystem MANUAL_PROXY proxy1:80 ""
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)