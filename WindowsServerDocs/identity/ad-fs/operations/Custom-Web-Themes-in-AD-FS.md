---
ms.assetid: 0379abc3-25c7-46ab-9a6b-80a5152365b0
title: "Thèmes Web personnalisés dans ADFS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 02/09/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 663623cf34fcfe715080945ebcc135587b5ddba1
ms.sourcegitcommit: 877a50cd8d6e727048cdfac9b614a98ac3220876
ms.translationtype: HT
ms.contentlocale: fr-FR
---
# <a name="custom-web-themes-in-ad-fs"></a>Thèmes Web personnalisés dans ADFS 

>S’applique à: Windows Server2016, Windows Server2012R2

Le thème est expédié out-maximum\-programme-box est appelé par défaut. Vous pouvez exporter le thème par défaut et l’utiliser afin que vous pouvez démarrer rapidement. Vous pouvez personnaliser l’apparence et le comportement, notamment la disposition en modifiant le fichier .css, importer et appliquer ce nouveau thème, vous pouvez utiliser l’apparence personnalisée et le comportement. En utilisant le fichier .css facilite également travailler avec vos concepteurs web.  
  
L’applet de commande suivant crée un thème web personnalisé, qui duplique le thème web par défaut.  
  
  
`New-AdfsWebTheme –Name custom –SourceName default ` 

  
Vous pouvez modifier le fichier .css et configurer le nouveau thème web à l’aide du nouveau fichier .css. Pour exporter un thème web, utilisez l’applet de commande suivant.  
  

    Export-AdfsWebTheme –Name default –DirectoryPath c:\theme  

  
Pour appliquer le fichier .css au nouveau thème, utilisez l’applet de commande suivant.  
  

    Set-AdfsWebTheme –TargetName custom –StyleSheet @{path=”c:\NewTheme.css”}  
  
  
L’applet de commande suivant crée un thème web personnalisé à partir d’une feuille de style.  
  
  
`New-AdfsWebTheme –Name custom –StyleSheet @{path=”c:\NewTheme.css”} –RTLStyleSheetPath c:\NewRtlTheme.css ` 
  
  
  
Pour appliquer le thème web personnalisé pour ADFS, utilisez l’applet de commande suivant.  
  

`Set-AdfsWebConfig -ActiveThemeName custom`  

  
Pour ajouter JavaScript à ADFS, utilisez l’applet de commande suivant.  
  
 
    Set-AdfsWebTheme -TargetName custom -AdditionalFileResource @{Uri=’ /adfs/portal/script/onload.js’;path="D:\inetpub\adfsassets\script\onload.js"}  


## <a name="additional-references"></a>Références supplémentaires 
[ADFS Sign-in personnalisation de l’utilisateur](AD-FS-user-sign-in-customization.md)  
