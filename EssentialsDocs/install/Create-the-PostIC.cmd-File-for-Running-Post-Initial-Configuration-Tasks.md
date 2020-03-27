---
title: Création du fichier PostIC.cmd pour exécuter les tâches de configuration initiales
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 99e258bc-0695-48c9-b694-a7f3cbe2a2d0
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 81a38f0baf3a47323f6bf8836e48d02bc955cde0
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80312061"
---
# <a name="create-the-posticcmd-file-for-running-post-initial-configuration-tasks"></a>Création du fichier PostIC.cmd pour exécuter les tâches de configuration initiales

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Vous pouvez ajouter des personnalisations après la configuration initiale en écrivant votre propre code, puis en appelant ce code depuis un fichier de script appelé PostIC.cmd. Lorsque vous utilisez le fichier PostIC.cmd, vous devez suivre les instructions suivantes :  
  
- Votre code de personnalisation doit être exécuté en mode silencieux (il ne peut pas afficher une interface utilisateur).  
  
- Votre code de personnalisation ne peut pas procéder au redémarrage du serveur. La configuration initiale se chargera de le faire au moment de la dernière tâche.  
  
- La durée d’exécution du code de personnalisation ne doit pas dépasser trois minutes.  
  
  Configurez votre fichier PostIC.cmd de façon à renvoyer la valeur 0 si l’exécution du code réussit. Si une autre valeur est retournée, le système d’exploitation recherche un fichier appelé[SetupFailure.cmd](Create-the-PostIC.cmd-File-for-Running-Post-Initial-Configuration-Tasks.md#BKMK_SetupFailure) dans lequel figure le code à exécuter en cas d’échec du fichier PostIC.cmd. Le fichier PostIC.cmd et le fichier SetupFailure.cmd doivent être placés tous les deux dans C:\Windows\Setup\Scripts.  
  
#### <a name="to-define-post-initial-configuration-customizations"></a>Pour définir des personnalisations à la suite de la configuration initiale  
  
1.  Écrivez le code appelé à partir du script PostIC.cmd.  
  
2.  À l’aide du Bloc-notes, créez un fichier appelé PostIC.cmd et ajoutez l’appel au code que vous avez créé à l’étape 1. Assurez-vous que votre code renvoie une valeur significative.  
  
3.  Enregistrez PostIC.cmd dans C:\Windows\Setup\Scripts.  
  
4.  (Facultatif) Créez un fichier SetupFailure.cmd contenant le code à exécuter si une valeur différente de 0 est renvoyée par PostIC.cmd.  
  
###  <a name="setupfailurecmd"></a><a name="BKMK_SetupFailure"></a>Fichier setupfailure. cmd  
 Vous pouvez notifier des problèmes lors de la configuration initiale en utilisant SetupFailure.cmd. Le fichier SetupFailure.cmd contient le code que vous souhaitez exécuter si un problème se produit. Le fichier SetupFailure.cmd, placé dans C:\Windows\Setup\Scripts, est exécuté dès qu’une tâche d’installation génère une erreur ou que le fichier PostIC.cmd renvoie une valeur différente de 0.  
  
##### <a name="to-define-notifications"></a>Pour définir des notifications  
  
1.  Écrivez le code appelé à partir du script SetupFailure.cmd.  
  
2.  À l’aide du Bloc-notes, créez un fichier appelé SetupFailure.cmd et ajoutez l’appel au code que vous avez créé à l’étape 1. Assurez-vous que votre code renvoie une valeur significative.  
  
3.  Enregistrez le fichier SetupFailure.cmd dans C:\Windows\Setup\Scripts.  
  
## <a name="see-also"></a>Voir aussi  
 [Prise en main avec Windows Server Essentials ADK](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Création et personnalisation de l’Image](Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](Additional-Customizations.md)   
 [Préparation de l’image pour le déploiement](Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience utilisateur](Testing-the-Customer-Experience.md)