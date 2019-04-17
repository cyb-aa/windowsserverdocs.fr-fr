---
ms.assetid: 22f53391-8c6a-4873-a1f4-08b4760ea621
title: "Le rôle de revendications"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 98765deaba67ffdc0ee18b6d8ef573e531d739cc
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

# <a name="the-role-of-claims"></a>Le rôle de revendications
Dans le modèle identité basée sur les claims\, revendications jouent un rôle essentiel dans le processus de fédération, ils sont le composant clé par lequel le résultat de toutes les demandes d’authentification et d’autorisation console sont déterminés. Ce modèle permet aux organisations de projet en toute sécurité identité numérique et les droits d’accès, ou *revendications*, sur la sécurité et les limites de l’entreprise dans une méthode normalisée.  
  
## <a name="what-are-claims"></a>Quelles sont les revendications?  
Dans sa forme la plus simple, les revendications sont simplement *instructions* \ (par exemple, nom, identité, group\), concernant des utilisateurs, qui sont principalement utilisées pour autoriser l’accès aux applications claims\ situés n’importe où sur Internet. Chaque instruction correspond à un *valeur* qui est stocké dans la revendication.  
  
### <a name="how-claims-are-sourced"></a>Génération des revendications  
Le Service de fédération dans ActiveDirectory Federation Services \(ADFS\) définit les revendications qui sont échangées entre les partenaires fédérés. Toutefois, avant de faire cela, il doit tout d’abord remplir ou la source de la revendication avec une valeur récupérée ou une valeur calculée. Chaque valeur de revendication représente une valeur d’un utilisateur, un groupe ou une entité et est générée dans un des deux façons:  
  
1.  Lorsque la valeur qui constitue la revendication est récupérée à partir d’un magasin d’attributs, par exemple, lorsqu’une valeur d’attribut du service commercial est récupérée à partir des propriétés d’un compte d’utilisateur ActiveDirectory. Pour plus d’informations, voir [le rôle des magasins d’attributs](The-Role-of-Attribute-Stores.md).  
  
2.  Lorsque la valeur d’une revendication entrante est transformée en une valeur basée sur la logique exprimée dans une règle. Par exemple, quand une revendication entrante avec la valeur du groupe Admins du domaine est transformée en une nouvelle valeur d’administrateurs avant qu’il est envoyé en tant qu’une revendication sortante. Pour plus d’informations, voir [The Role of Claim Rules](The-Role-of-Claim-Rules.md).  
  
Les revendications peuvent inclure des valeurs telles qu’une adresse de messagerie e\, nom d’utilisateur Principal \(UPN\), l’appartenance au groupe et d’autres attributs de compte.  
  
### <a name="how-claims-flow"></a>Le flux des revendications  
Autres parties s’appuient sur les valeurs des revendications pour effectuer des tâches d’autorisation pour les applications de console Web qu’ils hébergent. Ces parties sont appelées *parties de confiance* dans la gestion ADFS de composants. Le Service de fédération est chargé d’intermédiaire entre les nombreuses parties. Il est conçu pour traiter et d’organiser l’échange approuvé de revendications à partir d’une organisation qui a généré les revendications, également appelées *fournisseurs de revendications* dans la gestion ADFS logiciel enfichable, à une partie de confiance. Une partie de confiance utilise ensuite ces revendications pour prendre des décisions d’autorisation.  
  
Le flux des revendications à l’aide de ce processus est appelé le *pipeline de revendications*. Il existe trois étapes dans le flux des revendications à travers le pipeline de revendications:  
  
1.  Les revendications qui sont reçues du fournisseur de revendications sont traitées par les règles de transformation d’acceptation sur l’approbation de fournisseur de revendications. Ces règles déterminent les revendications qui sont acceptées du fournisseur de revendications.  
  
2.  La sortie des règles de transformation d’acceptation est utilisée comme entrée pour les règles d’autorisation d’émission. Ces règles déterminent si l’utilisateur est autorisé à accéder à la partie de confiance.  
  
3.  La sortie des règles de transformation d’acceptation est utilisée comme entrée pour les règles de transformation d’émission. Ces règles déterminent les revendications qui seront envoyées à la partie de confiance.  
  
Pour plus d’informations, voir [le rôle du Pipeline de revendications](The-Role-of-the-Claims-Pipeline.md)  
  
### <a name="how-claims-are-issued"></a>Conditions d’émission des revendications  
Lorsque vous écrivez des règles de revendication, la source des revendications entrantes pour les règles de revendication varie selon si vous écrivez des règles sur une approbation de fournisseur de revendications ou une partie de confiance. Lorsque vous écrivez des règles de revendication pour une approbation de fournisseur de revendications, les revendications entrantes sont les revendications envoyées depuis le fournisseur de revendications approuvé vers le Service de fédération. Lorsque vous écrivez des règles pour une approbation de partie de confiance, les revendications entrantes sont les revendications qui sont générées par les règles de revendication de l’approbation de fournisseur de revendications applicable. Pour plus d’informations sur les revendications entrantes et sortantes de revendications, voir [The Role of the Claims Pipeline](The-Role-of-the-Claims-Pipeline.md) et [The Role of the Claims Engine ](The-Role-of-the-Claims-Engine.md).  
  
