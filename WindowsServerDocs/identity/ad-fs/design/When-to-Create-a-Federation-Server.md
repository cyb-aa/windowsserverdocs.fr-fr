---
ms.assetid: 824005ae-c3c1-459b-9baa-1660158918ab
title: Quand créer un serveur de fédération
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 7e61c734780baa1482670af3f24697c10345b292
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190590"
---
# <a name="when-to-create-a-federation-server"></a>Quand créer un serveur de fédération

Lorsque vous créez un serveurdans de fédération Active Directory Federation Services \(AD FS\), vous fournissez un moyen par lequel votre organisation peut :  
  
-   Collaborer dans Web seul\-connexion\-sur \(SSO\)– en fonction de la communication avec une autre organisation \(ayant au moins un serveur de fédération\) et, si nécessaire, avec le employés de votre propre organisation \(qui ont besoin d’accéder via Internet\).  
  
-   Permettre aux services frontaux d’emprunter l’identité des utilisateurs dans les services d’infrastructure à l’aide de la délégation d’identité. Pour plus d'informations, voir [When to Use Identity Delegation](When-to-Use-Identity-Delegation.md).  
  
Les sections suivantes décrivent certaines décisions clés permettant de déterminer quand et où créer un ou plusieurs serveurs de fédération.  
  
## <a name="determine-the-organizational-role-for-the-federation-server"></a>Déterminer le rôle organisationnel du serveur de fédération  
Pour prendre une décision éclairée concernant quand créer un nouveau serveur de fédération, vous devez tout d’abord déterminer d’organisation dans laquelle réside le serveur. Le rôle joué par un serveur de fédération dans une organisation varie selon que vous placez le serveur de fédération dans l’organisation partenaire de compte ou de l’organisation partenaire de ressource.  
  
Lorsqu’un serveur de fédération est placé dans le réseau d’entreprise du partenaire de compte, son rôle consiste à authentifier les informations d’identification de l’utilisateur du navigateur, le service Web ou les clients du sélecteur d’identité et envoyer des jetons de sécurité pour les clients. Pour plus d’informations, consultez [revue du rôle du serveur de fédération du partenaire de compte](Review-the-Role-of-the-Federation-Server-in-the-Account-Partner.md).  
  
Lorsqu’un serveur de fédération est placé dans le réseau d’entreprise du partenaire de ressource, son rôle consiste à authentifier les utilisateurs, selon un jeton de sécurité émis par un serveur de fédération dans l’organisation partenaire de ressource, ou que son rôle consiste à rediriger les demandes de jeton à partir de les applications Web configurées ou des services Web de l’organisation partenaire de compte auquel appartient le client. Pour plus d’informations, consultez [revue du rôle du serveur de fédération du partenaire de ressource](Review-the-Role-of-the-Federation-Server-in-the-Resource-Partner.md).  
  
## <a name="determine-which-ad-fs-design-to-deploy"></a>Déterminer la conception AD FS à déployer  
Vous créez des serveurs de fédération dans votre organisation chaque fois que vous souhaitez déployer une des conceptions AD FS suivantes :  
  
-   [Conception SSO web](Web-SSO-Design.md)  
  
-   [Conception SSO de web fédéré](Federated-Web-SSO-Design.md)  
  
Si nécessaire, une organisation qui déploie une conception SSO de Web fédéré peut configurer un serveur de fédération unique afin qu’elle agisse dans les deux le rôle de partenaire de compte et le rôle du partenaire ressource. Dans ce cas, le serveur de fédération peut produire Security Assertion Markup Language \(SAML\) des jetons, basés sur les comptes d’utilisateur dans sa propre organisation ou rediriger les demandes de jeton à l’organisation, selon où se trouvent les comptes d’utilisateurs .  
  
> [!NOTE]  
> Pour la conception SSO de Web fédéré, il doit être au moins un serveur de fédération du partenaire de compte et au moins un serveur de fédération du partenaire de ressource.  
  
