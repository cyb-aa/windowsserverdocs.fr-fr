---
ms.assetid: f88238ea-d851-4129-8b4e-a3a62b813614
title: Revue du rôle du serveur de fédération du partenaire de ressource
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 6d4c7763d7204dd2340d10a234e48f47c96788dc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841840"
---
# <a name="review-the-role-of-the-federation-server-in-the-resource-partner"></a>Revue du rôle du serveur de fédération du partenaire de ressource

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Le serveur de fédération dans l’organisation partenaire de ressource intercepte les jetons de sécurité entrants qui sont envoyés par un serveur de fédération de compte valide et les signe et émet alors ses propres jetons de sécurité qui sont destinés pour le Web\-en fonction application.  
  
> [!NOTE]  
> Lorsque les utilisateurs fédérés utilisent leurs navigateurs Web pour accéder à Web\-basé sur les applications, le serveur de fédération dans l’organisation partenaire ressource génère un cookie d’authentification et l’écrit dans le navigateur. Ce cookie permet unique\-connexion\-sur \(SSO\) fonctionnalités afin que les utilisateurs n’ont pas à se reconnecter au serveur de fédération du partenaire de compte lorsque les utilisateurs tentent d’accéder aux différents Web\- applications basées sur le partenaire de ressource.  
  
Dans la conception SSO de Web, au moins un serveur de fédération doit être installé dans le réseau de périmètre. Dans la conception SSO de Web fédéré, il doit être au moins un serveur de fédération installé dans le réseau d’entreprise de l’organisation partenaire de compte et au moins un serveur de fédération installé dans le réseau d’entreprise de l’organisation partenaire ressource.  
  
> [!NOTE]  
> Avant de configurer un serveur de fédération dans l’organisation partenaire de ressource, vous devez tout d’abord joindre l’ordinateur à un domaine Active Directory dans l’organisation partenaire ressource. Pour plus d'informations, consultez [Liste de vérification : Configuration d’un serveur de fédération](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md).  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

