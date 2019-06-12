---
title: regini
description: Découvrez comment modifier le Registre à partir de l’invite de commandes ou en utilisant un script.
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
ms.openlocfilehash: e3f8454572b662c9327aeb4783c5e9651ad2022b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441889"
---
# <a name="regini"></a>regini

Modifie le Registre à partir de la ligne de commande ou un script et applique les modifications qui ont été prédéfinies dans un ou plusieurs fichiers texte. Vous pouvez créer, modifier ou supprimer des clés de Registre, en plus de modifier les autorisations sur les clés de Registre.

Pour plus d’informations sur le format et le contenu du fichier de script de texte Regini.exe utilise pour apporter des modifications au Registre, consultez [comment modifier les valeurs de Registre ou autorisations à partir d’une ligne de commande ou un script](https://support.microsoft.com/help/264584/how-to-change-registry-values-or-permissions-from-a-command-line-or-a).

## <a name="syntax"></a>Syntaxe

```
regini [-m \\machinename | -h hivefile hiveroot][-i n] [-o outputWidth][-b] textFiles...
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |

|-m \<\\\\ComputerName>|Spécifie le nom de l’ordinateur distant avec un Registre qui doit être modifié. Utilisez le format  **\\ \\Nom_Ordinateur**.|
|---------------------|-|
|-h \<hivefile Racine_Ruche >|Spécifie la ruche du Registre local à modifier. Vous devez spécifier le nom du fichier ruche et la racine de la ruche dans le format **hivefile Racine_Ruche**.|
|-i \<n>|Spécifie le niveau de mise en retrait à utiliser pour indiquer la structure arborescente des clés de Registre dans la sortie de commande. Le **Regdmp.exe** outil (qui obtient un Registre autorisations actuelles de la clé au format binaire) utilise la mise en retrait en multiples de quatre, donc la valeur par défaut est **4**.|
|-o \<outputwidth>|Spécifie la largeur de la sortie de commande, en caractères. Si la sortie s’affiche dans la fenêtre de commande, la valeur par défaut est la largeur de la fenêtre. Si la sortie est dirigée vers un fichier, la valeur par défaut est **240** caractères.|
|-b|Spécifie que **Regini.exe** sortie est à compatibilité descendante avec les versions précédentes de **Regini.exe**. Consultez la section Notes pour plus d’informations.|
|textfiles|Spécifie le nom d’un ou plusieurs fichiers texte qui contiennent des données de Registre. N’importe quel nombre de fichiers de texte ANSI ou Unicode peut être répertorié.|

## <a name="remarks"></a>Notes

Les instructions suivantes s’appliquent principalement pour le contenu des fichiers texte qui contiennent des données de Registre que vous appliquez à l’aide de **Regini.exe**.
-   Utilisez le point-virgule comme un caractère de commentaire de fin de ligne. Il doit être le premier caractère non vide dans une ligne.
-   Utilisez la barre oblique inverse pour indiquer la continuation d’une ligne. La commande ignore tous les caractères à partir de la barre oblique inverse jusqu'à (mais sans inclure) le premier caractère non vide de la ligne suivante. Si vous incluez plusieurs espaces avant la barre oblique inverse, il est remplacé par un espace unique.
-   Utilisez dur tabulations pour contrôler la mise en retrait. Cette mise en retrait indique la structure arborescente des clés de Registre ; Toutefois, ces caractères sont convertis en un seul espace, quel que soit leur position.

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)