---
title: 'Ksetup : setrealmflags'
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bcb2824e-fba7-4ebe-be62-e62b4fae5b17
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 29ef5de8130fa9b10ce6d999e16765b0aa2697a4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841282"
---
# <a name="ksetupsetrealmflags"></a>Ksetup : setrealmflags



Définit les indicateurs de domaine pour le domaine spécifié. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
ksetup /setrealmflags <RealmName> [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]
```

#### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<RealmName >|Le nom de domaine est indiqué en tant que nom DNS en majuscules, par exemple CORP. CONTOSO.COM.|
|Indicateur de domaine|Indique l’un des indicateurs suivants :</br>-SendAddress</br>- TcpSupported</br>-Délégué</br>- NcSupported</br>-RC4|

## <a name="remarks"></a>Notes

Les indicateurs de domaine spécifient des fonctionnalités supplémentaires d’un domaine Kerberos qui n’est pas basé sur le système d’exploitation Windows Server. Les ordinateurs qui exécutent Windows Server 2003, Windows Server 2008 ou Windows Server 2008 R2 peuvent utiliser un serveur Kerberos pour administrer l’authentification au lieu d’utiliser un domaine qui exécute un système d’exploitation Windows Server, et ces systèmes participent à un domaine Kerberos. Cette entrée établit les fonctionnalités du domaine. Le tableau suivant décrit chacune d’elles.

|Valeur|Indicateur de domaine|Description|
|-----|----------|-----------|
|0xF|Tout|Tous les indicateurs de domaine sont définis.|
|0x00|Aucune|Aucun indicateur de domaine n’est défini et aucune fonctionnalité supplémentaire n’est activée.|
|0x01|SendAddress|L’adresse IP sera incluse dans les tickets d’accord de tickets.|
|0x02|TcpSupported|Le protocole TCP (Transmission Control Protocol) et le protocole UDP (User Datagram Protocol) sont tous deux pris en charge dans ce domaine.|
|0x04|Délégué|Tous les membres de ce domaine sont approuvés pour la délégation.|
|0x08|NcSupported|Ce domaine prend en charge la canonicalisation des noms, ce qui permet les normes de nommage DNS et de domaine.|
|0x80|RC4|Ce domaine prend en charge le chiffrement RC4 pour permettre l’approbation inter-domaines, ce qui permet l’utilisation du protocole TLS.|

Les indicateurs de domaine sont stockés dans le Registre sous **HKEY_LOCAL_MACHINE \system\currentcontrolset\control\lsa\kerberos\domains\\** <em>RealmName</em>. Par défaut, cette entrée n’existe pas dans le Registre. Vous pouvez utiliser la commande [Ksetup : addrealmflags](ksetup-addrealmflags.md) pour remplir le registre.

Vous pouvez voir quels indicateurs de domaine sont disponibles et définis en affichant la sortie de **Ksetup**.

## <a name="examples"></a><a name=BKMK_Examples></a>Illustre

Répertoriez les indicateurs de domaine disponibles et définis pour le domaine CONTOSO :
```
ksetup
```
Définissez deux indicateurs qui ne sont pas actuellement définis :
```
ksetup /setrealmflags CONTOSO ncsupported delegate
```
Exécutez la commande **Ksetup** pour vérifier que l’indicateur de domaine est défini en affichant la sortie et en recherchant **indicateurs de domaine =** .

## <a name="additional-references"></a>Références supplémentaires

-   [Ksetup:listrealmflags](ksetup-listrealmflags.md)
-   [Ksetup:addrealmflags](ksetup-addrealmflags.md)
-   [Ksetup:delrealmflags](ksetup-delrealmflags.md)
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)