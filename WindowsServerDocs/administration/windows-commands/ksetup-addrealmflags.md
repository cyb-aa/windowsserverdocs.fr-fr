---
title: 'Ksetup : addrealmflags'
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 80ca1e16-8871-494b-b9be-6bc9d63de860
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 26f06e23f40216fe31403280d8946c56825967d6
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724709"
---
# <a name="ksetupaddrealmflags"></a>Ksetup : addrealmflags



Ajoute des indicateurs de domaine supplémentaires au domaine spécifié.

## <a name="syntax"></a>Syntaxe

```
ksetup /addrealmflags <RealmName> [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]
```

#### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Nom de domaine|Le nom de domaine est indiqué en tant que nom DNS en majuscules, par exemple CORP. CONTOSO.COM.|

## <a name="remarks"></a>Notes 

Les indicateurs de domaine spécifient des fonctionnalités supplémentaires d’un domaine Kerberos qui n’est pas basé sur le système d’exploitation Windows Server. Les ordinateurs qui exécutent Windows Server 2003, Windows Server 2008 ou Windows Server 2008 R2 peuvent utiliser un serveur Kerberos pour administrer l’authentification au lieu d’utiliser un domaine qui exécute un système d’exploitation Windows Server, et ces systèmes participent à un domaine Kerberos. Cette entrée établit les fonctionnalités du domaine. Le tableau suivant décrit chacune d’elles.

|Valeur|Indicateur de domaine|Description|
|-----|----------|-----------|
|0xF|Tous|Tous les indicateurs de domaine sont définis.|
|0x00|None|Aucun indicateur de domaine n’est défini et aucune fonctionnalité supplémentaire n’est activée.|
|0x01|SendAddress|L’adresse IP sera incluse dans les tickets d’accord de tickets.|
|0x02|TcpSupported|Le protocole TCP (Transmission Control Protocol) et le protocole UDP (User Datagram Protocol) sont tous deux pris en charge dans ce domaine.|
|0x04|Déléguer|Tous les membres de ce domaine sont approuvés pour la délégation.|
|0x08|NcSupported|Ce domaine prend en charge la canonicalisation des noms, ce qui permet les normes de nommage DNS et de domaine.|
|0x80|RC4|Ce domaine prend en charge le chiffrement RC4 pour permettre l’approbation inter-domaines, ce qui permet l’utilisation du protocole TLS.|

Les indicateurs de domaine sont stockés dans le Registre sous HKEY_LOCAL_MACHINE<em>nom de domaine</em> **\\\system\currentcontrolset\control\lsa\kerberos\domains**. Par défaut, cette entrée n’existe pas dans le Registre. Vous pouvez utiliser la commande [Ksetup : addrealmflags](ksetup-addrealmflags.md) pour remplir le registre.

Vous pouvez voir quels indicateurs de domaine sont disponibles et définis en affichant la sortie de Ksetup ou Ksetup/dumpstate.

## <a name="examples"></a>Exemples

Répertoriez les indicateurs de domaine disponibles pour le domaine CONTOSO :
```
Ksetup /listrealmflags
```
Définissez deux indicateurs sur le domaine CONTOSO :
```
ksetup /setrealmflags CONTOSO ncsupported delegate
```
Ajoutez un autre indicateur qui ne se trouve pas actuellement dans le jeu :
```
ksetup /addrealmflags CONTOSO SendAddress
```
Exécutez la commande **Ksetup** pour vérifier que l’indicateur de domaine est défini en affichant la sortie et en recherchant **indicateurs de domaine =**.

## <a name="additional-references"></a>Références supplémentaires

-   [Ksetup:listrealmflags](ksetup-listrealmflags.md)
-   [Ksetup:setrealmflags](ksetup-setrealmflags.md)
-   [Ksetup:delrealmflags](ksetup-delrealmflags.md)
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)