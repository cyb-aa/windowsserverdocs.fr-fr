---
title: Modifier les paramètres du serveur
description: En savoir plus sur les paramètres de MultiPoint Services
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: afb64b94-9055-4703-b8ce-a8839b2718da
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: 105b10428835d11a0ea0661fe2fa7d57f80a1aba
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885180"
---
# <a name="edit-server-settings"></a>Modifier les paramètres du serveur
Quand vous avez installé MultiPoint Services, vous avez configuré les paramètres de votre système et accepté certains programmes. Cette rubrique décrit les paramètres que vous pouvez définir pour le système MultiPoint Services et explique comment les modifier.  
  
## <a name="about-multipoint-services-settings"></a>À propos des paramètres MultiPoint Services  
Le tableau qui suit décrit les différents paramètres que vous pouvez modifier pour votre système MultiPoint Services.  
  
|Paramètre MultiPoint Services|Description|  
|-----------------------------------------------------------------------------------------|---------------|  
|Autoriser plusieurs sessions sur un même compte|Permet à un compte d’utilisateur unique de se connecter simultanément à plusieurs stations. Ceci peut être utile dans une salle de classe par exemple, où chaque étudiant utilise un seul compte partagé. À l’aide de ce paramètre, toute modification apportée aux ressources d’un compte, comme les dossiers de documents ou le bureau, est disponible pour tous les utilisateurs connectés avec ce même compte.|  
|Autoriser la gestion à distance de cet ordinateur|Permet à l’ordinateur qui exécute MultiPoint Services à être gérés par d’autres systèmes MultiPoint sur votre réseau. Si cette option est sélectionnée et que l’ordinateur de gestion se trouve sur le même sous-réseau, cet ordinateur apparaît dans la liste des serveurs disponibles à gérer. Si cette option est sélectionnée et que l’ordinateur de gestion se trouve sur un autre sous-réseau, l’ordinateur de gestion peut tout de même gérer cet ordinateur, mais vous devez préciser l’adresse IP de l’ordinateur.|
|Autoriser la surveillance des bureaux de cet ordinateur|Vous permet de contrôler si les bureaux peuvent être surveillés sur le système MultiPoint Services. Si ce paramètre est désactivé (non sélectionné), bureaux des stations (locales et distantes) qui sont connectées à l’ordinateur qui exécute MultiPoint Services ne s’affichera pas dans l’onglet Accueil du gestionnaire MultiPoint (y compris sur un ordinateur différent si l’ordinateur est en cours. gérés à distance).|  
|Toujours démarrer en mode console|Active la technologie RemoteFX, conçue pour des sessions de bureau à distance plus rapides et plus efficaces en déchargeant les tâches de traitement sur l’UC et la GPU. Si vous vous connectez à MultiPoint Services à l’aide d’un client compatible RemoteFX, vous pourrez peut-être obtenir de meilleures performances à l’aide de cette option. Les avantages dépendent des capacités de votre serveur et de votre réseau. Par exemple, ils dépendent en partie du fait de savoir si le temps passé au traitement supplémentaire nécessaire à la compression des flux de données est inférieur au temps gagné grâce à la transmission de moindres quantités de données.|  
|Ne pas afficher la notification de confidentialité à la première ouverture de session utilisateur|Quand un utilisateur ouvre une session sur une station MultiPoint pour la première fois, une notification s’affiche pour l’informer que ses activités sur la station peuvent être surveillées.|  
|Attribuer une adresse IP unique à chaque station|Attribue une adresse IP unique à chaque station. Par défaut, MultiPoint Services a une adresse IP, qui est partagée avec toutes les sessions exécutées sur le système. Cette configuration peut, toutefois, poser certains problèmes de compatibilité des applications. Par exemple, si une application nécessite une adresse IP unique, elle peut ne pas fonctionner correctement sur MultiPoint Services. Vous pouvez résoudre ce problème en sélectionnant cette option, appelée également virtualisation de l’adresse IP.<br /><br />La virtualisation de l’adresse IP est également utile pour surveiller les sessions actives sur MultiPoint Services. Certains outils de surveillance indiquent l’utilisation en fonction de l’adresse IP. Pour activer la surveillance des sessions, vous pouvez utiliser la virtualisation de l’adresse IP, afin d’attribuer une adresse IP unique à chaque session. Notez que, si vous sélectionnez cette option, chaque nouvelle session reçoit une adresse IP unique. Toutes les sessions existantes continuent d’utiliser l’adresse IP partagée jusqu’à leur déconnexion, puis leur reconnexion.|  
|Autoriser la messagerie instantanée entre le tableau de bord MultiPoint et une session utilisateur sur cet ordinateur|Active la conversation entre le Gestionnaire MultiPoint et une session utilisateur sur cet ordinateur. Pour plus d’informations, voir [Communiquer à l’aide de la messagerie instantanée](Use-IM.md).|  
|Autoriser l’orchestration de l’administrateur et des sessions utilisateur du tableau de bord MultiPoint|Quand ce paramètre est activé, permet aux administrateurs d’utiliser le tableau de bord MultiPoint pour l’orchestration des sessions. Ces sessions s’affichent sous forme de miniatures.|  
|Autoriser les stations à utiliser le rendu matériel de la GPU|Contrôle si les stations peuvent utiliser l’unité de traitement graphique (GPU) du système.|   
  
## <a name="editing-the-computer-settings"></a>Modification des paramètres du serveur  
  
1.  Ouvrez le gestionnaire MultiPoint dans [mode station](Switch-Between-Modes.md), puis cliquez sur le **accueil** onglet.  
  
2.  Dans le **ordinateur** colonne, cliquez sur le nom d’ordinateur, puis, sous la *nom de l’ordinateur* **tâches**, cliquez sur **modifier les paramètres de l’ordinateur**.  
  
3.  Activez ou désactivez les éléments que vous souhaitez modifier, puis cliquez sur **OK**.  
  
## <a name="see-also"></a>Voir aussi  
[Gérer des tâches système à l’aide du gestionnaire MultiPoint](Manage-System-Tasks-Using-MultiPoint-Manager.md)  
  
