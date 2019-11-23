---
ms.assetid: 204f5fe9-3611-4da0-b057-a386004b4598
title: Comprendre les concepts clés de Services ADFS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: d212c2f820204bae8e1e55dc1ebc44200e266a18
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407316"
---
# <a name="understanding-key-ad-fs-concepts"></a>Présentation des concepts AD FS clés
Il est recommandé d’en savoir plus sur les concepts importants pour Services ADFS et de vous familiariser avec son ensemble de fonctionnalités.  
  
> [!TIP]  
> Vous trouverez des liens vers des ressources AD FS supplémentaires à la page [Plan de contenu AD FS 2.0](https://social.technet.microsoft.com/wiki/contents/articles/2735.aspx) sur Microsoft TechNet Wiki. Cette page est gérée par les membres de la communauté AD FS et est régulièrement surveillée par l'équipe de produit AD FS.  
  
## <a name="ad-fs-terminology-used-in-this-guide"></a>Terminologie AD FS utilisée dans ce guide  
  
|Terme AD FS|Définition|  
|--------------|--------------|  
|Organisation partenaire de compte|Organisation partenaire de fédération représentée par une approbation de fournisseur de revendications dans le service de fédération. L’organisation partenaire de compte contient les utilisateurs qui accèdent aux applications Web\-dans le partenaire de ressource.|  
|Serveur de fédération de comptes|Serveur de fédération dans l'organisation partenaire de compte. Le serveur de fédération de comptes émet des jetons de sécurité pour les utilisateurs en fonction de l'authentification utilisateur. Le serveur authentifie l’utilisateur, extrait les informations d’appartenance de groupe et les attributs appropriés hors du magasin d’attributs, regroupe ces informations dans des revendications, puis génère et signe un jeton de sécurité \(qui contient les revendications\) à retourner à l’utilisateur, soit pour être utilisé dans sa propre organisation, soit pour être envoyé à une organisation partenaire.|  
|Base de données de configuration AD FS|Base de données dans laquelle sont stockées toutes les données de configuration qui représentent une instance AD FS ou un service de fédération spécifique. Ces données de configuration peuvent être stockées dans une base de données SQL Server ou à l’aide de la fonctionnalité de base de données interne Windows incluse dans Windows Server 2016, Windows Server 2012 et 2012 R2 et Windows Server 2008 et 2008 R2. </br></br>Vous pouvez créer la base de données de configuration AD FS pour SQL Server à l’aide de la commande fsconfig. exe\-outil en ligne et pour la base de données interne Windows à l’aide de l’Assistant Configuration du serveur de fédération AD FS.|  
|Fournisseur de revendications|Organisation qui fournit les revendications à ses utilisateurs. Consultez « Organisation partenaire de compte ».|  
|Approbation de fournisseur de revendications|Dans le\-du composant logiciel enfichable Gestion de l’AD FS dans, les approbations de fournisseur de revendications sont des objets d’approbation généralement créés dans des organisations partenaires de ressource pour représenter l’organisation dans la relation d’approbation dont les comptes accèdent aux ressources de l’organisation partenaire de ressource. Un objet d'approbation de fournisseur de revendications se compose d'un ensemble d'identificateurs, de noms et de règles qui identifient ce partenaire auprès du service de fédération local.|  
|Approbation de fournisseur de revendications local|Objet d’approbation qui représente AD LDS ou troisième répertoires basés sur des\-LDAP\-tiers dans une batterie de AD FS. Un objet d’approbation de fournisseur de revendications local se compose d’un ensemble d’identificateurs, de noms et de règles qui identifient ce répertoire LDAP\-sur le service FS (Federation Service) local.|  
|Métadonnées de fédération|Format de données utilisé pour la communication des informations de configuration entre un fournisseur de revendications et une partie de confiance pour faciliter la configuration des approbations de fournisseur de revendications et des approbations de partie de confiance. Le format de données est défini dans Security Assertion Markup Language \(SAML\) 2,0 et il est étendu dans WS\-Federation.|  
|Serveur de fédération|Un serveur Windows Server qui a été configuré à l’aide de l’Assistant Configuration du serveur de fédération AD FS pour agir dans le rôle de serveur de Fédération. Un serveur de fédération émet des jetons et agit dans le cadre d'un service de fédération.|  
|Serveur proxy de fédération|Un serveur Windows Server qui a été configuré à l’aide de AD FS Assistant Configuration du serveur proxy de Fédération pour agir en tant que service proxy intermédiaire entre un client Internet et un service FS (Federation Service) situé derrière un pare-feu sur un réseau d’entreprise.|  
|Serveur de fédération principal|Un serveur Windows Server qui a été configuré dans le rôle serveur de Fédération à l’aide de l’Assistant Configuration du serveur de fédération AD FS et possède une copie en lecture\/en écriture de la base de données de configuration AD FS. </br></br> Le serveur de Fédération principal est créé lorsque vous utilisez l’Assistant Configuration du serveur de fédération AD FS et que vous sélectionnez l’option permettant de créer un service FS (Federation Service) et de faire de cet ordinateur le premier serveur de Fédération de la batterie. Tous les autres serveurs de Fédération de cette batterie doivent répliquer les modifications apportées sur le serveur de Fédération principal vers une copie en lecture\-uniquement de la base de données de configuration AD FS stockée localement. Le terme « serveur de fédération principal » ne s'applique pas quand la base de données de configuration AD FS est stockée dans une base de données SQL, car tous les serveurs de fédération bénéficient du même accès en lecture et écriture à une base de données de configuration stockée sur un serveur SQL Server.|  
|Partie de confiance|Organisation qui reçoit et traite les revendications. Consultez « Organisation partenaire de ressource ».|  
|Approbation de partie de confiance|Dans le\-du composant logiciel enfichable Gestion de AD FS, les approbations de partie de confiance sont des objets d’approbation généralement créés dans :<br /><br />-Les organisations partenaires de compte pour représenter l’organisation dans la relation d’approbation dont les comptes accèdent aux ressources de l’organisation partenaire de ressource.<br />-Organisations partenaires de ressource pour représenter la relation d’approbation entre les service FS (Federation Service) et une application Web basée sur\-unique.<br /><br />Un objet d’approbation de partie de confiance se compose d’un ensemble d’identificateurs, de noms et de règles qui identifient ce partenaire ou cette application de\-Web dans le service FS (Federation Service) local.|  
|Serveur de fédération de ressources|Serveur de fédération dans l'organisation partenaire de ressource. En règle générale, le serveur de fédération de ressources émet des jetons de sécurité pour les utilisateurs en fonction d'un jeton de sécurité émis par un serveur de fédération de comptes. Le serveur reçoit le jeton de sécurité, vérifie la signature, applique une logique de règle de revendication aux revendications non empaquetées afin de produire les revendications sortantes souhaitées, génère un nouveau jeton de sécurité \(avec les revendications sortantes\) en fonction des informations contenues dans le jeton de sécurité entrant et signe le nouveau jeton à retourner à l’utilisateur et finalement à l’application Web.|  
|Organisation partenaire de ressource|Partenaire de fédération représenté par une approbation de partie de confiance dans le service de fédération. Le partenaire de ressource émet des jetons de sécurité basés sur des revendications\-qui contiennent des applications Web\-publiées auxquelles les utilisateurs du partenaire de compte peuvent accéder.|  
  
## <a name="overview-of-ad-fs"></a>Vue d'ensemble d'AD FS  
AD FS est une solution d’accès aux identités qui fournit aux ordinateurs clients des \(internes ou externes à votre réseau\) avec un accès SSO transparent à des applications ou des services Internet\-protégés, même lorsque les comptes d’utilisateurs et les applications se trouvent dans des réseaux ou organisations complètement différents.  
  
En règle générale, quand une application ou un service se trouve sur un réseau et qu'un compte d'utilisateur se trouve sur un autre réseau, l'utilisateur est invité à indiquer des informations d'identification secondaires pour accéder à l'application ou au service. Ces informations d'identification secondaires représentent l'identité de l'utilisateur dans le domaine qui abrite l'application ou le service. Le serveur web qui héberge l'application ou le service se sert généralement de ces informations pour prendre la décision la plus appropriée en matière d'autorisation.  
  
Avec AD FS, les organisations peuvent contourner les demandes d’informations d’identification secondaires en fournissant des relations d’approbation \(approbations de Fédération\) que ces organisations peuvent utiliser pour projeter l’identité numérique et les droits d’accès d’un utilisateur à des partenaires de confiance. Dans cet environnement fédéré, chaque organisation continue de gérer ses propres identités, tout en pouvant mutuellement partager des identités avec d'autres organisations de manière sécurisée.  
  
-   [Rôle des magasins d’attributs](The-Role-of-Attribute-Stores.md)  
  
-   [Rôle de la base de données de configuration AD FS](The-Role-of-the-AD-FS-Configuration-Database.md)  
  
-   [Rôle des revendications](The-Role-of-Claims.md)  
  
-   [Rôle des règles de revendication](The-Role-of-Claim-Rules.md)  
  
-   [Rôle du moteur de revendications](The-Role-of-the-Claims-Engine.md)  
  
-   [Rôle du pipeline de revendications](The-Role-of-the-Claims-Pipeline.md)  
  
-   [Rôle du langage de règle de revendication](The-Role-of-the-Claim-Rule-Language.md)  
  
-   [Déterminer le type de modèle de règle de revendication à utiliser](Determine-the-Type-of-Claim-Rule-Template-to-Use.md)  
  
-   [Utilisation des URI dans AD FS](How-URIs-Are-Used-in-AD-FS.md)  
  

