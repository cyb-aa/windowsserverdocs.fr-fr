---
ms.assetid: de7e1e4a-f96d-4b59-ac9b-f65f5d37a96f
title: "Fournir aux utilisateurs d’une autre organisation d’accéder à vos Services et Applications prenant en charge les revendications"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b4ec08182e2523b0fcb16088ec9c1d094a5923fe
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="provide-users-in-another-organization-access-to-your-claims-aware-applications-and-services"></a>Fournir aux utilisateurs d’une autre organisation d’accéder à vos Services et Applications prenant en charge les revendications

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Lorsque vous êtes administrateur dans l’organisation partenaire de ressource dans ActiveDirectory Federation Services \(ADFS\) et que vous avez l’objectif de fournir un accès fédéré pour les utilisateurs dans une autre organisation \ (l’organization\ de partenaire de compte) à un service de console Web qui se trouve dans votre organisation ou à une application prenant en charge claims\ \ (l’organization\ de partenaire de ressource):  
  
-   Dans votre organisation et des organisations qui ont configuré une fédération confiance à votre organisation des utilisateurs fédérés \ (organizations\ du partenaire de compte) peuvent accéder à ADFS sécurisés par l’application ou le service est hébergé par votre organisation. Pour plus d’informations, voir [Federated Web SSO Design](Federated-Web-SSO-Design.md).  
  
    Par exemple, Fabrikam souhaite que ses employés du réseau d’entreprise ont un accès fédéré aux services Web hébergés dans Contoso.  
  
-   Utilisateurs fédérés qui disposent d’aucune association directe avec une organisation approuvée \ (par exemple, customers\ individuels), qui sont connectés à un magasin d’attributs qui est hébergé dans votre réseau de périmètre, peut accéder à plusieurs applications sécurisées par ADFS\, qui sont aussi hébergées sur votre réseau de périmètre en se connectant une seule fois à partir d’ordinateurs clients qui se trouvent sur Internet. En d’autres termes, lorsque vous hébergez des comptes client pour permettre l’accès aux applications ou services dans votre réseau de périmètre, les clients que vous hébergez dans un magasin d’attributs peuvent accéder à un ou plusieurs applications ou services dans le réseau de périmètre simplement en se connectant une seule fois. Pour plus d’informations, voir [conception SSO de Web](Web-SSO-Design.md).  
  
    Par exemple, Fabrikam souhaite que ses clients single\ connexion-\(SSO\) accès à plusieurs applications ou services qui sont hébergées sur son réseau de périmètre.  
  
Les composants suivants sont requis pour cet objectif de déploiement:  
  
-   **Les Services de domaine ActiveDirectory \(ADDS\):** le serveur de fédération de partenaire de ressource doit être joint à un domaine ActiveDirectory.  
  
-   **DNS de périmètre:** domaine nom système \(DNS\) doit contenir un enregistrement de ressource hôte simple \(A\) afin que les ordinateurs clients puissent localiser le serveur de fédération de partenaire de ressource et le serveur Web. Le serveur DNS peut héberger d’autres enregistrements DNS également requis dans le réseau de périmètre. Pour plus d’informations, voir [Name Resolution Requirements for Federation Servers](Name-Resolution-Requirements-for-Federation-Servers.md).  
  
-   **Serveur de fédération partenaire de ressource:** le serveur de fédération de partenaire de ressource valide les jetons ADFS par les partenaires de compte. Découverte du partenaire de compte est effectuée par le biais de ce serveur de fédération. Pour plus d’informations, voir [revue du rôle du serveur de fédération du partenaire de ressource](Review-the-Role-of-the-Federation-Server-in-the-Resource-Partner.md).  
  
-   **Serveur Web:** le serveur Web peut héberger une application Web ou un service Web. Le serveur Web confirme qu’il reçoit les jetons ADFS valides des utilisateurs fédérés avant d’autoriser l’accès à un service Web ou application Web protégée.  
  
    À l’aide de WindowsIdentityFoundation \(WIF\), vous pouvez développer votre application Web ou le service afin qu’il accepte les demandes d’ouverture de session utilisateur fédéré effectuées avec n’importe quelle méthode d’ouverture de session standard, tels que le nom d’utilisateur et mot de passe.  
  
Après avoir vérifié les informations dans les rubriques associées, vous pouvez commencer à déployer cet objectif en suivant les étapes de [liste de vérification: implémentation d’une conception SSO de Web fédéré](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md) et [liste de vérification: implémentation d’une conception SSO de Web](../../ad-fs/deployment/Checklist--Implementing-a-Web-SSO-Design.md).  
  
L’illustration suivante montre chacun des composants requis pour cet objectif de déploiement ADFS.  
  
![l’accès à vos demandes](media/75358b16-2a6f-4e16-9cc4-b0e614480305.gif)  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception ADFS dans Windows Server2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
