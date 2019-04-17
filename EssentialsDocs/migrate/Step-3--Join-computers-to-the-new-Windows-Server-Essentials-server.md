---
title: "Étape3: Joindre des ordinateurs au nouveau serveur WindowsServerEssentials"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a0e07d1a-8409-429b-87d7-0f4a7e14d668
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: f71ac280e2de0b7d945f2d979fe52d173f7c3323
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="step-3-join-computers-to-the-new-windows-server-essentials-server"></a>Étape3: Joindre des ordinateurs au nouveau serveur WindowsServerEssentials

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

L’étape suivante du processus de migration consiste à connecter des ordinateurs clients vers le nouveau serveur exécutant WindowsServerEssentials.  
  
> [!NOTE]
>  Vous pouvez ignorer cette étape pour les ordinateurs qui exécutent WindowsXP ou des systèmes d’exploitation WindowsVista. Le logiciel Connecteur Windows Server ne prend pas en charge les ordinateurs qui exécutent WindowsXP ou WindowsVista.  
  
 Avant de pouvoir joindre un ordinateur client vers le nouveau serveur WindowsServerEssentials, vous devez le déconnecter du serveur Source en désinstallant le logiciel Connecteur Windows Server sur l’ordinateur client.  
  
### <a name="to-uninstall-windows-server-connector-on-a-client-computer"></a>Pour désinstaller le connecteur Windows Server sur un ordinateur client  
  
1.  À partir d’un ordinateur client, ouvrez le panneau de configuration, puis **programmes et fonctionnalités**.  
  
2.  Dans la liste des programmes, cliquez sur l’application de connexion qui s’exécute sur votre ordinateur.  
  
    > [!NOTE]
    >  L’application connecteur peut être **connecteur de WindowsSmallBusinessServer2011Essentials**, ou **connecteur WindowsServerEssentials**, en fonction de la version de l’ordinateur client a été connecté à WindowsServerEssentials.  
  
3.  Cliquez sur **désinstaller**.  
  
### <a name="to-reconnect-a-client-computer-to-the-server"></a>Pour reconnecter un ordinateur client au serveur  
  
1.  Connectez-vous à l’ordinateur que vous souhaitez vous connecter au serveur.  
  
    > [!NOTE]
    >  Si cet ordinateur comporte plusieurs comptes d’utilisateur, connectez-vous à l’aide du compte d’utilisateur qui a des documents, images et préférences personnelles que vous souhaitez conserver après que vous être connecté au serveur de l’ordinateur.  
  
2.  Ouvrez un navigateur Internet, telles qu’Internet Explorer.  
  
3.  Dans la barre d’adresses, tapez **http://<servername\>/Connect**, puis appuyez sur ENTRÉE.  
  
4.  Suivez les instructions à l’écran pour joindre l’ordinateur client vers le nouveau serveur WindowsServerEssentials.  
  
## <a name="next-steps"></a>Étapes suivantes  
 Vous avez joint vos ordinateurs clients vers le nouveau serveur exécutant WindowsServerEssentials. Passez maintenant à [étape4: déplacer les paramètres et les données pour la migration du serveur de Destination pour WindowsServerEssentials](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  
  

Pour afficher toutes les étapes, voir [migrer vers WindowsServerEssentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).

