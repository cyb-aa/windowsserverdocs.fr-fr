---
title: Vue scwcmd
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7995959a-d93e-4865-a6a0-2ab18c2bb47f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a69db16696f42950af97b62ba6f28c4083954137
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371186"
---
# <a name="scwcmd-view"></a>Scwcmd: view

> S'applique à : Windows Server 2012 R2, Windows Server 2012

Génère le rendu d’un fichier. XML à l’aide d’une transformation. XSL spécifiée. Cette commande peut être utile pour afficher des fichiers. XML de l’Assistant Configuration de la sécurité (SCW) à l’aide de différentes vues.

## <a name="syntax"></a>Syntaxe

```
scwcmd view /x:<Xmlfile.xml> [/s:<Xslfile.xsl>]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/x : @no__t -0Xmlfile. Xml >|Spécifie le fichier. XML à afficher. Ce paramètre doit être spécifié.|
|/s : @no__t -0Xslfile. xsl >|Spécifie la transformation. xsl à appliquer au fichier. xml dans le cadre du processus de rendu. Ce paramètre est facultatif pour les fichiers SCW. Xml. Lorsque la commande **View** est utilisée pour afficher un fichier SCW. xml, elle essaie automatiquement de charger la transformation appropriée par défaut pour le fichier. XML spécifié. Si une transformation. xsl est spécifiée, la transformation doit être écrite en supposant que le fichier. xml se trouve dans le même répertoire que la transformation. Xsl.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

Scwcmd. exe est disponible uniquement sur les ordinateurs exécutant Windows Server 2008 R2, Windows Server 2008 ou Windows Server 2003.

## <a name="BKMK_Examples"></a>Illustre

Pour afficher policyFile. XML à l’aide de la transformation Policyview. xsl, tapez :
```
scwcmd view /x:C:\policies\Policyfile.xml /s:C:\viewers\Policyview.xsl
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)