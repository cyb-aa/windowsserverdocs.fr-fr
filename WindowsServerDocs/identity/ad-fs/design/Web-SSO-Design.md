---
ms.assetid: eb778f63-f7be-438e-8c5e-1fd9b194b967
title: Conception SSO Web
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: d7f52cd36588a1e5de4536a760c38c147dd1e003
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407834"
---
# <a name="web-sso-design"></a>Conception SSO Web

Dans la conception Web unique @ no__t-0Sign @ no__t-1On \(SSO @ no__t-3 dans Services ADFS \(AD FS @ no__t-5, les utilisateurs doivent s’authentifier une seule fois pour accéder à plusieurs applications ou services AD FS @ no__t-6secured. Dans cette conception, tous les utilisateurs sont externes et en l’absence d’organisation partenaire, il n’existe aucune approbation de fédération. En règle générale, vous déployez cette conception lorsque vous souhaitez fournir un accès client ou client individuel à un ou plusieurs services ou applications sécurisés par AD FS via Internet, comme indiqué dans l’illustration suivante.  
  
![conception SSO de Web](media/adfs2_WebSSODesign.gif)  
  
Avec la conception SSO de Web, une organisation qui héberge généralement une application ou un service AD FS @ no__t-0secured dans un réseau de périmètre peut conserver un magasin distinct de comptes clients dans le réseau de périmètre, ce qui facilite l’isolement des comptes clients à partir de comptes d’employés.  
  
Vous pouvez gérer les comptes locaux pour les clients du réseau de périmètre à l’aide de Active Directory Domain Services \(AD DS @ no__t-1, SQL Server ou un magasin d’attributs personnalisé.  
  
Cette conception correspond à l’objectif de déploiement de [Provide Your Active Directory Users Access to Your Claims-Aware Applications and Services](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md).  
  
Pour obtenir la liste des tâches détaillées que vous pouvez utiliser pour planifier et déployer votre conception SSO de Web, voir [Checklist : Implémentation d’une conception SSO de Web @ no__t-0.  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
