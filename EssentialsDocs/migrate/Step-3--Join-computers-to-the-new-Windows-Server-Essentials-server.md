---
title: 'Étape 3 : Joindre des ordinateurs au nouveau serveur Windows Server Essentials'
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a0e07d1a-8409-429b-87d7-0f4a7e14d668
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 1ca1e3a031c95f19fb68aadcf203b13fa39d7558
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318761"
---
# <a name="step-3-join-computers-to-the-new-windows-server-essentials-server"></a>Étape 3 : Joindre des ordinateurs au nouveau serveur Windows Server Essentials

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

L’étape suivante du processus de migration consiste à connecter des ordinateurs clients au nouveau serveur exécutant Windows Server Essentials.  
  
> [!NOTE]
>  Vous pouvez ignorer cette étape pour les ordinateurs qui exécutent les systèmes d'exploitation Windows XP ou Windows Vista. Le logiciel Connecteur Windows Server ne prend pas en charge les ordinateurs qui exécutent Windows XP ou Windows Vista.  
  
 Avant de pouvoir joindre un ordinateur client au nouveau serveur Windows Server Essentials, vous devez le déconnecter du serveur source en désinstallant le logiciel connecteur Windows Server sur l’ordinateur client.  
  
### <a name="to-uninstall-windows-server-connector-on-a-client-computer"></a>Pour désinstaller le connecteur Windows Server d'un ordinateur client  
  
1.  À partir d'un ordinateur client, ouvrez le Panneau de configuration, puis ouvrez **Programmes et fonctionnalités**.  
  
2.  Dans la liste des programmes, cliquez sur l'application de connexion qui s'exécute sur votre ordinateur.  
  
    > [!NOTE]
    >  L’application connecteur peut être le connecteur **Windows Small Business Server 2011 Essentials**ou le **connecteur Windows Server Essentials**, selon la version de Windows Server Essentials à laquelle l’ordinateur client est connecté.  
  
3.  Cliquez sur **Désinstaller**.  
  
### <a name="to-reconnect-a-client-computer-to-the-server"></a>Pour reconnecter un ordinateur client au serveur  
  
1.  Connectez-vous à l'ordinateur que vous voulez connecter au serveur.  
  
    > [!NOTE]
    >  Si cet ordinateur comporte plusieurs comptes d'utilisateur, démarrez la session du compte d'utilisateur dont vous souhaitez conserver les documents, images et préférences personnelles une fois l'ordinateur connecté au serveur.  
  
2.  Ouvrez un navigateur Internet, par exemple Internet Explorer.  
  
3.  Dans la barre d’adresse, tapez **http://< servername\>/Connect**, puis appuyez sur entrée.  
  
4.  Suivez les instructions à l’écran pour joindre l’ordinateur client au nouveau serveur Windows Server Essentials.  
  
## <a name="next-steps"></a>Étapes suivantes :  
 Vous avez joint vos ordinateurs clients au nouveau serveur exécutant Windows Server Essentials. Passez maintenant à [l’étape 4 : déplacer les paramètres et les données vers le serveur de destination pour la migration vers Windows Server Essentials](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  
  

Pour afficher toutes les étapes, consultez [migrer vers Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).

