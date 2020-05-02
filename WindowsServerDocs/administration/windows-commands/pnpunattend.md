---
title: pnpunattend
description: Découvrez comment auditer les pilotes de périphériques sur un ordinateur, ainsi que pour effectuer des installations de pilote en mode silencieux.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4fa88932-cff0-4dfc-936c-98c0e3dfbeb8 britw
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 569b879caf29aac6d1592d822072f35021cec9d3
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723302"
---
# <a name="pnpunattend"></a>pnpunattend

Audite un ordinateur pour rechercher des pilotes de périphériques et effectuer des installations de pilotes sans assistance, ou Rechercher des pilotes sans installer et, éventuellement, signaler les résultats à la ligne de commande. Utilisez cette commande pour spécifier l’installation de pilotes spécifiques pour des périphériques matériels spécifiques. Consultez la section Notes.

## <a name="syntax"></a>Syntaxe

```
PnPUnattend.exe auditSystem [/help] [/?] [/h] [/s] [/L]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|auditSystem|Spécifie l’installation du pilote en ligne.</br>Obligatoire, sauf si **pnpunattend** est exécuté avec **/Help** ou **/ ?** paramètres.|
|/s|facultatif. Spécifie de rechercher les pilotes sans installer.|
|/L|facultatif. Spécifie l’affichage des informations de journal pour cette commande dans l’invite de commandes.|
|/?|facultatif. Affiche l’aide de cette commande à l’invite de commandes.|

## <a name="remarks"></a>Notes 

Une préparation préliminaire est nécessaire. Avant d’utiliser cette commande, vous devez effectuer les tâches suivantes :

1. Créez un répertoire pour les pilotes que vous souhaitez installer. Par exemple, créez un dossier dans **C:\Drivers\Video** pour les pilotes de carte vidéo.
2. Téléchargez et extrayez le package de pilotes de votre appareil. Copiez le contenu du sous-dossier qui contient le fichier INF de votre version du système d’exploitation et de tous les sous-dossiers dans le dossier vidéo que vous avez créé. Par exemple, copiez les fichiers de pilote vidéo dans C:\Drivers\Video.
3. Ajoutez une variable de chemin d’accès de l’environnement système au dossier que vous avez créé à l’étape 1. par exemple, **C:\Drivers\Video**.
4. Créez la clé de Registre suivante, puis, pour la clé **DriverPaths** que vous créez, définissez les données de la **valeur** sur **1**.
5. Pour Windows® 7, accédez au chemin d’accès au registre : **HKEY_LOCAL_Machine \Software\Microsoft\Windows NT\CurrentVersion\\**, puis créez les clés : **UnattendSettings\PnPUnattend\DriverPaths\\ **
6. Pour Windows Vista, accédez au chemin d’accès au registre : **HK_LM\\\Software\Microsoft\Windows NT\CurrentVersion**, puis créez les clés = **\UnattendSettings\PnPUnattend\DriverPaths**.

## <a name="examples"></a>Exemples

La commande vous montre comment utiliser **PNPUnattend. exe** pour effectuer l’audit d’un ordinateur pour les mises à jour de pilotes possibles, puis pour signaler les résultats à l’invite de commandes.

```
pnpunattend auditsystem /s /l 
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)