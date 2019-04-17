---
ms.assetid: 606f4196-b579-4806-a462-3abd4d93e87c
title: "Quand utiliser un envoyer les attributs LDAP en tant que règle de revendication"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 05a2dd88057b64675bbc3bd30724d1eda0880c44
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

# <a name="when-to-use-a-send-ldap-attributes-as-claims-rule"></a>Quand utiliser un envoyer les attributs LDAP en tant que règle de revendication
Vous pouvez utiliser cette règle dans Active Directory Federation Services \(AD FS\) lorsque vous souhaitez émettre des revendications sortantes qui contiennent des valeurs d’attribut Lightweight Directory Access Protocol \(LDAP\) réel qui existent dans un magasin d’attributs et l’associent un type de revendication à chacun des attributs LDAP. Pour plus d’informations sur les magasins d’attributs, voir [The Role of Attribute Stores](The-Role-of-Attribute-Stores.md).  
  
Lorsque vous utilisez cette règle, vous émettez une revendication pour chaque attribut LDAP que vous spécifiez et qui correspond à la logique de règle, comme décrit dans le tableau suivant.  
  
|Option de règle|Logique de règle|  
|---------------|--------------|  
|Mappage d’attributs LDAP à des types de revendications sortantes|Si le magasin d’attributs est égal à *magasin d’attributs spécifié* et un attribut LDAP est égal à *valeur spécifiée*, puis vous mappez la valeur d’attribut LDAP pour le *revendication sortante spécifiée* tapez et émettez la revendication.|  
  
Les sections suivantes fournissent une introduction aux règles de revendication. Elles expliquent également quand utiliser envoyer les attributs LDAP en tant que règle de revendication.  
  
## <a name="about-claim-rules"></a>À propos des règles de revendication  
Une règle de revendication représente une instance de logique métier qui prend une revendication entrante, lui appliquer une condition \ (s’il x y\ puis) et génère une revendication sortante basée sur les paramètres de condition. La liste suivante présente d’importantes astuces que vous devez connaître les règles de revendication avant de poursuivre la lecture de cette rubrique:  
  
-   Dans la gestion AD FS de composants, les règles de revendication ne peuvent être créés à l’aide de modèles de règle de revendication  
  
-   Processus de règles de revendication entrante de revendications directement à partir d’un fournisseur de revendications \ (par exemple, Active Directory ou un autre fédération Service\) ou de la sortie de l’acceptation de règles de transformation sur une approbation de fournisseur de revendications.  
  
-   Les règles de revendication sont traitées par le moteur d’émission des revendications dans l’ordre chronologique, dans un ensemble de règles donné. En définissant la hiérarchie des règles, vous pouvez affiner ou filtrer les revendications qui sont générées par les règles précédentes au sein d’un ensemble de règles donné.  
  
-   Modèles de règle de revendication vous obligent toujours à spécifier un type de revendication entrante. Toutefois, vous pouvez traiter plusieurs valeurs de revendication avec le même type de revendication à l’aide d’une règle unique.  
  
Pour plus d’informations sur les règles de revendication et les jeux de règles de revendication, consultez [le rôle des règles de revendication](The-Role-of-Claim-Rules.md). Pour plus d’informations sur le traitement des règles, voir [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md). Pour plus d’informations, mode de traitement des jeux de règles de revendication, voir [The Role of the Claims Pipeline](The-Role-of-the-Claims-Pipeline.md).  
  
## <a name="mapping-of-ldap-attributes-to-outgoing-claim-types"></a>Mappage d’attributs LDAP à des types de revendications sortantes  
Lorsque vous utilisez l’option Envoyer les attributs LDAP en tant que modèle de règle de revendications, vous pouvez sélectionner les attributs à partir d’un magasin d’attributs LDAP, comme Active Directory ou des Services de domaine Active Directory \(AD DS\) pour envoyer leurs valeurs en tant que revendications à la partie de confiance. Cette opération mappe certains attributs LDAP à partir d’un magasin d’attributs que vous définissez à un ensemble de revendications sortantes qui peuvent être utilisés pour l’autorisation.  
  
