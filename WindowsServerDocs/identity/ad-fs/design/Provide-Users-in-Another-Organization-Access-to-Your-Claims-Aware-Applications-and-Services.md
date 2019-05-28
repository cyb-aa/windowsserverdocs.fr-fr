---
ms.assetid: de7e1e4a-f96d-4b59-ac9b-f65f5d37a96f
title: Fournir aux utilisateurs d’une autre organisation un accès à vos applications et services prenant en charge les revendications
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4a13332cd7cf6361824f05ead4568a45211cc70a
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191018"
---
# <a name="provide-users-in-another-organization-access-to-your-claims-aware-applications-and-services"></a>Fournir aux utilisateurs d’une autre organisation un accès à vos applications et services prenant en charge les revendications


Lorsque vous êtes un administrateur de l’organisation partenaire de ressource dans Active Directory Federation Services \(AD FS\) et avoir un objectif de déploiement pour fournir un accès fédéré aux utilisateurs d’une autre organisation \(le organisation partenaire de compte\) en un cas de réclamation\-application prenant en charge ou un site Web\-en fonction de service qui se trouve dans votre organisation \(l’organisation partenaire ressource\):  
  
-   Les utilisateurs de votre organisation et dans les organisations qui ont configuré une fédération de confiance à votre organisation fédérés \(aux organisations de partenaires de compte\) peut accéder à AD FS application ou le service sécurisé qui est hébergé par votre organisation. Pour plus d'informations, voir [Federated Web SSO Design](Federated-Web-SSO-Design.md).  
  
    Par exemple, Fabrikam souhaite que les employés du réseau d’entreprise aient un accès fédéré aux services web hébergés par Contoso.  
  
-   Les utilisateurs fédérés qui disposent d’aucune association directe avec une organisation approuvée \(par exemple, les clients individuels\), qui sont connectés à un magasin d’attributs qui est hébergé dans votre réseau de périmètre, peuvent accéder à plusieurs AD FS\- applications sécurisées, qui sont aussi hébergées sur votre réseau de périmètre, en se connectant une seule fois à partir d’ordinateurs clients qui se trouvent sur Internet. En d’autres termes, lorsque vous hébergez des comptes client pour permettre l’accès aux applications ou services de votre réseau de périmètre, les clients que vous hébergez dans un magasin d’attributs peuvent accéder à un ou plusieurs services ou applications du réseau de périmètre en se connectant une seule fois. Pour plus d'informations, voir [Web SSO Design](Web-SSO-Design.md).  
  
    Par exemple, Fabrikam peut offrir à ses clients d’avoir unique\-connexion\-sur \(SSO\) accès à plusieurs applications ou services qui sont hébergés dans son réseau de périmètre.  
  
Les composants suivants sont requis pour cet objectif de déploiement :  
  
-   **Les Services de domaine Active Directory \(AD DS\):** Le serveur de fédération du partenaire ressource doit être joint à un domaine Active Directory.  
  
-   **DNS de périmètre :** Domain Name System \(DNS\) doit contenir un hôte simple \(A\) enregistrement de ressource pour que les ordinateurs clients puissent localiser le serveur de fédération du partenaire ressource et le serveur Web. Le serveur DNS peut héberger d’autres enregistrements DNS également requis dans le réseau de périmètre. Pour plus d'informations, voir [Name Resolution Requirements for Federation Servers](Name-Resolution-Requirements-for-Federation-Servers.md).  
  
-   **Serveur de fédération du partenaire de ressource :** Le serveur de fédération du partenaire ressource valide les jetons AD FS envoyés par les partenaires de compte. Découverte du partenaire de compte est effectuée via ce serveur de fédération. Pour plus d'informations, voir [Review the Role of the Federation Server in the Resource Partner](Review-the-Role-of-the-Federation-Server-in-the-Resource-Partner.md).  
  
-   **Serveur web :** Le serveur web peut héberger une application ou un service web. Le serveur web confirme qu’il reçoit les jetons AD FS valides des utilisateurs fédérés avant d’autoriser l’accès à l’application ou au service web protégé.  
  
    À l’aide de Windows Identity Foundation \(WIF\), vous pouvez développer votre application Web ou service afin qu’il accepte fédéré de demandes d’ouverture de session utilisateur qui sont effectués avec n’importe quelle méthode d’ouverture de session standard, telles que le nom d’utilisateur et mot de passe.  
  
Après avoir examiné les informations contenues dans les rubriques associées, vous pouvez commencer à déployer cet objectif en suivant les étapes de [liste de vérification : Implémentation d’une conception SSO Web fédéré](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md) et [liste de vérification : Implémentation d’une conception SSO de Web](../../ad-fs/deployment/Checklist--Implementing-a-Web-SSO-Design.md).  
  
L’illustration suivante montre chacun des composants requis pour cet objectif de déploiement AD FS.  
  
![l’accès à vos revendications](media/75358b16-2a6f-4e16-9cc4-b0e614480305.gif)  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
