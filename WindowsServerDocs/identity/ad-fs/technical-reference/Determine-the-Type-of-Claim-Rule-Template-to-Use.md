---
ms.assetid: 696a29b2-d627-4c9a-a384-9c8aaf50bd23
title: "Déterminer le Type de modèle de règle de revendication à utiliser"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 129cd83be4cd8302bd170ba87aad58c50f636006
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="determine-the-type-of-claim-rule-template-to-use"></a>Déterminer le Type de modèle de règle de revendication à utiliser

>S’applique à: Windows Server2016, Windows Server2012R2

Une partie importante de la conception d’une infrastructure de \(ADFS\) ActiveDirectory Federation Services consiste à déterminer l’ensemble complet des règles de revendication, et vous devez utiliser pour créer des modèles de règle de revendication qui correspondants, pour chaque partenaire participant à la fédération avec votre organisation. Pour créer des règles à l’aide de modèles de règle de revendication dans la gestion ADFS de composants.  
  
Chaque ensemble de règles de revendication que vous configurez peut uniquement être associé à une approbation fédérée. Cela signifie que vous ne pouvez pas créer un ensemble de règles sur une approbation et les utiliser pour d’autres approbations dans votre Service de fédération. Au lieu de cela vous pouvez facilement créer des règles de revendication modèles de règle pour aider plus rapidement à produire un ensemble de revendications convenu entre chaque partenaire fédéré et votre organisation souhaité.  
  
Pour plus d’informations sur les règles et les modèles de règle, voir [The Role of Claim Rules](The-Role-of-Claim-Rules.md).  
  
Avant de commencer à déterminer les types de modèles de règle de revendication que vous devez utiliser, posez-vous les questions suivantes:  
  
-   Quelles revendications seront fournies par vos fournisseurs de revendications approuvés?  
  
-   Les revendications à partir de chaque fournisseur de revendications approuvez-vous?  
  
-   Les revendications sont requis par les parties de confiance qui approuvent ce Service de fédération?  
  
-   Quelles revendications êtes-vous prêtes à divulguer à chaque partie de confiance?  
  
-   Les utilisateurs doivent avoir accès à chaque partie de confiance?  
  
Ces questions sera de conception de règle revendication vous aidera à planifier un solide. Il sera également vous aider à créer une autorisation fluide et stratégie de contrôle d’accès et votre équipe de déploiement plus efficace lors du déploiement.  
  
Dans la section suivante que vous pouvez en savoir plus sur le type de modèles de règle à sélectionner pour votre environnement en fonction de votre entreprise a besoin.  
  
## <a name="claim-rule-template-types"></a>Types de modèles de règle de revendication  
Le tableau suivant décrit tous les types de modèles de règle de revendication qui vous permet de créer des règles à l’aide de la gestion ADFS dans composants, et les avantages de l’utilisation d’un modèle de type sur un autre.  
  
