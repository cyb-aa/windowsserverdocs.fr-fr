---
ms.assetid: f88238ea-d851-4129-8b4e-a3a62b813614
title: "Passez en revue le rôle du serveur de fédération du partenaire de ressource"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 6d4c7763d7204dd2340d10a234e48f47c96788dc
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="review-the-role-of-the-federation-server-in-the-resource-partner"></a>Passez en revue le rôle du serveur de fédération du partenaire de ressource

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Le serveur de fédération dans les ressource partenaire organisation intercepte jetons de sécurité entrants qui sont envoyés par un serveur de fédération de comptes, valide les signe et envoie ensuite ses propres jetons de sécurité qui sont destinés à l’application de console Web.  
  
> [!NOTE]  
> Lorsque les utilisateurs fédérés utilisent leurs navigateurs Web pour accéder aux applications basées sur la console Web, le serveur de fédération dans l’organisation partenaire de ressource génère un cookie d’authentification et l’écrit dans le navigateur. Ce cookie permet single\-connexion-\(SSO\) fonctionnalités afin que les utilisateurs n’aient pas à se reconnecter au serveur de fédération du partenaire de compte lorsque les utilisateurs tentent d’accéder aux différentes applications de console Web du partenaire de ressource.  
  
Dans la conception SSO de Web, au moins un serveur de fédération doit être installé dans le réseau de périmètre. Dans la conception SSO de Web fédéré, il doit être au moins un serveur de fédération installé dans le réseau d’entreprise de l’organisation partenaire de compte et au moins un serveur de fédération installé dans le réseau d’entreprise de l’organisation partenaire de ressource.  
  
> [!NOTE]  
> Avant de configurer un ordinateur de serveur de fédération dans l’organisation partenaire de ressource, vous devez tout d’abord joindre l’ordinateur à un domaine ActiveDirectory de l’organisation partenaire de ressource. Pour plus d’informations, voir [liste de vérification: Setting Up a Federation Server](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md).  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception ADFS dans Windows Server2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

