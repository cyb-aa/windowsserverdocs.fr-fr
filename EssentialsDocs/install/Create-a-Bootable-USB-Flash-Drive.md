---
title: Création d'un lecteur flash de démarrage USB
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 05/04/2018
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2fe8e35c-69f9-40b3-a270-22e2402510d8
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 2716ffb7ce8f74d7c729565064de91e0598d0753
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884680"
---
# <a name="create-a-bootable-usb-flash-drive"></a>Création d'un lecteur flash de démarrage USB

>S'applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Vous pouvez créer un lecteur flash USB à utiliser pour déployer Windows Server Essentials. La première étape consiste à préparer le lecteur flash USB à l'aide de DiskPart, un utilitaire de ligne de commande. Pour plus d’informations sur DiskPart, consultez [Options de la ligne de commande DiskPart](https://go.microsoft.com/fwlink/?LinkId=207073).  


> [!TIP]
> Pour créer un lecteur flash USB pour une utilisation dans la récupération ou de réinstaller Windows sur un PC au lieu d’un serveur, consultez [créer un lecteur de récupération](https://support.microsoft.com/help/4026852/windows-create-a-recovery-drive).
  
 Pour obtenir d'autres scénarios dans lesquels vous pouvez créer ou utiliser un lecteur flash USB de démarrage, consultez les rubriques suivantes :  
  
-   [Restaurer un système complet à partir d’une sauvegarde d’ordinateur client existante](../manage/restore-a-full-system-from-an-existing-client-computer-backup.md)  
  
-   [Restaurer ou réparer votre serveur exécutant Windows Server Essentials](../manage/restore-or-repair-your-server-running-windows-server-essentials.md)  

  
### <a name="to-create-a-bootable-usb-flash-drive"></a>Pour créer un lecteur flash USB de démarrage  
  
1.  Insérez un lecteur flash USB dans un ordinateur en marche.  
  
2.  Ouvrez une fenêtre d'invite de commandes en tant qu'administrateur.  
  
3.  Entrez `diskpart`.  
  
4.  Dans la nouvelle fenêtre d'invite de commandes qui s'ouvre, entrez `list disk` dans l'invite de commande pour déterminer le numéro du lecteur flash USB ou la lettre de lecteur, puis cliquez sur ENTRER. La commande `list disk` affiche tous les disques sur l'ordinateur. Notez le numéro de disque ou de lettre de lecteur du lecteur flash USB  
  
5.  À l'invite de commande, tapez `select disk <X>`, où X est le numéro ou la lettre du lecteur flash USB, puis cliquez sur ENTRER.  
  
6.  Tapez `clean`, puis cliquez sur ENTRER. Cette commande supprime toutes les données du lecteur flash USB.  
  
7.  Pour créer une nouvelle partition principale sur le lecteur flash USB, tapez `create part pri`, puis cliquez sur ENTRER.  
  
8.  Pour sélectionner la partition que vous venez de créer, tapez `select part 1`, puis cliquez sur ENTRER.  
  
9. Pour formater la partition, tapez `format fs=ntfs quick`, puis cliquez sur ENTRER.  
  
    > [!IMPORTANT]
    >  Si votre plate-forme serveur prend en charge la Unified Extensible Firmware Interface (UEFI), vous devez formater le lecteur flash USB en FAT32 plutôt qu'en NTFS. Pour formater la partition en FAT32, tapez `format fs=fat32 quick`, puis cliquez sur ENTRER.  
  
10. Tapez `active`, puis cliquez sur ENTRER.  
  
11. Tapez `exit`, puis cliquez sur ENTRER.  
  
12. Lorsque vous avez terminé la préparation de votre image personnalisée, enregistrez-la à la racine du lecteur flash USB.  
  
## <a name="see-also"></a>Voir aussi  

 [Prise en main du ADK Windows Server Essentials](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Création et personnalisation de l’Image](Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](Additional-Customizations.md)   
 [Préparation de l’Image pour le déploiement](Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience client](Testing-the-Customer-Experience.md)   

 [Prise en main du ADK Windows Server Essentials](../install/Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Création et personnalisation de l’Image](../install/Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](../install/Additional-Customizations.md)   
 [Préparation de l’Image pour le déploiement](../install/Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience client](../install/Testing-the-Customer-Experience.md)   

 [Comment pouvons-nous vous aider ?](https://windows.microsoft.com/windows/support)
