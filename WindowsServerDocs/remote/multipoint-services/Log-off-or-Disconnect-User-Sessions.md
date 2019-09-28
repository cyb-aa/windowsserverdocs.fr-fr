---
title: Déconnecter des sessions utilisateur
description: Découvrez comment fermer la session d’un utilisateur manuellement
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: c636af35a78eab76d69c68b6f506b64dcb555f81
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71395268"
---
# <a name="log-off-or-disconnect-user-sessions"></a>Déconnecter des sessions utilisateur
Les utilisateurs de MultiPoint services peuvent se connecter et se déconnecter de leurs sessions de bureau comme pour toute session Windows. Les utilisateurs peuvent également se déconnecter ou interrompre leur session afin que la station MultiPoint services ne soit pas utilisée, mais que leur session reste active dans la mémoire de l’ordinateur du système MultiPoint services.  
  
En outre, les utilisateurs administratifs peuvent mettre fin à la session d’un utilisateur si celui-ci a quitté la session MultiPoint services ou a oublié de se déconnecter du système.  
  
## <a name="logging-off-or-disconnecting-a-session"></a>Fermeture de session ou déconnexion d’une session  
Le tableau suivant décrit les différentes options disponibles (pour tout utilisateur) pour fermer, suspendre ou terminer une session.  
  
|||  
|-|-|  
|**Action**|**Résultat**|  
|Cliquez sur **Démarrer**, sur paramètres, sur le nom d’utilisateur (coin supérieur droit), puis sur **déconnexion**.|La session se termine et la station est disponible pour la connexion d’un autre utilisateur.|  
|Cliquez sur **Démarrer**, sur **Paramètres**, sur Alimentation, puis sur **Se déconnecter**.|Votre session est déconnectée et conservée en mémoire. La station est disponible pour la connexion du même ou d’un autre utilisateur.|  
|Cliquez sur **Démarrer**, sur paramètres, sur le nom d’utilisateur (coin supérieur droit), puis sur **Verrouiller** .|La station est verrouillée et votre session est conservée en mémoire.|  
  
## <a name="suspending-or-ending-a-users-session"></a>Interruption ou fin de la session d’un utilisateur  
Le tableau suivant décrit les différentes options que vous, en tant qu’utilisateur administratif, peuvent utiliser pour déconnecter ou mettre fin à la session d’un utilisateur.  
  
|||  
|-|-|  
|**Action**|**Résultat**|  
|**Momentané** Dans le gestionnaire MultiPoint, utilisez l’onglet **stations** pour interrompre la session de l’utilisateur. Pour plus d’informations, voir la rubrique [Suspendre et laisser une session utilisateur active](Suspend-and-Leave-User-Session-Active.md).|La session de l’utilisateur se termine et est conservée dans la mémoire de l’ordinateur. La station est disponible pour la connexion du même ou d’un autre utilisateur. L’utilisateur peut se connecter à la même station ou à une autre et continuer à travailler.|  
|**Effet** Dans le gestionnaire MultiPoint, utilisez l’onglet **stations** pour mettre fin à la session de l’utilisateur. Vous pouvez également terminer toutes les sessions utilisateur dans l’onglet **Stations**. Pour plus d’informations, voir la rubrique [Terminer une session utilisateur](End-a-User-Session.md).|La session de l’utilisateur se termine et la station est disponible pour la connexion de n’importe quel utilisateur. La session de l’utilisateur ne s’affiche plus sous l’onglet **stations** et n’est pas dans la mémoire de l’ordinateur.|  
  
## <a name="see-also"></a>Voir aussi  
[Interrompre et conserver la session utilisateur active](Suspend-and-Leave-User-Session-Active.md)  
[Terminer une session utilisateur](End-a-User-Session.md)  
[Gérer les postes de travail des utilisateurs](manage-user-desktops-using-multipoint-dashboard.md)  
[Fermer les sessions utilisateur](Log-Off-User-Sessions.md)    