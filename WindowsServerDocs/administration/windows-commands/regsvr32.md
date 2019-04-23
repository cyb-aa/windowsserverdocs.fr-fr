---
title: regsvr32
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3345e964-7d3e-42b8-abeb-42ed6edfe2b2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 87d9291755ddb4484e85248cb01ad78b01a25965
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889990"
---
# <a name="regsvr32"></a>regsvr32



Inscrit les fichiers .dll en tant que composants de commande dans le Registre.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
regsvr32 [/u] [/s] [/n] [/i[:cmdline]] <DllName>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/u|Annule l’inscription du serveur.|
|/s|Exécutions **Regsvr32** sans afficher de messages.|
|/n|Exécutions **Regsvr32** sans appeler **DllRegisterServer**. (Nécessite le **/i** paramètre.)|
|/i:\<cmdline>|Transmet une chaîne de ligne de commande facultative (*cmdline*) à **DllInstall**. Si vous utilisez ce paramètre conjointement avec la **/u** paramètre, il appelle **DllUninstall**.|
|\<DllName>|Le nom du fichier .dll qui sera inscrit.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="BKMK_examples"></a>Exemples

Pour enregistrer le fichier .dll pour le schéma Active Directory, tapez :
```
regsvr32 schmmgmt.dll
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)