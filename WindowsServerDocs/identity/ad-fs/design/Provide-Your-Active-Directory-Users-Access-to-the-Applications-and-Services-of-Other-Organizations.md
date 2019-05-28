---
ms.assetid: 2d62386c-b466-4a54-b6fa-5d16cda120d8
title: Fournir à vos utilisateurs Active Directory un accès aux applications et services d'autres organisations
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fc0cbb461a48d04ddaa677d4de2369ef58fd5390
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190960"
---
# <a name="provide-your-active-directory-users-access-to-the-applications-and-services-of-other-organizations"></a>Fournir à vos utilisateurs Active Directory un accès aux applications et services d'autres organisations

Cette Active Directory Federation Services \(AD FS\) objectif de déploiement s’appuie sur l’objectif de [fournir Your Active Directory Users Access to Your Claims-Aware Applications and Services](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md).  
  
Lorsque vous êtes un administrateur dans l’organisation partenaire de compte et que votre objectif de déploiement consiste à fournir aux employés un accès fédéré aux ressources hébergées d'une autre organisation :  
  
-   Les employés qui sont connectés à un domaine Active Directory dans le réseau d’entreprise peuvent utiliser unique\-connexion\-sur \(SSO\) fonctionnalité pour accéder à plusieurs Web\-en fonction des applications ou services, qui sont sécurisés par AD FS, lorsque ces applications ou services se trouvent dans une autre organisation. Pour plus d'informations, voir [Federated Web SSO Design](Federated-Web-SSO-Design.md).  
  
    Par exemple, Fabrikam souhaite que les employés du réseau d'entreprise aient un accès fédéré aux services web hébergés dans Contoso.  
  
-   Les employés distants qui sont connectés à un domaine Active Directory peuvent obtenir des jetons AD FS à partir du serveur de fédération dans votre organisation pour obtenir un accès fédéré à AD FS sécurisée de Web\-en fonction des applications ou services qui sont hébergés dans un autre organisation.  
  
    Par exemple, Fabrikam souhaite que ses employés à distance ont un accès fédéré aux services AD FS sécurisée qui sont hébergés dans Contoso, sans nécessiter les employés de Fabrikam doivent se trouver sur le réseau d’entreprise Fabrikam.  
  
Outre les composants essentiels décrits dans [Provide Your Active Directory Users Access to Your Claims-Aware Applications and Services](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md) et ombrés dans la figure ci-dessous, les composants suivants sont obligatoires pour cet objectif de déploiement :  
  
-   **Proxy de serveur de fédération de partenaire de compte :** Les employés qui accèdent à l’application ou au service fédéré à partir d’Internet peuvent utiliser ce composant AD FS pour effectuer l’authentification. Par défaut, ce composant effectue une authentification basée sur les formulaires, mais il peut également effectuer une authentification de base. Vous pouvez également configurer ce composant pour effectuer le protocole SSL (Secure Sockets Layer) \(SSL\) l’authentification du client si les utilisateurs de votre organisation disposent des certificats à présenter. Pour plus d'informations, voir [Where to Place a Federation Server Proxy](Where-to-Place-a-Federation-Server-Proxy.md).  
  
-   **DNS de périmètre :** Cette implémentation du système de nom de domaine \(DNS\) fournit les noms d’hôte pour le réseau de périmètre. Pour plus d’informations sur la configuration DNS de périmètre pour un serveur proxy de fédération, consultez [Name Resolution Requirements for Federation Server Proxies](Name-Resolution-Requirements-for-Federation-Server-Proxies.md).  
  
-   **Employé distant :** L’employé distant accède à un site Web\-application basée sur \(via un navigateur Web pris en charge\) ou un site Web\-service basé sur \(via une application\), à l’aide des informations d’identification valides à partir de le réseau d’entreprise, tandis que l’employé navigue sur Internet. Ordinateur client de l’employé dans l’emplacement distant communique directement avec le serveur proxy de fédération pour générer un jeton et de s’authentifier à l’application ou le service.  
  
Après avoir examiné les informations contenues dans les rubriques associées, vous pouvez commencer à déployer cet objectif en suivant les étapes de [liste de vérification : Implémentation d’une conception SSO Web fédéré](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md).  
  
L’illustration suivante montre chacun des composants requis pour cet objectif de déploiement AD FS.  
  
![accéder à vos applications](media/50af4837-31e0-451f-a942-e705c2300065.gif)  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
