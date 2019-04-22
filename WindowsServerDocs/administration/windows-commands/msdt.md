---
title: msdt
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ead1b672-a120-4e16-94aa-a8e13602c1d0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 781e83615d44a4adb20fd85fe5ce6ebc7c2d4966
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818830"
---
# <a name="msdt"></a>msdt



Appelle un pack de dépannage en ligne de commande ou dans le cadre d’un script automatisé et active des options supplémentaires sans entrée utilisateur.

## <a name="syntax"></a>Syntaxe

```
msdt </id <name> | /path <name> | /cab < name>> <</parameter> [options] … <parameter> [options]>>
```

## <a name="parameters"></a>Paramètres

Le tableau suivant contient les paramètres et les options prises en charge par msdt.exe.

|Paramètre|Description|
|---------|-----------|
|/id \<package name>|Spécifie le package de diagnostic à exécuter. Pour obtenir la liste des packages disponibles, consultez l’ID de Pack de résolution des problèmes dans « disponible le dépannage des packs ? section plus loin dans cette rubrique.|
|/ Path \<directory | fichier de .diagpkg | .diagcfg file>|Spécifie le chemin d’accès complet à un package de diagnostic. Si vous spécifiez un répertoire, le répertoire doit contenir un package de diagnostic. Vous ne pouvez pas utiliser le paramètre/Path conjointement avec le */id*, */dci*, ou */cab* paramètre.|
|/dci \<passkey>|Pré-remplit le champ de clé d’accès dans msdt. Ce paramètre est utilisé uniquement lorsqu’un fournisseur de support a fourni une clé de sécurité.|
|/dt \<directory>|Affiche l’historique de résolution des problèmes dans le répertoire spécifié. Résultats de diagnostics sont stockés dans l’utilisateur **%LOCALAPPDATA%\Diagnostics** ou **%LOCALAPPDATA%\ElevatedDiagnostics** répertoires.|
|/AF \<fichier de réponses >|Spécifie un fichier de réponses au format XML qui contient les réponses à un ou plusieurs des interactions Diagnostics.|
