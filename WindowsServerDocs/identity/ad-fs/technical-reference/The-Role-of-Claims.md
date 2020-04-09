---
ms.assetid: 22f53391-8c6a-4873-a1f4-08b4760ea621
title: Rôle des revendications
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 2477152231489e309fc48fd57d38e09a9bf658eb
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860172"
---
# <a name="the-role-of-claims"></a>Rôle des revendications
Dans le modèle d’identité basé sur les revendications\-, les revendications jouent un rôle pivot dans le processus de Fédération, elles sont le composant clé par lequel le résultat de toutes les demandes d’authentification et d’autorisation basées sur le\-Web est déterminé. Ce modèle permet aux organisations de partager de manière sécurisée l'identité numérique et les droits d'accès , ou *revendications*, hors de l'entreprise de façon normalisée.  
  
## <a name="what-are-claims"></a>Définition des revendications  
Dans sa forme la plus simple, les revendications sont simplement des *instructions* \(par exemple, le nom, l’identité, le groupe\), effectuées sur les utilisateurs, qui sont principalement utilisés pour autoriser l’accès aux applications\-basées sur les revendications situées n’importe où sur Internet. Chaque instruction correspond à une *valeur* stockée dans la revendication.  
  
### <a name="how-claims-are-sourced"></a>Génération des revendications  
La service FS (Federation Service) dans Services ADFS \(AD FS\) définit les revendications qui sont échangées entre les partenaires fédérés. Toutefois, il doit auparavant remplir ou générer la revendication avec une valeur récupérée ou une valeur calculée. Chaque valeur de revendication représente une valeur d'un utilisateur, d'un groupe ou d'une entité et est générée de l'une des deux façons suivantes :  
  
