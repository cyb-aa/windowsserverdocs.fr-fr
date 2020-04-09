---
title: Manage-package BDE
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c631ef10-2a2f-4541-8578-292f2d4e9e80
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3a53edd060cb8c7a7a52e86130b2136893a42150
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80840102"
---
# <a name="manage-bde-keypackage"></a>Manage-bde : keypackage



Génère un package de clé pour un lecteur. Le package de clé peut être utilisé conjointement avec l’outil de réparation pour réparer les lecteurs endommagés. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
manage-bde -KeyPackage [<Drive>] [-ID <KeyProtectoryID>] [-path <PathToExternalKeyDirectory>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

#### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Lecteur \<>|Représente une lettre de lecteur suivie par un signe deux-points.|
|-ID|Créez un package de clés à l’aide du protecteur de clé avec l’identificateur spécifié par cette valeur d’ID.|
|-chemin d’accès|Emplacement dans lequel enregistrer le package de clés créé.|
|-ComputerName|Spécifie que Manage-bde. exe sera utilisé pour modifier la protection BitLocker sur un autre ordinateur. Vous pouvez également utiliser **-CN** comme version abrégée de cette commande.|
|Nom de l' \<>|Représente le nom de l’ordinateur sur lequel modifier la protection BitLocker. Les valeurs acceptées incluent le nom NetBIOS de l’ordinateur et l’adresse IP de l’ordinateur.|
|-? ou /?|Affiche une brève aide à l’invite de commandes.|
|-Help ou-h|Affiche l’aide complète à l’invite de commandes.|

## <a name="examples"></a><a name=BKMK_Examples></a>Illustre

L’exemple suivant illustre l’utilisation de la commande **-keypackage** pour créer un package de clé pour le lecteur C basé sur le protecteur de clé identifié par le GUID et pour enregistrer le package de clés dans F:\Folder.
```
manage-bde -KeyPackage C: -id {84E151C1...7A62067A512} -path f:\Folder
```

> [!TIP]
> Utilisez **Manage-bde – protectors – prenez en main** la lettre de lecteur pour laquelle vous souhaitez créer un package de clé afin d’obtenir la liste des GUID disponibles à utiliser comme valeur d’ID.

## <a name="additional-references"></a>Références supplémentaires

-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Gérer-bde](manage-bde.md)