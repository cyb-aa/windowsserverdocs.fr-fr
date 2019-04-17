---
ms.assetid: d254fca3-85a1-424d-ac22-d6687ec3798e
title: "Fournir vos utilisateurs Active Directory un accès à vos Services et Applications prenant en charge les revendications"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f6fb37c16c20915c0051e3a24cdb0c147ae92d9c
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="provide-your-active-directory-users-access-to-your-claims-aware-applications-and-services"></a>Fournir vos utilisateurs Active Directory un accès à vos Services et Applications prenant en charge les revendications

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Lorsque vous êtes administrateur dans l’organisation partenaire de compte dans un déploiement de \(ADFS\) ActiveDirectory Federation Services et que vous avez l’objectif de fournir single\ connexion-\(SSO\) accès pour les employés du réseau d’entreprise à vos ressources hébergées:  
  
-   Les employés qui sont connectés à une forêt ActiveDirectory dans le réseau d’entreprise peuvent utiliser l’authentification unique pour accéder à plusieurs applications ou services dans le réseau de périmètre de votre propre organisation. Ces applications et services sécurisés par ADFS.  
  
    Par exemple, Fabrikam souhaite que les employés du réseau d’entreprise ont un accès fédéré aux applications de console Web qui sont hébergées sur le réseau de périmètre pour Fabrikam.  
  
-   Les employés distants qui sont connectés à un domaine ActiveDirectory peuvent obtenir des jetons ADFS à partir du serveur de fédération dans votre organisation pour avoir un accès fédéré aux applications de console Web sécurisés par ADFS\ ou services qui se trouvent également dans votre organisation.  
  
-   Les informations dans le magasin d’attributs ActiveDirectory peuvent être remplies dans les jetons ADFS des employés.  
  
Les composants suivants sont requis pour cet objectif de déploiement:  
  
-   **Les Services de domaine ActiveDirectory \(ADDS\):** services ADDS contiennent les comptes d’utilisateur des employés qui sont utilisés pour générer des jetons ADFS. Informations, telles que les appartenances aux groupes et les attributs, sont renseignées dans les jetons ADFS en tant que revendications de groupe et revendications personnalisées.  
  
    > [!NOTE]  
    > Vous pouvez également utiliser \(LDAP\) Lightweight Directory Access Protocol ou Structured Query Language \(SQL\) pour contenir les identités pour la génération de jetons ADFS.  
  
-   **DNS d’entreprise:** cette implémentation du système de nom de domaine \(DNS\) contient un enregistrement de ressource hôte simple \(A\) afin que les clients intranet puissent localiser le serveur de fédération de comptes. Cette implémentation du système DNS peut également héberger d’autres enregistrements DNS requis dans le réseau d’entreprise. Pour plus d’informations, voir [Name Resolution Requirements for Federation Servers](Name-Resolution-Requirements-for-Federation-Servers.md).  
  
-   **Serveur de fédération partenaire de compte:** ce serveur de fédération est joint à un domaine dans la forêt du partenaire de compte. Il authentifie les comptes d’utilisateur des employés et génère des jetons ADFS. L’ordinateur client pour l’employé effectue l’authentification intégrée de Windows par rapport à ce serveur de fédération pour générer un jeton ADFS. Pour plus d’informations, voir [revue du rôle du serveur de fédération du partenaire de compte](Review-the-Role-of-the-Federation-Server-in-the-Account-Partner.md).  
  
    Le serveur de fédération de partenaire de compte peut authentifier les utilisateurs suivants:  
  
    -   Employés disposant de comptes d’utilisateur dans ce domaine  
  
    -   Employés disposant de comptes d’utilisateurs dans cette forêt  
  
    -   Les employés disposant de comptes d’utilisateur dans des forêts qui sont approuvées par cette forêt \ (via un trust\ Windows contribuent voies)  
  
-   **Employé:** un employé accède à un service basé sur la console Web \(through an application\) ou une application basée sur la console Web \ (via un browser\ Web pris en charge) qu’il est ouvert une session sur le réseau d’entreprise. Ordinateur client de l’employé sur le réseau d’entreprise communique directement avec le serveur de fédération pour l’authentification.  
  
Après avoir vérifié les informations dans les rubriques associées, vous pouvez commencer à déployer cet objectif en suivant les étapes de [liste de vérification: implémentation d’une conception SSO de Web fédéré](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md).  
  
L’illustration suivante montre chacun des composants requis pour cet objectif de déploiement ADFS.  
  
![l’accès à vos demandes](media/31394ea8-fecb-4372-ac3f-cc3cf566ffc9.gif)  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception ADFS dans Windows Server2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
