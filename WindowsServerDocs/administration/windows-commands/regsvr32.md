---
title: regsvr32
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3345e964-7d3e-42b8-abeb-42ed6edfe2b2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d3b775b0c49e4191a9fee6dc9e2e91f968142085
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836212"
---
# <a name="regsvr32"></a>regsvr32



Inscrit les fichiers. dll en tant que composants de commande dans le registre.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
regsvr32 [/u] [/s] [/n] [/i[:cmdline]] <DllName>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/u|Annule l’inscription du serveur.|
|/s|Exécute **regsvr32** sans afficher de messages.|
|/n|Exécute **regsvr32** sans appeler **DllRegisterServer**. (Requiert le paramètre **/i** .)|
|/i :\<cmdline >|Passe une chaîne de ligne de commande (*cmdline*) facultative à **DllInstall**. Si vous utilisez ce paramètre conjointement avec le paramètre **/u** , il appelle **DllUninstall**.|
|\<DllName >|Nom du fichier. dll qui sera inscrit.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

Pour inscrire le fichier. dll pour le schéma de Active Directory, tapez :
```
regsvr32 schmmgmt.dll
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)