---
title: ksetup:listrealmflags
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: aa96e4da-6b98-4c05-bccf-73cbf33258c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1bc4be8be747c31d60d75c90ad3aa831dd8dff93
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838300"
---
# <a name="ksetuplistrealmflags"></a>ksetup:listrealmflags



Répertorie les indicateurs de domaine disponibles qui peuvent être signalées à **ksetup**. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
ksetup /listrealmflags
```

### <a name="parameters"></a>Paramètres

Aucune

## <a name="remarks"></a>Notes

Les indicateurs de domaine spécifient des fonctionnalités supplémentaires d’un domaine Kerberos non-Windows Windows. Ordinateurs qui exécutent Windows Server 2003, Windows Server 2008 ou Windows Server 2008 R2 peuvent utiliser un serveur de Kerberos non basés sur Windows pour administrer l’authentification au lieu d’utiliser un domaine qui exécute un système d’exploitation Windows. Ces systèmes de participent à un domaine Kerberos au lieu d’un domaine Windows. Cette entrée établit les fonctionnalités du domaine. Le tableau suivant décrit chacun.

|Value|Indicateur de domaine|Description|
|-----|----------|-----------|
|0xF|Tous|Tous les indicateurs de domaine sont définies.|
|0x00|Aucune|Aucun indicateur de domaine est défini, et aucuns fonctionnalités supplémentaires ne sont activées.|
|0x01|SendAddress|L’adresse IP sera inclus dans les ticket-granting tickets.|
|0x02|TcpSupported|Le protocole TCP (Transmission Control) et le protocole UDP (User Datagram) sont pris en charge dans ce domaine.|
|0x04|Délégué|Tous les membres de ce domaine est approuvé pour délégation.|
|0x08|NcSupported|Ce domaine prend en charge la canonisation nom, ce qui permet des normes d’affectation de noms de domaine DNS.|
|0x80|RC4|Ce domaine prend en charge le chiffrement RC4 pour activer l’approbation entre domaines, ce qui permet l’utilisation de TLS.|

Indicateurs de domaine sont stockés dans le Registre **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\Kerberos\Domains\*** Realm-nom *. Par défaut, cette entrée n’existe pas dans le Registre. Vous pouvez utiliser la [Ksetup:addrealmflags](ksetup-addrealmflags.md) commande pour remplir le Registre.

## <a name="BKMK_Examples"></a>Exemples

Répertorier les indicateurs de domaine connu sur cet ordinateur :
```
ksetup /listrealmflags
```
Définir les indicateurs de domaine disponible qui **Ksetup** ne sait pas en tapant une des commandes suivantes à la ligne de commande :
```
ksetup /setrealmflags CORP.CONTOSO.COM sendaddress tcpsupported delete ncsupported
```
```
ksetup /setrealmflags CORP.CONTOSO.COM 0xF
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Ksetup:setrealmflags](ksetup-setrealmflags.md)
-   [Ksetup:addrealmflags](ksetup-addrealmflags.md)
-   [Ksetup:delrealmflags](ksetup-delrealmflags.md)
-   [Ksetup](ksetup.md)
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)