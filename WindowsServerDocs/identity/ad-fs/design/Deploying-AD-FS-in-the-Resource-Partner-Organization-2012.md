---
ms.assetid: 39acccd9-0402-49ca-8ce1-b239e1e7e455
title: "Déploiement d’ADFS dans l’organisation partenaire de ressource"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: a20fab1cca4c33485fd599de5525c7a718e9598e
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="deploying-ad-fs-in-the-resource-partner-organization"></a>Déploiement d’ADFS dans l’organisation partenaire de ressource

>S’applique à: Windows Server2012

L’organisation partenaire de ressource dans ActiveDirectory Federation Services \(ADFS\) représente l’organisation dont les serveurs Web peuvent être protégés par un serveur de fédération resource\ côté. Le serveur de fédération du partenaire de ressource utilise les jetons de sécurité produits par le partenaire de compte pour fournir des revendications pour les serveurs Web qui sont trouvent dans le partenaire de ressource.  
  
Dans les scénarios dans lesquels vous devez fournir l’accès aux services fédérés ou applications à de nombreux utilisateurs différents, lorsque des utilisateurs se trouvent dans des organisations différentes, vous pouvez configurer le serveur de fédération de ressources afin que vous pouvez déployer plusieurs partenaires de compte.  
  
Pour plus d’informations sur comment installer et configurer une organisation partenaire de ressource, consultez [liste de vérification: configuration de l’organisation partenaire de ressource](../../ad-fs/deployment/Checklist--Configuring-the-Resource-Partner-Organization.md).  
  
## <a name="in-this-section"></a>Dans cette section  
  
-   [Passez en revue le rôle du serveur de fédération du partenaire de ressource](Review-the-Role-of-the-Federation-Server-in-the-Resource-Partner.md)  
  
-   [Passez en revue le rôle de serveur Proxy de fédération du partenaire de ressource](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Resource-Partner.md)  
  
-   [Déterminer votre stratégie d’Application fédérée dans le partenaire de ressource](Determine-Your-Federated-Application-Strategy-in-the-Resource-Partner.md)  
  

## <a name="see-also"></a>Voir aussi
[Guide de conception ADFS dans Windows Server2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
