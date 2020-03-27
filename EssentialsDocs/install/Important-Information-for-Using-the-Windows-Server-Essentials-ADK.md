---
title: Informations importantes sur l'utilisation du Kit de déploiement et d'évaluation Windows Server Essentials
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 26cb2992-1250-4672-98ee-8b870baa45d5
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 9a06f3b6431ae6079869e1d7fe9bc3f0ef5e597b
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80311744"
---
# <a name="important-information-for-using-the-windows-server-essentials-adk"></a>Informations importantes sur l'utilisation du Kit de déploiement et d'évaluation Windows Server Essentials

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Pour créer et personnaliser une image de Windows Server Essentials, vous utilisez un grand nombre d’outils dans [Windows 8 ADK](https://go.microsoft.com/fwlink/?LinkId=248647), mais il existe des différences importantes entre Windows 8 ADK et Windows Server Essentials ADK.  
  
 Voici les principales différences à connaître :  
  
-   Certains paramètres ont été modifiés dans **%windir%\setup\script\SetupComplete.cmd**. Si vous souhaitez utiliser cette commande, vous pouvez ajouter des lignes de commande supplémentaires, mais pas supprimer les lignes existantes.  
  
## <a name="working-with-passwords"></a>Utilisation de mots de passe  
  
-   Le mot de passe de l’administrateur est défini sur Admin@123 et l’ouverture de session automatique est activée dans le wim\unattend.Xml. d’installation. Pour cette raison, vous n’avez pas à taper le mot de passe plusieurs fois pendant la configuration initiale du serveur. Si vous avez un fichier unattend.xml personnalisé dans la racine d'un support amovible, ces paramètres seront remplacés, et vous devrez définir le mot de passe et vous connecter au démarrage.  
  
-   Au cours de la configuration initiale, un message demande à l’utilisateur final de définir un nouveau compte et un mot de passe. Ce compte devient le compte d’administrateur réseau pour le système d’exploitation. Le compte Administrateur et l’ouverture de session automatique sont ensuite désactivés. Vous pouvez automatiser ce processus à l’aide du fichier cfg.ini pour effectuer un test d’assurance qualité.  
  
-   Pour plus d’informations sur la création d’un fichier unattend.xml, consultez la documentation du [Kit de déploiement et d’évaluation Windows 8](https://go.microsoft.com/fwlink/?LinkId=248694) .  
  
## <a name="see-also"></a>Voir aussi  

 [Prise en main avec Windows Server Essentials ADK](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Création et personnalisation de l’Image](Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](Additional-Customizations.md)   
 [Préparation de l’image pour le déploiement](Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience utilisateur](Testing-the-Customer-Experience.md)

 [Prise en main avec Windows Server Essentials ADK](../install/Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Création et personnalisation de l’Image](../install/Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](../install/Additional-Customizations.md)   
 [Préparation de l’image pour le déploiement](../install/Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience utilisateur](../install/Testing-the-Customer-Experience.md)

