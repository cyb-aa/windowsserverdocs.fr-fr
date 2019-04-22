---
title: Gérer les stations d’utilisateur
description: Découvrez comment gérer les stations d’utilisateur dans MultiPoint Services
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b418578d-3a4c-49b0-90db-8389b320b2f6
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 8c5351f3e8ec9890ef72905b646c37e9b049745e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823720"
---
# <a name="manage-user-stations"></a>Gérer les stations d’utilisateur
Cette section traite de la gestion des *stations* qui composent le système MultiPoint Services. Gestion d’un système MultiPoint Services implique de gérer les composants matériels et logiciels de gestionnaire MultiPoint. Dans un système MultiPoint Services, un ordinateur de bureau est l’interface utilisateur du logiciel affichée sur le moniteur pour chaque station d’utilisateur.  
  
## <a name="station-status"></a>Statut de la station  
Vous pouvez voir les types de statuts suivants pour chaque bureau dans l’onglet **Stations**. Les statuts sont les suivants :  
  
-   Utilisateurs connectés  
  
-   Sessions utilisateur suspendues, mais toujours actives sur l’ordinateur  
  
-   Stations en cours d’utilisation et par qui  
  
Pour plus d’informations sur l’affichage du statut du bureau, voir la rubrique [Afficher le statut de connexion de l’utilisateur](View-User-Connection-Status.md).  

>[!TIP] 
> Vous pouvez affecter des noms conviviaux à chacune des stations, ce qui vous permet de les identifier plus facilement. Utilisez **Identify station** (Identifier la station), qui affiche le nom de la station sur l’écran attribué.
  
## <a name="different-ways-to-log-standard-users-off-of-the-multipoint-services-system"></a>Différentes méthodes pour déconnecter des utilisateurs standard du système MultiPoint Services  
En tant qu’*utilisateur administratif*, vous pouvez fermer Windows à tout moment, sans impact sur les utilisateurs actifs du système MultiPoint Services. Les *utilisateurs standard* peuvent *déconnecter* leur session ou également *déconnecter* le système MultiPoint Services. Si la Protection des disques est activée et qu’un utilisateur s’absente pour la journée, il doit enregistrer son travail sur l’ordinateur ou sur un périphérique de stockage externe pour pouvoir le récupérer un autre jour si le système MultiPoint Services est arrêté.  
  
En tant qu’utilisateur administratif, vous serez peut-être amené à mettre fin à la *session* d’un utilisateur standard sans qu’il ne se déconnecte lui-même. Vous pouvez terminer la session d’un utilisateur standard de l’une des deux manières suivantes :  
  
-   Terminer la session et déconnecter l’utilisateur. Pour plus d’informations sur la manière de terminer la session d’un utilisateur, voir la rubrique [Terminer une session utilisateur](End-a-User-Session.md).  
  
-   Suspendre l’utilisateur pour mettre fin de façon temporaire à sa session, mais garder la session active dans la mémoire d’ordinateur du système MultiPoint Services. L’utilisateur suspendu peut ensuite se reconnecter à la session, à partir de la même station ou d’une autre station, et reprendre son travail. Pour plus d’informations sur la manière de suspendre la session d’un utilisateur, voir la rubrique [Suspendre et laisser une session utilisateur active](Suspend-and-Leave-User-Session-Active.md).  
  
## <a name="set-a-station-to-automatically-log-on"></a>Définir la connexion automatique d’une station  
En tant qu’utilisateur administratif, vous pouvez définir la connexion automatique d’une ou de plusieurs stations au démarrage de l’ordinateur qui exécute MultiPoint Services. Pour plus d’informations sur la connexion automatique, voir la rubrique [Installer la connexion automatique sur une station](Set-up-a-Station-for-Automatic-Logon.md).  
  
## <a name="split-a-station"></a>Diviser une station  
Tout moniteur d’une station d’une résolution supérieure à 1024 x 768 peut être divisé en deux stations. Pour plus d’informations sur la division d’une station, voir la rubrique [Diviser une station d’utilisateur](Split-a-User-Station.md).  
  
## <a name="see-also"></a>Voir aussi  
[Afficher l’état de connexion utilisateur](View-User-Connection-Status.md)  
[Fermer la session ou déconnecter des Sessions utilisateur](Log-off-or-Disconnect-User-Sessions.md)  
[Suspendre et laisser une Session utilisateur Active](Suspend-and-Leave-User-Session-Active.md)  
[Configuration d’une Station pour la connexion automatique](Set-up-a-Station-for-Automatic-Logon.md)  
[Terminer une Session utilisateur](End-a-User-Session.md)  
[Diviser une Station utilisateur](Split-a-User-Station.md)