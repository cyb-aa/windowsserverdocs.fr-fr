---
title: Vue scwcmd
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7995959a-d93e-4865-a6a0-2ab18c2bb47f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 36c6422a0118b0c6d6d70adbadfb401532121c3f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80835062"
---
# <a name="scwcmd-view"></a>Scwcmd: view

> S’applique à : Windows Server 2012 R2, Windows Server 2012

Génère le rendu d’un fichier. XML à l’aide d’une transformation. XSL spécifiée. Cette commande peut être utile pour afficher des fichiers. XML de l’Assistant Configuration de la sécurité (SCW) à l’aide de différentes vues.

## <a name="syntax"></a>Syntaxe

```
scwcmd view /x:<Xmlfile.xml> [/s:<Xslfile.xsl>]
```

#### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/x :\<xmlfile. Xml >|Spécifie le fichier. XML à afficher. Ce paramètre doit être spécifié.|
|/s :\<xslfile. xsl >|Spécifie la transformation. xsl à appliquer au fichier. xml dans le cadre du processus de rendu. Ce paramètre est facultatif pour les fichiers SCW. Xml. Lorsque la commande **View** est utilisée pour afficher un fichier SCW. xml, elle essaie automatiquement de charger la transformation appropriée par défaut pour le fichier. XML spécifié. Si une transformation. xsl est spécifiée, la transformation doit être écrite en supposant que le fichier. xml se trouve dans le même répertoire que la transformation. Xsl.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

Scwcmd. exe est disponible uniquement sur les ordinateurs exécutant Windows Server 2008 R2, Windows Server 2008 ou Windows Server 2003.

## <a name="examples"></a><a name=BKMK_Examples></a>Illustre

Pour afficher policyFile. XML à l’aide de la transformation Policyview. xsl, tapez :
```
scwcmd view /x:C:\policies\Policyfile.xml /s:C:\viewers\Policyview.xsl
```

## <a name="additional-references"></a>Références supplémentaires

-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)