## <a name="differences-between-a-federation-server-and-a-federation-server-proxy"></a>Différences entre un serveur de fédération et un serveur proxy de fédération  
Un serveur de fédération peut traiter des pages Web pour la connexion\-de stratégie, l’authentification et la découverte de la même manière qu’un serveur proxy de fédération. Les principales différences entre un serveur de fédération et un serveur proxy de fédération ont à faire avec les opérations d’une fédération serveur peut exécuter qu’un serveur proxy de fédération ne peut pas effectuer.  
  
Voici les opérations qu’un serveur de fédération peut effectuer :  
  
-   Le serveur de fédération effectue les opérations de chiffrement qui produisent le jeton. Bien que les serveurs proxy de fédération ne peut pas produire des jetons, ils peuvent être utilisés pour acheminer ou rediriger les jetons aux clients et, lorsque cela est nécessaire, sur le serveur de fédération. Pour plus d’informations sur l’utilisation de serveurs de fédération, consultez [quand créer un serveur Proxy de fédération](When-to-Create-a-Federation-Server-Proxy.md).  
  
-   Serveurs de fédération prennent en charge l’utilisation de l’authentification intégrée Windows pour les clients sur le réseau d’entreprise ; serveurs proxy de fédération ne peuvent pas. Pour plus d’informations sur l’utilisation de l’authentification intégrée de Windows avec le serveur de fédération, consultez [quand créer une batterie de serveurs de fédération](When-to-Create-a-Federation-Server-Farm.md).  
  
> [!CAUTION]  
> L’intégrité et la confidentialité de la communication entre les serveurs de fédération et les bases de données de configuration SQL Server, les magasins d’attributs SQL Server, les contrôleurs de domaine et les instances AD LDS ne sont pas protégées par défaut. Pour atténuer le risque, envisagez de protéger le canal de communication entre ces serveurs à l’aide d’IPsec ou en utilisant une connexion sécurisée physiquement entre tous ces serveurs. Pour la communication entre les serveurs de fédération et SQL Server, envisagez d’utiliser la protection SSL dans la chaîne de connexion. Pour les connexions entre les serveurs de fédération et les contrôleurs de domaine, envisagez d’activer la signature et le chiffrement Kerberos. Pour le protocole LDAP, LDAP\/S n’est pas pris en charge pour AD LDS\/AD DS.  
  
## <a name="how-to-create-a-federation-server"></a>Comment créer un serveur de fédération  
Vous pouvez créer un serveur de fédération à l’aide de l’Assistant Configuration du serveur de fédération AD FS ou de la commande Fsconfig.exe\-outil en ligne. Quand vous utilisez l’un de ces outils, vous pouvez choisir l’une des options suivantes pour créer un serveur de fédération.  
  
-   Créer un support\-serveur de fédération autonome  
  
    Pour plus d’informations sur la façon de configurer un support\-serveur de fédération autonome, consultez [Create a Stand-Alone Federation Server](../../ad-fs/deployment/Create-a-Stand-Alone-Federation-Server.md).  
  
-   Créer le premier serveur de fédération dans une batterie de serveurs de fédération  
  
    Pour plus d’informations sur la façon de configurer le premier serveur de fédération ou ajouter un serveur de fédération à une batterie de serveurs, consultez [créer le premier serveur de fédération dans une batterie de serveurs de fédération](../../ad-fs/deployment/Create-the-First-Federation-Server-in-a-Federation-Server-Farm.md).  
  
-   Ajouter un serveur de fédération à une batterie de serveurs de fédération  
  
    Pour plus d’informations sur l’ajout d’un serveur de fédération à une batterie de serveurs, consultez [ajouter un serveur de fédération à une batterie de serveurs de fédération](../../ad-fs/deployment/Add-a-Federation-Server-to-a-Federation-Server-Farm.md).  
  
Pour plus d’informations sur le fonctionnement de chacune de ces options, consultez [le rôle de la base de données de Configuration AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md).  
  
Pour plus d’informations sur la configuration de toutes les conditions préalables nécessaires au déploiement d’un serveur de fédération, consultez [liste de vérification : Configuration d’un serveur de fédération](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md).  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

