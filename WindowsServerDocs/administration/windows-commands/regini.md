---
title: regini
description: Découvrez comment modifier le registre à partir de l’invite de commandes ou à l’aide d’un script.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5ff18dc3-5bd8-400a-b311-fd73a3267e8c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 80f8f4212d2054fc54ce33993a1cef8a1501c6d5
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70868882"
---
# <a name="regini"></a>regini

Modifie le registre à partir de la ligne de commande ou d’un script, et applique les modifications prédéfinies dans un ou plusieurs fichiers texte. Vous pouvez créer, modifier ou supprimer des clés de Registre, en plus de modifier les autorisations sur les clés de registre.

Pour plus d’informations sur le format et le contenu du fichier de script texte que Regini. exe utilise pour apporter des modifications au registre, consultez [modification des valeurs ou des autorisations du Registre à partir d’une ligne de commande ou d’un script](https://support.microsoft.com/help/264584/how-to-change-registry-values-or-permissions-from-a-command-line-or-a).

## <a name="syntax"></a>Syntaxe

```
regini [-m \\machinename | -h hivefile hiveroot][-i n] [-o outputWidth][-b] textFiles...
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |

|-m \< \\ nom_ordinateur\\>|Spécifie le nom de l’ordinateur distant avec un registre à modifier. Utilisez le  **\\ format\\ComputerName**.|
|---------------------|-|
|-h \<hivefile hiveroot >|Spécifie la ruche de registre local à modifier. Vous devez spécifier le nom du fichier Hive et la racine de la ruche au format **hivefile hiveroot**.|
|-i \<n >|Spécifie le niveau de mise en retrait à utiliser pour indiquer la structure arborescente des clés de Registre dans la sortie de la commande. L’outil **Regdmp. exe** (qui obtient les autorisations actuelles d’une clé de Registre au format binaire) utilise la mise en retrait par multiple de quatre, donc la valeur par défaut est **4**.|
|-o \<outputwidth >|Spécifie la largeur de la sortie de commande, en caractères. Si la sortie s’affiche dans la fenêtre de commande, la valeur par défaut est la largeur de la fenêtre. Si la sortie est dirigée vers un fichier, la valeur par défaut est **240** caractères.|
|-b|Spécifie que la sortie **Regini. exe** est à compatibilité descendante avec les versions précédentes de **Regini. exe**. Pour plus d’informations, consultez la section Notes.|
|Textfiles|Spécifie le nom d’un ou plusieurs fichiers texte qui contiennent des données de registre. Il est possible de répertorier un nombre quelconque de fichiers texte ANSI ou Unicode.|

## <a name="remarks"></a>Notes

Les indications suivantes s’appliquent principalement au contenu des fichiers texte qui contiennent les données de registre que vous appliquez à l’aide de **Regini. exe**.
-   Utilisez le point-virgule comme caractère de commentaire de fin de ligne. Il doit s’agir du premier caractère non vide dans une ligne.
-   Utilisez la barre oblique inverse pour indiquer la continuation d’une ligne. La commande ignore tous les caractères de la barre oblique inverse (sans inclure) le premier caractère non vide de la ligne suivante. Si vous incluez plus d’un espace avant la barre oblique inverse, il est remplacé par un seul espace.
-   Utilisez des caractères de tabulation pour contrôler la mise en retrait. Cette mise en retrait indique la structure arborescente des clés de Registre ; Toutefois, ces caractères sont convertis en un seul espace, quelle que soit leur position.

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)