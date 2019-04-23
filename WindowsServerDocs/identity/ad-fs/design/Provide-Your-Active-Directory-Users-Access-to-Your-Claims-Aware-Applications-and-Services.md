---
ms.assetid: d254fca3-85a1-424d-ac22-d6687ec3798e
title: Fournir à vos utilisateurs Active Directory un accès à vos applications et services prenant en charge les revendications
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f6fb37c16c20915c0051e3a24cdb0c147ae92d9c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835870"
---
# <a name="provide-your-active-directory-users-access-to-your-claims-aware-applications-and-services"></a>Fournir à vos utilisateurs Active Directory un accès à vos applications et services prenant en charge les revendications

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Lorsque vous êtes un administrateur de l’organisation partenaire de compte dans un Active Directory Federation Services \(AD FS\) déploiement et que vous avez un objectif de déploiement pour fournir unique\-connexion\-sur \( L’authentification unique\) accès pour les employés du réseau d’entreprise à vos ressources hébergées :  
  
-   Les employés qui sont connectés à une forêt Active Directory dans le réseau d’entreprise peuvent utiliser l’authentification unique pour accéder à plusieurs applications ou services dans le réseau de périmètre de votre propre organisation. Ces applications et services sécurisés par AD FS.  
  
    Par exemple, Fabrikam souhaite que les employés du réseau d’entreprise ont un accès fédéré aux Web\-en fonction des applications qui sont hébergées dans le réseau de périmètre pour Fabrikam.  
  
-   Les employés distants qui sont connectés à un domaine Active Directory peuvent obtenir des jetons AD FS à partir du serveur de fédération dans votre organisation pour obtenir un accès fédéré à AD FS\-sécurisés Web\-en fonction des applications ou services qui se trouvent également dans votre organisation.  
  
-   Les informations contenues dans le magasin d’attributs Active Directory peuvent être remplies dans les jetons AD FS des employés.  
  
Les composants suivants sont requis pour cet objectif de déploiement :  
  
-   **Les Services de domaine Active Directory \(AD DS\):** Les services AD DS contiennent les comptes d’utilisateur des employés utilisés pour générer des jetons AD FS. Les informations, telles que les appartenances aux groupes et les attributs, sont remplies dans les jetons AD FS en tant que revendications de groupe et revendications personnalisées.  
  
    > [!NOTE]  
    > Vous pouvez également utiliser Lightweight Directory Access Protocol \(LDAP\) ou Structured Query Language \(SQL\) pour contenir les identités pour AD FS du jeton génération.  
  
-   **Système DNS d’entreprise :** Cette implémentation du système de nom de domaine \(DNS\) contient un hôte simple \(A\) enregistrement de ressource pour que les clients intranet puissent localiser le serveur de fédération de compte. Cette implémentation de système DNS peut également héberger d’autres enregistrements DNS requis dans le réseau d’entreprise. Pour plus d'informations, voir [Name Resolution Requirements for Federation Servers](Name-Resolution-Requirements-for-Federation-Servers.md).  
  
-   **Serveur de fédération du partenaire de compte :** Ce serveur de fédération est joint à un domaine dans la forêt du partenaire de compte. Il authentifie les comptes d’utilisateur des employés et génère des jetons AD FS. L’ordinateur client pour l’employé effectue l’authentification intégrée de Windows sur ce serveur de fédération pour générer un jeton AD FS. Pour plus d'informations, voir [Review the Role of the Federation Server in the Account Partner](Review-the-Role-of-the-Federation-Server-in-the-Account-Partner.md).  
  
    Le serveur de fédération de partenaire de compte peut authentifier les utilisateurs suivants :  
  
    -   Les employés disposant de comptes d’utilisateurs dans ce domaine  
  
    -   Les employés disposant de comptes d’utilisateurs dans cette forêt  
  
    -   Les employés avec des comptes d’utilisateur dans des forêts qui sont approuvés par cette forêt \(via deux\-moyen de confiance de Windows\)  
  
-   **Employé :** Un employé accède à un site Web\-service basé sur \(via une application\) ou un site Web\-application basée sur \(via un navigateur Web pris en charge\) il ou elle est une session sur le réseau d’entreprise. Ordinateur client de l’employé sur le réseau d’entreprise communique directement avec le serveur de fédération pour l’authentification.  
  
Après avoir examiné les informations contenues dans les rubriques associées, vous pouvez commencer à déployer cet objectif en suivant les étapes de [liste de vérification : Implémentation d’une conception SSO Web fédéré](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md).  
  
L’illustration suivante montre chacun des composants requis pour cet objectif de déploiement AD FS.  
  
![l’accès à vos revendications](media/31394ea8-fecb-4372-ac3f-cc3cf566ffc9.gif)  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
