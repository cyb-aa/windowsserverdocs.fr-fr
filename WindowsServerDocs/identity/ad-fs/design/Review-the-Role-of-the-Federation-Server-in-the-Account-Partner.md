---
ms.assetid: d0ba3c0d-869f-4e24-89d7-499da7576f22
title: "Passez en revue le rôle du serveur de fédération du partenaire de compte"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0914d32e8f24d5e7db0a25c733342c1bde3e0329
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="review-the-role-of-the-federation-server-in-the-account-partner"></a>Passez en revue le rôle du serveur de fédération du partenaire de compte

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Un serveur de fédération dans ActiveDirectory Federation Services \(ADFS\) fonctionne comme un émetteur de jeton de sécurité. Un serveur de fédération génère des revendications basées sur le compte stockent de valeurs qui se trouvent dans un attribut local et les empaquette dans des jetons de sécurité afin que les utilisateurs puissent accéder en toute transparence aux applications console Web-browser\ \ (à l’aide de la seule \(SSO\)\) de connexion qui sont hébergées dans une organisation partenaire de ressource.  
  
> [!NOTE]  
> Lorsque vos utilisateurs un accès fédéré des applications à l’aide d’un navigateur Web, un serveur de fédération émet automatiquement des cookies pour les utilisateurs à mettre à jour leur état d’ouverture de session pour cette application browser\ de console Web. Ces cookies incluent les revendications pour les utilisateurs. Les cookies activent les capacités SSO afin que les utilisateurs n’aient pas à entrer des informations d’identification chaque fois qu’ils accèdent à différentes applications browser\ de console Web du partenaire de ressource.  
  
Dans la conception SSO de Web, les organisations avec un réseau de périmètre et que vous souhaitez que les utilisateurs Internet puissent accéder aux applications doivent installer un serveur proxy de fédération dans le réseau de périmètre. Dans la conception SSO de Web fédéré, il doit être au moins un serveur de fédération installé dans le réseau d’entreprise de l’organisation partenaire de compte et au moins un serveur de fédération installé dans le réseau d’entreprise de l’organisation partenaire de ressource.  
  
> [!NOTE]  
> Avant de configurer un ordinateur de serveur de fédération dans l’organisation partenaire de compte, vous devez tout d’abord joindre l’ordinateur à un domaine de la forêt ActiveDirectory où le serveur de fédération servira à authentifier les utilisateurs de cette forêt. Pour plus d’informations, voir [liste de vérification: Setting Up a Federation Server](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md).  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception ADFS dans Windows Server2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
