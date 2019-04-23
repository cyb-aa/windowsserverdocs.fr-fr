---
ms.assetid: 41d6b897-1e72-4522-aad6-eece1154a154
title: Déploiement des services de fédération Active Directory (AD FS) dans l’organisation partenaire de ressource
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4a556c07e7d6e0bec4c947ea9d1a75eef9964cef
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877600"
---
# <a name="deploying-ad-fs-in-the-resource-partner-organization"></a>Déploiement des services de fédération Active Directory (AD FS) dans l’organisation partenaire de ressource

>S'applique à : Windows Server 2016, Windows Server 2012 R2

L’organisation du partenaire de ressource dans Active Directory Federation Services \(AD FS\) représente l’organisation dont les serveurs Web peuvent être protégés par une ressource\-serveur de fédération de côté. Le serveur de fédération du partenaire de ressource utilise les jetons de sécurité qui sont produites par le partenaire de compte pour fournir des revendications aux serveurs Web qui sont trouvent dans le partenaire de ressource.  
  
Dans les scénarios dans lesquels vous devez fournir l’accès aux services fédérés ou des applications à plusieurs utilisateurs différents, lorsque des utilisateurs se trouvent dans différentes organisations, vous pouvez configurer le serveur de fédération de ressources afin que vous puissiez déployer plusieurs partenaires de compte.  
  
Pour plus d’informations sur comment installer et configurer une organisation partenaire de ressource, consultez [liste de vérification : Configuration de l’organisation partenaire de ressource](../../ad-fs/deployment/Checklist--Configuring-the-Resource-Partner-Organization.md).  
  
## <a name="in-this-section"></a>Dans cette section  
  
-   [Passez en revue le rôle du serveur de fédération du partenaire de ressource](Review-the-Role-of-the-Federation-Server-in-the-Resource-Partner.md)  
  
-   [Passez en revue le rôle de serveur Proxy de fédération du partenaire de ressource](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Resource-Partner.md)  
  
-   [Déterminez votre stratégie d’Application fédérée dans le partenaire de ressource](Determine-Your-Federated-Application-Strategy-in-the-Resource-Partner.md)  
  

## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
