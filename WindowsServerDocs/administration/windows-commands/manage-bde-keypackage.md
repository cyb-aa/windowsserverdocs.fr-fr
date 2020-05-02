---
title: Manage-package BDE
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c631ef10-2a2f-4541-8578-292f2d4e9e80
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5c13502145d80693a64b284bf480fabfd03af0db
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724163"
---
# <a name="manage-bde-keypackage"></a>Manage-bde : keypackage



Génère un package de clé pour un lecteur. Le package de clé peut être utilisé conjointement avec l’outil de réparation pour réparer les lecteurs endommagés.

## <a name="syntax"></a>Syntaxe

```
manage-bde -KeyPackage [<Drive>] [-ID <KeyProtectoryID>] [-path <PathToExternalKeyDirectory>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

#### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<Lecteur>|Représente une lettre de lecteur suivie par un signe deux-points.|
|-ID|Créez un package de clés à l’aide du protecteur de clé avec l’identificateur spécifié par cette valeur d’ID.|
|-chemin d’accès|Emplacement dans lequel enregistrer le package de clés créé.|
|-ComputerName|Spécifie que Manage-bde. exe sera utilisé pour modifier la protection BitLocker sur un autre ordinateur. Vous pouvez également utiliser **-CN** comme version abrégée de cette commande.|
|\<Name>|Représente le nom de l’ordinateur sur lequel modifier la protection BitLocker. Les valeurs acceptées incluent le nom NetBIOS de l’ordinateur et l’adresse IP de l’ordinateur.|
|-? ou /?|Affiche une brève aide à l’invite de commandes.|
|-Help ou-h|Affiche l’aide complète à l’invite de commandes.|

## <a name="examples"></a>Exemples

Pour illustrer l’utilisation de la commande **-keypackage** pour créer un package de clé pour le lecteur C basé sur le protecteur de clé identifié par le GUID et pour enregistrer le package de clés dans F:\Folder.
```
manage-bde -KeyPackage C: -id {84E151C1...7A62067A512} -path f:\Folder
```

> [!TIP]
> Utilisez **Manage-bde – protectors – prenez en main** la lettre de lecteur pour laquelle vous souhaitez créer un package de clé afin d’obtenir la liste des GUID disponibles à utiliser comme valeur d’ID.

## <a name="additional-references"></a>Références supplémentaires

-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Gérer-bde](manage-bde.md)