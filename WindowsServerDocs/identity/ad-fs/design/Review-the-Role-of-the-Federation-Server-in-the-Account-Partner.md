---
ms.assetid: d0ba3c0d-869f-4e24-89d7-499da7576f22
title: Revue du rôle du serveur de fédération du partenaire de compte
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: ead8868f38faa570a0e524630e23d99e276a7c79
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407922"
---
# <a name="review-the-role-of-the-federation-server-in-the-account-partner"></a>Revue du rôle du serveur de fédération du partenaire de compte

Un serveur de Fédération dans Services ADFS \(AD FS\) fonctionne comme un émetteur de jeton de sécurité. Un serveur de Fédération génère des revendications basées sur les valeurs de compte qui résident dans un magasin d’attributs local et les empaquette dans des jetons de sécurité afin que les utilisateurs puissent accéder en toute transparence aux applications basées sur le navigateur Web\-\-\(à l’aide de l’authentification unique\-\(\)SSO hébergées dans une organisation partenaire de ressource.\)  
  
> [!NOTE]  
> Lorsque vos utilisateurs accèdent à des applications fédérées à l’aide d’un navigateur Web, un serveur de Fédération émet automatiquement des cookies aux utilisateurs afin de conserver leur état d’ouverture de session pour cette application Web\-Browser\-. Ces cookies incluent les revendications des utilisateurs. Les cookies activent les fonctionnalités SSO de sorte que les utilisateurs n’aient pas à entrer leurs informations d’identification chaque fois qu’ils accèdent à différentes applications basées sur le navigateur Web\-\-dans le partenaire de ressource.  
  
Dans la conception SSO de Web, les organisations disposant d’un réseau de périmètre souhaitant que les utilisateurs Internet puissent accéder aux applications doivent installer un serveur proxy de Fédération dans le réseau de périmètre. Dans la conception SSO de Web fédéré, au moins un serveur de Fédération doit être installé sur le réseau d’entreprise de l’organisation partenaire de compte et au moins un serveur de Fédération installé sur le réseau d’entreprise de l’organisation partenaire de ressource.  
  
> [!NOTE]  
> Avant de pouvoir configurer un ordinateur serveur de Fédération dans l’organisation partenaire de compte, vous devez d’abord joindre l’ordinateur à n’importe quel domaine de la forêt Active Directory dans laquelle le serveur de Fédération sera utilisé pour authentifier les utilisateurs de cette forêt. Pour plus d'informations, voir [Checklist: Setting Up a Federation Server](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md).  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