À l’aide de ce modèle, vous pouvez ajouter plusieurs attributs qui seront envoyés en tant que revendications multiples, à partir d’une règle unique. Par exemple, vous pouvez utiliser ce modèle de règle pour créer une règle qui va rechercher des valeurs d’attribut pour les utilisateurs authentifiés à partir de la **société** et **service** Active Directory des attributs et puis envoyer ces valeurs en tant que deux revendications sortantes.  
  
Vous pouvez également utiliser cette règle pour envoyer les appartenances à un groupe de tous les l’utilisateur. Si vous souhaitez envoyer uniquement les appartenances de groupe individuels, utilisez l’appartenance au groupe Envoyer en tant qu’un modèle de règle de revendication. Pour plus d’informations, voir [quand utiliser un envoyer l’appartenance comme une règle de revendication](When-to-Use-a-Send-Group-Membership-as-a-Claim-Rule.md).  
  
## <a name="how-to-create-this-rule"></a>Comment créer cette règle  
Vous pouvez créer cette règle à l’aide du langage de règle de revendication ou à l’aide d’envoyer les attributs LDAP en tant que revendications modèle dans la gestion AD FS composants dans la règle. Ce modèle de règle fournit les options de configuration suivantes:  
  
-   Spécifiez un nom de règle de revendication  
  
-   Sélectionnez un magasin d’attributs à partir de laquelle extraire les attributs LDAP  
  
-   Mappage d’attributs LDAP à des types de revendications sortantes  
  
