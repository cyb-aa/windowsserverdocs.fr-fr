---
title: "Personnalisation de connexion tâche du Service de sauvegarde en ligne Microsoft"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a7eafbb3-7728-487e-b287-90bbd6fee7f0
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: cd148e0e58cd80dbff7f7884ead95dc1e46b6257
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="customize-sign-up-for-microsoft-online-backup-service-task"></a>Personnalisation de connexion tâche du Service de sauvegarde en ligne Microsoft

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

Par défaut, le **s’inscrire pour le Service de sauvegarde en ligne Microsoft** de tâches sur le **périphériques** onglet du tableau de bord s’ouvre le site Web du Service de sauvegarde en ligne Microsoft. Le site Web fournit des informations sur le service et vous permet de vous abonner au service et télécharger le logiciel requis.  
  
 Vous pouvez personnaliser le **s’inscrire pour le Service de sauvegarde en ligne Microsoft** tâches de deux manières:  
  
-   Vous pouvez remplacer l’URL pour le site Web par défaut avec une URL qui représente une expérience utilisateur personnalisée. Pour remplacer l’URL par défaut, ouvrez l’Éditeur du Registre, créez la clé de Registre: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\OnlineBackup\LinkUrl**, puis affectez l’URL personnalisée en tant que la valeur de la clé.  
  
-   Vous pouvez masquer la tâche. Pour masquer la tâche, ouvrez l’Éditeur du Registre et créez la clé de Registre: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\OnlineBackup\OnlineBackupInstalled **.  
  
## <a name="see-also"></a>Voir aussi  
 [Création et personnalisation de l’Image](Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](Additional-Customizations.md)   
 [Préparation de l’Image pour le déploiement](Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience client](Testing-the-Customer-Experience.md)