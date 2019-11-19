---
ms.assetid: 824005ae-c3c1-459b-9baa-1660158918ab
title: Quand créer un serveur de fédération
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 91c260dad1bd260a7dad7320fecd15e6472c50a3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407885"
---
# <a name="when-to-create-a-federation-server"></a>Quand créer un serveur de fédération

Lorsque vous créez un serveur de Fédération Services ADFS \(AD FS\), vous fournissez un moyen permettant à votre organisation d’effectuer les opérations suivantes :  
  
-   Procédez à une authentification Web unique\-\-sur \(communication basée sur\)SSO avec une autre organisation \(qui possède également au moins un serveur de Fédération\) et, si nécessaire, avec les employés de votre propre organisation \(qui ont besoin d’un accès via Internet\).  
  
-   Permettre aux services frontaux d’emprunter l’identité des utilisateurs dans les services d’infrastructure à l’aide de la délégation d’identité. Pour plus d'informations, voir [When to Use Identity Delegation](When-to-Use-Identity-Delegation.md).  
  
Les sections suivantes décrivent certaines des décisions clés permettant de déterminer quand et où créer un ou plusieurs serveurs de Fédération.  
  
## <a name="determine-the-organizational-role-for-the-federation-server"></a>Déterminer le rôle organisationnel du serveur de fédération  
Pour prendre une décision informée concernant le moment de la création d’un serveur de Fédération, vous devez d’abord déterminer l’organisation dans laquelle le serveur résidera. Le rôle joué par un serveur de Fédération dans une organisation varie selon que vous placez le serveur de Fédération dans l’organisation partenaire de compte ou dans l’organisation partenaire de ressource.  
  
Lorsqu’un serveur de Fédération est placé dans le réseau d’entreprise du partenaire de compte, son rôle consiste à authentifier les informations d’identification de l’utilisateur des clients du navigateur, du service Web ou du sélecteur d’identité et à envoyer des jetons de sécurité aux clients. Pour plus d'informations, voir [Review the Role of the Federation Server in the Account Partner](Review-the-Role-of-the-Federation-Server-in-the-Account-Partner.md).  
  
Lorsqu’un serveur de Fédération est placé dans le réseau d’entreprise du partenaire de ressource, son rôle consiste à authentifier les utilisateurs, en fonction d’un jeton de sécurité émis par un serveur de Fédération dans l’organisation partenaire de ressource, ou son rôle est de rediriger les demandes de jeton à partir de des applications Web ou des services Web configurés pour l’organisation partenaire de compte à laquelle le client appartient. Pour plus d'informations, voir [Review the Role of the Federation Server in the Resource Partner](Review-the-Role-of-the-Federation-Server-in-the-Resource-Partner.md).  
  
## <a name="determine-which-ad-fs-design-to-deploy"></a>Déterminer la conception AD FS à déployer  
Vous créez des serveurs de Fédération dans votre organisation chaque fois que vous souhaitez déployer l’une des conceptions de AD FS suivantes :  
  
-   [Conception SSO web](Web-SSO-Design.md)  
  
-   [Conception SSO de web fédéré](Federated-Web-SSO-Design.md)  
  
Si nécessaire, une organisation qui déploie une conception SSO de Web fédéré peut configurer un serveur de Fédération unique pour qu’il agisse à la fois dans le rôle de partenaire de compte et dans le rôle de partenaire de ressource. Dans ce cas, le serveur de Fédération peut produire des Security Assertion Markup Language \(des jetons\) SAML, en fonction des comptes d’utilisateur de sa propre organisation, ou rediriger les demandes de jeton vers l’organisation, en fonction de l’endroit où résident les comptes des utilisateurs.  
  
> [!NOTE]  
> Pour la conception SSO de Web fédéré, il doit y avoir au moins un serveur de Fédération dans le partenaire de compte et au moins un serveur de Fédération dans le partenaire de ressource.  
  
