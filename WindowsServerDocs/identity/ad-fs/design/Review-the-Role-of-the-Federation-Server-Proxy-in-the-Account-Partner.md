---
ms.assetid: 1b3a03c0-5558-4177-9b2f-e9d6ce3271cd
title: "Passez en revue le rôle de serveur Proxy de fédération du partenaire de compte"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d2b60ce593c2ca7eb902595ee6a42850cb7605d9
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="review-the-role-of-the-federation-server-proxy-in-the-account-partner"></a>Passez en revue le rôle de serveur Proxy de fédération du partenaire de compte

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Le rôle principal du serveur proxy de fédération dans le réseau de périmètre de l’organisation partenaire de compte dans ActiveDirectory Federation Services \(ADFS\) est pour collecter les informations d’identification d’authentification à partir d’un ordinateur client qui se connecte via Internet et de transmettre ces informations d’identification pour le serveur de fédération qui se trouve à l’intérieur du réseau d’entreprise de l’organisation partenaire de compte. Le compte de l’ordinateur client est stocké dans le magasin d’attributs du partenaire de compte.  
  
Un serveur proxy de fédération peut également fonctionner dans un ou plusieurs des rôles suivants, selon la configuration permettant de répondre aux besoins de l’organisation partenaire de compte:  
  
-   Transmettre les jetons de sécurité, le serveur de fédération émet un jeton de sécurité pour le serveur proxy de fédération, qui transmet ensuite le jeton à l’ordinateur client. Le jeton de sécurité est utilisé pour fournir l’accès à une partie de confiance spécifique pour cet ordinateur client.  
  
-   Collecter les informations d’identification, le serveur proxy de fédération utilise une \(clientlogon.aspx\) par défaut client d’ouverture de session Web formulaire pour collecter les informations d’identification password\ par le biais de l’authentification basée sur les forms\. Toutefois, vous pouvez personnaliser ce formulaire pour accepter d’autres types d’authentification, telles que l’authentification du client Secure Sockets Layer \(SSL\) pris en charge. Pour plus d’informations sur la personnalisation de cette page, voir l’ouverture de session du Client de personnalisation et d’accueil des Pages de la découverte de domaine Kerberos \ ([http:///\/go.microsoft.com\/fwlink\/? LinkId\ = 104275](https://go.microsoft.com/fwlink/?LinkId=104275)\). Un serveur proxy de fédération n’accepte pas les informations d’identification via l’authentification Windows intégrée.  
  
Pour résumer, un serveur proxy de fédération du partenaire de compte agit comme un proxy pour les connexions client à un serveur de fédération qui se trouve dans le réseau d’entreprise. Le serveur proxy de fédération facilite également la distribution des jetons de sécurité pour les clients Internet qui sont destinés aux parties de confiance.  
  
> [!CAUTION]  
> Exposition d’un serveur proxy de fédération du partenaire de compte extranet de l’ouverture de session client formulaire Web accessible par tout le monde avec Internet accède. Cela peut potentiellement exposer votre organisation à certaines attaques password\, tels que les attaques de dictionnaire ou les attaques en force brute qui peuvent déclencher des verrouillages de compte pour les comptes d’utilisateur qui sont stockés dans le \(ADDS\) des Services de domaine ActiveDirectory d’entreprise.  
  

## <a name="see-also"></a>Voir aussi
[Guide de conception ADFS dans Windows Server2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