|Type de modèle de règle|Description|Avantages|Inconvénients|  
|----------------------|---------------|--------------|-----------------|  
|Passer ou filtrer une revendication entrante|Utilisé pour créer une règle qui passent par toutes les valeurs de revendication pour un type de revendication sélectionné ou filtrer des revendications basées sur les valeurs de revendication afin que seules certaines valeurs de revendication pour un type de revendication sélectionné transmettre.<br /><br />Pour plus d’informations, voir [quand utiliser une transmission ou règle de revendication filtre](When-to-Use-a-Pass-Through-or-Filter-Claim-Rule.md).|-Peut être utilisé pour sélectionner des revendications à accepter ou à émettre sans modification|-Type et la valeur ne peut pas être modifiés de revendication|  
|Transformer une revendication entrante|Utilisé pour créer une règle qui peut sélectionner une revendication entrante et le mapper à un type de revendication différent ou mapper sa valeur sur une nouvelle valeur de revendication.<br /><br />Pour plus d’informations, voir [When to Use a Transform Claim Rule](When-to-Use-a-Transform-Claim-Rule.md).|-Peut être utilisé pour normaliser les types de revendications ou de valeurs<br />-Peut remplacer un suffixe de messagerie e\ d’une revendication entrante|-Les remplacements de chaîne plus complexes nécessitent une règle personnalisée|  
|Envoyer les attributs LDAP en tant que revendications|Utilisé pour créer une règle qui sélectionne les attributs à partir d’un magasin d’attributs LDAP à envoyer en tant que revendications à la partie de confiance.<br /><br />Pour plus d’informations, voir [quand utiliser un envoyer les attributs LDAP en tant que revendications règle](When-to-Use-a-Send-LDAP-Attributes-as-Claims-Rule.md).|-Peut obtenir des revendications à partir de n’importe quel magasin d’attributs ADDS\/ADLDS<br />-Plusieurs revendications peuvent être émises à l’aide d’une règle unique|-Ralentissement des performances à la suite de recherche de compte<br />-Impossible d’utiliser un filtre LDAP personnalisé pour l’interrogation|  
|Envoyer l’appartenance au groupe sous la forme d’une revendication|Utilisé pour créer une règle qui peut envoyer une valeur et le type de revendication spécifié lorsqu’un utilisateur est membre d’un groupe de sécurité ActiveDirectory. Une seule revendication sera envoyée à l’aide de cette règle, en fonction du groupe que vous sélectionnez.<br /><br />Pour plus d’informations, voir [quand utiliser un envoyer l’appartenance comme une règle de revendication](When-to-Use-a-Send-Group-Membership-as-a-Claim-Rule.md).|-Rapidité d’émission des revendications de groupe: aucune recherche de compte|-Utilisateur doit être membre d’un groupe ActiveDirectory local|  
|Envoyer des revendications à l’aide d’une règle personnalisée|Utilisé pour créer une règle personnalisée qui fournit des options plus avancées qu’un modèle de règle standard. Vous écrivez des règles personnalisées avec les services ADFS de langage de règle de revendication.<br /><br />Pour plus d’informations, voir [quand utiliser une règle de revendication personnalisée](When-to-Use-a-Custom-Claim-Rule.md).|-Peut être utilisé pour obtenir des revendications à partir d’un magasin d’attributs SQL<br />-Peut être utilisée pour spécifier un filtre LDAP personnalisé<br />-Peut être utilisé pour émettre un PPID<br />-Peut être utilisé avec un magasin d’attributs personnalisés<br />-Peut être utilisé pour ajouter des revendications uniquement à l’ensemble de revendications d’entrée<br />-Peut être utilisé pour envoyer des revendications basées sur plusieurs revendications entrantes|-Plus difficile à configurer \-certains temps d’accélération peuvent être nécessaires pour prendre connaissance de la langue de règle de revendication,|  
|Autoriser ou refuser des utilisateurs en fonction d’une revendication entrante|Utilisé pour créer une règle qui autorise ou refuse l’accès des utilisateurs à la partie de confiance, selon le type et la valeur d’une revendication entrante.<br /><br />Pour plus d’informations, voir [quand utiliser une règle de revendication d’autorisation](When-to-Use-an-Authorization-Claim-Rule.md).|-Simplifie le processus d’autorisation|-Nécessite que le type de revendication qu’une seule et une valeur de revendication être spécifié<br />-Ne prend pas en charge les critères spéciaux pour les valeurs de revendication|  
|Autoriser tous les utilisateurs|Utilisé pour créer une règle qui autorise tous les utilisateurs à accéder à la partie de confiance.<br /><br />Pour plus d’informations, voir [quand utiliser une règle de revendication d’autorisation](When-to-Use-an-Authorization-Claim-Rule.md).|-Simple à configurer|-Moins sécurisée que l’utilisation de l’autoriser ou refuser les utilisateurs en fonction sur un modèle de revendication entrante|  
  

