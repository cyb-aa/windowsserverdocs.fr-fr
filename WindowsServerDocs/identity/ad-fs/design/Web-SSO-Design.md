---
ms.assetid: eb778f63-f7be-438e-8c5e-1fd9b194b967
title: Conception SSO Web
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 47c946ac617cc64c224c1bc3153fcaf55c2d069c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858512"
---
# <a name="web-sso-design"></a>Conception SSO Web

Dans le\-de la connexion Web unique\-sur \(conception de l’authentification unique\) dans Services ADFS \(AD FS\), les utilisateurs doivent s’authentifier une seule fois pour accéder à plusieurs AD FS\-applications ou services sécurisés. Dans cette conception, tous les utilisateurs sont externes et en l’absence d’organisation partenaire, il n’existe aucune approbation de fédération. En règle générale, vous déployez cette conception lorsque vous souhaitez fournir un accès client ou client individuel à un ou plusieurs services ou applications sécurisés par AD FS via Internet, comme indiqué dans l’illustration suivante.  
  
![conception SSO de Web](media/adfs2_WebSSODesign.gif)  
  
Avec la conception SSO de Web, une organisation qui héberge généralement un AD FS\-application ou un service sécurisé dans un réseau de périmètre peut maintenir un magasin distinct de comptes clients dans le réseau de périmètre, ce qui permet d’isoler plus facilement les comptes clients des comptes des employés.  
  
Vous pouvez gérer les comptes locaux pour les clients du réseau de périmètre à l’aide de Active Directory Domain Services \(AD DS\), SQL Server ou d’un magasin d’attributs personnalisé.  
  
Cette conception correspond à l’objectif de déploiement de [Provide Your Active Directory Users Access to Your Claims-Aware Applications and Services](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md).  
  
Pour obtenir une liste détaillée des tâches que vous pouvez utiliser pour planifier et déployer la conception de SSO de web, voir [Checklist: Implementing a Web SSO Design](../../ad-fs/deployment/Checklist--Implementing-a-Web-SSO-Design.md).  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
