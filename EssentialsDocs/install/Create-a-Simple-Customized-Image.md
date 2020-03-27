---
title: Création d’une image personnalisée simple
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 29f9a09f-e4e8-476d-ada1-ab9202a670d7
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 74a1729f9445710b6b9d279f1603295eff2a5d25
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80312138"
---
# <a name="create-a-simple-customized-image"></a>Création d’une image personnalisée simple

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Vous pouvez procéder de la façon suivante pour créer une image personnalisée simple :  
  
#### <a name="to-create-the-image"></a>Pour créer l’image  
  
1.  Après l'installation du serveur, appuyez à la première page de la configuration initiale sur Maj+F10 pour lancer la fenêtre cmd.  
  
2.  Créez un fichier SkipIC.txt sous la racine d’un lecteur système.  
  
3.  Redémarrez le serveur.  
  
4.  Démarrez le serveur en utilisant un lecteur flash USB ou un DVD comprenant le fichier unattend.xml. Pour plus d'informations sur la création d'un lecteur flash USB démarrable, voir [Création d'un lecteur flash USB démarrable](Create-a-Bootable-USB-Flash-Drive.md).  
  
5.  Ajoutez le logo de votre marque dans le Tableau de bord. Pour plus d’informations à ce sujet, voir [Ajout d’un logo au Tableau de bord, à l’Web Accès à distance et au Launchpad](Add-Branding-to-the-Dashboard--Remote-Web-Access--and-Launchpad.md) (peut-être en anglais).  
  
6.  Créez le fichier OOBE.xml destiné à afficher les informations personnalisées, telles que le nom de la société, le logo et le CLUF. Pour plus d’informations sur le fichier OOBE.xml, consultez [Create the Oobe.xml File Including Logo and EULA](Create-the-Oobe.xml-File-Including-Logo-and-EULA.md).  
  
7.  Modifier le nom du serveur par défaut si vous ne le définissez pas dans le fichier unattend.xml.  
  
8.  Par défaut, le nom de serveur sera une chaîne de caractères quelconque. Modifiez le nom du serveur en une autre chaîne de caractères (comme par exemple ContosoServer), puis informez vos clients du nouveau nom de serveur.  
  
9. Préparez l’image en vue du déploiement comme décrit à la rubrique [Préparation de l’image en vue du déploiement](Preparing-the-Image-for-Deployment.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Prise en main avec Windows Server Essentials ADK](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Création et personnalisation de l’Image](Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](Additional-Customizations.md)   
 [Préparation de l’image pour le déploiement](Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience utilisateur](Testing-the-Customer-Experience.md)