---
ms.assetid: aa20c8b3-7f01-4165-8b73-92642bff9676
title: "Création d’un serveur Proxy de fédération"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 1f0253dfb5a690371dae1a2bfcb6b7520077d473
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="when-to-create-a-federation-server-proxy"></a>Création d’un serveur Proxy de fédération

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Création d’un serveur proxy de fédération dans votre organisation ajoute des couches de sécurité supplémentaires à votre déploiement de \(AD FS\) Active Directory Federation Services. Envisagez de déployer un serveur proxy de fédération dans le réseau de périmètre de votre organisation lorsque vous souhaitez:  
  
-   Empêcher les ordinateurs clients externes d’accéder directement à vos serveurs de fédération. En déployant un serveur proxy de fédération dans votre réseau de périmètre, vous isolez efficacement vos serveurs de fédération afin qu’ils sont accessibles uniquement par les ordinateurs clients connectés au réseau d’entreprise par le biais de serveurs proxy de fédération, qui agissent au nom des ordinateurs clients externes. Serveurs proxy de fédération n’ont pas accès aux clés privées utilisées pour produire des jetons. Pour plus d’informations, voir [où placer un serveur Proxy de fédération](Where-to-Place-a-Federation-Server-Proxy.md).  
  
-   Fournir un moyen pratique pour différencier l’expérience de connexion pour les utilisateurs qui seront bientôt disponibles à partir d’Internet par opposition aux utilisateurs qui seront bientôt disponibles à partir de votre réseau d’entreprise à l’aide de l’authentification Windows intégrée. Un serveur proxy de fédération collecte les informations d’identification ou d’accueil de domaine à partir d’ordinateurs clients Internet à l’aide de l’ouverture de session, de déconnexion et identité fournisseur découverte \(homerealmdiscovery.aspx\) les pages qui sont stockés sur le serveur proxy de fédération.  
  
    En revanche, les ordinateurs clients qui proviennent rencontrés par le réseau d’entreprise une expérience différente, basée sur la configuration du serveur de fédération. Le serveur de fédération du réseau d’entreprise est souvent configuré pour l’authentification intégrée Windows, qui fournit une expérience de connexion transparente pour les utilisateurs sur le réseau d’entreprise.  
  
Le rôle joué par un serveur proxy de fédération dans votre organisation varie selon que vous placez le serveur proxy de fédération dans l’organisation partenaire de compte ou dans l’organisation partenaire de ressource. Par exemple, lorsqu’un serveur proxy de fédération est placé dans le réseau de périmètre du partenaire de compte, son rôle consiste à collecter les informations d’identification utilisateur à partir de clients de navigateur. Lorsqu’un serveur proxy de fédération est placé dans le réseau de périmètre du partenaire de ressource, il relaie les demandes de jeton de sécurité à un serveur de fédération de ressources et génère des jetons de sécurité de l’organisation en réponse aux jetons de sécurité qui sont fournis par ses partenaires de compte.  
  
Pour plus d’informations, voir [revue du rôle de serveur Proxy de fédération du partenaire de compte](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Account-Partner.md) et [revue du rôle de serveur Proxy de fédération du partenaire de ressource](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Resource-Partner.md)  
  
## <a name="how-to-create-a-federation-server-proxy"></a>Procédure de création d’un serveur proxy de fédération  
Vous pouvez créer un serveur proxy de fédération à l’aide de l’Assistant Configuration Proxy des serveurs de fédération AD FS ou l’outil de ligne de commande Fsconfig.exe. Pour savoir comment procéder, voir [configurer un ordinateur pour le rôle de Proxy de serveur de fédération](../../ad-fs/deployment/Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md).  
  
Pour obtenir des informations générales sur la façon de configurer les conditions préalables nécessaires au déploiement d’un serveur proxy de fédération, consultez [liste de vérification: configuration d’un serveur Proxy de fédération](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server-Proxy.md).  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception ADFS dans Windows Server2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
