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
ms.openlocfilehash: b08622cd9eefa153e2fe8403fbe85077644182f4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813110"
---
>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="when-to-use-a-custom-claim-rule"></a>Quand utiliser une règle de revendication personnalisée
Vous écrivez une règle de revendication personnalisée dans Active Directory Federation Services \(AD FS\) en utilisant le langage de règle de revendication, qui est le framework que le moteur d’émission de revendications utilise pour générer par programmation, transformer, passer et filtrer revendications. L’utilisation d’une règle personnalisée permet de créer des règles avec une logique plus complexe qu’un modèle de règle standard. Envisagez d’utiliser une règle personnalisée lorsque vous souhaitez :  
  
-   Envoyer des revendications basées sur les valeurs sont extraites à partir d’un langage de requête structuré \(SQL\) magasin d’attributs.  
  
-   Envoyer des revendications basées sur les valeurs sont extraites à partir d’un protocole Lightweight Directory Access \(LDAP\) magasin d’attributs à l’aide d’un filtre LDAP personnalisé.  
  
-   Envoyer des revendications basées sur les valeurs extraites d’un magasin d’attributs personnalisés.  
  
-   Envoyer des revendications uniquement en présence de deux ou plusieurs revendications entrantes.  
  
-   Envoyer des revendications uniquement lorsqu’une valeur de revendication entrante correspond à un modèle complexe.  
  
-   Envoyer des revendications avec des modifications complexes apportées à une valeur de revendication entrante.  
  
-   Créer des revendications destinées à être utilisées uniquement dans des règles ultérieures sans réellement envoyer les revendications.  
  
-   Élaborer une revendication sortante à partir du contenu de plusieurs revendications entrantes.  
  
Vous pouvez également utiliser une règle personnalisée lorsque la valeur de revendication de la revendication sortante doit être basée sur la valeur de la revendication entrante. Elle doit également inclure du contenu supplémentaire.  
  
Le langage des règles de revendication repose sur des règles. Il est composé d’une partie relative aux conditions et d’une autre relative à l’exécution. Vous pouvez utiliser la syntaxe du langage des règles de revendication pour énumérer, ajouter, supprimer ou modifier des revendications afin de répondre aux besoins de votre organisation. Pour plus d’informations sur la façon dont chacune de ces parties fonctionne, consultez [The Role of the Claim Rule Language](The-Role-of-the-Claim-Rule-Language.md).  
  
Les sections suivantes présentent brièvement les règles de revendication. Elles indiquent également à quel moment utiliser une règle de revendication personnalisée.  
  
## <a name="about-claim-rules"></a>À propos des règles de revendication  
Une règle de revendication représente une instance de logique métier qui prend une revendication entrante, lui appliquer une condition \(if x, puis y\) et génère une revendication sortante selon les paramètres de condition.  
  
> [!IMPORTANT]  
> -   Dans le composant logiciel enfichable Gestion AD FS\-, revendication règles peuvent être créées qu’à l’aide de modèles de règle de revendication  
> -   Processus de règles de revendication entrante de revendications directement à partir d’un fournisseur de revendications \(comme Active Directory ou un autre Service de fédération\) ou à partir de la sortie de l’acceptation des règles sur une approbation de fournisseur de revendications de transformation.  
> -   Les règles de revendication sont traitées par le moteur d’émission des revendications au sein d’un ensemble de règles donné et dans l’ordre chronologique. En définissant la hiérarchie des règles, vous pouvez affiner ou filtrer les revendications qui sont générées par les règles précédentes au sein d’un ensemble de règles donné.  
> -   Les modèles de règle de revendication nécessitent toujours de spécifier un type de revendication entrante. Toutefois, vous pouvez traiter plusieurs valeurs de revendication avec le même type de revendication, en vous appuyant sur une règle unique.  
  
Pour plus d’informations sur les règles de revendication et des ensembles de règles de revendication, consultez [le rôle des règles de revendication](The-Role-of-Claim-Rules.md). Pour plus d’informations sur le traitement des règles, consultez [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md). Pour plus d’informations mode de traitement des ensembles de règles de revendication, consultez [The Role of the Claims Pipeline](The-Role-of-the-Claims-Pipeline.md).  
  
## <a name="how-to-create-this-rule"></a>Comment créer cette règle  
Vous créez cette règle en commençant par créer la syntaxe dont vous avez besoin pour votre opération en utilisant le langage de règle de revendication et le collage puis le résultat dans le texte de zone qui est fournie à l’envoi les revendications à l’aide un modèle de règle personnalisée sur les propriétés d’un fournisseur de revendications tr USt ou une partie de confiance approuver dans le composant logiciel enfichable Gestion AD FS\-dans.  
  
Ce modèle de règle propose les options suivantes :  
  
-   Spécifier un nom de règle de revendication  
  
-   Entrez une ou plusieurs conditions facultatives et une instruction d’émission à l’aide d’AD FS de langage de règle de revendication  
  
Pour plus d’instructions pour la création d’une règle personnalisée à l’aide de ce modèle, consultez [créer une règle pour envoyer des revendications à l’aide une règle personnalisée](https://technet.microsoft.com/library/dd807049.aspx) dans le Guide de déploiement d’AD FS.  
  
Pour une meilleure compréhension du fonctionne du langage de règle de revendication, affichez la syntaxe de langage de règle de revendication d’autres règles qui existent déjà dans le composant logiciel enfichable\-dans en cliquant sur le **afficher le langage de règle** onglet dans les propriétés de la règle. L’utilisation des informations contenues dans cette section et des informations de syntaxe de cet onglet peuvent indiquer comment construire vos propres règles personnalisées.  
  
Pour plus d’informations sur l’utilisation du langage de règle de revendication, consultez [The Role of du langage des règles de revendication](The-Role-of-the-Claim-Rule-Language.md).  
  
## <a name="using-the-claim-rule-language"></a>Utilisation du langage des règles de revendication  
  
### <a name="example-how-to-combine-first-and-last-names-based-on-a-users-name-attribute-values"></a>Exemple : Combinaison de noms et de prénoms selon les valeurs d’attribut d’un nom d’utilisateur  
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
La règle suivante émet un identificateur de personnel privé \(PPID\) revendication selon le **windowsaccountname** et **originalissuer** attributs d’utilisateurs dans un attribut LDAP boutique :  
  
```  
c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]  
 => issue(store = "_OpaqueIdStore", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/privatepersonalidentifier"), query = "{0};{1};{2}", param = "ppid", param = c.Value, param = c.OriginalIssuer);  
```  
  
Les attributs communs qui peuvent être utilisés pour identifier l’utilisateur pour cette requête sont les suivants :  
  
-   **SID de l’utilisateur**  
  
-   **windowsaccountname**  
  
-   **samaccountname**  
  

