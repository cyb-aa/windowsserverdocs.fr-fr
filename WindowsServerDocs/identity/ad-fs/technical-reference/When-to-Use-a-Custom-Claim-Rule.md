---
ms.assetid: 20d183f0-ef94-44bb-9dfc-ed93799dd1a6
title: "Quand utiliser une règle de revendication personnalisée"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f5398f0b10d9e62548145fdde0354a3d047eb0ae
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

# <a name="when-to-use-a-custom-claim-rule"></a>Quand utiliser une règle de revendication personnalisée
Vous écrivez une règle de revendication personnalisée dans \(AD FS\) Active Directory Federation Services à l’aide du langage de règle de revendication, qui est l’infrastructure par le moteur d’émission des revendications pour générer, transformer, transmettre par programme et filtrez les revendications. En utilisant une règle personnalisée, vous pouvez créer des règles avec une logique plus complexe qu’un modèle de règle standard. Envisagez d’utiliser une règle personnalisée lorsque vous souhaitez:  
  
-   Envoyer des revendications basées sur les valeurs extraites d’un magasin d’attributs \(SQL\) Structured Query Language.  
  
-   Envoyer des revendications basées sur les valeurs extraites d’un magasin d’attributs Lightweight Directory Access Protocol \(LDAP\) à l’aide d’un filtre LDAP personnalisé.  
  
-   Envoyer des revendications basées sur les valeurs extraites d’un magasin d’attributs personnalisés.  
  
-   Envoyer des revendications uniquement lorsque deux ou plusieurs revendications entrantes sont présentes.  
  
-   Envoyer des revendications uniquement lorsqu’une revendication entrante correspond à valeur un modèle complexe.  
  
-   Envoyer des revendications avec des modifications complexes apportées à une entrée valeur de revendication.  
  
-   Créer des revendications pour une utilisation dans des règles ultérieures sans réellement envoyer les revendications.  
  
-   Élaborer une revendication sortante à partir du contenu de plusieurs revendications entrantes.  
  
Vous pouvez également utiliser une règle personnalisée lorsque la valeur de revendication de la revendication sortante doit être basée sur la valeur de la revendication entrante, mais il doit également inclure du contenu supplémentaire.  
  
Le langage des règles de revendications est repose sur des règles. Il a une partie de la condition et une partie de l’exécution. Vous pouvez utiliser la syntaxe du langage de règle revendication pour énumérer, ajouter, supprimer ou modifier des revendications pour répondre aux besoins de votre organisation. Pour plus d’informations sur la façon dont chacune de ces parties, voir [The Role of the Claim Rule Language](The-Role-of-the-Claim-Rule-Language.md).  
  
Les sections suivantes fournissent une introduction aux règles de revendication. Elles expliquent également quand utiliser une règle de revendication personnalisée.  
  
## <a name="about-claim-rules"></a>À propos des règles de revendication  
Une règle de revendication représente une instance de logique métier qui prend une revendication entrante, lui appliquer une condition \ (s’il x, puis y\) et génère une revendication sortante basée sur les paramètres de condition.  
  
> [!IMPORTANT]  
> -   Dans la gestion AD FS de composants, de revendications règles peuvent être créées uniquement à l’aide de modèles de règle de revendication  
> -   Processus de règles de revendication entrante de revendications directement à partir d’un fournisseur de revendications \ (par exemple, Active Directory ou un autre fédération Service\) ou de la sortie de l’acceptation de règles de transformation sur une approbation de fournisseur de revendications.  
> -   Les règles de revendication sont traitées par le moteur d’émission des revendications dans l’ordre chronologique, dans un ensemble de règles donné. En définissant les règles de priorité, vous pouvez affiner ou filtrer les revendications qui sont générées par les règles précédentes au sein d’un ensemble de règles donné.  
> -   Modèles de règle de revendication nécessitent toujours de spécifier un type de revendication entrante. Toutefois, vous pouvez traiter plusieurs valeurs de revendication avec le même type de revendication à l’aide d’une règle unique.  
  
Pour plus d’informations sur les règles de revendication et les jeux de règles de revendication, consultez [le rôle des règles de revendication](The-Role-of-Claim-Rules.md). Pour plus d’informations sur le traitement des règles, voir [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md). Pour plus d’informations, mode de traitement des jeux de règles de revendication, voir [The Role of the Claims Pipeline](The-Role-of-the-Claims-Pipeline.md).  
  
