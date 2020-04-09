---
ms.assetid: d254fca3-85a1-424d-ac22-d6687ec3798e
title: Fournir à vos utilisateurs Active Directory un accès à vos applications et services prenant en charge les revendications
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 0cb530eacfa8239f3a2a135397e54becfadb602b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858572"
---
# <a name="provide-your-active-directory-users-access-to-your-claims-aware-applications-and-services"></a>Fournir à vos utilisateurs Active Directory un accès à vos applications et services prenant en charge les revendications

Lorsque vous êtes administrateur de l’organisation partenaire de compte dans un Services ADFS \(AD FS\) déploiement et que vous avez un objectif de déploiement pour fournir une\-de signature\-sur \(SSO\) accès pour les employés du réseau d’entreprise à vos ressources hébergées :  
  
-   Les employés qui sont connectés à une forêt Active Directory dans le réseau d’entreprise peuvent utiliser l’authentification unique pour accéder à plusieurs applications ou services dans le réseau de périmètre de votre propre organisation. Ces applications et services sont sécurisés par AD FS.  
  
    Par exemple, Fabrikam souhaite que les employés du réseau d’entreprise aient un accès fédéré aux applications Web\-qui sont hébergées dans le réseau de périmètre pour Fabrikam.  
  
-   Les employés distants qui sont connectés à un domaine Active Directory peuvent obtenir des jetons AD FS à partir du serveur de Fédération de votre organisation pour obtenir un accès fédéré à AD FS\-des applications ou des services Web\-sécurisés qui se trouvent également dans votre organisation.  
  
-   Les informations contenues dans le magasin d’attributs Active Directory peuvent être remplies dans les jetons AD FS des employés.  
  
Les composants suivants sont requis pour cet objectif de déploiement :  
  
-   **Active Directory Domain Services \(AD DS\):** AD DS contient les comptes d’utilisateur des employés utilisés pour générer des jetons AD FS. Les informations, telles que les appartenances aux groupes et les attributs, sont remplies dans les jetons AD FS en tant que revendications de groupe et revendications personnalisées.  
  
    > [!NOTE]  
    > Vous pouvez également utiliser le protocole LDAP (Lightweight Directory Access Protocol) \(\) ou langage SQL \(SQL\) pour contenir les identités pour la génération de jetons AD FS.  
  
-   **DNS d’entreprise :** Cette implémentation du système DNS (Domain Name System) \(DNS\) contient un hôte simple \(un enregistrement de ressource\) afin que les clients intranet puissent localiser le serveur de Fédération de compte. Cette implémentation de système DNS peut également héberger d’autres enregistrements DNS requis dans le réseau d’entreprise. Pour plus d'informations, consultez [Configuration de la résolution de noms pour les serveurs de fédération](Name-Resolution-Requirements-for-Federation-Servers.md).  
  
-   **Serveur de Fédération du partenaire de compte :** Ce serveur de Fédération est joint à un domaine dans la forêt du partenaire de compte. Il authentifie les comptes d’utilisateur des employés et génère des jetons AD FS. L’ordinateur client de l’employé procède à l’authentification intégrée de Windows sur ce serveur de Fédération pour générer un jeton de AD FS. Pour plus d'informations, voir [Review the Role of the Federation Server in the Account Partner](Review-the-Role-of-the-Federation-Server-in-the-Account-Partner.md).  
  
    Le serveur de Fédération du partenaire de compte peut authentifier les utilisateurs suivants :  
  
    -   Les employés disposant de comptes d’utilisateurs dans ce domaine  
  
    -   Les employés disposant de comptes d’utilisateurs dans cette forêt  
  
    -   Les employés disposant de comptes d’utilisateur n’importe où dans les forêts approuvées par cette forêt \(par le biais d’une\-d’approbation Windows\)  
  
-   **Employé :** Un employé accède à un service Web\-\(par le biais d’une application\) ou d’une application Web\-\(via un navigateur Web pris en charge\) lorsqu’il est connecté au réseau d’entreprise. L’ordinateur client de l’employé sur le réseau d’entreprise communique directement avec le serveur de Fédération pour l’authentification.  
  
Après avoir vérifié les informations contenues dans les rubriques associées, vous pouvez commencer à déployer cet objectif en suivant les étapes de [Checklist: Implementing a Federated Web SSO Design](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md).  
  
L’illustration suivante montre chacun des composants requis pour cet objectif de déploiement AD FS.  
  
![accès à vos revendications](media/31394ea8-fecb-4372-ac3f-cc3cf566ffc9.gif)  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
