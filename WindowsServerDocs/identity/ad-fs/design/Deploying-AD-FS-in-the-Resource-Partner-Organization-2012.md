---
ms.assetid: 39acccd9-0402-49ca-8ce1-b239e1e7e455
title: Déploiement des services de fédération Active Directory (AD FS) dans l’organisation partenaire de ressource
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: f4741fcc683a8a22318caa47d5cbd66352862a86
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408133"
---
# <a name="deploying-ad-fs-in-the-resource-partner-organization"></a>Déploiement des services de fédération Active Directory (AD FS) dans l’organisation partenaire de ressource

L’organisation partenaire de ressource dans Services ADFS \(AD FS\) représente l’organisation dont les serveurs Web peuvent être protégés par un serveur de Fédération côté\-des ressources. Le serveur de Fédération du partenaire de ressource utilise les jetons de sécurité produits par le partenaire de compte pour fournir des revendications aux serveurs Web qui se trouvent dans le partenaire de ressource.  
  
Dans les scénarios où vous devez fournir un accès à des services ou des applications fédérés à de nombreux utilisateurs différents, lorsque certains utilisateurs résident dans des organisations différentes, vous pouvez configurer le serveur de Fédération de ressources afin de pouvoir déployer plusieurs partenaires de compte.  
  
Pour plus d’informations sur l’installation et la configuration d’une organisation partenaire de ressource, voir [Checklist: Configuring the Resource Partner Organization](../../ad-fs/deployment/Checklist--Configuring-the-Resource-Partner-Organization.md).  
  
## <a name="in-this-section"></a>Dans cette section  
  
-   [Passer en revue le rôle du serveur de fédération du partenaire de ressource](Review-the-Role-of-the-Federation-Server-in-the-Resource-Partner.md)  
  
-   [Passer en revue le rôle du serveur proxy de fédération du partenaire de ressource](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Resource-Partner.md)  
  
-   [Déterminer votre stratégie d’application fédérée dans le partenaire de ressource](Determine-Your-Federated-Application-Strategy-in-the-Resource-Partner.md)  
  

## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
