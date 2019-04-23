---
title: Déconnecter des sessions utilisateur
description: Découvrez comment manuellement déconnecter un utilisateur
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3e9bbcdc-e33b-481e-8b46-787a4f6d58bc
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 518e9dc9ba9603d988a7e21e08caa29db9f04bde
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854940"
---
# <a name="log-off-or-disconnect-user-sessions"></a>Déconnecter des sessions utilisateur
Utilisateurs multiPoint Services peuvent se connecter et se déconnecter les sessions de bureau comme ils le feraient avec n’importe quelle session Windows. Les utilisateurs peuvent également vous déconnecter ou interrompre leur session afin que la station MultiPoint Services n’est pas utilisée, mais leur session reste active dans la mémoire de l’ordinateur du système MultiPoint Services.  
  
En outre, les utilisateurs administratifs peuvent arrêter une session utilisateur si l’utilisateur a délaissé sa session MultiPoint Services ou a oublié de se déconnecter du système.  
  
## <a name="logging-off-or-disconnecting-a-session"></a>Fermeture de session ou déconnexion d’une session  
Le tableau suivant décrit les différentes options disponibles (pour tout utilisateur) pour fermer, suspendre ou terminer une session.  
  
|||  
|-|-|  
|**Action**|**Effect**|  
|Cliquez sur **Démarrer**, cliquez sur paramètres, cliquez sur le nom d’utilisateur (en haut à droite), puis cliquez sur **se déconnecter**.|La session se termine et la station est disponible pour la connexion d’un autre utilisateur.|  
|Cliquez sur **Démarrer**, sur **Paramètres**, sur Alimentation, puis sur **Se déconnecter**.|Votre session est déconnectée et conservée en mémoire. La station est disponible pour la connexion du même ou d’un autre utilisateur.|  
|Cliquez sur **Démarrer**, cliquez sur paramètres, cliquez sur le nom d’utilisateur (en haut à droite), puis cliquez sur **verrou**|La station est verrouillée et votre session est conservée en mémoire.|  
  
## <a name="suspending-or-ending-a-users-session"></a>Suspension ou fin d’une session utilisateur  
Le tableau suivant décrit les différentes options dont vous disposez en tant qu’utilisateur administratif pour déconnecter ou terminer une session utilisateur.  
  
|||  
|-|-|  
|**Action**|**Effect**|  
|**Suspendre :** Dans le gestionnaire MultiPoint, utilisez le **Stations** onglet suspendre la session utilisateur. Pour plus d’informations, voir la rubrique [Suspendre et laisser une session utilisateur active](Suspend-and-Leave-User-Session-Active.md).|La session utilisateur se termine et est conservée en mémoire. La station est disponible pour la connexion du même ou d’un autre utilisateur. L’utilisateur peut se connecter à la même station ou à une autre et continuer à travailler.|  
|**Fin :** Dans le gestionnaire MultiPoint, utilisez le **Stations** onglet à la fin de la session utilisateur. Vous pouvez également terminer toutes les sessions utilisateur dans l’onglet **Stations**. Pour plus d’informations, voir la rubrique [Terminer une session utilisateur](End-a-User-Session.md).|La session utilisateur se termine et la station est disponible pour la connexion d’un utilisateur. La session utilisateur n’est plus affichée dans l’onglet **Stations** et n’est plus en mémoire.|  
  
## <a name="see-also"></a>Voir aussi  
[Suspendre et laisser une Session utilisateur Active](Suspend-and-Leave-User-Session-Active.md)  
[Terminer une Session utilisateur](End-a-User-Session.md)  
[Gérer des bureaux d’utilisateurs](manage-user-desktops-using-multipoint-dashboard.md)  
[Fermer les Sessions utilisateur](Log-Off-User-Sessions.md)    