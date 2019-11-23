---
ms.assetid: 2d62386c-b466-4a54-b6fa-5d16cda120d8
title: Fournir à vos utilisateurs Active Directory un accès aux applications et services d'autres organisations
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: bcf9b9ec91c1757ad060747a6aa1589012c1ec14
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359029"
---
# <a name="provide-your-active-directory-users-access-to-the-applications-and-services-of-other-organizations"></a>Fournir à vos utilisateurs Active Directory un accès aux applications et services d'autres organisations

Cette Services ADFS \(AD FS objectif de déploiement de\) s’appuie sur l’objectif de [fournir à vos utilisateurs Active Directory l’accès à vos applications et services prenant en charge les revendications](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md).  
  
Lorsque vous êtes un administrateur dans l’organisation partenaire de compte et que votre objectif de déploiement consiste à fournir aux employés un accès fédéré aux ressources hébergées d'une autre organisation :  
  
-   Les employés qui sont connectés à un domaine Active Directory dans le réseau d’entreprise peuvent utiliser l’authentification unique\-\-sur \(fonctionnalité SSO\) pour accéder à plusieurs applications ou services basés sur le Web\-, qui sont sécurisés par AD FS, lorsque les applications ou les services se trouvent dans une autre organisation. Pour plus d'informations, voir [Federated Web SSO Design](Federated-Web-SSO-Design.md).  
  
    Par exemple, Fabrikam souhaite que les employés du réseau d'entreprise aient un accès fédéré aux services web hébergés dans Contoso.  
  
-   Les employés distants qui sont connectés à un domaine Active Directory peuvent obtenir des jetons AD FS à partir du serveur de Fédération de votre organisation pour obtenir un accès fédéré AD FS à des applications ou des services basés sur\-Web sécurisés qui sont hébergés dans une autre organisation.  
  
    Par exemple, Fabrikam peut souhaiter que ses employés distants aient un accès fédéré à des services sécurisés par AD FS qui sont hébergés dans contoso, sans que les employés de Fabrikam se trouvent sur le réseau d’entreprise de fabrikam.  
  
Outre les composants essentiels décrits dans [Provide Your Active Directory Users Access to Your Claims-Aware Applications and Services](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md) et ombrés dans la figure ci-dessous, les composants suivants sont obligatoires pour cet objectif de déploiement :  
  
-   **Proxy du serveur de Fédération du partenaire de compte :** Les employés qui accèdent à l’application ou au service fédéré à partir d’Internet peuvent utiliser ce composant AD FS pour effectuer l’authentification. Par défaut, ce composant effectue une authentification basée sur les formulaires, mais il peut également effectuer une authentification de base. Vous pouvez également configurer ce composant pour qu’il exécute protocole SSL \(SSL\) l’authentification du client si les employés de votre organisation ont des certificats à présenter. Pour plus d'informations, voir [Where to Place a Federation Server Proxy](Where-to-Place-a-Federation-Server-Proxy.md).  
  
-   **DNS de périmètre :** Cette implémentation du système DNS \(DNS\) fournit les noms d’hôte du réseau de périmètre. Pour plus d’informations sur la configuration du DNS de périmètre pour un serveur proxy de Fédération, consultez [Configuration requise pour la résolution de noms pour les serveurs proxys de Fédération](Name-Resolution-Requirements-for-Federation-Server-Proxies.md).  
  
-   **Employé distant :** L’employé distant accède à une application Web\-\(via un navigateur Web pris en charge\) ou à un service Web\-\(via une application\), en utilisant des informations d’identification valides du réseau d’entreprise, tandis que l’employé est hors site à l’aide d’Internet. L’ordinateur client de l’employé dans l’emplacement distant communique directement avec le serveur proxy de Fédération pour générer un jeton et s’authentifier auprès de l’application ou du service.  
  
Après avoir vérifié les informations contenues dans les rubriques associées, vous pouvez commencer à déployer cet objectif en suivant les étapes de [Checklist: Implementing a Federated Web SSO Design](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md).  
  
L’illustration suivante montre chacun des composants requis pour cet objectif de déploiement AD FS.  
  
![accès à vos applications](media/50af4837-31e0-451f-a942-e705c2300065.gif)  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
