---
title: "Créer le fichier PostIC.cmd pour exécuter les tâches de Configuration initiales"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 99e258bc-0695-48c9-b694-a7f3cbe2a2d0
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: f5042204cd189e3101f5e0126fd98e786a49032d
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="create-the-posticcmd-file-for-running-post-initial-configuration-tasks"></a>Créer le fichier PostIC.cmd pour exécuter les tâches de Configuration initiales

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

Vous pouvez ajouter des personnalisations de la configuration initiale en écrivant votre propre code, puis en appelant ce code à partir d’un fichier de script appelé PostIC.cmd. Lorsque vous utilisez le fichier PostIC.cmd, vous devez respecter les recommandations suivantes:  
  
-   Votre code de personnalisation doit s’exécuter en mode silencieux (il ne peut pas afficher une Interface utilisateur).  
  
-   Votre code de personnalisation ne peuvent pas lancer un redémarrage du serveur. La Configuration initiale va redémarrer le serveur en tant que la dernière tâche.  
  
-   Votre code de personnalisation doit s’exécuter dans trois minutes ou moins.  
  
 Définir votre fichier PostIC.cmd pour renvoyer la valeur 0 si le code s’exécute correctement. Si une autre valeur est retournée, le système d’exploitation recherche un fichier nommé [SetupFailure.cmd](Create-the-PostIC.cmd-File-for-Running-Post-Initial-Configuration-Tasks.md#BKMK_SetupFailure), qui contient le code qui doit être exécutée si le code dans le fichier PostIC.cmd ne s’exécute pas correctement. Fichier PostIC.cmd et le fichier SetupFailure.cmd doivent être situés C:\Windows\Setup\Scripts.  
  
#### <a name="to-define-post-initial-configuration-customizations"></a>Pour définir des personnalisations de la configuration initiale  
  
1.  Écrivez le code qui est appelé à partir du script PostIC.cmd.  
  
2.  À l’aide du bloc-notes, créez un fichier appelé PostIC.cmd et ajoutez l’appel au code que vous avez créé à l’étape1. Assurez-vous que votre code renvoie une valeur significative.  
  
3.  Enregistrez PostIC.cmd dans C:\Windows\Setup\Scripts.  
  
4.  (Facultatif) Créez un fichier SetupFailure.cmd qui exécute le code si PostIC.cmd renvoie 0.  
  
###  <a name="BKMK_SetupFailure"></a>SetupFailure.cmd  
 Vous pouvez notifier des problèmes lors de la Configuration initiale en utilisant SetupFailure.cmd. Le fichier SetupFailure.cmd contient le code que vous souhaitez exécuter si des problèmes surviennent. Le fichier SetupFailure.cmd est placé dans C:\Windows\Setup\Scripts et s’exécute lorsqu’un problème se produit avec une tâche de configuration ou lorsque le fichier PostIC.cmd renvoie une valeur différente de 0.  
  
##### <a name="to-define-notifications"></a>Pour définir des notifications  
  
1.  Écrivez le code qui est appelé à partir du script SetupFailure.cmd.  
  
2.  À l’aide du bloc-notes, créez un fichier appelé SetupFailure.cmd et ajoutez l’appel au code que vous avez créé à l’étape1. Assurez-vous que votre code renvoie une valeur significative.  
  
3.  Enregistrer les C:\Windows\Setup\Scripts SetupFailure.cmd.  
  
## <a name="see-also"></a>Voir aussi  
 [Prise en main du kit ADK WindowsServerEssentials](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Création et personnalisation de l’Image](Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](Additional-Customizations.md)   
 [Préparation de l’Image pour le déploiement](Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience client](Testing-the-Customer-Experience.md)