---
ms.assetid: de7e1e4a-f96d-4b59-ac9b-f65f5d37a96f
title: Fournir aux utilisateurs d’une autre organisation un accès à vos applications et services prenant en charge les revendications
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: a0b2429599036f2893f23df7921a11c8232d9f67
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359068"
---
# <a name="provide-users-in-another-organization-access-to-your-claims-aware-applications-and-services"></a>Fournir aux utilisateurs d’une autre organisation un accès à vos applications et services prenant en charge les revendications


Lorsque vous êtes administrateur de l’organisation partenaire de ressource dans Services ADFS \(AD FS @ no__t-1 et que vous avez un objectif de déploiement pour fournir un accès fédéré pour les utilisateurs d’une autre organisation \(Le partenaire de compte Organisation @ no__t-3 vers une application claims @ no__t-4aware ou un service Web @ no__t-5based situé dans votre organisation \(The organisation partenaire de ressource @ no__t-7 :  
  
-   Les utilisateurs fédérés de votre organisation et des organisations qui ont configuré une approbation de Fédération pour votre organisation @no__t-les organisations partenaires 0account @ no__t-1 peuvent accéder à l’application ou au service sécurisé AD FS qui est hébergé par votre organisation. Pour plus d'informations, voir [Federated Web SSO Design](Federated-Web-SSO-Design.md).  
  
    Par exemple, Fabrikam souhaite que les employés du réseau d’entreprise aient un accès fédéré aux services web hébergés par Contoso.  
  
-   Les utilisateurs fédérés qui n’ont pas d’association directe avec une organisation approuvée \(such en tant que clients individuels @ no__t-1, qui sont connectés à un magasin d’attributs hébergé dans votre réseau de périmètre, peuvent accéder à plusieurs applications AD FS @ no__t-2secured, qui sont également hébergés dans votre réseau de périmètre, en se connectant une seule fois à partir d’ordinateurs clients qui se trouvent sur Internet. En d’autres termes, lorsque vous hébergez des comptes client pour permettre l’accès aux applications ou services de votre réseau de périmètre, les clients que vous hébergez dans un magasin d’attributs peuvent accéder à un ou plusieurs services ou applications du réseau de périmètre en se connectant une seule fois. Pour plus d'informations, voir [Web SSO Design](Web-SSO-Design.md).  
  
    Par exemple, Fabrikam peut souhaiter que ses clients disposent d’un seul @ no__t-0sign @ no__t-1On \(SSO @ no__t-3 pour accéder à plusieurs applications ou services hébergés sur son réseau de périmètre.  
  
Les composants suivants sont requis pour cet objectif de déploiement :  
  
-   **Active Directory Domain Services \(AD DS @ no__t-2 :** Le serveur de Fédération du partenaire de ressource doit être joint à un domaine Active Directory.  
  
-   **DNS de périmètre :** Domain Name System \(DNS @ no__t-1 doit contenir un enregistrement de ressource hôte \(A @ no__t-3 simple, afin que les ordinateurs clients puissent localiser le serveur de Fédération du partenaire de ressource et le serveur Web. Le serveur DNS peut héberger d’autres enregistrements DNS également requis dans le réseau de périmètre. Pour plus d'informations, voir [Name Resolution Requirements for Federation Servers](Name-Resolution-Requirements-for-Federation-Servers.md).  
  
-   **Serveur de Fédération du partenaire de ressource :** Le serveur de Fédération du partenaire de ressource valide AD FS jetons envoyés par les partenaires de compte. La découverte de partenaire de compte est effectuée via ce serveur de Fédération. Pour plus d’informations, consultez [revue du rôle du serveur de fédération du partenaire de ressource](Review-the-Role-of-the-Federation-Server-in-the-Resource-Partner.md).  
  
-   **Serveur web :** Le serveur web peut héberger une application ou un service web. Le serveur web confirme qu’il reçoit les jetons AD FS valides des utilisateurs fédérés avant d’autoriser l’accès à l’application ou au service web protégé.  
  
    À l’aide de Windows Identity Foundation \(WIF @ no__t-1, vous pouvez développer votre application ou service Web afin qu’il accepte les demandes d’ouverture de session des utilisateurs fédérés effectuées avec une méthode d’ouverture de session standard, telle que le nom d’utilisateur et le mot de passe.  
  
Après avoir vérifié les informations contenues dans les rubriques associées, vous pouvez commencer à déployer cet objectif en suivant les étapes décrites dans [Checklist : Implémentation d’une conception SSO de Web fédéré @ no__t-0 et [Checklist : Implémentation d’une conception SSO de Web @ no__t-0.  
  
L’illustration suivante montre chacun des composants requis pour cet objectif de déploiement AD FS.  
  
![accès à vos revendications](media/75358b16-2a6f-4e16-9cc4-b0e614480305.gif)  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
