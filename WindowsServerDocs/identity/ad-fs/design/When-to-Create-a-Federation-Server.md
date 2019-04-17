---
ms.assetid: 824005ae-c3c1-459b-9baa-1660158918ab
title: "Création d’un serveur de fédération"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 8013764b88a1061cfcaa3a507466c111bfd59aad
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="when-to-create-a-federation-server"></a>Création d’un serveur de fédération

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Lorsque vous créez une fédération serveurdans Active Directory Federation Services \(AD FS\), vous offrez à laquelle votre organisation peut:  
  
-   Impliquer dans le site Web de connexion de single\ sur \ (SSO\) – en fonction de la communication avec une autre organisation \ (qui a également au moins un serveur de fédération) et, si nécessaire, avec les employés de votre propre organisation \ (qui ont besoin d’accéder via l’Internet).  
  
-   Permettre aux services frontaux d’emprunter l’identité des utilisateurs aux services d’infrastructure à l’aide de la délégation d’identité. Pour plus d’informations, voir [When to Use Identity Delegation](When-to-Use-Identity-Delegation.md).  
  
Les sections suivantes décrivent certaines décisions clés permettant de déterminer quand et où créer un ou plusieurs serveurs de fédération.  
  
## <a name="determine-the-organizational-role-for-the-federation-server"></a>Déterminer le rôle organisationnel du serveur de fédération  
Pour prendre une décision informée concernant la création d’un nouveau serveur de fédération, vous devez tout d’abord déterminer d’organisation dans laquelle réside le serveur. Le rôle joué par un serveur de fédération dans une organisation varie selon que vous placez le serveur de fédération dans l’organisation partenaire de compte ou dans l’organisation partenaire de ressource.  
  
Lorsqu’un serveur de fédération est placé dans le réseau d’entreprise du partenaire de compte, son rôle consiste à authentifier les informations d’identification de l’utilisateur du navigateur, le service Web ou les clients du sélecteur d’identité et d’envoyer des jetons de sécurité pour les clients. Pour plus d’informations, voir [revue du rôle du serveur de fédération du partenaire de compte](Review-the-Role-of-the-Federation-Server-in-the-Account-Partner.md).  
  
Lorsqu’un serveur de fédération est placé dans le réseau d’entreprise du partenaire de ressource, son rôle consiste à authentifier les utilisateurs, en fonction d’un jeton de sécurité émis par un serveur de fédération dans l’organisation partenaire de ressource, ou son rôle consiste à rediriger les demandes de jeton à partir d’applications Web configurées ou pour l’organisation partenaire de compte auquel appartient le client. Pour plus d’informations, voir [revue du rôle du serveur de fédération du partenaire de ressource](Review-the-Role-of-the-Federation-Server-in-the-Resource-Partner.md).  
  
## <a name="determine-which-ad-fs-design-to-deploy"></a>Déterminer la conception AD FS à déployer  
Vous créez les serveurs de fédération dans votre organisation chaque fois que vous souhaitez déployer un des modèles AD FS suivants:  
  
-   [Conception SSO de Web](Web-SSO-Design.md)  
  
-   [Conception SSO de Web fédéré](Federated-Web-SSO-Design.md)  
  
Si nécessaire, une organisation qui déploie une conception SSO de Web fédéré peut configurer un serveur de fédération unique afin qu’il agisse dans les deux le rôle de partenaire de compte et le rôle de partenaire de ressource. Dans ce cas, le serveur de fédération peut produire des jetons \(SAML\) Security Assertion Markup Language, basées sur les comptes d’utilisateur dans sa propre organisation, ou jeton réacheminer les demandes à l’organisation, fonction où résident les comptes des utilisateurs.  
  
> [!NOTE]  
> Pour la conception SSO de Web fédéré, il doit être au moins un serveur de fédération du partenaire de compte et au moins un serveur de fédération du partenaire de ressource.  
  
