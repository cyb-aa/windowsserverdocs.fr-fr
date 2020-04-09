---
ms.assetid: f88238ea-d851-4129-8b4e-a3a62b813614
title: Revue du rôle du serveur de fédération du partenaire de ressource
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 8cda394800bf0554e00e2897d240e2c08a72a00b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858552"
---
# <a name="review-the-role-of-the-federation-server-in-the-resource-partner"></a>Revue du rôle du serveur de fédération du partenaire de ressource

Le serveur de Fédération dans l’organisation du partenaire de ressource intercepte les jetons de sécurité entrants qui sont envoyés par un serveur de Fédération de comptes, les valide et les signe, puis émet ses propres jetons de sécurité destinés à l’application Web\-.  
  
> [!NOTE]  
> Lorsque les utilisateurs fédérés utilisent leurs navigateurs Web pour accéder à des applications Web\-, le serveur de Fédération dans l’organisation du partenaire de ressource crée un nouveau cookie d’authentification et l’écrit dans le navigateur. Ce cookie active l’authentification unique\-\-sur les fonctionnalités de l'\) authentification unique (SSO) de \(, de sorte que les utilisateurs n’aient pas à se reconnecter au serveur de Fédération du partenaire de compte lorsque les utilisateurs tentent d’accéder à différentes applications basées sur le Web\-dans le partenaire de ressource.  
  
Dans la conception SSO de Web, au moins un serveur de Fédération doit être installé dans le réseau de périmètre. Dans la conception SSO de Web fédéré, au moins un serveur de Fédération doit être installé sur le réseau d’entreprise de l’organisation partenaire de compte et au moins un serveur de Fédération installé sur le réseau d’entreprise de l’organisation partenaire de ressource.  
  
> [!NOTE]  
> Avant de pouvoir configurer un ordinateur serveur de Fédération dans l’organisation partenaire de ressource, vous devez d’abord joindre l’ordinateur à un domaine Active Directory de l’organisation partenaire de ressource. Pour plus d'informations, voir [Checklist: Setting Up a Federation Server](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md).  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