1.  Quand la valeur qui constitue la revendication est récupérée d'un magasin d'attributs, à l'image d'une valeur d'attribut du service commercial extraite des propriétés d'un compte d'utilisateur Active Directory. Pour plus d'informations, consultez [Rôle des magasins d'attributs](The-Role-of-Attribute-Stores.md).  
  
2.  Quand la valeur d'une revendication entrante est transformée en une valeur basée sur la logique exprimée dans une règle. C'est par exemple le cas quand une revendication entrante comportant la valeur Administrateurs du domaine est transformée en valeur Administrateurs avant d'être envoyée sous la forme d'une revendication sortante. Pour plus d'informations, consultez [Rôle des règles de revendication](The-Role-of-Claim-Rules.md).  
  
Les revendications peuvent inclure des valeurs telles qu’une adresse e\-mail, un nom d’utilisateur principal \(\)UPN, l’appartenance à un groupe et d’autres attributs de compte.  
  
### <a name="how-claims-flow"></a>Organisation du flux des revendications  
Les autres parties s’appuient sur les valeurs des revendications pour effectuer des tâches d’autorisation pour les applications Web\-qu’elles hébergent. Ces parties sont appelées *parties de confiance* dans le\-du composant logiciel enfichable de gestion AD FS dans. La service FS (Federation Service) est responsable de la négociation de l’approbation entre plusieurs parties disparates. Il est conçu pour traiter et transmettre l’échange approuvé de revendications d’une organisation qui, à l’origine, les revendications, également appelées *fournisseurs de revendications* dans le\-du composant logiciel enfichable de gestion AD FS dans, à une partie de confiance. Une partie de confiance utilise ensuite ces revendications pour prendre des décisions d'autorisation.  
  
Dans le cadre de ce processus, le flux des revendications est appelé *pipeline de revendications*. Le flux de revendications dans le pipeline de revendications comprend trois étapes :  
  
1.  Les revendications reçues du fournisseur de revendications sont traitées par les règles de transformation d'acceptation sur l'approbation de fournisseur de revendications. Ces règles déterminent les revendications qui sont acceptées du fournisseur de revendications.  
  
2.  La sortie des règles de transformation d'acceptation sert de paramètre d'entrée pour les règles d'autorisation d'émission. Ces règles déterminent si l'utilisateur est autorisé à accéder à la partie de confiance.  
  
3.  La sortie des règles de transformation d'acceptation sert de paramètre d'entrée pour les règles de transformation d'émission. Ces règles déterminent les revendications à envoyer à la partie de confiance.  
  
Pour plus d'informations, consultez [Rôle du pipeline de revendications](The-Role-of-the-Claims-Pipeline.md)  
  
### <a name="how-claims-are-issued"></a>Processus d'émission des revendications  
Quand vous écrivez des règles de revendication, la source des revendications entrantes pour ces règles varie selon que celles-ci concernent une approbation de fournisseur de revendications ou une approbation de partie de confiance. Quand vous écrivez des règles de revendication pour une approbation de fournisseur de revendications, les revendications entrantes sont les revendications envoyées depuis le fournisseur de revendications approuvé vers le service de fédération. Quand vous écrivez des règles pour une approbation de partie de confiance, les revendications entrantes sont les revendications qui sont générées par les règles de revendication de l'approbation de fournisseur de revendications applicable. Pour plus d'informations sur les revendications entrantes et sur les revendications sortantes, consultez [Rôle du pipeline de revendications](The-Role-of-the-Claims-Pipeline.md) et [Rôle du moteur de revendications](The-Role-of-the-Claims-Engine.md).  
  
## <a name="what-are-claim-types"></a>Définition des types de revendication  
Un type de revendication fournit le contexte de la valeur de la revendication. Elle est généralement exprimée sous la forme d’un Uniform Resource Identifier URI \(\). AD FS peut prendre en charge tout type de revendication et il est configuré par défaut avec les types de revendication dans le tableau suivant.  
  
|Nom|Description|URI|  
|--------|---------------|-------|  
|E\-adresse de messagerie|Adresse de messagerie\-de l’utilisateur|http :\/\/schemas.xmlsoap.org\/WS\/2005\/05\/\/les revendications d’identité\/EmailAddress|  
|Prénom|Prénom de l'utilisateur|http :\/\/schemas.xmlsoap.org\/WS\/2005\/05\/des revendications d’identité\/GivenName\/|  
|Nom|Nom unique de l'utilisateur|http :\/\/schemas.xmlsoap.org\/WS\/2005\/05\/Identity\/revendications\/Name|  
|UPN|Nom d’utilisateur principal \(\) UPN de l’utilisateur|http :\/\/schemas.xmlsoap.org\/WS\/2005\/05\/identité\/revendications\/UPN|  
|Nom commun|Nom commun de l'utilisateur|http :\/\/schemas.xmlsoap.org\/revendications\/CommonName|  
|AD FS 1. x E\-adresse de messagerie|L’e\-adresse de messagerie de l’utilisateur lors de l’interopérabilité avec AD FS 1,1 ou ADFS 1,0|http :\/\/schemas.xmlsoap.org\/revendications\/EmailAddress|  
|Groupe|Groupe dont l'utilisateur est membre|http :\/\/schemas.xmlsoap.org\/revendications\/groupe|  
|UPN AD FS 1.x|Nom d'utilisateur principal de l'utilisateur quand il interagit avec AD FS 1.1 ou AD FS 1.0|http :\/\/schemas.xmlsoap.org\/revendications\/UPN|  
|Role|Rôle de l'utilisateur|\/rôle\/\/schemas.microsoft.com\/WS\/2008\/\/\/|  
|Nom|Nom de l'utilisateur|http :\/\/schemas.xmlsoap.org\/WS\/2005\/05\/identité\/les revendications\/nom de famille|  
|PPID|Identificateur privé de l'utilisateur|http :\/\/schemas.xmlsoap.org\/WS\/2005\/05\/Identity\/claims\/PrivatePersonalIdentifier|  
|Identificateur de nom|Identificateur de nom SAML de l'utilisateur|http :\/\/schemas.xmlsoap.org\/WS\/2005\/05\/Identity\/claims\/NameIdentifier|  
|Méthode d'authentification|Méthode utilisée pour authentifier l'utilisateur|schemas.microsoft.com \/\/\/2008\/\/06\/d’identité\/de revendications\/le protocole AuthenticationMethod|  
|SID de groupe en refus seul|Le SID de groupe de refus\-uniquement de l’utilisateur|http :\/\/schemas.xmlsoap.org\/WS\/2005\/05\/Identity\/claims\/DenyOnlySid|  
|SID principal en refus seul|L’autorisation Deny\-uniquement le SID principal de l’utilisateur|DenyOnlyPrimarySid\/\/2008 \/schemas.microsoft.com ws\/\/06\/revendications\/\/|  
|SID de groupe principal en refus seul|L’autorisation Deny\-uniquement le SID du groupe principal de l’utilisateur|DenyOnlyPrimaryGroupSid\/\/2008 \/schemas.microsoft.com ws\/\/06\/revendications\/\/|  
|SID de groupe|SID de groupe de l'utilisateur|GroupSID\/\/2008 \/schemas.microsoft.com ws\/\/06\/revendications\/\/|  
|SID de groupe principal|SID de groupe principal de l'utilisateur|PrimaryGroupSid\/\/2008 \/schemas.microsoft.com ws\/\/06\/revendications\/\/|  
|SID principal|SID principal de l'utilisateur|PrimarySid\/\/2008 \/schemas.microsoft.com ws\/\/06\/revendications\/\/|  
|Nom de compte Windows|Nom du compte de domaine de l’utilisateur sous la forme d' \<\>de domaine \\\<utilisateur\>|attributs WindowsAccountName\/\/2008 \/schemas.microsoft.com ws\/\/06\/revendications\/\/|  
  
## <a name="what-are-claim-descriptions"></a>Définition des descriptions de revendication  
Les descriptions des revendications représentent la liste des types de revendications que AD FS prend en charge et qui peuvent être publiés dans les métadonnées de Fédération. Les types de revendication mentionnés dans le tableau précédent sont configurés en tant que descriptions de revendications dans le\-du composant logiciel enfichable de gestion AD FS dans.  
  
La collection des descriptions de revendication à publier dans les métadonnées de fédération est stockée dans la base de données de configuration AD FS. Ces descriptions de revendication sont utilisées par différents composants du service de fédération.  
  
Chaque description de revendication comprend un URI de type de revendication, un nom, un état de publication et une description. Vous pouvez gérer la collection de description de revendication à l’aide du nœud **descriptions des revendications** dans le\-du composant logiciel enfichable de gestion AD FS dans. Vous pouvez modifier l’état de publication d’une description de revendication à l’aide de l'\-d’accrochage dans. Les options suivantes sont disponibles :  
  
-   **Publiez cette revendication dans les métadonnées de Fédération en tant que type de revendication que ce service FS (Federation Service) peut accepter** \(publier comme\)acceptées — indique les types de revendication qui seront acceptés par ce service FS (Federation Service) par d’autres fournisseurs de revendications.  
  
-   **Publiez cette revendication dans les métadonnées de Fédération en tant que type de revendication que ce service FS (Federation Service) peut envoyer** \(publication comme étant envoyée\)— indique les types de revendication proposés par cette service FS (Federation Service). Il s'agit des types de revendication que le service de fédération publie comme étant ceux qu'il consent à envoyer. En règle générale, le fournisseur de revendications n'envoie qu'une partie de cette liste de types de revendication.  
  
Pour plus d’informations sur la définition de l’état de publication d’un type de revendication, consultez [Ajouter une description de revendication](https://technet.microsoft.com/library/dd807051.aspx) dans le Guide de déploiement AD FS.  
  
### <a name="when-generating-federation-metadata"></a>Génération des métadonnées de fédération  
Les métadonnées de fédération comprennent toutes les descriptions de revendication marquées pour publication.  
  
### <a name="when-claims-rules-are-processed"></a>Traitement des règles de revendication  
Quand vous conservez les informations de configuration sur les descriptions de revendication, vous pouvez plus facilement configurer des règles sur les revendications. Pour plus d'informations sur les règles de revendication utilisables dans l'organisation fournisseur de revendications, consultez [Rôle des règles de revendication](The-Role-of-Claim-Rules.md).  
  

