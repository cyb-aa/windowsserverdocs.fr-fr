---
ms.assetid: 4ddb927d-d65e-491d-840a-16049c083d13
title: Rôle des magasins d'attributs
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 730411ed7efbb9cf0db3d7e94a486cec4c363849
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860410"
---
 >S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="the-role-of-attribute-stores"></a>Rôle des magasins d'attributs
Active Directory Federation Services utilise le terme « magasins d’attributs » pour faire référence aux répertoires ou bases de données utilisés par une organisation pour stocker ses comptes d’utilisateur et leurs valeurs d’attribut associées. Une fois qu’il est configuré dans une organisation de fournisseur d’identité, AD FS récupère ces valeurs d’attribut à partir du magasin et crée des revendications basées sur ces informations afin qu’une application Web ou le service est hébergé dans une organisation partie de confiance puisse faire approprié les décisions d’autorisation chaque fois qu’un utilisateur fédéré \(un utilisateur dont le compte est stocké dans l’organisation du fournisseur identité\) tente d’accéder à l’application ou le service.  
  
Pour plus d’informations sur la façon dont les revendications sont générées, consultez [The Role of Claims](The-Role-of-Claims.md).  
  
## <a name="how-attribute-stores-fit-in-with-your-ad-fs-deployment-goals"></a>En quoi les magasins d’attributs s’inscrivent-ils dans vos objectifs de déploiement d’AD FS ?  
L’emplacement du magasin d’attributs utilisateur et l’emplacement à partir duquel les utilisateurs s’authentifient déterminent la façon dont vous concevez AD FS pour prendre en charge les identités des utilisateurs. Selon où se trouve le magasin d’attributs et où les utilisateurs accéderont à l’application \(dans un intranet ou sur Internet\), vous pouvez utiliser un des objectifs de déploiement suivants :  
  
-   [Fournir votre Active Directory Users Access Your Claims-Aware Applications et Services](https://technet.microsoft.com/library/dd807071.aspx)— dans cet objectif, les utilisateurs de votre organisation accéder à AD FS sécurisée application ou service \(votre propre application ou un service ou un application ou le service du partenaire\) lorsque les utilisateurs sont connectés à Active Directory dans l’intranet d’entreprise.  
  
-   [Fournir votre Active Directory Users Access pour les Applications et les Services d’autres organisations](https://technet.microsoft.com/library/dd807123.aspx)— dans cet objectif, les utilisateurs de votre organisation accéder à AD FS sécurisée application ou service \(votre propre application ou un service ou un application ou le service du partenaire\) lorsque les utilisateurs sont connectés à un magasin d’attributs dans l’intranet d’entreprise et lorsqu’ils se connectent à distance à partir d’Internet.  
  
-   [Fournir aux utilisateurs d’une autre organisation un accès aux Services et Applications prenant en charge les revendications votre](https://technet.microsoft.com/library/dd807099.aspx)— dans cet objectif, les comptes d’utilisateur dans une autre organisation qui sont trouvent dans un magasin d’attributs sur l’intranet d’entreprise de cette organisation doivent accéder à un AD FS : sécurisation de l’application dans votre organisation. Cet objectif fonctionne également lorsque consommateur\-des comptes d’utilisateurs en qui sont trouvent dans un magasin d’attributs dans le réseau de périmètre de votre organisation doivent être fournis avec un accès à un AD FS sécurisée des applications de votre organisation.  
  
Selon l’emplacement du magasin d’attributs et les autres exigences de votre organisation, vous pouvez combiner plusieurs de ces objectifs de déploiement pour réaliser la conception de votre déploiement AD FS.  
  
## <a name="attribute-stores-that-are-supported-by-ad-fs"></a>Magasins d’attributs pris en charge par AD FS  
AD FS prend en charge un large éventail de répertoire et stocke de base de données que vous pouvez utiliser pour l’extraction administrateur\-défini les valeurs d’attribut et renseigner les revendications avec ces valeurs. AD FS prend en charge tous les répertoires suivants ni les bases de données en tant que magasins d’attributs :  
  
-   Active Directory dans Windows Server 2003, Active Directory Domain Services \(AD DS\) dans Windows Server 2008, AD DS dans Windows Server 2012 et 2012 R2 et Windows Server 2016. 
  
-   Toutes les éditions de Microsoft SQL Server 2005, SQL Server 2008, SQL Server 2012, SQL Server 2014 et SQL Server 2016  
  
-   Magasins d'attributs personnalisés  
  

