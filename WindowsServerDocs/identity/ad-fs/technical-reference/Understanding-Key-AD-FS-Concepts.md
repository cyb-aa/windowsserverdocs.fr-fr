---
ms.assetid: 204f5fe9-3611-4da0-b057-a386004b4598
title: Présentation de clé Active Directory Federation Services Concepts
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: ac666539170bb7aabf0b7f58a7ef003ebe87c2a8
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188393"
---
# <a name="understanding-key-ad-fs-concepts"></a>Présentation des concepts AD FS clés
Il est recommandé d’en savoir plus sur les concepts importants pour les Services de fédération Active Directory et de vous familiariser avec son jeu de fonctionnalités.  
  
> [!TIP]  
> Vous trouverez des liens vers des ressources AD FS supplémentaires à la page [Plan de contenu AD FS 2.0](https://social.technet.microsoft.com/wiki/contents/articles/2735.aspx) sur Microsoft TechNet Wiki. Cette page est gérée par les membres de la communauté AD FS et est régulièrement surveillée par l'équipe de produit AD FS.  
  
## <a name="ad-fs-terminology-used-in-this-guide"></a>Terminologie AD FS utilisée dans ce guide  
  
|Terme AD FS|Définition|  
|--------------|--------------|  
|Organisation partenaire de compte|Organisation partenaire de fédération représentée par une approbation de fournisseur de revendications dans le service de fédération. L’organisation partenaire de compte contient les utilisateurs qui accéderont Web\-en fonction des applications dans le partenaire de ressource.|  
|Serveur de fédération de comptes|Serveur de fédération dans l'organisation partenaire de compte. Le serveur de fédération de comptes émet des jetons de sécurité pour les utilisateurs en fonction de l'authentification utilisateur. Le serveur authentifie l’utilisateur, extrait les attributs appropriés et les informations d’appartenance du magasin de l’attribut, rassemble ces informations en revendications et génère et signe un jeton de sécurité \(qui contient les revendications\)pour revenir à l’utilisateur, à utiliser dans sa propre organisation ou à envoyer à une organisation partenaire.|  
|Base de données de configuration AD FS|Base de données dans laquelle sont stockées toutes les données de configuration qui représentent une instance AD FS ou un service de fédération spécifique. Ces données de configuration peuvent être stockées dans une base de données SQL Server ou à l’aide de la fonctionnalité de base de données interne Windows incluse avec Windows Server 2016, Windows Server 2012 et 2012 R2 et Windows Server 2008 et 2008 R2. </br></br>Vous pouvez créer la base de données de configuration AD FS pour SQL Server à l’aide de la commande Fsconfig.exe\-outil en ligne et de la base de données interne Windows à l’aide de l’Assistant Configuration du serveur de fédération AD FS.|  
|Fournisseur de revendications|Organisation qui fournit les revendications à ses utilisateurs. Consultez « Organisation partenaire de compte ».|  
|Approbation de fournisseur de revendications|Dans le composant logiciel enfichable Gestion AD FS\-, sont les objets d’approbation généralement créés dans des organisations de partenaires de ressource pour représenter l’organisation dans la relation d’approbation dont les comptes des approbations de fournisseur de revendications seront accèdent aux ressources dans la ressource organisation partenaire. Un objet d'approbation de fournisseur de revendications se compose d'un ensemble d'identificateurs, de noms et de règles qui identifient ce partenaire auprès du service de fédération local.|  
|Approbation de fournisseur de revendications local|Un objet d’approbation qui représente les services AD LDS ou le troisième\-tiers LDAP\-en fonction des répertoires dans une batterie de serveurs AD FS. Un objet se compose d’un ensemble d’identificateurs, les noms et les règles qui identifient ce LDAP d’approbation de fournisseur de revendications\-en fonction du répertoire pour le Service de fédération local.|  
|Métadonnées de fédération|Format de données utilisé pour la communication des informations de configuration entre un fournisseur de revendications et une partie de confiance pour faciliter la configuration des approbations de fournisseur de revendications et des approbations de partie de confiance. Le format de données est défini dans Security Assertion Markup Language \(SAML\) 2.0 et il est étendu dans WS\-fédération.|  
|Serveur de fédération|Un serveur Windows qui a été configuré à l’aide de l’Assistant Configuration du serveur de fédération AD FS pour jouer le rôle de serveur de fédération. Un serveur de fédération émet des jetons et agit dans le cadre d'un service de fédération.|  
|Serveur proxy de fédération|Un serveur Windows qui a été configuré à l’aide de l’Assistant Configuration Proxy des serveur de fédération AD FS en tant que service par un proxy intermédiaire entre un client Internet et un Service de fédération qui se trouve derrière un pare-feu sur un réseau d’entreprise.|  
|Serveur de fédération principal|Un serveur Windows qui a été configuré dans le rôle de serveur de fédération à l’aide de l’Assistant Configuration du serveur de fédération AD FS et a une lecture\/écrire la copie de la base de données de configuration AD FS. </br></br> Le serveur de fédération principal est créé lorsque vous utilisez l’Assistant Configuration du serveur de fédération AD FS et que vous sélectionnez l’option permettant de créer un Service de fédération et faire de cet ordinateur le premier serveur de fédération dans la batterie de serveurs. Tous les autres serveurs de fédération dans cette batterie doivent répliquer les modifications apportées sur le serveur de fédération principal pour une lecture\-uniquement une copie de la base de données de configuration AD FS qui est stockée localement. Le terme « serveur de fédération principal » ne s'applique pas quand la base de données de configuration AD FS est stockée dans une base de données SQL, car tous les serveurs de fédération bénéficient du même accès en lecture et écriture à une base de données de configuration stockée sur un serveur SQL Server.|  
|Partie de confiance|Organisation qui reçoit et traite les revendications. Consultez « Organisation partenaire de ressource ».|  
|Approbation de partie de confiance|Dans le composant logiciel enfichable Gestion AD FS\-, approbations de partie de confiance sont des objets d’approbation généralement créés dans :<br /><br />-Organisations partenaires de compte pour représenter l’organisation dans la relation d’approbation dont les comptes seront accèdent aux ressources de l’organisation partenaire de ressource.<br />-Les organisations de partenaires de ressource pour représenter l’approbation entre le Service de fédération et un seul site web\-application basée sur.<br /><br />Un objet d’approbation de partie de confiance se compose d’un ensemble d’identificateurs, les noms et les règles qui identifient ce partenaire ou le web\-application au Service de fédération local.|  
|Serveur de fédération de ressources|Serveur de fédération dans l'organisation partenaire de ressource. En règle générale, le serveur de fédération de ressources émet des jetons de sécurité pour les utilisateurs en fonction d'un jeton de sécurité émis par un serveur de fédération de comptes. Le serveur reçoit le jeton de sécurité, vérifie la signature, applique la logique de règle de revendication aux revendications décompressées pour produire les revendications sortantes souhaitées, génère un nouveau jeton de sécurité \(avec les revendications sortantes\) en fonction des informations dans le jeton de sécurité entrant et signe le jeton de nouveau pour revenir à l’utilisateur et finalement à l’application Web.|  
|Organisation partenaire de ressource|Partenaire de fédération représenté par une approbation de partie de confiance dans le service de fédération. Le partenaire de ressource émet des revendications\-en fonction des jetons de sécurité qui contient le site Web publié\-en fonction des applications accessibles aux utilisateurs du partenaire de compte.|  
  
## <a name="overview-of-ad-fs"></a>Vue d'ensemble d'AD FS  
AD FS est une solution d’accès identité qui permet aux ordinateurs clients \(internes ou externes à votre réseau\) avec un accès transparent à l’authentification unique protégée Internet\-accessible sur les applications ou services, même lorsque les comptes d’utilisateur et les applications se trouvent dans des réseaux ou organisations complètement différents.  
  
En règle générale, quand une application ou un service se trouve sur un réseau et qu'un compte d'utilisateur se trouve sur un autre réseau, l'utilisateur est invité à indiquer des informations d'identification secondaires pour accéder à l'application ou au service. Ces informations d'identification secondaires représentent l'identité de l'utilisateur dans le domaine qui abrite l'application ou le service. Le serveur web qui héberge l'application ou le service se sert généralement de ces informations pour prendre la décision la plus appropriée en matière d'autorisation.  
  
Avec AD FS, les organisations peuvent contourner les demandes pour les informations d’identification secondaires en fournissant des relations d’approbation \(approbations de fédération\) que ces entreprises peuvent utiliser pour projeter des droits d’un utilisateur numériques identités et des accès approuvé partenaires. Dans cet environnement fédéré, chaque organisation continue de gérer ses propres identités, tout en pouvant mutuellement partager des identités avec d'autres organisations de manière sécurisée.  
  
-   [Le rôle des magasins d’attributs](The-Role-of-Attribute-Stores.md)  
  
-   [Rôle de la base de données de configuration AD FS](The-Role-of-the-AD-FS-Configuration-Database.md)  
  
-   [Rôle des revendications](The-Role-of-Claims.md)  
  
-   [Rôle des règles de revendication](The-Role-of-Claim-Rules.md)  
  
-   [Rôle du moteur de revendications](The-Role-of-the-Claims-Engine.md)  
  
-   [Rôle du pipeline de revendications](The-Role-of-the-Claims-Pipeline.md)  
  
-   [Rôle du langage de règle de revendication](The-Role-of-the-Claim-Rule-Language.md)  
  
-   [Déterminer le Type de modèle de règle de revendication à utiliser](Determine-the-Type-of-Claim-Rule-Template-to-Use.md)  
  
-   [Utilisation des URI dans AD FS](How-URIs-Are-Used-in-AD-FS.md)  
  