## <a name="how-to-create-this-rule"></a>Comment créer cette règle  
Vous créez cette règle en commençant par créer la syntaxe que vous avez besoin pour votre opération à l’aide du langage de règle de revendication et coller le résultat dans la zone de texte qui est fournie dans l’envoyer une revendication à l’aide d’un modèle de règle personnalisée sur les propriétés d’un fournisseur de revendications approbation ou une confiance dans la gestion AD FS de composants.  
  
Ce modèle de règle fournit les options suivantes:  
  
-   Spécifiez un nom de règle de revendication  
  
-   Entrez une ou plusieurs conditions facultatives et une instruction d’émission à l’aide de l’ADFS de langage de règle de revendication  
  
Pour plus d’instructions pour la création d’une règle personnalisée à l’aide de ce modèle, voir [créer une règle pour envoyer revendications en utilisant une règle personnalisée](https://technet.microsoft.com/library/dd807049.aspx) dans le Guide de déploiement d’AD FS.  
  
Pour mieux comprendre comment fonctionne le langage de règle de revendication, afficher la syntaxe du langage de règle revendication d’autres règles qui existent déjà dans le composant logiciel enfichable en cliquant sur le **afficher le langage de règle** onglet dans les propriétés de la règle. À l’aide les informations contenues dans cette section et les informations de syntaxe de cet onglet peut indiquer comment construire vos propres règles personnalisées.  
  
Pour plus d’informations sur l’utilisation du langage de règle de revendication, voir [le rôle du langage de règle de revendication](The-Role-of-the-Claim-Rule-Language.md).  
  
## <a name="using-the-claim-rule-language"></a>À l’aide du langage de règle de revendication  
  
### <a name="example-how-to-combine-first-and-last-names-based-on-a-users-name-attribute-values"></a>Exemple: Comment combiner des noms et de prénoms selon les valeurs d’attribut nom d’un utilisateur  
La syntaxe de règle suivante combine les prénoms et les noms de valeurs d’attribut dans un magasin d’attributs donné. Le moteur de stratégie constitue un produit cartésien des correspondances pour chaque condition. Par exemple, la sortie pour le prénom {«Frank», «Alan»} et les noms {«Miller», «Shen»} est {«Frank Miller», «Frank Shen», «Alan Miller», «Alan Shen»}:  
  
```  
c1:[type == "http://exampleschema/firstname" ]  
&&  c2:[type == "http://exampleschema/lastname",]   
=> issue(type = "http://exampleschema/name", value = c1.value + “  “ + c2.value);  
```  
  
### <a name="example-how-to-issue-a-manager-claim-based-on-whether-users-have-direct-reports"></a>Exemple: Comment émettre une revendication de gestionnaire basée sur indique si les utilisateurs disposent de rapports directs  
La règle suivante émet une revendication de gestionnaire uniquement si l’utilisateur a des rapports directs:  
  
```  
c:[type == "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name"] => add(store = "SQL Store", types = ("http://schemas.xmlsoap.org/claims/Reports"), query = "SELECT Reports FROM dbo.DirectReports WHERE UserName = {0}", param = c.value );  
count([type == “http://schemas.xmlsoap.org/claims/Reports“] ) > 0 => issue(= "http://schemas.xmlsoap.org/claims/ismanager", value = "true");  
```  
  
### <a name="example-how-to-issue-a-ppid-claim-based-on-an-ldap-attribute"></a>Exemple: Comment émettre une revendication PPID basée sur un attribut LDAP  
La règle suivante émet une revendication d’identificateur du personnel privé \(PPID\) basée sur la **windowsaccountname** et **originalissuer** les attributs des utilisateurs dans un magasin d’attributs LDAP:  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]  
 => issue(store = "_OpaqueIdStore", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/privatepersonalidentifier"), query = "{0};{1};{2}", param = "ppid", param = c.Value, param = c.OriginalIssuer);  
```  
  
Des attributs communs qui peuvent être utilisés pour identifier l’utilisateur pour cette requête sont les suivantes:  
  
-   **SID de l’utilisateur**  
  
-   **windowsaccountname**  
  
-   **sAMAccountName**  
  

