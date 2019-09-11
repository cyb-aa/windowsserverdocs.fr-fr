---
ms.assetid: 606f4196-b579-4806-a462-3abd4d93e87c
title: Quand utiliser une règle Envoyer les attributs LDAP en tant que revendications
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: a1210918067bbb71ea26dff4db11561bbeb09dbd
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70869247"
---
# <a name="when-to-use-a-send-ldap-attributes-as-claims-rule"></a>Quand utiliser une règle Envoyer les attributs LDAP en tant que revendications
Vous pouvez utiliser cette règle dans services ADFS \(AD FS\) lorsque vous souhaitez émettre des revendications sortantes contenant des valeurs d’attribut \(LDAP LDAP\) réelles qui existent dans un magasin d’attributs, puis associez un type de revendication à chacun des attributs LDAP. Pour plus d’informations sur les magasins d’attributs, consultez [rôle des magasins d’attributs](The-Role-of-Attribute-Stores.md).  
  
Lorsque vous utilisez cette règle, vous émettez une revendication pour chaque attribut LDAP que vous spécifiez et qui correspond à la logique de la règle, comme décrit dans le tableau suivant.  
  
|Option de règle|Logique de règle|  
|---------------|--------------|  
|Mappage d’attributs LDAP à des types de revendications sortantes|Si le magasin d'attributs est le *magasin d'attributs spécifié* et l'attribut LDAP est égal à la *valeur spécifiée*, mappez la valeur de l'attribut LDAP au type de *revendication sortante spécifiée*, puis émettez la revendication.|  
  
Les sections suivantes présentent brièvement les règles de revendication. Elles expliquent également quand utiliser la règle Envoyer les attributs LDAP en tant que revendications.  
  
## <a name="about-claim-rules"></a>À propos des règles de revendication  
Une règle de revendication représente une instance de logique métier qui prend une revendication entrante, lui applique une condition \(si x Then y\) et génère une revendication sortante basée sur les paramètres de condition. La liste suivante présente d’importantes astuces sur les règles de revendication dont vous devez prendre connaissance avant de poursuivre la lecture de cette rubrique :  
  
-   Dans le composant logiciel enfichable\-gestion des AD FS, les règles de revendication ne peuvent être créées qu’à l’aide de modèles de règles de revendication  
  
-   Les règles de revendication traitent les revendications entrantes soit \(directement à partir d’un fournisseur\) de revendications, par exemple Active Directory ou une autre service FS (Federation Service), soit à partir de la sortie des règles de transformation d’acceptation sur une approbation de fournisseur de revendications.  
  
-   Les règles de revendication sont traitées par le moteur d’émission des revendications au sein d’un ensemble de règles donné et dans l’ordre chronologique. En définissant la hiérarchie des règles, vous pouvez affiner ou filtrer des revendications générées par des règles précédentes dans un ensemble de règles donné.  
  
-   Les modèles de règles de revendication vous obligent toujours à spécifier un type de revendication entrante. Toutefois, vous pouvez traiter plusieurs valeurs de revendication avec le même type de revendication en vous appuyant sur une règle unique.  
  
Pour plus d’informations sur les règles de revendication et les ensembles de règles de revendication, consultez [rôle des règles de revendication](The-Role-of-Claim-Rules.md). Pour plus d’informations sur la façon dont les règles sont traitées, consultez [le rôle du moteur de revendications](The-Role-of-the-Claims-Engine.md). Pour plus d’informations sur le traitement des ensembles de règles de revendication, consultez [le rôle du pipeline de revendications](The-Role-of-the-Claims-Pipeline.md).  
  
## <a name="mapping-of-ldap-attributes-to-outgoing-claim-types"></a>Mappage d’attributs LDAP à des types de revendications sortantes  
Lorsque vous utilisez le modèle de règle envoyer les attributs LDAP en tant que revendications, vous pouvez sélectionner des attributs à partir d’un magasin d' \(attributs\) LDAP, par exemple Active Directory ou Active Directory Domain Services AD DS pour envoyer leurs valeurs en tant que revendications à la partie de confiance. . Cette opération mappe certains attributs LDAP d’un magasin d’attributs que vous définissez à un ensemble de revendications sortantes que vous pouvez utiliser pour l'autorisation.  
  
Avec ce modèle, vous pouvez ajouter plusieurs attributs qui seront envoyés en tant que revendications multiples, à partir d'une seule règle. Par exemple, vous pouvez utiliser ce modèle pour créer une règle qui va rechercher des valeurs d’attribut pour des utilisateurs authentifiés dans les attributs Active Directory **company** et **department**, puis envoyer ces valeurs sous la forme de deux revendications sortantes.  
  
