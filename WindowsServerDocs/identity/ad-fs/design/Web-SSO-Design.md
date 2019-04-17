---
ms.assetid: eb778f63-f7be-438e-8c5e-1fd9b194b967
title: Conception SSO de Web
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 1b8344594c9fc477ed8424c716ec8d7f7fd91ef3
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="web-sso-design"></a>Conception SSO de Web

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Dans la conception de connexion de Single\ sur \(SSO\) dans Active Directory Federation Services \(AD FS\) Web, les utilisateurs doivent s’authentifier une seule fois pour accéder à plusieurs applications sécurisées par AD FS\ ou services. Dans cette conception, tous les utilisateurs sont externes, et étant donné qu’aucun organisations partenaires n’existe aucune approbation de fédération. En règle générale, vous déployez cette conception lorsque vous souhaitez fournir un accès consommateur ou des clients individuel à un ou plusieurs AD FS sécurisé services ou applications sur Internet, comme illustré dans l’illustration suivante.  
  
![conception sso de Web](media/adfs2_WebSSODesign.gif)  
  
Avec l’authentification unique Web de conception, une organisation qui héberge généralement une application sécurisée par AD FS\ ou service dans un réseau de périmètre peut maintenir un magasin distinct de comptes clients dans le réseau de périmètre, ce qui facilite les comptes clients des comptes des employés.  
  
Vous pouvez gérer les comptes locaux pour les clients du réseau de périmètre à l’aide des Services de domaine Active Directory \(AD DS\), SQL Server, soit un magasin d’attributs personnalisés.  
  
Cette conception coïncide avec l’objectif de déploiement dans [fournir Your Active Directory Users Access to Your Claims-Aware Applications and Services](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md).  
  
Pour obtenir une liste détaillée des tâches que vous pouvez utiliser pour planifier et déployer votre conception SSO de Web, voir [liste de vérification: implémentation d’une conception SSO de Web](../../ad-fs/deployment/Checklist--Implementing-a-Web-SSO-Design.md).  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception ADFS dans Windows Server2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
