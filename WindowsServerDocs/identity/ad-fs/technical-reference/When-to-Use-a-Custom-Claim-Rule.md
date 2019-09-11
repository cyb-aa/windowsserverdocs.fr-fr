---
ms.assetid: 20d183f0-ef94-44bb-9dfc-ed93799dd1a6
title: Quand utiliser une règle de revendication personnalisée
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 1a3f3e711d8e8443eb80109245eef42c668353d9
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70869288"
---
# <a name="when-to-use-a-custom-claim-rule"></a>Quand utiliser une règle de revendication personnalisée
Vous écrivez une règle de revendication personnalisée dans \(services ADFS\) AD FS à l’aide du langage de règle de revendication, qui est l’infrastructure que le moteur d’émission de revendications utilise pour générer, transformer, transmettre et filtrer par programmation légitimité. L’utilisation d’une règle personnalisée permet de créer des règles avec une logique plus complexe qu’un modèle de règle standard. Envisagez d’utiliser une règle personnalisée lorsque vous souhaitez :  
  
-   Envoyer des revendications basées sur les valeurs extraites d’un \(magasin\) d’attributs SQL langage SQL.  
  
-   Envoyer des revendications en fonction des valeurs extraites d’un \(magasin d’attributs LDAP LDAP\) à l’aide d’un filtre LDAP personnalisé.  
  
-   Envoyer des revendications basées sur les valeurs extraites d’un magasin d’attributs personnalisés.  
  
-   Envoyer des revendications uniquement en présence de deux ou plusieurs revendications entrantes.  
  
-   Envoyer des revendications uniquement lorsqu’une valeur de revendication entrante correspond à un modèle complexe.  
  
-   Envoyer des revendications avec des modifications complexes apportées à une valeur de revendication entrante.  
  
-   Créer des revendications destinées à être utilisées uniquement dans des règles ultérieures sans réellement envoyer les revendications.  
  
-   Élaborer une revendication sortante à partir du contenu de plusieurs revendications entrantes.  
  
Vous pouvez également utiliser une règle personnalisée lorsque la valeur de revendication de la revendication sortante doit être basée sur la valeur de la revendication entrante. Elle doit également inclure du contenu supplémentaire.  
  
Le langage des règles de revendication repose sur des règles. Il est composé d’une partie relative aux conditions et d’une autre relative à l’exécution. Vous pouvez utiliser la syntaxe du langage des règles de revendication pour énumérer, ajouter, supprimer ou modifier des revendications afin de répondre aux besoins de votre organisation. Pour plus d’informations sur le fonctionnement de chacune de ces parties, consultez [le rôle du langage de règle de revendication](The-Role-of-the-Claim-Rule-Language.md).  
  
Les sections suivantes présentent brièvement les règles de revendication. Elles indiquent également à quel moment utiliser une règle de revendication personnalisée.  
  
## <a name="about-claim-rules"></a>À propos des règles de revendication  
Une règle de revendication représente une instance de logique métier qui prend une revendication entrante, lui applique une \(condition si x, alors\) y et génère une revendication sortante basée sur les paramètres de condition.  
  
> [!IMPORTANT]  
> -   Dans le composant logiciel enfichable\-gestion des AD FS, les règles de revendication ne peuvent être créées qu’à l’aide de modèles de règle de revendication  
> -   Les règles de revendication traitent les revendications entrantes soit \(directement à partir d’un fournisseur\) de revendications, par exemple Active Directory ou une autre service FS (Federation Service), soit à partir de la sortie des règles de transformation d’acceptation sur une approbation de fournisseur de revendications.  
> -   Les règles de revendication sont traitées par le moteur d’émission des revendications au sein d’un ensemble de règles donné et dans l’ordre chronologique. En définissant la hiérarchie des règles, vous pouvez affiner ou filtrer les revendications qui sont générées par les règles précédentes au sein d’un ensemble de règles donné.  
> -   Les modèles de règle de revendication nécessitent toujours de spécifier un type de revendication entrante. Toutefois, vous pouvez traiter plusieurs valeurs de revendication avec le même type de revendication, en vous appuyant sur une règle unique.  
  
Pour plus d’informations sur les règles de revendication et les ensembles de règles de revendication, consultez [rôle des règles de revendication](The-Role-of-Claim-Rules.md). Pour plus d’informations sur la façon dont les règles sont traitées, consultez [le rôle du moteur de revendications](The-Role-of-the-Claims-Engine.md). Pour plus d’informations sur le traitement des ensembles de règles de revendication, consultez [le rôle du pipeline de revendications](The-Role-of-the-Claims-Pipeline.md).  
  
