---
title: Personnaliser la tâche d’inscription au service Microsoft Online Backup Service
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a7eafbb3-7728-487e-b287-90bbd6fee7f0
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: f6e0a499fd149706dc3d7cce21935a7f8b5c5a48
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80311904"
---
# <a name="customize-sign-up-for-microsoft-online-backup-service-task"></a>Personnaliser la tâche d’inscription au service Microsoft Online Backup Service

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Par défaut, la tâche **Inscription au service de sauvegarde en ligne Microsoft** dans l'onglet **PÉRIPHÉRIQUES** du Tableau de bord ouvre le site Web des services de sauvegarde en ligne Microsoft. Le site Web fournit des informations sur le service et vous aide à vous inscrire au service et à télécharger le logiciel requis.  
  
 Vous pouvez personnaliser la tâche **Inscription au service de sauvegarde en ligne Microsoft** de deux manières :  
  
-   Vous pouvez remplacer l'URL du site Web par défaut par un URL qui représente une expérience utilisateur personnalisée. Pour remplacer l'URL par défaut, ouvrez l'Éditeur du registre, créez la clé de registre : **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\OnlineBackup\LinkUrl**, puis affectez l'URL personnalisée en tant que valeur de la clé.  
  
-   Vous pouvez masquer la tâche. Pour masquer la tâche, ouvrez l'Éditeur du Registre et créez  la clé de registre : **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\OnlineBackup\OnlineBackupInstalled**.  
  
## <a name="see-also"></a>Voir aussi  
 [Création et personnalisation de l’Image](Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](Additional-Customizations.md)   
 [Préparation de l’image pour le déploiement](Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience utilisateur](Testing-the-Customer-Experience.md)