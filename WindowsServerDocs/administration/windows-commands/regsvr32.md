---
title: regsvr32
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3345e964-7d3e-42b8-abeb-42ed6edfe2b2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: beadc9e9e614e2fe4cffad5dc263cfb1d4aecf67
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722476"
---
# <a name="regsvr32"></a>regsvr32



Inscrit les fichiers. dll en tant que composants de commande dans le registre.



## <a name="syntax"></a>Syntaxe

```
regsvr32 [/u] [/s] [/n] [/i[:cmdline]] <DllName>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/U|Annule l’inscription du serveur.|
|/s|Exécute **regsvr32** sans afficher de messages.|
|/n|Exécute **regsvr32** sans appeler **DllRegisterServer**. (Requiert le paramètre **/i** .)|
|/i :\<cmdline>|Passe une chaîne de ligne de commande (*cmdline*) facultative à **DllInstall**. Si vous utilisez ce paramètre conjointement avec le paramètre **/u** , il appelle **DllUninstall**.|
|\<> DllName|Nom du fichier. dll qui sera inscrit.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="examples"></a>Exemples

Pour inscrire le fichier. dll pour le schéma de Active Directory, tapez :
```
regsvr32 schmmgmt.dll
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)