## <a name="how-to-create-this-rule"></a>Comment créer cette règle  
Pour créer cette règle, commencez par créer la syntaxe dont vous avez besoin pour votre opération à l’aide du langage de règle de revendication, puis collez le résultat dans la zone de texte fournie dans le modèle envoyer une revendication à l’aide d’une règle personnalisée sur les propriétés d’un fournisseur de revendications TR. UST ou une approbation de partie de confiance dans le composant logiciel\-enfichable de gestion AD FS.  
  
Ce modèle de règle propose les options suivantes :  
  
-   Spécifier un nom de règle de revendication  
  
-   Tapez une ou plusieurs conditions facultatives et une instruction d’émission à l’aide du langage de règle de revendication AD FS  
  
Pour plus d’instructions sur la création d’une règle personnalisée à l’aide de ce modèle, consultez [créer une règle pour envoyer des revendications à l’aide d’une règle personnalisée](https://technet.microsoft.com/library/dd807049.aspx) dans le Guide de déploiement AD FS.  
  
Pour mieux comprendre le fonctionnement du langage de règle de revendication, consultez la syntaxe du langage de règle de revendication des autres règles qui existent déjà\-dans le composant logiciel enfichable en cliquant sur l’onglet **afficher le langage de règle** dans les propriétés de cette règle. L’utilisation des informations contenues dans cette section et des informations de syntaxe de cet onglet peuvent indiquer comment construire vos propres règles personnalisées.  
  
Pour plus d’informations sur l’utilisation du langage de règle de revendication, consultez [le rôle du langage de règle de revendication](The-Role-of-the-Claim-Rule-Language.md).  
  
## <a name="using-the-claim-rule-language"></a>Utilisation du langage des règles de revendication  
  
### <a name="example-how-to-combine-first-and-last-names-based-on-a-users-name-attribute-values"></a>Exemple : Comment combiner le prénom et le nom en fonction des valeurs d’attribut Name d’un utilisateur  
La syntaxe de règle suivante combine les prénoms et les noms des valeurs d’attribut dans un magasin d’attributs donné. Le moteur de stratégie constitue un produit cartésien des correspondances pour chaque condition. Par exemple, le résultat pour le prénom {« Frank », « Alan »} et les noms {« Miller », « Shen »} est {« Frank Miller », « Frank Shen », « Alan Miller », « Alan Shen »} :  
  
```  
c1:[type == "http://exampleschema/firstname" ]  
&&  c2:[type == "http://exampleschema/lastname",]   
=> issue(type = "http://exampleschema/name", value = c1.value + “  “ + c2.value);  
```  
  
### <a name="example-how-to-issue-a-manager-claim-based-on-whether-users-have-direct-reports"></a>Exemple : Comment émettre une revendication de gestionnaire basée sur le fait que des utilisateurs disposent de rapports directs  
La règle suivante émet une revendication de gestionnaire uniquement si l’utilisateur a des rapports directs :  
  
```  
c:[type == "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name"] => add(store = "SQL Store", types = ("http://schemas.xmlsoap.org/claims/Reports"), query = "SELECT Reports FROM dbo.DirectReports WHERE UserName = {0}", param = c.value );  
count([type == “http://schemas.xmlsoap.org/claims/Reports“] ) > 0 => issue(= "http://schemas.xmlsoap.org/claims/ismanager", value = "true");  
```  
  
### <a name="example-how-to-issue-a-ppid-claim-based-on-an-ldap-attribute"></a>Exemple : Comment émettre une revendication PPID (Private Personal Identifier) basée sur un attribut LDAP  
La règle suivante émet une \(revendication PPID\) d’identificateur personnel privé basée sur les attributs **attributs WindowsAccountName** et **OriginalIssuer** des utilisateurs dans un magasin d’attributs LDAP :  
  
```  
c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]  
 => issue(store = "_OpaqueIdStore", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/privatepersonalidentifier"), query = "{0};{1};{2}", param = "ppid", param = c.Value, param = c.OriginalIssuer);  
```  
  
Les attributs communs qui peuvent être utilisés pour identifier l’utilisateur pour cette requête sont les suivants :  
  
-   **SID de l’utilisateur**  
  
-   **attributs WindowsAccountName**  
  
-   **sAMAccountName**  
  

