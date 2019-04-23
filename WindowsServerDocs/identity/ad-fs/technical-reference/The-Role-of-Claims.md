---
ms.assetid: 22f53391-8c6a-4873-a1f4-08b4760ea621
title: Rôle des revendications
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: df35348cb51a9021f4aaa2fc6516cb119bdb8b8f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849430"
---
>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="the-role-of-claims"></a>Rôle des revendications
Dans les revendications\-modèle d’identité basé sur, revendications jouent un rôle essentiel dans le processus de fédération, ils constituent le composant clé par lequel le résultat de toutes les applications Web\-basé sur les demandes d’authentification et d’autorisation sont déterminées. Ce modèle permet aux organisations de partager de manière sécurisée l'identité numérique et les droits d'accès , ou *revendications*, hors de l'entreprise de façon normalisée.  
  
## <a name="what-are-claims"></a>Définition des revendications  
Dans sa forme la plus simple, les revendications sont simplement *instructions* \(, par exemple, nom, identité, groupe\), effectuées sur les utilisateurs, qui sont principalement utilisées pour autoriser l’accès aux revendications\-en fonction des applications situés n’importe où sur Internet. Chaque instruction correspond à une *valeur* stockée dans la revendication.  
  
### <a name="how-claims-are-sourced"></a>Génération des revendications  
Le Service de fédération dans Active Directory Federation Services \(AD FS\) définit les revendications qui sont échangées entre partenaires fédérés. Toutefois, il doit auparavant remplir ou générer la revendication avec une valeur récupérée ou une valeur calculée. Chaque valeur de revendication représente une valeur d'un utilisateur, d'un groupe ou d'une entité et est générée de l'une des deux façons suivantes :  
  
