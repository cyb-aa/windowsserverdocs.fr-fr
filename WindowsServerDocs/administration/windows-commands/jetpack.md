---
title: jetpack
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 82a2b7ef-0db5-4575-a028-8acb0bf6c7ba
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a3bffc29519df139921bdb1de53e67acd558b306
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858010"
---
# <a name="jetpack"></a>jetpack

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

compacte une base de données Windows Internet WINS (Name Service) ou DHCP Dynamic Host Configuration Protocol (). Microsoft vous recommande de compacter la base de données WINS chaque fois qu’elle approche de 30 Mo. 

## <a name="syntax"></a>Syntaxe
```
jetpack.EXE <database name> <temp database name>
```

### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|<database name>|Spécifie le fichier de base de données d’origine.|
|<temp database name>|Spécifie le fichier de base de données temporaire.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="BKMK_Examples"></a>Exemples
Pour compacter la base de données WINS :
```
cd %SYSTEMROOT%\SYSTEM32\WINS
NET STOP WINS
jetpack WINS.MDB TMP.MDB
NET start WINS
```
Pour compacter la base de données DHCP :
```
cd %SYSTEMROOT%\SYSTEM32\DHCP
NET STOP DHCPSERver
jetpack DHCP.MDB TMP.MDB
NET start DHCPSERver
```
Dans les exemples ci-dessus, **TMP** est une base de données temporaire utilisée par jetpack.exe. **WINS.mdb** est la base de données WINS. **DHCP.mdb** est la base de données DHCP.
Jetpack.exe compacte le service WINS ou base de données DHCP en procédant comme suit :
1.  Copies de base de données d’informations dans un fichier de base de données temporaire appelé **TMP**.
2.  Supprime le fichier de base de données d’origine, **Wins.mdb** ou **DHCP.mdb**.
3.  Renomme les fichiers de base de données temporaire avec le nom de fichier d’origine.

> [!NOTE]
> Pendant le processus compact, jetpack.exe crée un fichier temporaire avec le nom spécifié par le *nom de la base de données temporaire* paramètre. Le fichier temporaire est supprimé lorsque le processus de compactage est terminé. Assurez-vous que vous n’avez pas d’un fichier déjà existant dans WINS ou DHCP dossier portant le même nom que celui spécifié dans le *nom de la base de données temporaire* paramètre.

## <a name="additional-references"></a>Références supplémentaires
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
