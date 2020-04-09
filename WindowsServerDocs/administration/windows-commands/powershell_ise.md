---
title: PowerShell_ise
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 32c41b5b-a210-47d9-bd8c-91eb9830b4f0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b6ae96dcd40c894e0a528c06b461173f626fb2d2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80837392"
---
# <a name="powershell_ise"></a>PowerShell_ise



Environnement d’écriture de scripts intégré de Windows PowerShell (ISE) est une application hôte graphique qui vous permet de lire, d’écrire, d’exécuter, de déboguer et de tester des scripts et des modules dans un environnement assisté par graphique. Les fonctionnalités clés, telles qu’IntelliSense, les commandes Show-Command, les extraits de code, la saisie semi-automatique par tabulation, la coloration syntaxique, le débogage visuel et l’aide contextuelle, fournissent une expérience de script riche.

L’outil **PowerShell_ISE. exe** démarre une session Windows PowerShell ISE. Quand vous utilisez **PowerShell_ISE. exe**, vous pouvez utiliser ses paramètres facultatifs pour ouvrir des fichiers dans Windows PowerShell ISE ou pour démarrer une session de Windows PowerShell ISE sans profil ou avec un cloisonnement multithread.

**PowerShell_ISE. exe** a été introduit dans windows PowerShell 2,0 et développé de manière significative dans windows PowerShell 3,0.

## <a name="using-powershell_iseexe"></a>Utilisation de PowerShell_ISE. exe

Vous pouvez utiliser **PowerShell_ISE. exe** pour démarrer et terminer une session Windows PowerShell comme suit :
- Pour démarrer une session Windows PowerShell ISE, dans une fenêtre d’invite de commandes, dans Windows PowerShell ou dans le menu Démarrer, tapez :  
  ```
  PowerShell_Ise
  ```  
- Pour ouvrir un script (. ps1), un module de script (. psm1), un manifeste de module (. psd1), un fichier XML ou tout autre fichier pris en charge dans Windows PowerShell ISE, utilisez le format de commande suivant :  
  ```
  PowerShell_Ise <FilePath>
  ```  
  Dans Windows PowerShell 3,0, vous pouvez utiliser le paramètre de **fichier** facultatif comme suit :  
  ```
  PowerShell_Ise -File <FilePath>
  ```  
- Pour démarrer une session de Windows PowerShell ISE sans vos profils Windows PowerShell, utilisez le paramètre **noprofile** . (Le paramètre **noprofile** est introduit dans Windows PowerShell 3,0.)  
  ```
  PowerShell_Ise -NoProfile
  ```  
- Pour afficher le fichier d’aide **PowerShell_ISE. exe** dans une fenêtre d’invite de commandes, utilisez le format de commande suivant :  
  ```
  PowerShell_Ise -help, -?, /?
  ```  
  Pour obtenir la liste complète des paramètres de ligne de commande **PowerShell_ISE. exe** , consultez [about_PowerShell_Ise. exe](https://go.microsoft.com/fwlink/?LinkId=256512).

## <a name="start-windows-powershell-ise-in-other-ways"></a>Démarrer Windows PowerShell ISE d’une autre manière

Pour plus d’informations sur les autres façons de démarrer Windows PowerShell ISE, consultez [démarrage de Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=135259).

## <a name="remarks"></a>Notes

Windows PowerShell s’exécute sur l’option d’installation minimale des systèmes d’exploitation Windows Server. Toutefois, étant donné que Windows PowerShell ISE nécessite une interface graphique utilisateur, il ne s’exécute pas sur les installations minimales.

## <a name="additional-references"></a>Références supplémentaires

[about_PowerShell_Ise. exe](https://go.microsoft.com/fwlink/?LinkId=256512)
[about_PowerShell. exe](https://go.microsoft.com/fwlink/?LinkID=113439)
l' [écriture de scripts](https://technet.microsoft.com/scriptcenter/dd742419) [Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=107116)
avec Windows PowerShell, voir aussi