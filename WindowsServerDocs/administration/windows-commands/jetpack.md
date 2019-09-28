---
title: jetpack
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: b011658c6a745d62707cf88404379b17b0e05eef
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375324"
---
# <a name="jetpack"></a>jetpack

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

compacte une base de données WINS (Windows Internet Name Service) ou DHCP (Dynamic Host Configuration Protocol). Microsoft vous recommande de compacter la base de données WINS chaque fois qu’elle est proche de 30 Mo. 

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

## <a name="BKMK_Examples"></a>Illustre
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
Dans les exemples ci-dessus, **tmp. mdb** est une base de données temporaire utilisée par Jetpack. exe. **WINS. mdb** est la base de données WINS. **DHCP. mdb** est la base de données DHCP.
Jetpack. exe compacte la base de données WINS ou DHCP en procédant comme suit :
1.  Copie les informations de la base de données dans un fichier de base de données temporaire appelé **tmp. mdb**.
2.  supprime le fichier de base de données d’origine, **WINS. mdb** ou **DHCP. mdb**.
3.  renomme les fichiers de base de données temporaires avec le nom de fichier d’origine.

> [!NOTE]
> Pendant le processus compact, Jetpack. exe crée un fichier temporaire portant le nom spécifié par le paramètre *nom de la base de données temp* . Le fichier temporaire est supprimé une fois le processus de compactage terminé. Assurez-vous que vous n’avez pas de fichier déjà existant dans le dossier WINS ou DHCP portant le même nom que celui spécifié dans le paramètre *nom de la base de données temporaire* .

## <a name="additional-references"></a>Références supplémentaires
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
