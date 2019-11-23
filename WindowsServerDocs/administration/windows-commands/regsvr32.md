---
title: regsvr32
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 444af0ccf7c9bbe21c013f32b396997b7cb2e00f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371629"
---
# <a name="regsvr32"></a>regsvr32



Inscrit les fichiers. dll en tant que composants de commande dans le registre.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
regsvr32 [/u] [/s] [/n] [/i[:cmdline]] <DllName>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/u.|Annule l’inscription du serveur.|
|/s|Exécute **regsvr32** sans afficher de messages.|
|/n|Exécute **regsvr32** sans appeler **DllRegisterServer**. (Requiert le paramètre **/i** .)|
|/i :\<cmdline >|Passe une chaîne de ligne de commande (*cmdline*) facultative à **DllInstall**. Si vous utilisez ce paramètre conjointement avec le paramètre **/u** , il appelle **DllUninstall**.|
|\<DllName >|Nom du fichier. dll qui sera inscrit.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="BKMK_examples"></a>Illustre

Pour inscrire le fichier. dll pour le schéma de Active Directory, tapez :
```
regsvr32 schmmgmt.dll
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)