Vous pouvez également utiliser cette règle pour envoyer toutes les appartenances aux groupes de l’utilisateur. Si vous souhaitez n’envoyer qu’une appartenance à un groupe, utilisez le modèle de règle Envoyer l’appartenance au groupe en tant que revendication. Pour plus d’informations, consultez [quand utiliser un envoyer l’appartenance comme une règle de revendication](When-to-Use-a-Send-Group-Membership-as-a-Claim-Rule.md).  
  
## <a name="how-to-create-this-rule"></a>Comment créer cette règle  
Vous pouvez créer cette règle à l’aide du langage de règle de revendication ou à l’aide du modèle de règle envoyer les attributs LDAP en tant\-que revendications dans le composant logiciel enfichable Gestion des AD FS. Ce modèle de règle fournit les options de configuration suivantes :  
  
-   Spécifier un nom de règle de revendication  
  
-   Sélectionner un magasin d'attributs duquel extraire les attributs LDAP  
  
-   Mappage d’attributs LDAP à des types de revendications sortantes  
  
Pour plus d’informations sur la création de cette règle, consultez [créer une règle pour envoyer des attributs LDAP en tant que revendications](https://technet.microsoft.com/library/dd807115.aspx).  
  
## <a name="using-the-claim-rule-language"></a>Utilisation du langage des règles de revendication  
Si la requête à Active Directory, AD DS ou services AD LDS (Active Directory Lightweight Directory Services) \(AD LDS\) doit être comparée à un attribut LDAP autre que **samAccountname**, vous devez utiliser une règle personnalisée à la place. S'il n'existe aucune revendication de nom de compte Windows dans le jeu d'entrée, vous devez également utiliser une règle personnalisée afin de spécifier la revendication à utiliser pour interroger le service AD DS ou AD LDS.  
  
Les exemples suivants vous aident à comprendre les différentes méthodes permettant de créer une règle personnalisée à l'aide du langage des règles de revendication pour interroger et extraire des données d’un magasin d'attributs.  
  
### <a name="example-how-to-query-an-adlds-attribute-store-and-return-a-specified-value"></a>Exemple : Comment interroger un magasin d’attributs AD LDS et retourner une valeur spécifiée  
Les paramètres doivent être séparés par un point-virgule. Le premier paramètre est le filtre LDAP. Les paramètres suivants sont les attributs à retourner aux objets correspondants.  
  
L’exemple suivant montre comment rechercher un utilisateur par l’attribut **sAMAccountName** et émettre une\-revendication d’adresse de messagerie à l’aide de la valeur de l’attribut mail de l’utilisateur :  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer == "AD AUTHORITY"]  
=> issue(store = "AD LDS", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress"), query = "sAMAccountName={0};mail", param = regexreplace(c.Value, "(?<domain>[^\\]+)\\(?<user>.+)", "${user}"));  
  
```  
  
L’exemple suivant montre comment rechercher un utilisateur par l’attribut **mail** et les revendications title et Display Name, en utilisant les valeurs des attributs **title** et **DisplayName** de l’utilisateur :  
  
```  
c:[Type == " http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress ", Issuer == "AD AUTHORITY"]  
=> issue(store = "AD LDS ", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/title","http://schemas.xmlsoap.org/ws/2005/05/identity/claims/displayname"), query = "mail={0};title;displayname", param = c.Value);  
```  
  
L’exemple suivant montre comment rechercher un utilisateur par courrier et titre, puis émettre une revendication de nom complet à l’aide de l’attribut **DisplayName** de l’utilisateur :  
  
```  
c1:[Type == " http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress"] && c2:[Type == "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/title"]  
=> issue(store = "AD LDS ", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/displayname"), query = "(&(mail={0})(title={1}));displayname", param = c1.Value, param = c2.Value);  
```  
  
### <a name="example-how-to-query-an-activedirectory-attribute-store-and-return-a-specified-value"></a>Exemple : Comment interroger un magasin d’attributs Active Directory et retourner une valeur spécifiée  
La requête Active Directory doit inclure le nom \(de l’utilisateur avec le nom\) de domaine comme paramètre final afin que le magasin d’attributs Active Directory puisse interroger le domaine correct. Sinon, la syntaxe prise en charge est la même.  
  
L'exemple suivant montre comment rechercher un utilisateur par l’attribut **sAMAccountName** dans son domaine, puis retourner l’attribut **mail** :  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer == "AD AUTHORITY"]  
=> issue(store = "Active Directory", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress"), query = "sAMAccountName={0};mail;{1}", param = regexreplace(c.Value, "(?<domain>[^\\]+)\\(?<user>.+)", "${user}"), param = c.Value);  
```  
  
### <a name="example-how-to-query-an-activedirectory-attribute-store-based-on-the-value-of-an-incoming-claim"></a>Exemple : Comment interroger un magasin d'attributs Active Directory en fonction de la valeur d'une revendication entrante  
  
```  
c:[Type == "http://test/name"]  
  
   => issue(store = "Enterprise AD Attribute Store",  
  
         types = ("http://test/email"),  
  
         query = ";mail;{0}",  
  
         param = c.Value)  
  
```  
  
La demande précédente comprend les trois éléments suivants :  
  
-   Le filtre LDAP : spécifiez cette partie de la demande pour récupérer les objets dont vous souhaitez interroger les attributs. Pour obtenir des informations générales sur les demandes LDAP valides, consultez la RFC 2254. Lorsque vous interrogez un magasin d’attributs Active Directory et que vous ne spécifiez pas de filtre LDAP,\= la requête sAMAccountName{0} est supposée et le magasin d’attributs Active Directory attend un paramètre pouvant alimenter la valeur pour {0}. Sinon, la demande génère une erreur. Pour un magasin d'attributs LDAP autre qu'Active Directory, vous ne pouvez pas omettre le filtre LDAP de la demande. Sinon, une erreur est générée.  
  
-   Spécification d’attribut : dans cette deuxième partie de la requête, vous spécifiez \(les attributs qui\-sont séparés par des virgules si\) vous utilisez plusieurs valeurs d’attribut que vous souhaitez pour les objets filtrés. Le nombre d'attributs spécifiés doit correspondre au nombre de types de revendications que vous définissez dans la requête.  
  
-   Domaine Active Directory : ne spécifiez la dernière partie de la demande que si le magasin d'attributs est Active Directory. \(Il n’est pas nécessaire lorsque vous interrogez d’autres magasins d’attributs.\) Cette partie de la requête est utilisée pour spécifier le compte d’utilisateur sous la forme\\nom de domaine. Le magasin d'attributs Active Directory utilise la partie domaine pour déterminer le contrôleur de domaine approprié auquel se connecter, pour exécuter la demande et obtenir les attributs.  
  
### <a name="example-how-to-use-two-custom-rules-to-extract-the-manager-e-mail-from-an-attribute-in-activedirectory"></a>Exemple : Comment utiliser deux règles personnalisées pour extraire le courrier électronique\-Manager d’un attribut dans Active Directory  
Les deux règles personnalisées suivantes, lorsqu’elles sont utilisées ensemble dans l’ordre indiqué ci-dessous , interrogent Active Directory pour l' \(attribut Manager\) de la règle de compte d’utilisateur 1, puis utilisent cet attribut pour interroger le compte d’utilisateur du gestionnaire pour règle d’attribut \(de messagerie\)2. Enfin, l’attribut **mail** est émis en tant que revendication « ManagerEmail ». En résumé, les requêtes de la règle 1 Active Directory et transmettent le résultat de la requête à la règle 2, qui extrait\-ensuite les valeurs de la messagerie électronique Manager.  
  
Par exemple, lorsque l’exécution de ces règles est terminée, une revendication est émise et contient l'\-adresse de messagerie du responsable d’un utilisateur dans le domaine Corp.fabrikam.com.  
  
**Règle 1**  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]  
=> add(store = "Active Directory", types = ("http://schemas.xmlsoap.org/claims/ManagerDistinguishedName"), query = "sAMAccountName=  
{0};mail,userPrincipalName,extensionAttribute5,manager,department,extensionAttribute2,cn;{1}", param = regexreplace(c.Value, "(?  
<domain>[^\\]+)\\(?<user>.+)", "${user}"), param = c.Value);  
```  
  
**Règle 2**  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]  
&& c1:[Type == "http://schemas.xmlsoap.org/claims/ManagerDistinguishedName"]  
=> issue(store = "Active Directory", types = ("http://schemas.xmlsoap.org/claims/ManagerEmail"), query = "distinguishedName={0};mail;{1}", param = c1.Value,   
param = regexreplace(c1.Value, ".*DC=(?<domain>.+),DC=corp,DC=fabrikam,DC=com", "${domain}\username"));  
```  
  
> [!NOTE]  
> Ces règles ne fonctionnent que si le responsable de l’utilisateur se trouve dans le même domaine \(que l’utilisateur Corp.fabrikam.com\)dans cet exemple.  
  
## <a name="additional-references"></a>Références supplémentaires  
[Créer une règle pour envoyer des attributs LDAP en tant que revendications](https://technet.microsoft.com/library/dd807115.aspx)  
  

