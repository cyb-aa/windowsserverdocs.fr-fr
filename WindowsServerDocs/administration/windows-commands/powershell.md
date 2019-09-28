---
title: PowerShell
description: Découvrez comment ouvrir la console PowerShell à partir d’une invite de commandes.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 694fc970-0b6c-4046-b1b5-7eb1a0d26609
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 2c43c71fce9bb25efcf3f03284160d5534475a8a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372198"
---
# <a name="powershell"></a>PowerShell

Windows PowerShell est un interpréteur de ligne de commande basé sur des tâches et un langage de script conçu spécialement pour l’administration du système. Basé sur le .NET Framework, Windows PowerShell aide les professionnels de l’informatique et les utilisateurs avancés à contrôler et à automatiser l’administration du système d’exploitation Windows et des applications s’exécutant sur Windows.

L’outil en ligne de commande **PowerShell. exe** démarre une session Windows PowerShell dans une fenêtre d’invite de commandes. Lorsque vous utilisez **PowerShell. exe**, vous pouvez utiliser ses paramètres facultatifs pour personnaliser la session. Par exemple, vous pouvez démarrer une session qui utilise une stratégie d’exécution particulière ou une autre qui exclut un profil Windows PowerShell. Dans le cas contraire, la session est la même qu’une session démarrée dans la console Windows PowerShell.

## <a name="using-powershellexe"></a>Utilisation de PowerShell. exe

Vous pouvez utiliser l’outil en ligne de commande **PowerShell. exe** pour démarrer une session Windows PowerShell dans une fenêtre d’invite de commandes.

- Pour démarrer une session Windows PowerShell dans une fenêtre d’invite de commandes, tapez `PowerShell`. Un préfixe **PS** est ajouté à l’invite de commandes pour indiquer que vous êtes dans une session Windows PowerShell.

- Pour démarrer une session avec une stratégie d’exécution particulière, utilisez le paramètre **ExecutionPolicy** .

    ```
    PowerShell.exe -ExecutionPolicy Restricted
    ```

- Pour démarrer une session Windows PowerShell sans vos profils Windows PowerShell, utilisez le paramètre **noprofile** .

    ```
    PowerShell.exe -NoProfile
    ```
  
- Pour démarrer une session, utilisez le paramètre **ExecutionPolicy** .

    ```
    PowerShell.exe -ExecutionPolicy Restricted
    ```
  
- Pour afficher le fichier d’aide PowerShell. exe, utilisez le format de commande suivant.  
    
    ```
    PowerShell.exe -help, -?, /?
    ```

- Pour mettre fin à une session Windows PowerShell dans une fenêtre d’invite de commandes, tapez `exit`. L’invite de commandes classique retourne.

Pour obtenir la liste complète des paramètres de ligne de commande **PowerShell. exe** , consultez [about_PowerShell. exe](https://go.microsoft.com/fwlink/?LinkID=113439).

## <a name="other-ways-to-start-windows-powershell"></a>Autres façons de démarrer Windows PowerShell

Pour plus d’informations sur les autres façons de démarrer Windows PowerShell, consultez [démarrage de Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=135259).

## <a name="remarks"></a>Notes

Windows PowerShell s’exécute sur l’option d’installation minimale des systèmes d’exploitation Windows Server. Toutefois, les fonctionnalités qui requièrent une interface utilisateur graphique, telles que le [environnement d’écriture de scripts intégré de Windows PowerShell (ISE)](https://technet.microsoft.com/library/hh849182), les applets de commande [Out-GridView](https://go.microsoft.com/fwlink/?LinkID=113364) et [Show-Command](https://go.microsoft.com/fwlink/?LinkID=217448) , ne s’exécutent pas sur les installations minimales.

## <a name="additional-references"></a>Références supplémentaires

[about_PowerShell. exe](https://go.microsoft.com/fwlink/?LinkID=113439)
[about_PowerShell_Ise. exe](https://go.microsoft.com/fwlink/?LinkId=256512)
 scripts[Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=107116)
[avec Windows PowerShell](https://technet.microsoft.com/scriptcenter/dd742419) Voir aussi