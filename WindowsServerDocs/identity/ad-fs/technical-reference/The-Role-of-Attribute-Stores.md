---
ms.assetid: 4ddb927d-d65e-491d-840a-16049c083d13
title: "Le rôle des magasins d’attributs"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 730411ed7efbb9cf0db3d7e94a486cec4c363849
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
 >S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

# <a name="the-role-of-attribute-stores"></a>Le rôle des magasins d’attributs
ActiveDirectory Federation Services utilise le terme «magasins d’attributs» pour faire référence aux répertoires ou bases de données une organisation utilise pour stocker ses comptes d’utilisateur et leurs valeurs d’attribut associées. Une fois qu’il est configuré dans une organisation de fournisseur d’identité, ADFS récupère ces valeurs d’attribut dans le magasin et crée des revendications basées sur ces informations afin qu’une application Web ou un service est hébergé dans une organisation de partie de confiance peut les décisions d’autorisation appropriées chaque fois qu’un utilisateur fédéré \ (un utilisateur dont le compte est stocké dans l’organization\ de fournisseur d’identité) tente d’accéder à l’application ou service.  
  
Pour plus d’informations sur la façon dont les revendications sont générées, consultez [The Role of Claims](The-Role-of-Claims.md).  
  
## <a name="how-attribute-stores-fit-in-with-your-ad-fs-deployment-goals"></a>Magasins d’attributs ajuster avec vos objectifs de déploiement ADFS  
L’emplacement du magasin d’attributs de l’utilisateur et l’emplacement à partir duquel les utilisateurs s’authentifient déterminent le mode de conception ADFS pour prendre en charge les identités des utilisateurs. En fonction d’où se trouve le magasin d’attributs et où les utilisateurs accès à l’application \ (dans un intranet ou sur l’Internet), vous pouvez utiliser un des objectifs de déploiement suivants:  
  
-   [Fournir votre ActiveDirectory Users Access à Your Claims-Aware Applications and Services](https://technet.microsoft.com/library/dd807071.aspx)— dans cet objectif, les utilisateurs de votre organisation accèdent à une application sécurisée par ADFS d’ActiveDirectory ou un service \ (votre propre application ou service ou d’un partenaire application ou service\) lorsque les utilisateurs sont connectés à ActiveDirectory dans l’intranet d’entreprise.  
  
-   [Fournir votre ActiveDirectory Users Access pour les Applications et les Services d’autres organisations](https://technet.microsoft.com/library/dd807123.aspx)— dans cet objectif, les utilisateurs de votre organisation accèdent à une application sécurisée par ADFS d’ActiveDirectory ou un service \ (votre propre application ou service ou d’un partenaire application ou service\) lorsque les utilisateurs sont connectés à un magasin d’attributs dans l’intranet d’entreprise et lorsqu’ils se connectent à distance à partir d’Internet.  
  
-   [Fournir aux utilisateurs d’une autre organisation un accès à Your Claims-Aware Applications and Services](https://technet.microsoft.com/library/dd807099.aspx)— dans cet objectif, les comptes d’utilisateur dans une autre organisation qui sont trouvent dans un magasin d’attributs sur l’intranet d’entreprise de cette organisation doivent accéder à une application sécurisée par ADFS dans votre organisation d’AD. Cet objectif fonctionne également lorsque des comptes d’utilisateurs basés sur consumer\ qui sont trouvent dans un magasin d’attributs dans le réseau de périmètre de votre organisation doivent être fournis avec accès à une application sécurisée par ADFS dans votre organisation ActiveDirectory.  
  
Selon l’emplacement du magasin d’attributs et d’autres exigences de votre organisation, vous pouvez combiner plusieurs de ces objectifs de déploiement pour réaliser la conception de votre déploiement ADFS.  
  
## <a name="attribute-stores-that-are-supported-by-ad-fs"></a>Pris en charge par ADFS magasins d’attributs  
ADFS prend en charge un large éventail de répertoire et la base de données stocke que vous pouvez utiliser pour extraire les valeurs des attributs définis administrator\ et renseigner les revendications avec ces valeurs. ADFS prend en charge les bases de données ou les répertoires suivants en tant que magasins d’attributs:  
  
-   ActiveDirectory dans Windows Server2003, des Services de domaine ActiveDirectory \(ADDS\) dans Windows Server2008, ADDS dans Windows Server2012 et 2012R2 et Windows Server2016. 
  
-   Toutes les éditions de MicrosoftSQLServer2005, SQLServer2008, SQLServer2012, SQLServer2014 et SQLServer2016  
  
-   Magasins d’attributs personnalisés  
  

