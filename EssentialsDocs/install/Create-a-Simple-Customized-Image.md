---
title: "Créer une Image personnalisée Simple"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 29f9a09f-e4e8-476d-ada1-ab9202a670d7
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: e18ff5ded94127449072d28d00b98e17dbe63c3a
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="create-a-simple-customized-image"></a>Créer une Image personnalisée Simple

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

Vous pouvez utiliser la procédure suivante pour créer une image personnalisée simple:  
  
#### <a name="to-create-the-image"></a>Pour créer l’image  
  
1.  Après l’installation du serveur, à la première page de la Configuration initiale, appuyez sur MAJ + F10 pour lancer la fenêtre cmd.  
  
2.  Créez un fichier SkipIC.txt sous la racine du lecteur système.  
  
3.  Redémarrez le serveur.  
  
4.  Démarrez le serveur à l’aide d’un disque mémoire flash USB ou un DVD, qui comprend le fichier unattend.xml. Pour plus d’informations sur la création d’un lecteur flash USB, consultez [créer un lecteur Flash de démarrage USB](Create-a-Bootable-USB-Flash-Drive.md).  
  
5.  Ajout d’un logo logo au tableau de bord. Pour plus d’informations sur l’ajout d’un logo, consultez [Add Branding au tableau de bord, accès Web à distance et Launchpad](Add-Branding-to-the-Dashboard--Remote-Web-Access--and-Launchpad.md).  
  
6.  Créer le fichier OOBE.xml pour afficher des informations personnalisées, telles que le nom de la société, logo et le CLUF. Pour plus d’informations sur le fichier OOBE.xml, voir [créer le Oobe.xml File Including Logo et le CLUF](Create-the-Oobe.xml-File-Including-Logo-and-EULA.md).  
  
7.  Modifier le nom du serveur par défaut si vous ne le définissez pas dans unattend.xml.  
  
8.  Par défaut, le nom du serveur sera une chaîne de caractères quelconque. Modifier le nom du serveur à une autre chaîne (par exemple, ContosoServer), puis informez vos clients sur le nouveau nom du serveur.  
  
9. Préparez l’Image pour le déploiement comme décrit dans [préparation de l’Image pour le déploiement](Preparing-the-Image-for-Deployment.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Prise en main du kit ADK WindowsServerEssentials](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Création et personnalisation de l’Image](Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](Additional-Customizations.md)   
 [Préparation de l’Image pour le déploiement](Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience client](Testing-the-Customer-Experience.md)