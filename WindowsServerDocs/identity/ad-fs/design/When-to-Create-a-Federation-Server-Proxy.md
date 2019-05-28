---
ms.assetid: aa20c8b3-7f01-4165-8b73-92642bff9676
title: Création d’un serveur proxy de fédération
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b41c2194940c85e39e5a3724f747dd12c2544259
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190636"
---
# <a name="when-to-create-a-federation-server-proxy"></a>Création d’un serveur proxy de fédération

Création d’un serveur proxy de fédération dans votre organisation ajoute des couches de sécurité supplémentaires à votre Active Directory Federation Services \(AD FS\) déploiement. Envisagez de déployer un serveur proxy de fédération dans le réseau de périmètre de votre organisation lorsque vous souhaitez :  
  
-   Empêcher les ordinateurs clients externes d’accéder directement à vos serveurs de fédération. En déployant un serveur proxy de fédération dans votre réseau de périmètre, vous isolez efficacement vos serveurs de fédération afin qu’ils soient accessibles uniquement par les ordinateurs clients qui sont connectés au réseau d’entreprise via des serveurs proxy de fédération, qui agissent en nom des ordinateurs clients externes. Les proxies de serveurs de fédération n’ont pas accès aux clés privées utilisées pour produire des jetons. Pour plus d'informations, voir [Where to Place a Federation Server Proxy](Where-to-Place-a-Federation-Server-Proxy.md).  
  
-   Fournir un moyen pratique pour différencier le signe\-dans l’expérience des utilisateurs qui viennent d’Internet par opposition aux utilisateurs qui proviennent de votre réseau d’entreprise à l’aide de l’authentification intégrée Windows. Un serveur proxy de fédération collecte les informations d’identification ou les détails du domaine d’accueil à partir d’ordinateurs clients Internet à l’aide de la découverte de fournisseur d’ouverture de session, déconnexion et l’identité \(homerealmdiscovery.aspx\) pages qui sont stockées sur la fédération serveur proxy.  
  
    En revanche, les ordinateurs clients qui proviennent de la rencontre de réseau d’entreprise une expérience différente, basée sur la configuration du serveur de fédération. Le serveur de fédération du réseau d’entreprise est souvent configuré pour une authentification intégrée Windows, qui fournit un authentification transparente\-dans l’expérience des utilisateurs sur le réseau d’entreprise.  
  
Le rôle joué par un serveur proxy de fédération dans votre organisation varie selon que vous placez le serveur proxy de fédération dans l’organisation partenaire de compte ou de l’organisation partenaire de ressource. Par exemple, lorsqu’un serveur proxy de fédération est placé dans le réseau de périmètre du partenaire de compte, son rôle consiste à collecter les informations d’identification utilisateur à partir de clients de navigateur. Lorsqu’un serveur proxy de fédération est placé dans le réseau de périmètre du partenaire de ressource, il relaie les demandes à un serveur de fédération de ressources de jeton de sécurité et génère des jetons de sécurité de l’organisation en réponse aux jetons de sécurité qui sont fournies par son partenaires de compte.  
  
Pour plus d’informations, voir [Review the Role of the Federation Server Proxy in the Account Partner](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Account-Partner.md) et [Review the Role of the Federation Server Proxy in the Resource Partner](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Resource-Partner.md).  
  
## <a name="how-to-create-a-federation-server-proxy"></a>Comment créer un serveur proxy de fédération ?  
Vous pouvez créer un serveur proxy de fédération à l’aide de l’Assistant Configuration Proxy des serveur de fédération AD FS ou de la commande Fsconfig.exe\-outil en ligne. Pour savoir comment procéder, voir [Configure a Computer for the Federation Server Proxy Role](../../ad-fs/deployment/Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md).  
  
Pour obtenir des informations générales sur la configuration de toutes les conditions préalables nécessaires au déploiement d’un serveur proxy de fédération, consultez [liste de vérification : Configuration d’un serveur Proxy de fédération](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server-Proxy.md).  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
