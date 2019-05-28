---
ms.assetid: 0379abc3-25c7-46ab-9a6b-80a5152365b0
title: Thèmes Web personnalisés dans AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2bce52a5704706ad72799d00879e2f4e48f9d703
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189248"
---
# <a name="custom-web-themes-in-ad-fs"></a>Thèmes Web personnalisés dans AD FS 

Le thème qui est livré out\-de\-le\-est appelée par défaut. Vous pouvez exporter le thème par défaut de manière à démarrer rapidement. Vous pouvez personnaliser l'apparence et le comportement, notamment la disposition par le biais du fichier .css, importer et appliquer ce nouveau thème, puis utiliser l'apparence et le comportement personnalisés. L'utilisation du fichier .css facilite par ailleurs la collaboration avec vos concepteurs web.  
  
L'applet de commande suivante crée un thème web personnalisé, qui duplique le thème web par défaut.  
  
  
`New-AdfsWebTheme –Name custom –SourceName default ` 

  
Vous pouvez modifier le fichier .css, puis utiliser ce dernier pour configurer le nouveau thème web. Pour exporter un thème web, utilisez l'applet de commande suivante.  
  

    Export-AdfsWebTheme –Name default –DirectoryPath c:\theme  

  
Pour appliquer le fichier .css au nouveau thème, utilisez l'applet de commande suivante.  
  

    Set-AdfsWebTheme –TargetName custom –StyleSheet @{path=”c:\NewTheme.css”}  
  
  
L'applet de commande suivante crée un thème web personnalisé à partir d'une nouvelle feuille de style.  
  
  
`New-AdfsWebTheme –Name custom –StyleSheet @{path=”c:\NewTheme.css”} –RTLStyleSheetPath c:\NewRtlTheme.css ` 
  
  
  
Pour appliquer le thème web personnalisé à AD FS, utilisez l’applet de commande suivante.  
  

`Set-AdfsWebConfig -ActiveThemeName custom`  

  
Pour ajouter JavaScript à AD FS, utilisez l’applet de commande suivante.  
  
 
    Set-AdfsWebTheme -TargetName custom -AdditionalFileResource @{Uri=’ /adfs/portal/script/onload.js’;path="D:\inetpub\adfsassets\script\onload.js"}  


## <a name="additional-references"></a>Références supplémentaires 
[AD FS Sign-in personnalisation de l’utilisateur](AD-FS-user-sign-in-customization.md)  
