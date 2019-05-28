---
ms.assetid: 696a29b2-d627-4c9a-a384-9c8aaf50bd23
title: Déterminer le type de modèle de règle de revendication à utiliser
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f1d38e23d7f1671f729b03b7e6f8000471d2e9f9
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188647"
---
# <a name="determine-the-type-of-claim-rule-template-to-use"></a>Déterminer le type de modèle de règle de revendication à utiliser


Une partie importante de la conception d’un Active Directory Federation Services \(AD FS\) infrastructure consiste à déterminer l’ensemble complet de règles de revendication, et vous devez utiliser pour créer des modèles de règle de revendication les correspondants, pour chaque partenaire qui fera partie de la fédération avec votre organisation. Créer des règles à l’aide de modèles de règle de revendication dans le composant logiciel enfichable Gestion AD FS\-dans.  
  
Chaque ensemble de règles de revendication configuré peut uniquement être associé à une approbation fédérée. Cela signifie que vous ne pouvez pas créer d’ensemble de règles sur une approbation et l’utiliser pour d’autres approbations de votre service FS (Federation Service). Au lieu de cela, vous pouvez facilement créer des règles à partir de modèles de règles de revendication de manière à produire plus rapidement un ensemble de revendications souhaité convenu entre chaque partenaire fédéré et votre organisation.  
  
Pour plus d’informations sur les règles et les modèles de règle, voir [The Role of Claim Rules](The-Role-of-Claim-Rules.md).  
  
Avant de commencer à déterminer les types de modèles de règle de revendication que vous devez utiliser, posez-vous les questions suivantes :  
  
-   Quelles revendications seront fournies par vos fournisseurs de revendications approuvés ?  
  
-   Quelles revendications issues de quel fournisseur approuvez-vous ?  
  
-   Quelles sont les revendications requises par les parties de confiance qui approuvent ce service FS (Federation Service) ?  
  
-   Quelles revendications êtes-vous prêtes à divulguer à chaque partie de confiance ?  
  
-   Quels utilisateurs doivent avoir accès à chaque partie de confiance ?  
  
La réponse à ces questions vous aidera à planifier une conception de règle de revendication solide. Cela vous aidera également à créer une stratégie de contrôle d’accès et d’autorisation en continu et d’améliorer l’efficacité de votre équipe lors du déploiement.  
  
La section suivante vous permet de découvrir le type de modèles de règle à sélectionner pour votre environnement en fonction des besoins de votre entreprise.  
  
## <a name="claim-rule-template-types"></a>Types de modèles de règle de revendication  
Le tableau suivant décrit tous les types de modèles de règle de revendication que vous pouvez utiliser pour créer des règles à l’aide du composant logiciel enfichable Gestion AD FS\-dans et les avantages de l’utilisation d’un type de modèle plutôt qu’un autre.  
  
|Type de modèle de règle|Description|Avantages|Inconvénients|  
|----------------------|---------------|--------------|-----------------|  
|Passer ou filtrer une revendication entrante|Utilisé pour créer une règle qui transmet toutes les valeurs de revendication d’un type de revendication sélectionné ou filtre les revendications en fonction de leurs valeurs afin de ne transmettre que certaines valeurs de revendication pour un type de revendication sélectionné.<br /><br />Pour plus d'informations, voir [When to Use a Pass Through or Filter Claim Rule](When-to-Use-a-Pass-Through-or-Filter-Claim-Rule.md).|-Peut être utilisé pour sélectionner des revendications à accepter ou à émettre inchangé|-Type et la valeur ne peut pas être modifiés de revendication|  
|Transformer une revendication entrante|Utilisé pour créer une règle qui peut sélectionner une revendication entrante et la mapper à un type de revendication différent ou mapper sa valeur à une nouvelle valeur de revendication.<br /><br />Pour plus d'informations, voir [When to Use a Transform Claim Rule](When-to-Use-a-Transform-Claim-Rule.md).|-Peut être utilisé pour normaliser les valeurs ou les types de revendication<br />-Vous pouvez remplacer un e\-suffixe de messagerie d’une revendication entrante|-Remplacements de chaîne plus complexes nécessitent une règle personnalisée|  
|Envoyer les attributs LDAP en tant que revendications|Utilisé pour créer une règle qui sélectionne les attributs à partir d’un magasin d’attributs LDAP à envoyer en tant que revendications à la partie de confiance.<br /><br />Pour plus d'informations, voir [When to Use a Send LDAP Attributes as Claims Rule](When-to-Use-a-Send-LDAP-Attributes-as-Claims-Rule.md).|-Vous pouvez obtenir des revendications à partir d’un serveur AD DS\/magasin d’attributs AD LDS<br />-Plusieurs revendications peuvent être émises à l’aide d’une seule règle|-Ralentissement des performances en raison de la recherche de compte<br />-Impossible d’utiliser un filtre LDAP personnalisé pour l’interrogation|  
|Envoyer l’appartenance à un groupe en tant que revendication|Utilisé pour créer une règle qui peut envoyer une valeur et un type de revendication spécifié lorsqu’un utilisateur est membre d’un groupe de sécurité Active Directory. Une seule revendication sera envoyée à l’aide de cette règle, en fonction du groupe que vous sélectionnez.<br /><br />Pour plus d'informations, voir [When to Use a Send Group Membership as a Claim Rule](When-to-Use-a-Send-Group-Membership-as-a-Claim-Rule.md).|-Rapidité d’émission de revendications de groupe : aucune recherche de compte|-Utilisateur doit être un membre d’un groupe Active Directory local|  
|Envoyer des revendications à l’aide d’une règle personnalisée|Utilisé pour créer une règle personnalisée qui offre des options plus avancées qu’un modèle de règle standard. Vous écrivez des règles personnalisées avec les services AD FS de langage de règle de revendication.<br /><br />Pour plus d'informations, voir [When to Use a Custom Claim Rule](When-to-Use-a-Custom-Claim-Rule.md).|-Peut être utilisé pour obtenir des revendications à partir d’un magasin d’attributs SQL<br />-Peut être utilisé pour spécifier un filtre LDAP personnalisé<br />-Peut être utilisé pour émettre un PPID<br />-Peut être utilisé avec un magasin d’attributs personnalisés<br />-Peut être utilisé pour ajouter des revendications uniquement à l’ensemble de revendications d’entrée<br />-Peut être utilisé pour envoyer des revendications basées sur plus d’une revendication entrante|-Plus difficile à configurer \- des temps d’accélération peut être nécessaire d’initialement prendre connaissance du langage de règle de revendication|  
|Autoriser ou refuser des utilisateurs en fonction d'une revendication entrante|Utilisé pour créer une règle qui autorise ou refuse l’accès à la partie de confiance aux utilisateurs, selon le type et la valeur d’une revendication entrante.<br /><br />Pour plus d'informations, voir [When to Use an Authorization Claim Rule](When-to-Use-an-Authorization-Claim-Rule.md).|-Simplifie le processus d’autorisation|-Nécessite qu’une seule revendication et une autre valeur de revendication être spécifié<br />-Ne prend pas en charge les critères spéciaux pour les valeurs de revendication|  
|Autoriser tous les utilisateurs|Utilisé pour créer une règle qui autorise tous les utilisateurs à accéder à la partie de confiance.<br /><br />Pour plus d'informations, voir [When to Use an Authorization Claim Rule](When-to-Use-an-Authorization-Claim-Rule.md).|-Facile à configurer|-Moins sécurisé que l’utilisation de l’autoriser ou refuser les utilisateurs basés sur un modèle de revendication entrante|  
  

