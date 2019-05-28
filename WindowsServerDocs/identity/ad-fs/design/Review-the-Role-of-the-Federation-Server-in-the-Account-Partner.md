---
ms.assetid: d0ba3c0d-869f-4e24-89d7-499da7576f22
title: Revue du rôle du serveur de fédération du partenaire de compte
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 5bc304277b872bd9b99b79b84694dd0cb1eb73ba
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190885"
---
# <a name="review-the-role-of-the-federation-server-in-the-account-partner"></a>Revue du rôle du serveur de fédération du partenaire de compte

Serveur de fédération Active Directory Federation Services \(AD FS\) fonctionne comme un émetteur de jeton de sécurité. Un serveur de fédération génère des revendications basées sur le compte stocker des valeurs qui se trouvent dans un attribut local et les empaquette dans des jetons de sécurité afin que les utilisateurs peuvent accéder en toute transparence Web\-navigateur\-des applications basées sur \(à l’aide de l’authentification unique\-sur \(SSO\) \) qui sont hébergés dans une organisation partenaire de ressource.  
  
> [!NOTE]  
> Lorsque vos utilisateurs accéder aux applications fédérées à l’aide d’un navigateur Web, un serveur de fédération émet automatiquement des cookies aux utilisateurs de maintenir leur état d’ouverture de session pour ce site Web\-navigateur\-application basée sur. Ces cookies incluent les revendications des utilisateurs. Les cookies activent les capacités SSO afin que les utilisateurs n’ont pas à entrer chaque fois qu’ils accèdent à différentes Web\-navigateur\-en fonction des applications dans le partenaire de ressource.  
  
Dans la conception SSO de Web, les organisations avec un réseau de périmètre et que les utilisateurs Internet puissent accéder à des applications doivent installer un serveur proxy de fédération dans le réseau de périmètre. Dans la conception SSO de Web fédéré, il doit être au moins un serveur de fédération installé dans le réseau d’entreprise de l’organisation partenaire de compte et au moins un serveur de fédération installé dans le réseau d’entreprise de l’organisation partenaire ressource.  
  
> [!NOTE]  
> Avant de configurer un serveur de fédération dans l’organisation partenaire de compte, vous devez tout d’abord joindre l’ordinateur à un domaine de la forêt Active Directory où le serveur de fédération servira à authentifier les utilisateurs de cette forêt. Pour plus d'informations, consultez [Liste de vérification : Configuration d’un serveur de fédération](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md).  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
