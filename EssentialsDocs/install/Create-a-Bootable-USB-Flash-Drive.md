---
title: "Créer un lecteur Flash USB de démarrage"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2fe8e35c-69f9-40b3-a270-22e2402510d8
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 9d587329e1141040b2511e1574649f1844dcec90
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-bootable-usb-flash-drive"></a>Créer un lecteur Flash USB de démarrage

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

Vous pouvez créer un lecteur flash USB à utiliser pour déployer WindowsServerEssentials. La première étape consiste à préparer le lecteur flash USB à l’aide de DiskPart, qui est un utilitaire de ligne de commande. Pour plus d’informations sur DiskPart, consultez [Options de ligne de commande DiskPart ](https://go.microsoft.com/fwlink/?LinkId=207073).  
  
 Pour d’autres scénarios dans lesquels vous devrez créer ou utiliser une clé de démarrage USB lecteur flash, consultez les rubriques suivantes:  
  
-   [Restaurer un système complet à partir d’une sauvegarde d’ordinateur client existante](https://technet.microsoft.com/library/jj713539.aspx#BKMK_CreateBootable)  
  
-   [Restaurer ou réparer votre serveur exécutant WindowsServerEssentials](https://technet.microsoft.com/library/jj593197.aspx#BKMK_Restore_2)  
  
### <a name="to-create-a-bootable-usb-flash-drive"></a>Pour créer un lecteur flash USB  
  
1.  Insérez un lecteur flash USB dans un ordinateur en cours d’exécution.  
  
2.  Ouvrez une fenêtre d’invite de commandes en tant qu’administrateur.  
  
3.  Type `diskpart`.  
  
4.  Dans la nouvelle fenêtre de ligne de commande qui s’ouvre, pour déterminer la mémoire flash USB numéro de disque ou la lettre de lecteur, à l’invite de commandes, tapez `list disk`, puis cliquez sur ENTRÉE. Le `list disk`commande affiche tous les disques sur l’ordinateur. Notez le numéro de disque ou la lettre de lecteur du lecteur flash USB.  
  
5.  À l’invite de commandes, tapez `select disk <X>`, où X est le numéro ou la lettre de lecteur de la souris USB lecteur flash, puis cliquez sur ENTRÉE.  
  
6.  Type `clean`, puis cliquez sur ENTRÉE. Cette commande supprime toutes les données à partir du lecteur flash USB.  
  
7.  Pour créer une nouvelle partition principale sur le lecteur flash USB, tapez `create part pri`, puis cliquez sur ENTRÉE.  
  
8.  Pour sélectionner la partition que vous venez de créer, tapez `select part 1`, puis cliquez sur ENTRÉE.  
  
9. Pour formater la partition, tapez `format fs=ntfs quick`, puis cliquez sur ENTRÉE.  
  
    > [!IMPORTANT]
    >  Si votre plate-forme serveur prend en charge la Unified Extensible Firmware Interface (UEFI), vous devez formater le lecteur flash USB en FAT32 plutôt qu’en NTFS. Pour formater la partition en FAT32, tapez `format fs=fat32 quick`, puis cliquez sur ENTRÉE.  
  
10. Type `active`, puis cliquez sur ENTRÉE.  
  
11. Type `exit`, puis cliquez sur ENTRÉE.  
  
12. Lorsque vous avez terminé la préparation de votre image personnalisée, enregistrez-le à la racine du lecteur flash USB.  
  
## <a name="see-also"></a>Voir aussi  

 [Prise en main du kit ADK WindowsServerEssentials](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Création et personnalisation de l’Image](Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](Additional-Customizations.md)   
 [Préparation de l’Image pour le déploiement](Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience client](Testing-the-Customer-Experience.md)   

 [Prise en main du kit ADK WindowsServerEssentials](../install/Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Création et personnalisation de l’Image](../install/Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](../install/Additional-Customizations.md)   
 [Préparation de l’Image pour le déploiement](../install/Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience client](../install/Testing-the-Customer-Experience.md)   

 [Comment pouvons-nous vous aider?](https://windows.microsoft.com/windows/support)
