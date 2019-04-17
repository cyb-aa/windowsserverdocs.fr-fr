---
title: Informations importantes pour utiliser le kit ADK WindowsServerEssentials
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 26cb2992-1250-4672-98ee-8b870baa45d5
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 4dec1fdf01538ca119b991675f932d2d8ec1e097
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="important-information-for-using-the-windows-server-essentials-adk"></a>Informations importantes pour utiliser le kit ADK WindowsServerEssentials

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

Pour créer et personnaliser une image de WindowsServerEssentials, vous utilisez plusieurs outils dans le [Windows8 ADK](https://go.microsoft.com/fwlink/?LinkId=248647), mais il existe certaines différences importantes entre Windows ADK 8 et WindowsServerEssentialsADK.  
  
 Vous devez être attentif aux différences importantes suivantes:  
  
-   Certains paramètres ont été modifiés dans **%windir%\setup\script\SetupComplete.cmd**. Si vous souhaitez utiliser cette commande, vous pouvez ajouter des lignes de commande supplémentaires, mais ne supprimez pas les lignes existantes.  
  
## <a name="working-with-passwords"></a>Utilisation de mots de passe  
  
-   Le mot de passe d’administrateur est défini sur Admin@123et ouverture de session automatique est activée dans le Install.wim\unattend.xml. Par conséquent, vous n’avez pas besoin de retapez le mot de passe plusieurs fois pendant la configuration initiale du serveur. Si vous avez un personnalisée unattend.xml à la racine du support amovible, ces paramètres seront remplacés, et vous devez définir le mot de passe et vous connecter au démarrent.  
  
-   Lors de la Configuration initiale, l’utilisateur final est invité à créer un nouveau compte et un mot de passe. Ce compte devient le compte d’administrateur réseau pour le système d’exploitation. La connexion administrateur de compte et automatique est désactivée puis. Vous pouvez automatiser ce processus à l’aide du fichier cfg.ini pour le test d’assurance qualité.  
  
-   Reportez-vous à la [Windows8 ADK](https://go.microsoft.com/fwlink/?LinkId=248694) documentation pour plus d’informations sur la création d’un fichier unattend.xml.  
  
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