1.  Quand la valeur qui constitue la revendication est récupérée d'un magasin d'attributs, à l'image d'une valeur d'attribut du service commercial extraite des propriétés d'un compte d'utilisateur Active Directory. Pour plus d'informations, consultez [Rôle des magasins d'attributs](The-Role-of-Attribute-Stores.md).  
  
2.  Quand la valeur d'une revendication entrante est transformée en une valeur basée sur la logique exprimée dans une règle. C'est par exemple le cas quand une revendication entrante comportant la valeur Administrateurs du domaine est transformée en valeur Administrateurs avant d'être envoyée sous la forme d'une revendication sortante. Pour plus d'informations, consultez [Rôle des règles de revendication](The-Role-of-Claim-Rules.md).  
  
Revendications peuvent inclure des valeurs telles qu’un e\-adresse électronique, nom d’utilisateur Principal \(UPN\), l’appartenance au groupe et autres attributs de compte.  
  
### <a name="how-claims-flow"></a>Organisation du flux des revendications  
Autres parties tierces s’appuient sur les valeurs des revendications pour effectuer des tâches d’autorisation pour le Web\-en fonction des applications qu’ils hébergent. Ces parties sont appelés *parties de confiance* dans le composant logiciel enfichable Gestion AD FS\-dans. Le Service de fédération est chargé pour négocier l’approbation entre les nombreuses parties. Il est conçu pour traiter et d’organiser l’échange approuvé de revendications à partir d’une organisation qui a généré les revendications, également appelées *fournisseurs de revendications* dans le composant logiciel enfichable Gestion AD FS\-dans, à une partie de confiance. Une partie de confiance utilise ensuite ces revendications pour prendre des décisions d'autorisation.  
  
Dans le cadre de ce processus, le flux des revendications est appelé *pipeline de revendications*. Le flux de revendications dans le pipeline de revendications comprend trois étapes :  
  
1.  Les revendications reçues du fournisseur de revendications sont traitées par les règles de transformation d'acceptation sur l'approbation de fournisseur de revendications. Ces règles déterminent les revendications qui sont acceptées du fournisseur de revendications.  
  
2.  La sortie des règles de transformation d'acceptation sert de paramètre d'entrée pour les règles d'autorisation d'émission. Ces règles déterminent si l'utilisateur est autorisé à accéder à la partie de confiance.  
  
3.  La sortie des règles de transformation d'acceptation sert de paramètre d'entrée pour les règles de transformation d'émission. Ces règles déterminent les revendications à envoyer à la partie de confiance.  
  
Pour plus d'informations, consultez [Rôle du pipeline de revendications](The-Role-of-the-Claims-Pipeline.md)  
  
### <a name="how-claims-are-issued"></a>Processus d'émission des revendications  
Quand vous écrivez des règles de revendication, la source des revendications entrantes pour ces règles varie selon que celles-ci concernent une approbation de fournisseur de revendications ou une approbation de partie de confiance. Quand vous écrivez des règles de revendication pour une approbation de fournisseur de revendications, les revendications entrantes sont les revendications envoyées depuis le fournisseur de revendications approuvé vers le service de fédération. Quand vous écrivez des règles pour une approbation de partie de confiance, les revendications entrantes sont les revendications qui sont générées par les règles de revendication de l'approbation de fournisseur de revendications applicable. Pour plus d'informations sur les revendications entrantes et sur les revendications sortantes, consultez [Rôle du pipeline de revendications](The-Role-of-the-Claims-Pipeline.md) et [Rôle du moteur de revendications](The-Role-of-the-Claims-Engine.md).  
  
## <a name="what-are-claim-types"></a>Définition des types de revendication  
Un type de revendication fournit le contexte de la valeur de la revendication. Elle est généralement exprimée en un identificateur de ressource uniforme \(URI\). AD FS peut prendre en charge n’importe quel type de revendication, et il est configuré avec les types de revendication dans le tableau suivant par défaut.  
  
|Nom|Description|URI|  
|--------|---------------|-------|  
|E\-adresse de messagerie|Le e\-adresse de l’utilisateur de messagerie|http:\/\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/emailaddress|  
|Prénom|Prénom de l'utilisateur|http:\/\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/givenname|  
|Nom|Nom unique de l'utilisateur|http:\/\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/name|  
|UPN|Le nom d’utilisateur principal \(UPN\) de l’utilisateur|http:\/\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/upn|  
|Nom commun|Nom commun de l'utilisateur|http:\/\/schemas.xmlsoap.org\/claims\/CommonName|  
|AD FS 1.x E\-adresse de messagerie|Le e\-messagerie adresse de l’utilisateur lorsqu’il interagit avec AD FS 1.1 ou AD FS 1.0|http:\/\/schemas.xmlsoap.org\/claims\/EmailAddress|  
|Regrouper|Groupe dont l'utilisateur est membre|http:\/\/schemas.xmlsoap.org\/claims\/Group|  
|UPN AD FS 1.x|Nom d'utilisateur principal de l'utilisateur quand il interagit avec AD FS 1.1 ou AD FS 1.0|http :\/\/schemas.xmlsoap.org\/revendications\/UPN|  
|Rôle|Rôle de l'utilisateur|http:\/\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/role|  
|Nom|Nom de l'utilisateur|http:\/\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/surname|  
|PPID|Identificateur privé de l'utilisateur|http:\/\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/privatepersonalidentifier|  
|Identificateur de nom|Identificateur de nom SAML de l'utilisateur|http:\/\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/nameidentifier|  
|Méthode d'authentification|Méthode utilisée pour authentifier l'utilisateur|http:\/\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/authenticationmethod|  
|SID de groupe en refus seul|L’option deny\-uniquement SID de groupe de l’utilisateur|http:\/\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/denyonlysid|  
|SID principal en refus seul|L’option deny\-uniquement le SID principal de l’utilisateur|http:\/\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/denyonlyprimarysid|  
|SID de groupe principal en refus seul|L’option deny\-uniquement SID de groupe principal de l’utilisateur|http:\/\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/denyonlyprimarygroupsid|  
|SID de groupe|SID de groupe de l'utilisateur|http:\/\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/groupsid|  
|SID de groupe principal|SID de groupe principal de l'utilisateur|http:\/\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/primarygroupsid|  
|SID principal|SID principal de l'utilisateur|http:\/\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/primarysid|  
|Nom de compte Windows|Le nom du compte de domaine de l’utilisateur sous la forme de \<domaine\>\\\<utilisateur\>|http:\/\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/windowsaccountname|  
  
## <a name="what-are-claim-descriptions"></a>Définition des descriptions de revendication  
Descriptions des revendications représentent une liste des types de revendications prend en charge AD FS et qui peuvent être publiés dans les métadonnées de fédération. Les types de revendication mentionnés dans le tableau précédent sont configurés comme les descriptions de revendication dans le composant logiciel enfichable Gestion AD FS\-dans.  
  
La collection des descriptions de revendication à publier dans les métadonnées de fédération est stockée dans la base de données de configuration AD FS. Ces descriptions de revendication sont utilisées par différents composants du service de fédération.  
  
Chaque description de revendication comprend un URI de type de revendication, un nom, un état de publication et une description. Vous pouvez gérer la collection de descriptions de revendication à l’aide de la **descriptions des revendications** nœud dans le composant logiciel enfichable Gestion AD FS\-dans. Vous pouvez modifier l’état de publication d’une description de revendication à l’aide du composant logiciel enfichable\-dans. Les options suivantes sont disponibles :  
  
-   **Publier cette revendication dans les métadonnées de fédération en tant que type de revendication ce Service de fédération peut accepter** \(publié comme accepté\)— indique les types de revendications qui seront acceptées par cette fédération à partir d’autres fournisseurs de revendications Service.  
  
-   **Publier cette revendication dans les métadonnées de fédération comme un type de revendication que ce Service de fédération peut envoyer** \(publié comme envoyé\)— indique les types de revendications qui sont offertes par ce Service de fédération. Il s'agit des types de revendication que le service de fédération publie comme étant ceux qu'il consent à envoyer. En règle générale, le fournisseur de revendications n'envoie qu'une partie de cette liste de types de revendication.  
  
Pour plus d’informations sur la façon de définir l’état de publication d’un type de revendication, consultez [ajouter une Description de revendication](https://technet.microsoft.com/library/dd807051.aspx) dans le Guide de déploiement d’AD FS.  
  
### <a name="when-generating-federation-metadata"></a>Génération des métadonnées de fédération  
Les métadonnées de fédération comprennent toutes les descriptions de revendication marquées pour publication.  
  
### <a name="when-claims-rules-are-processed"></a>Traitement des règles de revendication  
Quand vous conservez les informations de configuration sur les descriptions de revendication, vous pouvez plus facilement configurer des règles sur les revendications. Pour plus d'informations sur les règles de revendication utilisables dans l'organisation fournisseur de revendications, consultez [Rôle des règles de revendication](The-Role-of-Claim-Rules.md).  
  

