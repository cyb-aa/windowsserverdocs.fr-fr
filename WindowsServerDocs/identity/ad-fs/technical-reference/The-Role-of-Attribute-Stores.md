---
ms.assetid: 4ddb927d-d65e-491d-840a-16049c083d13
title: Rôle des magasins d'attributs
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 0a1543f2c935c2ef76ea014567b18bfc778c7401
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407386"
---
# <a name="the-role-of-attribute-stores"></a>Rôle des magasins d'attributs
Services ADFS utilise le terme « magasins d’attributs » pour faire référence aux répertoires ou aux bases de données utilisés par une organisation pour stocker ses comptes d’utilisateur et leurs valeurs d’attribut associées. Une fois qu’elle est configurée dans une organisation de fournisseur d’identité, AD FS récupère ces valeurs d’attribut à partir du magasin et crée des revendications en fonction de ces informations afin qu’une application ou un service Web hébergé dans une organisation de partie de confiance puisse faire le décisions d’autorisation lorsqu’un utilisateur \(fédéré dont le compte est stocké dans l’organisation\) du fournisseur d’identité tente d’accéder à l’application ou au service.  
  
Pour plus d’informations sur la façon dont les revendications sont générées, consultez [The Role of Claims](The-Role-of-Claims.md).  
  
## <a name="how-attribute-stores-fit-in-with-your-ad-fs-deployment-goals"></a>En quoi les magasins d’attributs s’inscrivent-ils dans vos objectifs de déploiement d’AD FS ?  
L’emplacement du magasin d’attributs utilisateur et l’emplacement à partir duquel les utilisateurs s’authentifient déterminent la façon dont vous concevez AD FS pour prendre en charge les identités des utilisateurs. En fonction de l’emplacement du magasin d’attributs et de l’emplacement où les utilisateurs \(accèdent à l’application dans\)un intranet ou sur Internet, vous pouvez utiliser l’un des objectifs de déploiement suivants :  
  
-   [Fournir à vos utilisateurs Active Directory un accès à vos applications et services prenant en charge les revendications](https://technet.microsoft.com/library/dd807071.aspx): dans cet objectif, les utilisateurs de votre organisation accèdent \(à une application ou à un service sécurisé AD FS, que ce soit votre propre application ou service ou un partenaire application ou service\) lorsque les utilisateurs sont connectés à Active Directory dans l’intranet d’entreprise.  
  
-   [Fournir à vos utilisateurs Active Directory un accès aux applications et services d’autres organisations](https://technet.microsoft.com/library/dd807123.aspx): dans cet objectif, les utilisateurs de votre organisation accèdent à une application \(ou à un service sécurisé AD FS, que ce soit votre propre application ou service ou un application ou service\) du partenaire lorsque les utilisateurs sont connectés à un magasin d’attributs dans l’intranet d’entreprise et lorsqu’ils ouvrent une session à distance à partir d’Internet.  
  
-   [Fournir aux utilisateurs d’une autre organisation un accès à vos applications et services prenant en charge les revendications](https://technet.microsoft.com/library/dd807099.aspx): dans cet objectif, les comptes d’utilisateurs d’une autre organisation qui se trouvent dans un magasin d’attributs sur l’intranet d’entreprise de cette organisation doivent accéder à une AD FS – application sécurisée dans votre organisation. Cet objectif fonctionne également lorsque des\-comptes d’utilisateurs basés sur des consommateurs situés dans un magasin d’attributs du réseau de périmètre de votre organisation doivent disposer d’un accès à une application sécurisée par AD FS dans votre organisation.  
  
Selon le placement des magasins d’attributs et les autres exigences de votre organisation, vous pouvez combiner plusieurs de ces objectifs de déploiement pour terminer la conception de votre déploiement AD FS.  
  
## <a name="attribute-stores-that-are-supported-by-ad-fs"></a>Magasins d’attributs pris en charge par AD FS  
AD FS prend en charge un large éventail de magasins d’annuaires et de bases de données que\-vous pouvez utiliser pour extraire les valeurs d’attribut définies par l’administrateur et remplir les revendications avec ces valeurs. AD FS prend en charge les répertoires ou bases de données suivants en tant que magasins d’attributs :  
  
-   Active Directory dans Windows Server 2003, Active Directory Domain Services \(AD DS\) dans Windows Server 2008, AD DS dans Windows Server 2012 et 2012 R2 et Windows Server 2016. 
  
-   Toutes les éditions de Microsoft SQL Server 2005, SQL Server 2008, SQL Server 2012, SQL Server 2014 et SQL Server 2016  
  
-   Magasins d'attributs personnalisés  
  

