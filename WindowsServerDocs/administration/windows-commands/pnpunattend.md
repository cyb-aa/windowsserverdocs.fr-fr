---
title: pnpunattend
description: Découvrez comment auditer les pilotes de périphérique sur un ordinateur, ainsi que pour effectuer des installations de pilotes en mode silencieux.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4fa88932-cff0-4dfc-936c-98c0e3dfbeb8 britw
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 7aa09636f8c23678f003bfa5ebf8be164e7fc683
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879520"
---
# <a name="pnpunattend"></a>pnpunattend

Un ordinateur pour les pilotes de périphériques, des audits et effectuer des installations sans assistance de pilote, ou rechercher des pilotes sans installer et, éventuellement, signaler les résultats à la ligne de commande. Cette commande permet de spécifier l’installation de pilotes spécifiques pour des périphériques matériels spécifiques. Voir Notes.

## <a name="syntax"></a>Syntaxe

```
PnPUnattend.exe auditSystem [/help] [/?] [/h] [/s] [/L]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|auditSystem|Spécifie l’installation du pilote en ligne.</br>Obligatoire, sauf lorsque **pnpunattend** est exécuté avec soit le **/Help** ou **/ ?** paramètres.|
|/s|Facultatif. Spécifie pour rechercher les pilotes sans installer.|
|/L|Facultatif. Spécifie d’afficher les informations du journal pour cette commande dans l’invite de commandes.|
|/?|Facultatif. Affiche l’aide de cette commande à l’invite de commandes.|

## <a name="remarks"></a>Notes

Préparation préliminaire est nécessaire. Avant d’utiliser cette commande, vous devez effectuer les tâches suivantes :

1.  Créez un répertoire pour les pilotes que vous souhaitez installer. Par exemple, créez un dossier à **C:\Drivers\Video** pour les pilotes de carte vidéo.
2.  Téléchargez et extrayez le package de pilotes pour votre appareil. Copiez le contenu du sous-dossier contenant le fichier INF pour votre version du système d’exploitation et tous les sous-dossiers dans le dossier vidéo que vous avez créé. Par exemple, copiez les fichiers de pilote vidéo à C:\Drivers\Video.
3.  Ajouter une variable de chemin d’accès d’environnement système dans le dossier que vous avez créé dans l’étape 1, par exemple, **C:\Drivers\Video**.
4.  Créez la clé de Registre suivante, puis pour le **Chemins_de_pilotes** clé que vous créez, définissez le **données de la valeur** à **1**.
-   Pour Windows® 7 parcourir le chemin d’accès du Registre : **HKEY_LOCAL_Machine\Software\Microsoft\Windows NT\CurrentVersion\**, puis créez les clés : **UnattendSettings\PnPUnattend\DriverPaths\**
-   Pour Windows Vista, naviguez jusqu'à l’emplacement de Registre : **HK_LM\Software\Microsoft\Windows NT\CurrentVersion\**, puis créez les clés = **\UnattendSettings\PnPUnattend\DriverPaths** .

## <a name="examples"></a>Exemples

L’exemple de commande suivant montre comment utiliser le **PNPUnattend.exe** et d’audit d’un ordinateur pour les mises à jour éventuelle du pilote, puis signaler les résultats à l’invite de commandes.

```
pnpunattend auditsystem /s /l 
```

## <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)