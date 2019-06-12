---
title: PowerShell_ise
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 32c41b5b-a210-47d9-bd8c-91eb9830b4f0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a5619396e29b446dbc6804ece7444f355dae4c0a
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436301"
---
# <a name="powershellise"></a>PowerShell_ise



Windows PowerShell Integrated Scripting Environment (ISE) est une application hôte graphique qui vous permet de lire, écrire, exécuter, déboguer et tester des scripts et modules dans un environnement assisté par graphique. Clé des fonctionnalités telles qu’IntelliSense, Show-Command, des extraits de code, saisie semi-automatique par tabulation, la coloration de syntaxe, visual débogage et une aide contextuelle fournissent une riche expérience de script.

Le **PowerShell_ISE.exe** outil démarre une session Windows PowerShell ISE. Lorsque vous utilisez **PowerShell_ISE.exe**, vous pouvez utiliser des paramètres facultatifs pour ouvrir des fichiers dans Windows PowerShell ISE ou pour démarrer une session Windows PowerShell ISE sans profil ou avec un multithread cloisonné.

**PowerShell_ISE.exe** a été introduit dans Windows PowerShell 2.0 et développé de manière significative dans Windows PowerShell 3.0.

## <a name="using-powershelliseexe"></a>À l’aide de PowerShell_ISE.exe

Vous pouvez utiliser **PowerShell_ISE.exe** pour commencer et se terminer une session Windows PowerShell comme suit :
- Pour démarrer une session Windows PowerShell ISE, dans une fenêtre d’invite de commandes, dans Windows PowerShell, ou dans le menu Démarrer, tapez :  
  ```
  PowerShell_Ise
  ```  
- Pour ouvrir un script (.ps1), module de script (.psm1), le manifeste de module (.psd1), fichier XML ou tout autre fichier pris en charge dans Windows PowerShell ISE, utilisez le format de commande suivant :  
  ```
  PowerShell_Ise <FilePath>
  ```  
  Dans Windows PowerShell 3.0, vous pouvez utiliser le paramètre facultatif **fichier** paramètre comme suit :  
  ```
  PowerShell_Ise -File <FilePath>
  ```  
- Pour démarrer une session Windows PowerShell ISE sans votre profil Windows PowerShell, utilisez le **NoProfile** paramètre. (Le **NoProfile** paramètre est introduit dans Windows PowerShell 3.0.)  
  ```
  PowerShell_Ise -NoProfile
  ```  
- Pour voir les **PowerShell_ISE.exe** aide dans une fenêtre d’invite de commandes, utilisez le format de la commande suivante :  
  ```
  PowerShell_Ise -help, -?, /?
  ```  
  Pour obtenir la liste complète de la **PowerShell_ISE.exe** des paramètres de ligne de commande, consultez [about_PowerShell_Ise.exe](https://go.microsoft.com/fwlink/?LinkId=256512).

## <a name="start-windows-powershell-ise-in-other-ways"></a>Démarrez Windows PowerShell ISE par d’autres moyens

Pour plus d’informations sur les autres méthodes pour démarrer Windows PowerShell ISE, consultez [démarrage de Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=135259).

## <a name="remarks"></a>Notes

Windows PowerShell s’exécute sur l’option d’installation Server Core de systèmes d’exploitation Windows Server. Toutefois, étant donné que Windows PowerShell ISE nécessite une interface utilisateur graphique, il ne s’exécute pas sur les installations Server Core.

## <a name="additional-references"></a>Références supplémentaires

[about_PowerShell_Ise.exe](https://go.microsoft.com/fwlink/?LinkId=256512)
[about_PowerShell.exe](https://go.microsoft.com/fwlink/?LinkID=113439)
[Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=107116)
[écriture de scripts avec Windows PowerShell](https://technet.microsoft.com/scriptcenter/dd742419) Voir aussi