## <a name="differences-between-a-federation-server-and-a-federation-server-proxy"></a>Différences entre un serveur de fédération et un serveur proxy de fédération  
Un serveur de Fédération peut servir des pages Web pour la signature\-dans, la stratégie, l’authentification et la découverte de la même façon qu’un serveur proxy de Fédération. Les principales différences entre un serveur de Fédération et un serveur proxy de Fédération sont liées aux opérations qu’un serveur de Fédération peut effectuer qu’un serveur proxy de Fédération ne peut pas exécuter.  
  
Voici les opérations que seul un serveur de Fédération peut effectuer :  
  
-   Le serveur de Fédération effectue les opérations de chiffrement qui produisent le jeton. Bien que les serveurs proxys de Fédération ne puissent pas produire de jetons, ils peuvent être utilisés pour acheminer ou rediriger les jetons vers les clients et, le cas échéant, vers le serveur de Fédération. Pour plus d’informations sur l’utilisation des serveurs de Fédération, consultez [quand créer un serveur proxy de Fédération](When-to-Create-a-Federation-Server-Proxy.md).  
  
-   Les serveurs de fédération prennent en charge l’utilisation de l’authentification intégrée de Windows pour les clients sur le réseau d’entreprise. les serveurs proxys de Fédération ne le font pas. Pour plus d’informations sur l’utilisation de l’authentification intégrée de Windows avec le serveur de Fédération, consultez [quand créer une batterie de serveurs de Fédération](When-to-Create-a-Federation-Server-Farm.md).  
  
> [!CAUTION]  
> L’intégrité et la confidentialité de la communication entre les serveurs de fédération et les bases de données de configuration SQL Server, les magasins d’attributs SQL Server, les contrôleurs de domaine et les instances AD LDS ne sont pas protégées par défaut. Pour atténuer ce risque, pensez à protéger le canal de communication entre ces serveurs à l’aide d’IPSEC ou à utiliser une connexion sécurisée physiquement entre tous ces serveurs. Pour la communication entre les serveurs de Fédération et les serveurs SQL, envisagez d’utiliser la protection SSL dans la chaîne de connexion. Pour les connexions entre les serveurs de Fédération et les contrôleurs de domaine, envisagez d’activer la signature et le chiffrement Kerberos. Pour LDAP, LDAP\/S n’est pas pris en charge pour AD LDS\/AD DS.  
  
## <a name="how-to-create-a-federation-server"></a>Comment créer un serveur de fédération  
Vous pouvez créer un serveur de Fédération à l’aide de l’Assistant Configuration du serveur de fédération AD FS ou de l’outil en ligne de commande fsconfig. exe\-. Quand vous utilisez l’un de ces outils, vous pouvez choisir l’une des options suivantes pour créer un serveur de fédération.  
  
-   Créer un serveur de Fédération autonome\-  
  
    Pour plus d’informations sur la configuration d’un serveur de Fédération stand\-autonome, consultez [créer un serveur de Fédération autonome](../../ad-fs/deployment/Create-a-Stand-Alone-Federation-Server.md).  
  
-   Créer le premier serveur de fédération dans une batterie de serveurs de fédération  
  
    Pour plus d’informations sur la façon de configurer le premier serveur de fédération ou d’ajouter un serveur de fédération à une batterie de serveurs, consultez [Create the First Federation Server in a Federation Server Farm](../../ad-fs/deployment/Create-the-First-Federation-Server-in-a-Federation-Server-Farm.md).  
  
-   Ajouter un serveur de fédération à une batterie de serveurs de fédération  
  
    Pour plus d’informations sur la façon d’ajouter un serveur de fédération à une batterie de serveurs, consultez [Add a Federation Server to a Federation Server Farm](../../ad-fs/deployment/Add-a-Federation-Server-to-a-Federation-Server-Farm.md).  
  
Pour obtenir des informations plus détaillées sur le fonctionnement de chacune de ces options, consultez [The Role of the AD FS Configuration Database](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md).  
  
Pour plus d’informations sur la configuration générale requise pour déployer un serveur de fédération, consultez [Checklist: Setting Up a Federation Server](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md).  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

