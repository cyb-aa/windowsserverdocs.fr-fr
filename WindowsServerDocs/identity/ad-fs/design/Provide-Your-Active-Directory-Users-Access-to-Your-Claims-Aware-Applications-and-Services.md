---
ms.assetid: d254fca3-85a1-424d-ac22-d6687ec3798e
title: Fournir à vos utilisateurs Active Directory un accès à vos applications et services prenant en charge les revendications
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 48436f8e98af965f2bc2b38d296c4a15924e4db1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407951"
---
# <a name="provide-your-active-directory-users-access-to-your-claims-aware-applications-and-services"></a>Fournir à vos utilisateurs Active Directory un accès à vos applications et services prenant en charge les revendications

Lorsque vous êtes administrateur de l’organisation partenaire de compte dans un déploiement Services ADFS \(AD FS @ no__t-1 et que vous avez un objectif de déploiement pour fournir un accès unique à @ no__t-2sign @ no__t-3on \(SSO @ no__t-5 pour les employés sur le réseau d’entreprise à vos ressources hébergées :  
  
-   Les employés qui sont connectés à une forêt Active Directory dans le réseau d’entreprise peuvent utiliser l’authentification unique pour accéder à plusieurs applications ou services dans le réseau de périmètre de votre propre organisation. Ces applications et services sont sécurisés par AD FS.  
  
    Par exemple, Fabrikam souhaite que les employés du réseau d’entreprise aient un accès fédéré aux applications Web @ no__t-0based qui sont hébergées dans le réseau de périmètre pour Fabrikam.  
  
-   Les employés distants qui sont connectés à un domaine Active Directory peuvent obtenir des jetons AD FS à partir du serveur de Fédération de votre organisation pour obtenir l’accès fédéré AD FS aux applications ou services Web @ no__t-0secured @ no__t-1based Web @-qui résident également dans votre dernière.  
  
-   Les informations contenues dans le magasin d’attributs Active Directory peuvent être remplies dans les jetons AD FS des employés.  
  
Les composants suivants sont requis pour cet objectif de déploiement :  
  
-   **Active Directory Domain Services \(AD DS @ no__t-2 :** Les services AD DS contiennent les comptes d’utilisateur des employés utilisés pour générer des jetons AD FS. Les informations, telles que les appartenances aux groupes et les attributs, sont remplies dans les jetons AD FS en tant que revendications de groupe et revendications personnalisées.  
  
    > [!NOTE]  
    > Vous pouvez également utiliser le protocole Lightweight Directory Access \(LDAP @ no__t-1 ou langage SQL \(SQL @ no__t-3 pour contenir les identités pour la génération de jetons AD FS.  
  
-   **Système DNS d’entreprise :** Cette implémentation de Domain Name System \(DNS @ no__t-1 contient un enregistrement de ressource hôte \(A @ no__t-3 simple, afin que les clients intranet puissent localiser le serveur de Fédération de compte. Cette implémentation de système DNS peut également héberger d’autres enregistrements DNS requis dans le réseau d’entreprise. Pour plus d'informations, voir [Name Resolution Requirements for Federation Servers](Name-Resolution-Requirements-for-Federation-Servers.md).  
  
-   **Serveur de Fédération du partenaire de compte :** Ce serveur de Fédération est joint à un domaine dans la forêt du partenaire de compte. Il authentifie les comptes d’utilisateur des employés et génère des jetons AD FS. L’ordinateur client de l’employé procède à l’authentification intégrée de Windows sur ce serveur de Fédération pour générer un jeton de AD FS. Pour plus d’informations, consultez [revue du rôle du serveur de fédération du partenaire de compte](Review-the-Role-of-the-Federation-Server-in-the-Account-Partner.md).  
  
    Le serveur de Fédération du partenaire de compte peut authentifier les utilisateurs suivants :  
  
    -   Les employés disposant de comptes d’utilisateurs dans ce domaine  
  
    -   Les employés disposant de comptes d’utilisateurs dans cette forêt  
  
    -   Les employés disposant de comptes d’utilisateur n’importe où dans les forêts approuvées par cette forêt \(through a deux no__t de sécurité Windows 1way de confiance @ no__t-2  
  
-   **Employé :** Un employé accède à un service Web @ no__t-0based \(through une application @ no__t-2 ou une application Web @ no__t-3based \(through un navigateur Web pris en charge @ no__t-5 pendant qu’il est connecté au réseau d’entreprise. L’ordinateur client de l’employé sur le réseau d’entreprise communique directement avec le serveur de Fédération pour l’authentification.  
  
Après avoir vérifié les informations contenues dans les rubriques associées, vous pouvez commencer à déployer cet objectif en suivant les étapes décrites dans [Checklist : Implémentation d’une conception SSO de Web fédéré @ no__t-0.  
  
L’illustration suivante montre chacun des composants requis pour cet objectif de déploiement AD FS.  
  
![accès à vos revendications](media/31394ea8-fecb-4372-ac3f-cc3cf566ffc9.gif)  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
