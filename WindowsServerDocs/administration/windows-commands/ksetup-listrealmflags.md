---
title: 'Ksetup : listrealmflags'
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: aa96e4da-6b98-4c05-bccf-73cbf33258c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f0646be8daaad4bc3303389cfca1f3a09136fe1a
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724626"
---
# <a name="ksetuplistrealmflags"></a>Ksetup : listrealmflags



Répertorie les indicateurs de domaine disponibles qui peuvent être signalés par **Ksetup**.

## <a name="syntax"></a>Syntaxe

```
ksetup /listrealmflags
```

#### <a name="parameters"></a>Paramètres

None

## <a name="remarks"></a>Notes 

Les indicateurs de domaine spécifient des fonctionnalités supplémentaires d’un domaine Kerberos non basé sur Windows. Les ordinateurs qui exécutent Windows Server 2003, Windows Server 2008 ou Windows Server 2008 R2 peuvent utiliser un serveur Kerberos non basé sur Windows pour administrer l’authentification au lieu d’utiliser un domaine qui exécute un système d’exploitation Windows Server. Ces systèmes participent à un domaine Kerberos au lieu d’un domaine Windows. Cette entrée établit les fonctionnalités du domaine. Le tableau suivant décrit chacune d’elles.

|Valeur|Indicateur de domaine|Description|
|-----|----------|-----------|
|0xF|Tous|Tous les indicateurs de domaine sont définis.|
|0x00|None|Aucun indicateur de domaine n’est défini et aucune fonctionnalité supplémentaire n’est activée.|
|0x01|SendAddress|L’adresse IP sera incluse dans les tickets d’accord de tickets.|
|0x02|TcpSupported|Le protocole TCP (Transmission Control Protocol) et le protocole UDP (User Datagram Protocol) sont pris en charge dans ce domaine.|
|0x04|Déléguer|Tous les membres de ce domaine sont approuvés pour la délégation.|
|0x08|NcSupported|Ce domaine prend en charge la canonicalisation des noms, ce qui permet les normes de nommage DNS et de domaine.|
|0x80|RC4|Ce domaine prend en charge le chiffrement RC4 pour permettre l’approbation inter-domaines, ce qui permet l’utilisation du protocole TLS.|

Les indicateurs de domaine sont stockés dans le registre dans HKEY_LOCAL_MACHINE<em>nom de domaine</em> **\system\currentcontrolset\control\lsa\kerberos\domains\\**. Par défaut, cette entrée n’existe pas dans le Registre. Vous pouvez utiliser la commande [Ksetup : addrealmflags](ksetup-addrealmflags.md) pour remplir le registre.

## <a name="examples"></a>Exemples

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