## <a name="differences-between-a-federation-server-and-a-federation-server-proxy"></a>Différences entre un serveur de fédération et d’un serveur proxy de fédération  
Un serveur de fédération peut traiter des pages Web pour la connexion dans, stratégie, l’authentification et de découverte dans la même façon qu’un serveur proxy de fédération. Les principales différences entre un serveur de fédération et d’un serveur proxy de fédération faire avec les opérations une fédération serveur peut exécuter qu’un serveur proxy de fédération ne peut pas effectuer.  
  
Voici les opérations que seul un serveur de fédération peut effectuer:  
  
-   Le serveur de fédération effectue les opérations de chiffrement qui produisent le jeton. Bien que les serveurs proxy de fédération ne peuvent pas produire des jetons, ils peuvent être utilisés pour acheminer ou rediriger les jetons pour les clients et, si nécessaire, sur le serveur de fédération. Pour plus d’informations sur l’utilisation de serveurs de fédération, consultez [quand créer un serveur Proxy de fédération](When-to-Create-a-Federation-Server-Proxy.md).  
  
-   Les serveurs de fédération prennent en charge l’utilisation de l’authentification Windows intégrée pour les clients sur le réseau d’entreprise; serveurs proxy de fédération ne le faites pas. Pour plus d’informations sur l’utilisation de l’authentification Windows intégrée avec le serveur de fédération, consultez [quand créer une batterie de serveurs de fédération](When-to-Create-a-Federation-Server-Farm.md).  
  
> [!CAUTION]  
> Communication entre les serveurs de fédération et bases de données de configuration SQL Server, les magasins d’attributs SQL Server, les contrôleurs de domaine et les instances AD LDS n’est pas l’intégrité et la confidentialité protégées par défaut. Pour atténuer le risque, envisagez de protéger le canal de communication entre ces serveurs à l’aide d’IPSEC ou à l’aide d’une connexion sécurisée physiquement entre tous ces serveurs. Pour la communication entre les serveurs de fédération et les serveurs SQL Server, envisagez d’utiliser la protection SSL dans la chaîne de connexion. Pour les connexions entre les serveurs de fédération et les contrôleurs de domaine, envisagez d’activer la signature Kerberos et le chiffrement. Pour LDAP, LDAP\/S n'est pas pris en charge pour AD LDS\/AD DS.  
  
## <a name="how-to-create-a-federation-server"></a>Procédure de création d’un serveur de fédération  
Vous pouvez créer un serveur de fédération à l’aide de l’Assistant Configuration du serveur de fédération AD FS ou l’outil de ligne de commande Fsconfig.exe. Lorsque vous utilisez un de ces outils, vous pouvez sélectionner une des options suivantes pour créer un serveur de fédération.  
  
-   Créer un serveur de fédération autonome  
  
    Pour plus d’informations sur comment configurer un serveur de fédération autonome, consultez [créer un serveur de fédération autonome](../../ad-fs/deployment/Create-a-Stand-Alone-Federation-Server.md).  
  
-   Créer le premier serveur de fédération dans une batterie de serveurs de fédération  
  
    Pour plus d’informations sur comment configurer le premier serveur de fédération ou ajouter un serveur de fédération à une batterie de serveurs, voir [créer le premier serveur de fédération dans une batterie de serveurs de fédération](../../ad-fs/deployment/Create-the-First-Federation-Server-in-a-Federation-Server-Farm.md).  
  
-   Ajouter un serveur de fédération à une batterie de serveurs de fédération  
  
    Pour plus d’informations sur l’ajout d’un serveur de fédération à une batterie de serveurs, voir [ajouter un serveur de fédération à une batterie de serveurs de fédération](../../ad-fs/deployment/Add-a-Federation-Server-to-a-Federation-Server-Farm.md).  
  
Pour plus d’informations sur le fonctionnement de chacune de ces options, voir [le rôle de la base de données de Configuration AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md).  
  
Pour plus d’informations sur la façon de configurer les conditions préalables nécessaires au déploiement d’un serveur de fédération, consultez [liste de vérification: Setting Up a Federation Server](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md).  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception ADFS dans Windows Server2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

