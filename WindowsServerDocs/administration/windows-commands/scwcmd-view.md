---
title: Scwcmd vue
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 2ef1dd72903108edd6c5fb450c536a9325fcf546
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889550"
---
# <a name="scwcmd-view"></a>Scwcmd: view

> S'applique à : Windows Server 2012 R2, Windows Server 2012

Restitue un fichier .xml à l’aide d’une transformation XSL spécifié. Cette commande peut être utile pour l’affichage des fichiers .xml d’Assistant de Configuration de sécurité (SCW) à l’aide des vues différentes.

## <a name="syntax"></a>Syntaxe

```
scwcmd view /x:<Xmlfile.xml> [/s:<Xslfile.xsl>]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/x:\<Xmlfile.xml>|Spécifie le fichier .xml à afficher. Ce paramètre doit être spécifié.|
|/ s:\<Xslfile.xsl >|Spécifie la transformation XSL à appliquer au fichier .xml dans le cadre du processus de rendu. Ce paramètre est facultatif pour les fichiers .xml SCW. Lorsque le **vue** commande est utilisée pour restituer un fichier .xml de l’Assistant, il tente automatiquement de chargement de la transformation par défaut approprié pour le fichier .xml spécifié. Si une transformation XSL est spécifiée, la transformation doit être écrites en supposant que le fichier .xml est dans le même répertoire que la transformation XSL.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

Scwcmd.exe est uniquement disponible sur les ordinateurs exécutant Windows Server 2008 R2, Windows Server 2008 ou Windows Server 2003.

## <a name="BKMK_Examples"></a>Exemples

Pour afficher les Policyfile.xml à l’aide de la transformation Policyview.xsl, tapez :
```
scwcmd view /x:C:\policies\Policyfile.xml /s:C:\viewers\Policyview.xsl
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)