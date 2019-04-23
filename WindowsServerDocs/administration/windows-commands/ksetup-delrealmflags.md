---
title: ksetup:delrealmflags
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 22053041-1eb4-47f5-bed9-3d5681bcde7d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9522d384c4f6996534b47e98dc8d8003bda504cd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877140"
---
# <a name="ksetupdelrealmflags"></a>ksetup:delrealmflags



Supprime les indicateurs de domaine à partir du domaine Kerberos spécifié.  Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
ksetup /delrealmflags <RealmName> [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<RealmName>|Le nom de domaine est indiqué comme un nom DNS en majuscules, telles que CORP. CONTOSO.COM et est répertorié en tant que le domaine par défaut lorsque **Ksetup** est exécuté.|

## <a name="remarks"></a>Notes

Les indicateurs de domaine spécifient des fonctionnalités supplémentaires d’un domaine Kerberos qui ne repose pas sur le système d’exploitation Windows Server. Ordinateurs qui exécutent Windows Server 2003, Windows Server 2008 ou Windows Server 2008 R2 peuvent utiliser un serveur de Kerberos pour administrer l’authentification au lieu d’utiliser un domaine qui exécute un système d’exploitation Windows, et participent à ces systèmes un Domaine Kerberos. Cette entrée établit les fonctionnalités du domaine. Le tableau suivant décrit chacun.

|Value|Indicateur de domaine|Description|
|-----|----------|-----------|
|0xF|Tous|Tous les indicateurs de domaine sont définies.|
|0x00|Aucune|Aucun indicateur de domaine est défini, et aucuns fonctionnalités supplémentaires ne sont activées.|
|0x01|SendAddress|L’adresse IP sera inclus dans le ticket granting ticket.|
|0x02|TcpSupported|Le protocole TCP (Transmission Control) et le protocole UDP (User Datagram) sont pris en charge dans ce domaine.|
|0x04|Délégué|Tous les membres de ce domaine est approuvé pour délégation.|
|0x08|NcSupported|Ce domaine prend en charge la canonisation nom, ce qui permet des normes d’affectation de noms de domaine DNS.|
|0x80|RC4|Ce domaine prend en charge le chiffrement RC4 pour activer l’approbation entre domaines, ce qui permet l’utilisation de TLS.|

Indicateurs de domaine sont stockés dans le Registre **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\Kerberos\Domains\*** RealmName *. Par défaut, cette entrée n’existe pas dans le Registre. Vous pouvez utiliser la [Ksetup:addrealmflags](ksetup-addrealmflags.md) commande pour remplir le Registre.

Vous pouvez voir quels indicateurs de domaine disponible et défini en affichant la sortie de **ksetup** ou **ksetup /dumpstate**.

## <a name="BKMK_Examples"></a>Exemples

Répertorier les indicateurs de domaine disponible pour le domaine CONTOSO :
```
Ksetup /listrealmflags
```
Supprimez les deux indicateurs sont actuellement dans le jeu :
```
ksetup /delrealmflags CONTOSO ncsupported delegate
```
Exécutez le **ksetup** pour vérifier que l’indicateur de domaine est défini par l’affichage de la sortie et en recherchant **indicateurs de domaine =**.

#### <a name="additional-references"></a>Références supplémentaires

-   [Ksetup:listrealmflags](ksetup-listrealmflags.md)
-   [Ksetup:setrealmflags](ksetup-setrealmflags.md)
-   [Ksetup:addrealmflags](ksetup-addrealmflags.md)
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)