Pour plus d’informations sur la création de cette règle, voir [créer une règle pour envoyer les attributs LDAP en tant que revendications](https://technet.microsoft.com/library/dd807115.aspx).  
  
## <a name="using-the-claim-rule-language"></a>À l’aide du langage de règle de revendication  
Si la requête à Active Directory, les services AD DS ou Active Directory Lightweight Directory Services \(AD LDS\) doit comparer un attribut LDAP autre que **samAccountname**, vous devez utiliser une règle personnalisée à la place. S’il n’existe aucune revendication de nom de compte Windows dans le jeu d’entrée, vous devez également utiliser une règle personnalisée permet de spécifier la revendication à utiliser pour l’interrogation des services AD DS ou AD LDS.  
  
Les exemples suivants sont fournis pour vous aider à comprendre les différentes méthodes que vous pouvez créer une règle personnalisée à l’aide du langage de règle de revendication pour interroger et extraire des données dans un magasin d’attributs.  
  
### <a name="example-how-to-query-an-ad-lds-attribute-store-and-return-a-specified-value"></a>Exemple: Comment interroger un magasin d’attributs AD LDS et retourner une valeur spécifiée  
Les paramètres doivent être séparés par un point-virgule. Le premier paramètre est le filtre LDAP. Les paramètres suivants sont les attributs à retourner aux objets correspondants.  
  
L’exemple suivant montre comment rechercher un utilisateur par le **sAMAccountName** attribut et émettre une revendication d’adresse de messagerie e\, à l’aide de la valeur d’attribut de messagerie de l’utilisateur:  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer == "AD AUTHORITY"]  
=> issue(store = "AD LDS", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress"), query = "sAMAccountName={0};mail", param = regexreplace(c.Value, "(?<domain>[^\\]+)\\(?<user>.+)", "${user}"));  
  
```  
  
L’exemple suivant montre comment rechercher un utilisateur par le **mail** attribut et émettre les revendications de titre et le nom d’affichage, en utilisant les valeurs de l’utilisateur **titre** et **displayname** attributs:  
  
```  
c:[Type == " http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress ", Issuer == "AD AUTHORITY"]  
=> issue(store = "AD LDS ", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/title","http://schemas.xmlsoap.org/ws/2005/05/identity/claims/displayname"), query = "mail={0};title;displayname", param = c.Value);  
```  
  
L’exemple suivant montre comment rechercher un utilisateur par courrier et le titre et puis émettre une revendication de nom d’affichage à l’aide de l’utilisateur **displayname** attribut:  
  
```  
c1:[Type == " http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress"] && c2:[Type == "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/title"]  
=> issue(store = "AD LDS ", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/displayname"), query = "(&(mail={0})(title={1}));displayname", param = c1.Value, param = c2.Value);  
```  
  
### <a name="example-how-to-query-an-active-directory-attribute-store-and-return-a-specified-value"></a>Exemple: Comment interroger un magasin d’attributs Active Directory et retourner une valeur spécifiée  
La requête Active Directory doit inclure le nom d’utilisateur \ (avec le domaine nom\) comme dernier paramètre afin que l’attribut Active Directory stockent peut interroger le domaine approprié. Dans le cas contraire, la même syntaxe est pris en charge.  
  
L’exemple suivant montre comment rechercher un utilisateur par le **sAMAccountName** d’attribut dans son domaine, puis retourner la **mail** attribut:  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer == "AD AUTHORITY"]  
=> issue(store = "Active Directory", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress"), query = "sAMAccountName={0};mail;{1}", param = regexreplace(c.Value, "(?<domain>[^\\]+)\\(?<user>.+)", "${user}"), param = c.Value);  
```  
  
### <a name="example-how-to-query-an-active-directory-attribute-store-based-on-the-value-of-an-incoming-claim"></a>Exemple: Comment interroger un magasin d’attributs Active Directory en fonction de la valeur d’une revendication entrante  
  
```  
c:[Type == "http://test/name"]  
  
   => issue(store = "Enterprise AD Attribute Store",  
  
         types = ("http://test/email"),  
  
         query = ";mail;{0}",  
  
         param = c.Value)  
  
```  
  
La requête précédente est constituée de trois parties suivantes:  
  
-   Le filtre LDAP: spécifiez cette partie de la requête pour récupérer les objets pour lesquels vous souhaitez interroger les attributs. Pour obtenir des informations générales sur les requêtes LDAP valides, consultez la RFC 2254. Lorsque vous interrogez un magasin d’attributs Active Directory et que vous ne spécifiez pas un filtre LDAP, le samAccountName\ = {0} est supposée et le magasin d’attributs Active Directory attend un paramètre qui peut fournir la valeur de {0}. Dans le cas contraire, la requête entraîne une erreur. Pour un magasin d’attributs LDAP autre qu’Active Directory, vous ne pouvez pas omettre la partie de filtre LDAP de la requête ou la demande génère une erreur.  
  
-   Spécification des attributs: dans cette deuxième partie de la requête, vous spécifiez les attributs \ (qui sont séparées et entourées si vous utilisez plusieurs attributs values\) que vous souhaitez que les objets filtrés. Le nombre d’attributs que vous spécifiez doit correspondre le nombre de types de revendication que vous définissez dans la requête.  
  
-   Domaine Active Directory, vous spécifiez la dernière partie de la requête uniquement lorsque le magasin d’attributs est Active Directory. \ (Il n’est pas nécessaire lorsque vous interrogez autres magasins d’attributs. \) cette partie de la requête est utilisée pour spécifier le compte d’utilisateur dans le formulaire domain\\name. Le magasin d’attributs Active Directory utilise la partie domaine pour déterminer le contrôleur de domaine approprié pour se connecter à et exécutez la requête et les attributs de demande.  
  
### <a name="example-how-to-use-two-custom-rules-to-extract-the-manager-e-mail-from-an-attribute-in-active-directory"></a>Exemple: Comment utiliser deux règles personnalisées pour extraire le courrier e\ manager à partir d’un attribut dans Active Directory  
Les deux règles personnalisées suivantes, utilisés ensemble dans l’ordre indiqué ci-dessous, interroger Active Directory pour le **manager** attribut de l’utilisateur du compte \(Rule 1\) et ensuite utiliser cet attribut pour interroger le compte d’utilisateur du gestionnaire pour le **mail** \(Rule 2\) d’attribut. Enfin, le **mail** attribut est émis en une revendication «ManagerEmail». En résumé, règle 1 interroge Active Directory et passe le résultat de la requête à la règle 2 qui extrait le Gestionnaire de valeurs e\ de messagerie.  
  
Par exemple, lorsque ces règles est terminée en cours d’exécution, une revendication est émise qui contient l’adresse de messagerie e\ du responsable pour un utilisateur dans le domaine corp.fabrikam.com.  
  
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
> Ces règles ne fonctionnent que si le Gestionnaire de l’utilisateur est dans le même domaine que l’utilisateur \ (corp.fabrikam.com dans ce example\).  
  
## <a name="additional-references"></a>Références supplémentaires  
[Créer une règle pour envoyer les attributs LDAP en tant que revendications](https://technet.microsoft.com/library/dd807115.aspx)  
  

