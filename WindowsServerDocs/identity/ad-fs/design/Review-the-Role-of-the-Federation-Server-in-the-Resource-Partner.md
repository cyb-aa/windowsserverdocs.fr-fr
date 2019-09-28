---
ms.assetid: f88238ea-d851-4129-8b4e-a3a62b813614
title: Revue du rôle du serveur de fédération du partenaire de ressource
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: fd9f20eb7559f5862ee50bdd8364fa1604d3c1b6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358968"
---
# <a name="review-the-role-of-the-federation-server-in-the-resource-partner"></a>Revue du rôle du serveur de fédération du partenaire de ressource

Le serveur de Fédération dans l’organisation partenaire de ressource intercepte les jetons de sécurité entrants qui sont envoyés par un serveur de Fédération de compte, les valide et les signe, puis émet ses propres jetons de sécurité destinés à l’application Web @ no__t-0based. .  
  
> [!NOTE]  
> Lorsque les utilisateurs fédérés utilisent leurs navigateurs Web pour accéder aux applications Web @ no__t-0based, le serveur de Fédération dans l’organisation partenaire de ressource crée un nouveau cookie d’authentification et l’écrit dans le navigateur. Ce cookie active les fonctionnalités uniques @ no__t-0sign @ no__t-1On \(SSO @ no__t-3 afin que les utilisateurs n’aient pas à se reconnecter au serveur de Fédération du partenaire de compte lorsque les utilisateurs tentent d’accéder à différentes applications Web @ no__t-4based dans la ressource associer.  
  
Dans la conception SSO de Web, au moins un serveur de Fédération doit être installé dans le réseau de périmètre. Dans la conception SSO de Web fédéré, au moins un serveur de Fédération doit être installé sur le réseau d’entreprise de l’organisation partenaire de compte et au moins un serveur de Fédération installé sur le réseau d’entreprise de l’organisation partenaire de ressource.  
  
> [!NOTE]  
> Avant de pouvoir configurer un ordinateur serveur de Fédération dans l’organisation partenaire de ressource, vous devez d’abord joindre l’ordinateur à un domaine Active Directory de l’organisation partenaire de ressource. Pour plus d'informations, consultez [Liste de vérification : Configuration d’un serveur de Fédération @ no__t-0.  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

