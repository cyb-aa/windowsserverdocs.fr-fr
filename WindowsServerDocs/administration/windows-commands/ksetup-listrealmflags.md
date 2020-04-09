---
title: 'Ksetup : listrealmflags'
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: aa96e4da-6b98-4c05-bccf-73cbf33258c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 265f988d85deb7602e91677626d207bc3a7873ef
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841492"
---
# <a name="ksetuplistrealmflags"></a>Ksetup : listrealmflags



Répertorie les indicateurs de domaine disponibles qui peuvent être signalés par **Ksetup**. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
ksetup /listrealmflags
```

#### <a name="parameters"></a>Paramètres

Aucune

## <a name="remarks"></a>Notes

Les indicateurs de domaine spécifient des fonctionnalités supplémentaires d’un domaine Kerberos non basé sur Windows. Les ordinateurs qui exécutent Windows Server 2003, Windows Server 2008 ou Windows Server 2008 R2 peuvent utiliser un serveur Kerberos non basé sur Windows pour administrer l’authentification au lieu d’utiliser un domaine qui exécute un système d’exploitation Windows Server. Ces systèmes participent à un domaine Kerberos au lieu d’un domaine Windows. Cette entrée établit les fonctionnalités du domaine. Le tableau suivant décrit chacune d’elles.

|Valeur|Indicateur de domaine|Description|
|-----|----------|-----------|
|0xF|Tout|Tous les indicateurs de domaine sont définis.|
|0x00|Aucune|Aucun indicateur de domaine n’est défini et aucune fonctionnalité supplémentaire n’est activée.|
|0x01|SendAddress|L’adresse IP sera incluse dans les tickets d’accord de tickets.|
|0x02|TcpSupported|Le protocole TCP (Transmission Control Protocol) et le protocole UDP (User Datagram Protocol) sont pris en charge dans ce domaine.|
|0x04|Délégué|Tous les membres de ce domaine sont approuvés pour la délégation.|
|0x08|NcSupported|Ce domaine prend en charge la canonicalisation des noms, ce qui permet les normes de nommage DNS et de domaine.|
|0x80|RC4|Ce domaine prend en charge le chiffrement RC4 pour permettre l’approbation inter-domaines, ce qui permet l’utilisation du protocole TLS.|

Les indicateurs de domaine sont stockés dans le registre dans **HKEY_LOCAL_MACHINE \system\currentcontrolset\control\lsa\kerberos\domains\\** <em>nom de domaine</em>. Par défaut, cette entrée n’existe pas dans le Registre. Vous pouvez utiliser la commande [Ksetup : addrealmflags](ksetup-addrealmflags.md) pour remplir le registre.

## <a name="examples"></a><a name=BKMK_Examples></a>Illustre

Répertorier les indicateurs de domaine connus sur cet ordinateur :
```
ksetup /listrealmflags
```
Définissez les indicateurs de domaine disponibles qui ne sont pas connus par **Ksetup** en tapant l’une des commandes suivantes sur la ligne de commande :
```
ksetup /setrealmflags CORP.CONTOSO.COM sendaddress tcpsupported delete ncsupported
```
```
ksetup /setrealmflags CORP.CONTOSO.COM 0xF
```

## <a name="additional-references"></a>Références supplémentaires

-   [Ksetup:setrealmflags](ksetup-setrealmflags.md)
-   [Ksetup:addrealmflags](ksetup-addrealmflags.md)
-   [Ksetup:delrealmflags](ksetup-delrealmflags.md)
-   [Ksetup](ksetup.md)
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)