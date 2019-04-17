---
ms.assetid: 2d62386c-b466-4a54-b6fa-5d16cda120d8
title: "Fournir un accès à vos utilisateurs Active Directory pour les Applications et Services d’autres organisations"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d50d26c5c654e5c91b82f6f209e21f257221c12d
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="provide-your-active-directory-users-access-to-the-applications-and-services-of-other-organizations"></a>Fournir un accès à vos utilisateurs Active Directory pour les Applications et Services d’autres organisations

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Cet objectif de déploiement ActiveDirectory Federation Services \(ADFS\) s’appuie sur l’objectif dans [fournir Your ActiveDirectory Users Access to Your Claims-Aware Applications and Services](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md).  
  
Lorsque vous êtes administrateur dans l’organisation partenaire de compte et que vous avez un objectif de fournir un accès fédéré pour les employés hébergé des ressources dans une autre organisation:  
  
-   Les employés qui sont connectés à un domaine ActiveDirectory dans le réseau d’entreprise peuvent utiliser single\ connexion-\(SSO\) fonctionnalité pour accéder à plusieurs applications basées sur la console Web ou des services, qui sont sécurisés par ADFS, lorsque ces applications ou services se trouvent dans une autre organisation. Pour plus d’informations, voir [Federated Web SSO Design](Federated-Web-SSO-Design.md).  
  
    Par exemple, Fabrikam souhaite que les employés du réseau d’entreprise ont un accès fédéré aux services Web hébergés dans Contoso.  
  
-   Les employés distants qui sont connectés à un domaine ActiveDirectory peuvent obtenir des jetons ADFS à partir du serveur de fédération dans votre organisation pour avoir un accès fédéré aux applications de console Web ADFS sécurisé ou services qui sont hébergées dans une autre organisation.  
  
    Par exemple, Fabrikam souhaite que ses employés distants ont un accès fédéré aux services ADFS sécurisé qui sont hébergés dans Contoso, sans nécessiter les employés de Fabrikam fassent sur le réseau d’entreprise Fabrikam.  
  
Outre les composants essentiels décrits dans [fournir Your ActiveDirectory Users Access to Your Claims-Aware Applications and Services](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md) et qui sont grisées dans l’illustration suivante, les composants suivants sont requis pour cet objectif de déploiement:  
  
-   **Serveur proxy de fédération partenaire de compte:** les employés qui accèdent à l’application ou au service fédéré à partir d’Internet peuvent utiliser ce composant ADFS pour effectuer l’authentification. Par défaut, ce composant effectue l’authentification par formulaire, mais il peut également effectuer l’authentification de base. Vous pouvez également configurer ce composant pour effectuer l’authentification du client Secure Sockets Layer \(SSL\) si les employés de votre organisation disposent des certificats à présenter. Pour plus d’informations, voir [où placer un serveur Proxy de fédération](Where-to-Place-a-Federation-Server-Proxy.md).  
  
-   **DNS de périmètre:** cette implémentation du système de nom de domaine \(DNS\) fournit les noms d’hôte du réseau de périmètre. Pour plus d’informations sur la configuration DNS de périmètre pour un serveur proxy de fédération, consultez [Name Resolution Requirements for Federation Server Proxies](Name-Resolution-Requirements-for-Federation-Server-Proxies.md).  
  
-   **Employé distant:** l’employé distant accède à une application basée sur la console Web \ (via un browser\ Web pris en charge) ou un service basé sur la console Web \ (via un appel à leurs), à l’aide des informations d’identification valides du réseau d’entreprise, tandis que l’employé navigue sur Internet. Ordinateur client de l’employé dans l’emplacement distant communique directement avec le serveur proxy de fédération pour générer un jeton et de s’authentifier auprès de l’application ou le service.  
  
Après avoir vérifié les informations dans les rubriques associées, vous pouvez commencer à déployer cet objectif en suivant les étapes de [liste de vérification: implémentation d’une conception SSO de Web fédéré](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md).  
  
L’illustration suivante montre chacun des composants requis pour cet objectif de déploiement ADFS.  
  
![Accéder à vos applications](media/50af4837-31e0-451f-a942-e705c2300065.gif)  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception ADFS dans Windows Server2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
