---
ms.assetid: 204f5fe9-3611-4da0-b057-a386004b4598
title: "Concepts de présentation de clé ActiveDirectory Federation Services"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 27282c6b88b0457af3b4cf031fdadced7b40268c
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

# <a name="understanding-key-ad-fs-concepts"></a>Présentation des Concepts AD FS clés
Il est recommandé d’en savoir plus sur les concepts importants pour les Services de fédération ActiveDirectory et de vous familiariser avec leurs fonctionnalités.  
  
> [!TIP]  
> Vous pouvez trouver des liens de ressources AD FS supplémentaires à la [AD FS Content Map](https://social.technet.microsoft.com/wiki/contents/articles/2735.aspx) page sur Microsoft TechNet Wiki. Cette page est gérée par les membres de la Communauté AD FS et est analysée régulièrement par l’équipe de produit AD FS.  
  
## <a name="ad-fs-terminology-used-in-this-guide"></a>Terminologie ADFS utilisée dans ce guide  
  
|Terme ADFS|Définition|  
|--------------|--------------|  
|Organisation partenaire de compte|Une organisation partenaire de fédération qui est représentée par une approbation de fournisseur de revendications dans le Service de fédération. L’organisation partenaire de compte contient les utilisateurs qui accèdent aux applications basées sur la console Web dans le partenaire de ressource.|  
|Serveur de fédération de comptes|Le serveur de fédération dans l’organisation partenaire de compte. Le serveur de fédération de comptes émet des jetons de sécurité pour les utilisateurs en fonction de l’authentification des utilisateurs. Le serveur authentifie l’utilisateur, extrait les informations d’appartenance hors du magasin d’attributs et les attributs appropriés, rassemble ces informations en revendications et génère et signe un jeton de sécurité \ (qui contient le claims\) pour revenir à l’utilisateur, à utiliser dans sa propre organisation ou à envoyer à une organisation partenaire.|  
|Base de données de configuration ADFS|Une base de données utilisé pour stocker toutes les données de configuration qui représente une instance ADFS ou le Service de fédération unique. Ces données de configuration peuvent être stockées dans une base de données SQLServer ou à l’aide de la fonctionnalité de base de données interne Windows incluse avec Windows Server2016, Windows Server2012 et 2012R2 et Windows Server2008 et 2008R2. </br></br>Vous pouvez créer la base de données de configuration ADFS pour SQLServer à l’aide de l’outil de ligne de commande Fsconfig.exe et pour la base de données interne Windows à l’aide de l’Assistant Configuration du serveur de fédération ADFS.|  
|Fournisseur de revendications|L’organisation qui fournit des revendications à ses utilisateurs. Consultez l’organisation partenaire de compte.|  
|Approbation de fournisseur de revendications|Dans la gestion ADFS dans composants, approbations de fournisseur de revendications sont des objets d’approbation généralement créés dans les organisations partenaires de ressource pour représenter l’organisation dans la relation de confiance dont les comptes accèdent aux ressources de l’organisation partenaire de ressource. Un objet d’approbation de fournisseur de revendications se compose d’un ensemble d’identificateurs, les noms et les règles qui identifient ce partenaire auprès du Service de fédération local.|  
|Approbation de fournisseur de revendications local|Un objet d’approbation qui représente les services ADLDS ou des annuaires LDAP\ third\ tiers dans une batterie ADFS. Un objet se compose d’un ensemble de règles qui identifient cet annuaire LDAP\ au Service de fédération local, les noms et les identificateurs d’approbation de fournisseur de revendications.|  
|Métadonnées de fédération|Le format de données pour communiquer des informations de configuration entre un fournisseur de revendications et une partie de confiance pour faciliter la configuration des approbations de fournisseur de revendications et approbations de partie de confiance. Le format de données est défini dans \(SAML\) Security Assertion Markup Language 2.0, et il est étendu dans WS-Federation.|  
|Serveur de fédération|Un serveur Windows qui a été configuré à l’aide de l’Assistant Configuration du serveur de fédération ADFS pour jouer le rôle de serveur de fédération. Un serveur de fédération émet des jetons et sert dans le cadre d’un Service de fédération.|  
|Serveur proxy de fédération|Un serveur Windows qui a été configuré à l’aide de l’Assistant Configuration Proxy des serveurs de fédération ADFS pour agir en tant que service par un proxy intermédiaire entre un client Internet et un Service de fédération qui se trouve derrière un pare-feu sur un réseau d’entreprise.|  
|Serveur de fédération principal|Un serveur Windows qui a été configuré dans le rôle de serveur de fédération à l’aide de l’Assistant Configuration du serveur de fédération ADFS et dispose d’une copie en lecture/écriture de la base de données de configuration ADFS. </br></br> Le serveur de fédération principal est créé lorsque vous utilisez l’Assistant Configuration du serveur de fédération ADFS et que vous sélectionnez l’option pour créer un nouveau Service de fédération et de faire de cet ordinateur le premier serveur de fédération dans la batterie de serveurs. Tous les autres serveurs de fédération de cette batterie doivent répliquer les modifications apportées sur le serveur de fédération principal vers une copie en lecture seule de la base de données de configuration ADFS qui est stockée localement. Le terme «serveur de fédération principal» ne s’applique pas quand la base de données de configuration ADFS est stockée dans une base de données SQL, car tous les serveurs de fédération bénéficient du même lire et écrire dans une base de données de configuration stockée sur un serveur SQLServer.|  
|Partie de confiance|L’organisation qui reçoit et traite les revendications. Consultez l’organisation partenaire de ressource.|  
|Approbation de partie de confiance|Dans la gestion ADFS dans composants, approbations de partie de confiance sont objets d’approbation généralement créés dans:<br /><br />-Organisations partenaires de compte pour représenter l’organisation dans la relation de confiance dont les comptes accèdent aux ressources de l’organisation partenaire de ressource.<br />-Organisations partenaires de ressource pour représenter la relation d’approbation entre le Service de fédération et une application de console Web unique.<br /><br />Un objet d’approbation de partie confiance se compose d’un ensemble d’identificateurs, les noms et les règles qui identifient ce partenaire ou une application console Web pour le Service de fédération local.|  
|Serveur de fédération de ressources|Le serveur de fédération dans l’organisation partenaire de ressource. Le serveur de fédération de ressources émet généralement des jetons de sécurité pour les utilisateurs basés sur un jeton de sécurité émis par un serveur de fédération de comptes. Le serveur reçoit le jeton de sécurité, vérifie la signature, applique la logique de règle de revendication pour les revendications désassemblées pour produire les revendications sortantes souhaitées, génère un nouveau jeton de sécurité \ (avec le trafic sortant claims\) en fonction des informations dans le jeton de sécurité entrant et signe le nouveau jeton à retourner à l’utilisateur et finalement à l’application Web.|  
|Organisation partenaire de ressource|Un partenaire de fédération qui est représenté par une approbation de partie de confiance dans le Service de fédération. Le partenaire de ressource émet des jetons de sécurité basée sur les claims\ qui contient les applications publiées de console Web que les utilisateurs du partenaire de compte peuvent accéder.|  
  
## <a name="overview-of-ad-fs"></a>Vue d’ensemble des services ADFS  
ADFS est une solution d’accès identité qui permet aux ordinateurs clients \ (internes ou externes à votre Updatable) avec un accès SSO transparent aux applications côté Internet ou services protégés, même si les comptes d’utilisateur et les applications sont trouvent dans des réseaux ou organisations complètement différents.  
  
Lorsqu’une application ou un service est dans un réseau et un compte d’utilisateur est dans un autre réseau, généralement l’utilisateur est invité pour des informations d’identification secondaires lorsqu’il ou elle tente d’accéder à l’application ou le service. Ces informations d’identification secondaires représentent l’identité de l’utilisateur dans le domaine dans lequel l’application ou le service réside. Ils sont généralement requis par le serveur Web qui héberge l’application ou le service afin qu’il puisse effectuer la décision d’autorisation la plus appropriée.  
  
Avec ADFS, les organisations peuvent contourner les demandes d’informations d’identification secondaires en fournissant \(federation trusts\) relations d’approbation qui ces organisations peuvent utiliser pour un utilisateur numérique identité et accès droits partenaires approuvés. Dans cet environnement fédéré, chaque organisation continue à gérer ses propres identités, mais peut également en toute sécurité du projet et accepter les identités d’autres organisations.  
  
-   [Le rôle des magasins d’attributs](The-Role-of-Attribute-Stores.md)  
  
-   [Le rôle de la base de données de Configuration ADFS](The-Role-of-the-AD-FS-Configuration-Database.md)  
  
-   [Le rôle de revendications](The-Role-of-Claims.md)  
  
-   [Le rôle de règles de revendication](The-Role-of-Claim-Rules.md)  
  
-   [Le rôle du moteur de revendications](The-Role-of-the-Claims-Engine.md)  
  
-   [Le rôle du Pipeline de revendications](The-Role-of-the-Claims-Pipeline.md)  
  
-   [Le rôle du langage de règle de revendication](The-Role-of-the-Claim-Rule-Language.md)  
  
-   [Déterminer le Type de modèle de règle de revendication à utiliser](Determine-the-Type-of-Claim-Rule-Template-to-Use.md)  
  
-   [Utilisation des URI dans AD FS](How-URIs-Are-Used-in-AD-FS.md)  
  

