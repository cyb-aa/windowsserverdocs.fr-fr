---
title: PowerShell
description: Découvrez comment ouvrir la console PowerShell à partir d’une invite de commandes.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 1e2ccf6187e4480f94b30632b6f8f9f092052541
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811079"
---
# <a name="powershell"></a>PowerShell

Windows PowerShell est un interpréteur de commandes de ligne de commande basé sur les tâches et le langage de script conçu spécialement pour l’administration du système. Basé sur le .NET Framework, Windows PowerShell aide les professionnels de l’informatique et les utilisateurs avancés à contrôler et à automatiser l’administration du système d’exploitation Windows et des applications s’exécutant sur Windows.

Le **PowerShell.exe** outil de ligne de commande démarre une session Windows PowerShell dans une fenêtre d’invite de commandes. Lorsque vous utilisez **PowerShell.exe**, vous pouvez utiliser des paramètres facultatifs pour personnaliser la session. Par exemple, vous pouvez démarrer une session qui utilise une stratégie d’exécution particulière ou qui exclut un profil Windows PowerShell. Sinon, la session est identique à n’importe quelle session est démarrée dans la console Windows PowerShell.

## <a name="using-powershellexe"></a>À l’aide PowerShell.exe

Vous pouvez utiliser la **PowerShell.exe** outil de ligne de commande pour démarrer une session Windows PowerShell dans une fenêtre d’invite de commandes.

- Pour démarrer une session Windows PowerShell dans une fenêtre d’invite de commandes, tapez `PowerShell`. Un **PS** préfixe est ajouté à l’invite de commandes pour indiquer que vous êtes dans une session Windows PowerShell.

- Pour démarrer une session avec une stratégie d’exécution particulière, utilisez le **ExecutionPolicy** paramètre.

    ```
    PowerShell.exe -ExecutionPolicy Restricted
    ```

- Pour démarrer une session Windows PowerShell sans votre profil Windows PowerShell, utilisez le **NoProfile** paramètre.

    ```
    PowerShell.exe -NoProfile
    ```
  
- Pour démarrer une session, utilisez le **ExecutionPolicy** paramètre.

    ```
    PowerShell.exe -ExecutionPolicy Restricted
    ```
  
- Pour afficher le PowerShell.exe fichier d’aide, utilisez le format de commande suivant.  
    
    ```
    PowerShell.exe -help, -?, /?
    ```

- Pour mettre fin à une session Windows PowerShell dans une fenêtre d’invite de commandes, tapez `exit`. Retourne de l’invite de commandes classique.

Pour obtenir la liste complète de la **PowerShell.exe** des paramètres de ligne de commande, consultez [about_PowerShell.Exe](https://go.microsoft.com/fwlink/?LinkID=113439).

## <a name="other-ways-to-start-windows-powershell"></a>Autres façons de démarrer Windows PowerShell

Pour plus d’informations sur les autres méthodes pour démarrer Windows PowerShell, consultez [démarrage de Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=135259).

## <a name="remarks"></a>Notes

Windows PowerShell s’exécute sur l’option d’installation Server Core de systèmes d’exploitation Windows Server. Toutefois, les fonctionnalités qui nécessitent un utilisateur graphique de l’interface, comme le [Windows PowerShell Integrated Scripting Environment (ISE)](https://technet.microsoft.com/library/hh849182)et le [Out-GridView](https://go.microsoft.com/fwlink/?LinkID=113364) et [Show-Command](https://go.microsoft.com/fwlink/?LinkID=217448)applets de commande, ne s’exécutent pas sur les installations Server Core.

## <a name="additional-references"></a>Références supplémentaires

[about_PowerShell.exe](https://go.microsoft.com/fwlink/?LinkID=113439)
[about_PowerShell_Ise.exe](https://go.microsoft.com/fwlink/?LinkId=256512)
[Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=107116)
[écriture de scripts avec Windows PowerShell](https://technet.microsoft.com/scriptcenter/dd742419) Voir aussi