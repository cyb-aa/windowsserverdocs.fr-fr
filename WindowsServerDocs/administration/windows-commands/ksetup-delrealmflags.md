---
title: 'Ksetup : delrealmflags'
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 8e2e67d7af4fdd31b79ad633c9df844483bb1ea3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375102"
---
# <a name="ksetupdelrealmflags"></a>Ksetup : delrealmflags



Supprime les indicateurs de domaine du domaine spécifié.  Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
ksetup /delrealmflags <RealmName> [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|@no__t 0RealmName >|Le nom de domaine est indiqué en tant que nom DNS en majuscules, par exemple CORP. CONTOSO.COM et est indiqué comme domaine par défaut lors de l’exécution de **Ksetup** .|

## <a name="remarks"></a>Notes

Les indicateurs de domaine spécifient des fonctionnalités supplémentaires d’un domaine Kerberos qui n’est pas basé sur le système d’exploitation Windows Server. Les ordinateurs qui exécutent Windows Server 2003, Windows Server 2008 ou Windows Server 2008 R2 peuvent utiliser un serveur Kerberos pour administrer l’authentification au lieu d’utiliser un domaine qui exécute un système d’exploitation Windows Server, et ces systèmes participent à un Domaine Kerberos. Cette entrée établit les fonctionnalités du domaine. Le tableau suivant décrit chacune d’elles.

|Value|Indicateur de domaine|Description|
|-----|----------|-----------|
|0xF|Tous|Tous les indicateurs de domaine sont définis.|
|0x00|Aucune|Aucun indicateur de domaine n’est défini et aucune fonctionnalité supplémentaire n’est activée.|
|0x01|SendAddress|L’adresse IP sera incluse dans les tickets d’accord de tickets.|
|0x02|TcpSupported|Le protocole TCP (Transmission Control Protocol) et le protocole UDP (User Datagram Protocol) sont tous deux pris en charge dans ce domaine.|
|0x04|Délégué|Tous les membres de ce domaine sont approuvés pour la délégation.|
|0x08|NcSupported|Ce domaine prend en charge la canonicalisation des noms, ce qui permet les normes de nommage DNS et de domaine.|
|0x80|RC4|Ce domaine prend en charge le chiffrement RC4 pour permettre l’approbation inter-domaines, ce qui permet l’utilisation du protocole TLS.|

Les indicateurs de domaine sont stockés dans le registre dans **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\Kerberos\Domains @ no__t-1**<em>RealmName</em>. Par défaut, cette entrée n’existe pas dans le Registre. Vous pouvez utiliser la commande [Ksetup : addrealmflags](ksetup-addrealmflags.md) pour remplir le registre.

Vous pouvez voir quels indicateurs de domaine sont disponibles et définis en affichant la sortie de **Ksetup** ou **Ksetup/dumpstate**.

## <a name="BKMK_Examples"></a>Illustre

Répertoriez les indicateurs de domaine disponibles pour le domaine CONTOSO :
```
Ksetup /listrealmflags
```
Supprimez deux indicateurs qui se trouvent actuellement dans le jeu :
```
ksetup /delrealmflags CONTOSO ncsupported delegate
```
Exécutez la commande **Ksetup** pour vérifier que l’indicateur de domaine est défini en affichant la sortie et en recherchant **indicateurs de domaine =** .

#### <a name="additional-references"></a>Références supplémentaires

-   [Ksetup:listrealmflags](ksetup-listrealmflags.md)
-   [Ksetup:setrealmflags](ksetup-setrealmflags.md)
-   [Ksetup:addrealmflags](ksetup-addrealmflags.md)
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)