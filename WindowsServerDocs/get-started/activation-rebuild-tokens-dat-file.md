---
title: Regénérer le fichier Tokens.dat
description: Explique comment regénérer le fichier Tokens.dat quand vous résolvez des problèmes d’activation de Windows.
ms.topic: troubleshooting
ms.date: 09/18/2019
ms.technology: server-general
author: Teresa-Motiv
ms.author: v-tea
manager: dcscontentpm
ms.localizationpriority: medium
ms.openlocfilehash: 8a5835cd601b2eb327c8605d70bf075e6c8e8414
ms.sourcegitcommit: 9855d6b59b1f8722f39ae74ad373ce1530da0ccf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/04/2019
ms.locfileid: "71962995"
---
# <a name="rebuild-the-tokensdat-file"></a>Regénérer le fichier Tokens.dat

Lors de la résolution des problèmes d’activation de Windows, vous pouvez être amené à regénérer le fichier Tokens.dat. Cet article décrit en détail comment procéder.

## <a name="resolution"></a>Résolution

Pour regénérer le fichier Tokens.dat, effectuez les étapes suivantes :

1. Ouvrez une fenêtre d’invite de commandes avec élévation de privilèges :  
   **Pour Windows 10**

   1. Ouvrez le menu **Démarrer**, puis tapez **cmd**.
   1. Dans les résultats de la recherche, cliquez sur **Invite de commandes**, puis sélectionnez **Exécuter en tant qu’administrateur**.  

   **Pour Windows 8.1**
   1. Balayez à partir du bord droit de l’écran, puis appuyez sur **Rechercher**. Ou, si vous utilisez une souris, pointez en bas à droite de l’écran, puis sélectionnez **Rechercher**.
   1. Dans la zone de recherche, tapez **cmd**.
   1. Balayez l’écran ou cliquez avec le bouton droit sur l’icône d’**invite de commandes** affichée.
   1. Appuyez ou cliquez sur **Exécuter en tant qu’administrateur**.

   **Pour Windows 7**
   1. Ouvrez le menu **Démarrer**, puis tapez **cmd**.
   1. Dans les résultats de la recherche, cliquez avec le bouton droit sur **cmd.exe** et sélectionnez **Exécuter en tant qu’administrateur**.

1. Entrez la liste des commandes qui conviennent à votre système d’exploitation.  

   Pour Windows 10, Windows Server 2016 et ultérieur de Windows, entrez les commandes suivantes dans l’ordre :
   ```cmd
   net stop sppsvc
   cd %windir%\system32\spp\store\2.0
   ren tokens.dat tokens.bar
   net start sppsvc
   cscript.exe %windir%\system32\slmgr.vbs /rilc
   ```
   Pour Windows 8.1, Windows Server 2012 et Windows Server 2012 R2, entrez les commandes suivantes dans l’ordre :
   ```cmd
   net stop sppsvc
   cd %windir%\ServiceProfiles\LocalService\AppData\Local\Microsoft\WSLicense
   ren tokens.dat tokens.bar
   net start sppsvc
   cscript.exe %windir%\system32\slmgr.vbs /rilc
   ```
   Pour Windows 7, Windows Server 2008 et Windows Server 2008 R2, entrez les commandes suivantes dans l’ordre :
   ```cmd
   net stop sppsvc
   cd %windir%\ServiceProfiles\NetworkService\AppData\Roaming\Microsoft\SoftwareProtectionPlatform
   ren tokens.dat tokens.bar
   net start sppsvc
   cscript.exe %windir%\system32\slmgr.vbs /rilc
   ```
1. Redémarrez l’ordinateur.

## <a name="more-information"></a>Informations supplémentaires

Après avoir regénéré le fichier Tokens.dat, vous devez réinstaller votre clé de produit en appliquant l’une des méthodes suivantes :

- À la même invite de commandes avec élévation de privilèges, tapez la commande suivante, puis appuyez sur Entrée :

   ```cmd
   cscript.exe %windir%\system32\slmgr.vbs /ipk <Product key>
   ```

  > [!IMPORTANT]
  > N’utilisez pas le commutateur **/upk** pour désinstaller une clé de produit. Pour installer une clé de produit par-dessus une clé de produit existante, utilisez le commutateur **/ipk**.
- Cliquez avec le bouton droit sur **Poste de travail**, sélectionnez **Propriétés**, puis **Modifier la clé de produit (Product Key)** .

Pour plus d’informations sur les clés d’installation de client KMS, consultez [Clés d’installation du client KMS](kmsclientkeys.md).