## <a name="what-are-claim-types"></a>Quels sont les types de revendication?  
Un type de revendication fournit le contexte de la valeur de revendication. Il est généralement exprimée comme un \(URI\) Uniform Resource Identifier. ADFS peut prendre en charge n’importe quel type de revendication, et il est configuré avec les types de revendication dans le tableau suivant par défaut.  
  
|Nom|Description|URI|  
|--------|---------------|-------|  
|Adresse de messagerie E\|L’adresse de messagerie e\ de l’utilisateur|http:///\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/EmailAddress|  
|Prénom|Prénom de l’utilisateur|http:///\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/givenName|  
|Nom|Le nom unique de l’utilisateur|http:///\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/Name|  
|UPN|Le principal de l’utilisateur nom \(UPN\) de l’utilisateur|http:///\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/UPN|  
|Nom commun|Le nom commun de l’utilisateur|http:///\/schemas.xmlsoap.org\/claims\/CommonName|  
|Adresse E\ messagerie ADFS 1.x|L’adresse de messagerie e\ de l’utilisateur lorsqu’il interagit avec ADFS 1.1 ou ADFS 1.0|http:///\/schemas.xmlsoap.org\/claims\/EmailAddress|  
|Groupe|Un groupe de l’utilisateur est membre de|http:///\/schemas.xmlsoap.org\/claims\/Group|  
|UPN ADFS 1.x|Le nom UPN de l’utilisateur lorsqu’il interagit avec ADFS 1.1 ou ADFS 1.0|http:///\/schemas.xmlsoap.org\/claims\/UPN|  
|Rôle|Un rôle de l’utilisateur|http:///\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/Role|  
|Nom de famille|Le nom de l’utilisateur|http:///\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/Surname|  
|PPID|Identificateur privé de l’utilisateur|http:///\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/privatepersonalidentifier|  
|Identificateur de nom|L’identificateur de nom SAML de l’utilisateur|http:///\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/NameIdentifier|  
|Méthode d’authentification|La méthode utilisée pour authentifier l’utilisateur|http:///\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/AuthenticationMethod|  
|Refuser SID de groupe uniquement|Deny\ uniquement SID de groupe de l’utilisateur|http:///\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/denyonlysid|  
|Refuser uniquement les SID principal|SID principal deny\ uniquement de l’utilisateur|http:///\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/denyonlyprimarysid|  
|Refuser uniquement SID de groupe principal|Deny\ uniquement SID de groupe principal de l’utilisateur|http:///\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/denyonlyprimarygroupsid|  
|SID de groupe|Le SID du groupe de l’utilisateur|http:///\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/GroupSID|  
|SID de groupe principal|SID de groupe principal de l’utilisateur|http:///\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/primarygroupsid|  
|SID principal|SID principal de l’utilisateur|http:///\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/primarysid|  
|Nom du compte Windows|Le nom de l’utilisateur sous la forme de compte de domaine<domain>\\<user>|http:///\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/windowsaccountname|  
  
## <a name="what-are-claim-descriptions"></a>Quelles sont les descriptions des revendications?  
Descriptions des revendications représentent une liste de types de revendications prend en charge ADFS et qui peuvent être publiés dans les métadonnées de fédération. Les types de revendication mentionnés dans le tableau précédent sont configurés en tant que les descriptions de revendication dans la gestion ADFS de composants.  
  
La collection de descriptions des revendications qui sera publié dans les métadonnées de fédération est stockée dans la base de données de configuration ADFS. Ces descriptions sont utilisées par différents composants du Service de fédération de revendication.  
  
Chaque description de revendication comprend un URI de type de revendication, un nom, un état de publication et une description. Vous pouvez gérer la collection de descriptions de revendication à l’aide de la **descriptions des revendications** nœud dans la gestion ADFS de composants. Vous pouvez modifier l’état de publication d’une description de revendication à l’aide du composant logiciel enfichable. Les paramètres suivants sont disponibles:  
  
-   **Publier cette revendication dans les métadonnées de fédération sous la forme d’un type de revendication que ce Service de fédération peut accepter** \(Publish as Accepted\) — indique les types de revendication qui vont être acceptées par les autres fournisseurs de revendications par ce Service de fédération.  
  
-   **Publier cette revendication dans les métadonnées de fédération sous la forme d’un type de revendication que ce Service de fédération peut envoyer** \(Publish as Sent\) — indique les types de revendication offerts par ce Service de fédération. Voici les types de revendication que le Service de fédération publie à d’autres que celles qu’il est prêt à envoyer. Types de revendication envoyés par le fournisseur de revendications sont souvent un sous-ensemble de cette liste.  
  
Pour plus d’informations sur la définition de l’état de publication d’un type de revendication, voir [ajouter une Description de revendication](https://technet.microsoft.com/library/dd807051.aspx) dans le Guide de déploiement d’ADFS.  
  
### <a name="when-generating-federation-metadata"></a>Lors de la génération des métadonnées de fédération  
Métadonnées de fédération comprennent toutes les descriptions de revendication marquées pour publication.  
  
### <a name="when-claims-rules-are-processed"></a>Lorsque les règles de revendication sont traitées  
Quand vous conservez les informations de configuration sur les descriptions de revendication, il est plus facile de configurer des règles sur les revendications. Pour plus d’informations sur les règles de revendication pouvant être utilisés dans l’organisation du fournisseur de revendications, voir [le rôle des règles de revendication ](The-Role-of-Claim-Rules.md).  
  

