---
title: Modifier les paramètres du serveur
description: En savoir plus sur les paramètres MultiPoint services
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: afb64b94-9055-4703-b8ce-a8839b2718da
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: 1812db96c4acf2e3e820f44660c91d2a67b4348f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859272"
---
# <a name="edit-server-settings"></a>Modifier les paramètres du serveur
Quand vous avez installé MultiPoint Services, vous avez configuré les paramètres de votre système et accepté certains programmes. Cette rubrique décrit les paramètres que vous pouvez définir pour le système MultiPoint Services et explique comment les modifier.  
  
## <a name="about-multipoint-services-settings"></a>À propos des paramètres MultiPoint Services  
Le tableau qui suit décrit les différents paramètres que vous pouvez modifier pour votre système MultiPoint Services.  
  
|Paramètre MultiPoint Services|Description|  
|-----------------------------------------------------------------------------------------|---------------|  
|Autoriser plusieurs sessions sur un même compte|Permet à un compte d’utilisateur unique de se connecter simultanément à plusieurs stations. Ceci peut être utile dans une salle de classe par exemple, où chaque étudiant utilise un seul compte partagé. À l’aide de ce paramètre, toute modification apportée aux ressources d’un compte, comme les dossiers de documents ou le bureau, est disponible pour tous les utilisateurs connectés avec ce même compte.|  
|Autoriser la gestion à distance de cet ordinateur|Permet à l’ordinateur qui exécute MultiPoint services d’être géré par d’autres systèmes MultiPoint sur votre réseau. Si cette option est sélectionnée et que l’ordinateur de gestion se trouve sur le même sous-réseau, cet ordinateur apparaît dans la liste des serveurs disponibles à gérer. Si cette option est sélectionnée et que l’ordinateur de gestion se trouve sur un autre sous-réseau, l’ordinateur de gestion peut tout de même gérer cet ordinateur, mais vous devez préciser l’adresse IP de l’ordinateur.|
|Autoriser l’analyse des ordinateurs de bureau de cet ordinateur|Vous permet de contrôler si les bureaux peuvent être surveillés sur le système MultiPoint Services. Si ce paramètre est désactivé (non sélectionné), les bureaux de stations (à la fois locaux et distants) qui sont connectés à l’ordinateur qui exécute MultiPoint services ne s’affichent pas dans l’onglet de démarrage du gestionnaire MultiPoint (y compris sur un autre ordinateur si l’ordinateur est géré à distance).|  
|Toujours démarrer en mode console|Active la technologie RemoteFX, conçue pour des sessions de bureau à distance plus rapides et plus efficaces en déchargeant les tâches de traitement sur l’UC et la GPU. Si vous vous connectez à MultiPoint services à l’aide d’un client RemoteFX, vous pourrez peut-être obtenir de meilleures performances à l’aide de cette option. Les avantages dépendent des capacités de votre serveur et de votre réseau. Par exemple, ils dépendent en partie du fait de savoir si le temps passé au traitement supplémentaire nécessaire à la compression des flux de données est inférieur au temps gagné grâce à la transmission de moindres quantités de données.|  
|Ne pas afficher la notification de confidentialité à la première ouverture de session utilisateur|Quand un utilisateur ouvre une session sur une station MultiPoint pour la première fois, une notification s’affiche pour l’informer que ses activités sur la station peuvent être surveillées.|  
|Attribuer une adresse IP unique à chaque station|Attribue une adresse IP unique à chaque station. Par défaut, MultiPoint Services a une adresse IP, qui est partagée avec toutes les sessions exécutées sur le système. Cette configuration peut, toutefois, poser certains problèmes de compatibilité des applications. Par exemple, si une application nécessite une adresse IP unique, elle peut ne pas fonctionner correctement sur MultiPoint Services. Vous pouvez résoudre ce problème en sélectionnant cette option, appelée également virtualisation de l’adresse IP.<p>La virtualisation de l’adresse IP est également utile pour surveiller les sessions actives sur MultiPoint Services. Certains outils de surveillance indiquent l’utilisation en fonction de l’adresse IP. Pour activer la surveillance des sessions, vous pouvez utiliser la virtualisation de l’adresse IP, afin d’attribuer une adresse IP unique à chaque session. Notez que, si vous sélectionnez cette option, chaque nouvelle session reçoit une adresse IP unique. Toutes les sessions existantes continuent d’utiliser l’adresse IP partagée jusqu’à leur déconnexion, puis leur reconnexion.|  
|Autoriser la messagerie instantanée entre le tableau de bord MultiPoint et une session utilisateur sur cet ordinateur|Active la conversation entre le Gestionnaire MultiPoint et une session utilisateur sur cet ordinateur. Pour plus d’informations, voir [Communiquer à l’aide de la messagerie instantanée](Use-IM.md).|  
|Autoriser l’orchestration de l’administrateur et des sessions utilisateur du tableau de bord MultiPoint|Quand ce paramètre est activé, permet aux administrateurs d’utiliser le tableau de bord MultiPoint pour l’orchestration des sessions. Ces sessions s’affichent sous forme de miniatures.|  
|Autoriser les stations à utiliser le rendu matériel de la GPU|Contrôle si les stations peuvent utiliser l’unité de traitement graphique (GPU) du système.|   
  
## <a name="editing-the-computer-settings"></a>Modification des paramètres du serveur  
  
1.  Ouvrez le gestionnaire MultiPoint en [mode station](Switch-Between-Modes.md), puis cliquez sur l’onglet dossier de **démarrage** .  
  
2.  Dans la colonne **ordinateur** , cliquez sur le nom de l’ordinateur, puis, sous le nom de l' *ordinateur* **tâches**, cliquez sur **modifier les paramètres**de l’ordinateur.  
  
3.  Activez ou désactivez les éléments que vous souhaitez modifier, puis cliquez sur **OK**.  
  
## <a name="see-also"></a>Voir aussi  
[Gérer des tâches système à l’aide du Gestionnaire MultiPoint](Manage-System-Tasks-Using-MultiPoint-Manager.md)  
  
