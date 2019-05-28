---
ms.assetid: 1b3a03c0-5558-4177-9b2f-e9d6ce3271cd
title: Revue du rôle du serveur proxy de fédération du partenaire de compte
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: dbceb19d31738bdc5b628a9a2b069e5d3022d145
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190954"
---
# <a name="review-the-role-of-the-federation-server-proxy-in-the-account-partner"></a>Revue du rôle du serveur proxy de fédération du partenaire de compte

Le rôle principal du serveur proxy de fédération dans le réseau de périmètre de l’organisation partenaire de compte dans Active Directory Federation Services \(AD FS\) consiste à collecter des informations d’identification de l’authentification à partir d’un ordinateur client qui se connecte sur Internet et transmettre ces informations d’identification au serveur de fédération, qui se trouve à l’intérieur du réseau d’entreprise de l’organisation partenaire de compte. Le compte de l’ordinateur client est stocké dans le magasin d’attributs du partenaire de compte.  
  
Un serveur proxy de fédération peut également fonctionner dans un ou plusieurs des rôles suivants, selon la configuration pour répondre aux besoins de l’organisation partenaire de compte :  
  
-   Jetons de sécurité de relais, le serveur de fédération émet un jeton de sécurité pour le serveur proxy de fédération, qui relaie ensuite le jeton à l’ordinateur client. Le jeton de sécurité est utilisé pour fournir l’accès à une partie de confiance spécifique pour cet ordinateur client.  
  
-   Collecter les informations d’identification, le serveur proxy de fédération utilise un formulaire Web de l’ouverture de session client de la valeur par défaut \(clientlogon.aspx\) pour collecter le mot de passe\-en fonction des informations d’identification grâce à des formulaires\-en fonction de l’authentification. Toutefois, vous pouvez personnaliser ce formulaire pour accepter les autres types pris en charge l’authentification, par exemple de protocole SSL (Secure Sockets Layer) \(SSL\) l’authentification du client. Pour plus d’informations sur la personnalisation de cette page, consultez ouverture de session du Client de personnalisation et d’accueil des Pages de découverte de domaine \( [http :\/\/go.microsoft.com\/fwlink\/? LinkId\=104275](https://go.microsoft.com/fwlink/?LinkId=104275)\). Un serveur proxy de fédération n’accepte pas les informations d’identification via l’authentification intégrée Windows.  
  
Pour résumer, un serveur proxy de fédération du partenaire de compte agit comme un proxy pour les connexions client à un serveur de fédération qui se trouve dans le réseau d’entreprise. Le serveur proxy de fédération facilite également la distribution des jetons de sécurité pour les clients Internet qui sont destinés aux parties de confiance.  
  
> [!CAUTION]  
> Exposition d’un serveur proxy de fédération du partenaire de compte extranet de l’ouverture de session du client Web form accessible par toute personne ayant Internet accédera. Cela peut potentiellement exposer votre organisation à un mot de passe\-les attaques, telles que les attaques de dictionnaire ou les attaques par force brute qui peuvent déclencher des verrouillages de compte pour les comptes d’utilisateur qui sont stockés dans le domaine Active Directory d’entreprise Services \(AD DS\).  
  

## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
