---
ms.assetid: aa20c8b3-7f01-4165-8b73-92642bff9676
title: Création d’un serveur proxy de fédération
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: f3325efe7acf8b0b0469489e8d9a42614a5af54a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358848"
---
# <a name="when-to-create-a-federation-server-proxy"></a>Création d’un serveur proxy de fédération

La création d’un serveur proxy de Fédération dans votre organisation ajoute des couches de sécurité supplémentaires à votre déploiement Services ADFS \(AD FS @ no__t-1. Envisagez de déployer un serveur proxy de Fédération dans le réseau de périmètre de votre organisation lorsque vous souhaitez :  
  
-   Empêcher les ordinateurs clients externes d’accéder directement à vos serveurs de Fédération. En déployant un serveur proxy de Fédération dans votre réseau de périmètre, vous isolez efficacement vos serveurs de Fédération afin qu’ils soient accessibles uniquement par les ordinateurs clients qui sont connectés au réseau d’entreprise par le biais de serveurs proxys de Fédération, qui agissent pour le compte des ordinateurs clients externes. Les proxies de serveurs de fédération n’ont pas accès aux clés privées utilisées pour produire des jetons. Pour plus d'informations, voir [Where to Place a Federation Server Proxy](Where-to-Place-a-Federation-Server-Proxy.md).  
  
-   Fournir un moyen pratique pour différencier l’expérience @ no__t-0in pour les utilisateurs provenant d’Internet par opposition aux utilisateurs provenant de votre réseau d’entreprise à l’aide de l’authentification intégrée de Windows. Un serveur proxy de fédération collecte des informations d’identification ou des informations de domaine d’hébergement à partir d’ordinateurs clients Internet à l’aide des pages de connexion, de déconnexion et de découverte de fournisseur d’identité @no__t -0homerealmdiscovery. aspx @ no__t-1 pages stockées sur le serveur proxy de Fédération.  
  
    En revanche, les ordinateurs clients qui proviennent du réseau d’entreprise rencontrent une expérience différente, en fonction de la configuration du serveur de Fédération. Le serveur de Fédération du réseau d’entreprise est souvent configuré pour l’authentification intégrée de Windows, qui offre une expérience de connexion transparente à @ no__t-0in pour les utilisateurs du réseau d’entreprise.  
  
Le rôle joué par un proxy de serveur de Fédération dans votre organisation varie selon que vous placez le serveur proxy de Fédération dans l’organisation partenaire de compte ou dans l’organisation partenaire de ressource. Par exemple, lorsqu’un serveur proxy de Fédération est placé dans le réseau de périmètre du partenaire de compte, son rôle consiste à collecter les informations d’identification de l’utilisateur auprès des clients du navigateur. Lorsqu’un serveur proxy de Fédération est placé dans le réseau de périmètre du partenaire de ressource, il relaie les demandes de jeton de sécurité vers un serveur de Fédération de ressources et produit des jetons de sécurité organisationnels en réponse aux jetons de sécurité fournis par son partenaires de compte.  
  
Pour plus d’informations, consultez [revue du rôle de serveur Proxy de fédération du partenaire de compte](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Account-Partner.md) et [revue du rôle de serveur Proxy de fédération du partenaire de ressource](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Resource-Partner.md)  
  
## <a name="how-to-create-a-federation-server-proxy"></a>Comment créer un serveur proxy de fédération ?  
Vous pouvez créer un serveur proxy de Fédération à l’aide de l’Assistant Configuration du serveur proxy de fédération AD FS ou de l’outil @ no__t-0line de la commande fsconfig. exe. Vous pouvez créer un serveur proxy de fédération à l’aide de l’Assistant Configuration Proxy des serveur de fédération AD FS ou de la commande Fsconfig.exe[outil en ligne.  
  
Pour obtenir des informations générales sur la façon de configurer toutes les conditions préalables nécessaires au déploiement d’un serveur proxy de Fédération, voir [Checklist : Configuration d’un serveur proxy de Fédération @ no__t-0.  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
