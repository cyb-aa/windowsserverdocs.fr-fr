---
ms.assetid: 1b3a03c0-5558-4177-9b2f-e9d6ce3271cd
title: Revue du rôle du serveur proxy de fédération du partenaire de compte
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 876dee31a38506f7d7934bd8adfb51766bb60ce5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407936"
---
# <a name="review-the-role-of-the-federation-server-proxy-in-the-account-partner"></a>Revue du rôle du serveur proxy de fédération du partenaire de compte

Le rôle principal du serveur proxy de Fédération dans le réseau de périmètre de l’organisation partenaire de compte \(dans\) services ADFS AD FS consiste à collecter les informations d’authentification à partir d’un ordinateur client qui ouvre une session. via Internet et pour transmettre ces informations d’identification au serveur de Fédération, qui se trouve dans le réseau d’entreprise de l’organisation partenaire de compte. Le compte de l’ordinateur client est stocké dans le magasin d’attributs du partenaire de compte.  
  
Un serveur proxy de Fédération peut également fonctionner dans un ou plusieurs des rôles suivants, selon la façon dont vous le configurez pour répondre aux besoins de l’organisation partenaire de compte :  
  
-   Relais des jetons de sécurité : le serveur de Fédération émet un jeton de sécurité pour le serveur proxy de Fédération, qui relaie ensuite le jeton vers l’ordinateur client. Le jeton de sécurité est utilisé pour fournir l’accès à une partie de confiance spécifique pour cet ordinateur client.  
  
-   Collecter les informations d’identification : le serveur proxy de Fédération \(utilise un formulaire Web d’ouverture de session client par défaut clientlogon. aspx\) pour\-collecter les informations d’identification basées sur le mot de passe via l’authentification basée sur les\-formulaires. Toutefois, vous pouvez personnaliser ce formulaire pour accepter d’autres types d’authentification pris en charge, \(par\) exemple protocole SSL l’authentification du client SSL. Pour plus d’informations sur la personnalisation de cette page, consultez Personnalisation de l’ouverture de session client et \(des pages de découverte du domaine d’hébergement [http :\/\/\/Go.Microsoft.com\/fwlink ? LinkId\=104275](https://go.microsoft.com/fwlink/?LinkId=104275).\) Un serveur proxy de Fédération n’accepte pas les informations d’identification via l’authentification intégrée de Windows.  
  
Pour résumer, un serveur proxy de Fédération du partenaire de compte agit comme un proxy pour les ouvertures de session client sur un serveur de Fédération qui se trouve dans le réseau d’entreprise. Le serveur proxy de Fédération facilite également la distribution des jetons de sécurité aux clients Internet qui sont destinés aux parties de confiance.  
  
> [!CAUTION]  
> L’exposition d’un serveur proxy de Fédération sur l’extranet du partenaire de compte rendra le formulaire Web d’ouverture de session client accessible à toute personne disposant d’un accès à Internet. Cela peut potentiellement exposer votre organisation à certaines attaques basées\-sur un mot de passe, telles que les attaques de dictionnaire ou les attaques en force brute qui peuvent déclencher des verrouillages de compte pour les comptes d’utilisateur stockés dans le domaine Active Directory d’entreprise. Services \(AD DS\).  
  

## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
