---
ms.assetid: af16e847-47c2-461e-9df1-cc352a322043
title: "Quand utiliser une appartenance de groupe Envoyer en tant qu’une règle de revendication"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 10e027b4de463aad2b05eae40a622aebf8f12a3b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

# <a name="when-to-use-a-send-group-membership-as-a-claim-rule"></a>Quand utiliser une appartenance de groupe Envoyer en tant qu’une règle de revendication
Vous pouvez utiliser cette règle dans Active Directory Federation Services \(AD FS\) lorsque vous souhaitez émettre une nouvelle valeur de revendication sortante uniquement aux utilisateurs qui sont membres d’un groupe de sécurité Active Directory spécifié. Lorsque vous utilisez cette règle, vous émettez une seule revendication pour le groupe que vous spécifiez et qui correspond à la logique de règle, comme décrit dans le tableau suivant.  
  
|Option de règle|Logique de règle|  
|---------------|--------------|  
|Valeur de revendication sortante|Si l’appartenance au groupe de l’utilisateur est égale à la *groupe spécifié* et type de revendication sortante est égal à *type de revendication spécifiée*, puis remplacez la valeur de nom de groupe existante avec la *valeur de revendication sortante spécifiée* et émettez la revendication.|  
  
Les sections suivantes fournissent une introduction aux règles de revendication. Elles expliquent également quand utiliser l’appartenance au groupe Envoyer en tant qu’une règle de revendication.  
  
## <a name="about-claim-rules"></a>À propos des règles de revendication  
Une règle de revendication représente une instance de logique métier qui prend une revendication entrante, lui appliquer une condition \ (s’il x y\ puis) et génère une revendication sortante basée sur les paramètres de condition. La liste suivante présente d’importantes astuces que vous devez connaître les règles de revendication avant de poursuivre la lecture de cette rubrique:  
  
-   Dans la gestion AD FS de composants, les règles de revendication ne peuvent être créés à l’aide de modèles de règle de revendication  
  
-   Processus de règles de revendication entrante de revendications directement à partir d’un fournisseur de revendications \ (par exemple, Active Directory ou un autre fédération Service\) ou de la sortie de l’acceptation de règles de transformation sur une approbation de fournisseur de revendications.  
  
-   Les règles de revendication sont traitées par le moteur d’émission des revendications dans l’ordre chronologique, dans un ensemble de règles donné. En définissant la hiérarchie des règles, vous pouvez affiner ou filtrer les revendications qui sont générées par les règles précédentes au sein d’un ensemble de règles donné.  
  
-   Modèles de règle de revendication vous obligent toujours à spécifier un type de revendication entrante. Toutefois, vous pouvez traiter plusieurs valeurs de revendication avec le même type de revendication à l’aide d’une règle unique.  
  
Pour plus d’informations sur les règles de revendication et les jeux de règles de revendication, consultez [le rôle des règles de revendication](The-Role-of-Claim-Rules.md). Pour plus d’informations sur le traitement des règles, voir [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md). Pour plus d’informations, mode de traitement des jeux de règles de revendication, voir [The Role of the Claims Pipeline](The-Role-of-the-Claims-Pipeline.md).  
  
## <a name="outgoing-claim-value"></a>Valeur de revendication sortante  
À l’aide de l’appartenance au groupe Envoyer en tant qu’un modèle de règle de revendication, vous pouvez émettre une revendication qui est subordonnée à si un utilisateur est membre d’un groupe que vous spécifiez.  
  
En d’autres termes, ce problème de modèle de règle une revendication uniquement lorsque l’utilisateur est la sécurité de groupe ID \(SID\) qui correspond au groupe Active Directory qui spécifie l’administrateur. Tous les utilisateurs qui s’authentifient auprès des Services de domaine Active Directory \(AD DS\) auront groupe entrant que revendications SID pour chaque groupe auquel ils appartiennent. Par défaut, les règles de transformation d’acceptation dans l’Active Directory revendications une approbation de fournisseur passent par ces revendications SID de groupe. À l’aide de ces SID comme base pour l’émission de revendications sont beaucoup plus rapide que la recherche des groupes de l’utilisateur dans AD DS de groupe.  
  
Lorsque vous utilisez cette règle, une seule demande est envoyée, en fonction du groupe Active Directory que vous sélectionnez. Par exemple, vous pouvez utiliser ce modèle de règle pour créer une règle qui envoie une revendication de groupe avec la valeur «Admin» si l’utilisateur est membre du groupe de sécurité Admins du domaine.  
  
## <a name="configuring-this-rule-on-a-claims-provider-trust"></a>Configuration de cette règle sur une approbation de fournisseur de revendications  
Les administrateurs doivent utiliser ce type de règle dans les règles de transformation d’acceptation d’une approbation de fournisseur de revendications uniquement lorsque le SID de groupe sont reçus à partir du fournisseur de revendications, ce qui est très rare pour les fournisseurs de revendications à l’exception d’Active Directory ou les services AD DS.  
  
## <a name="how-to-create-this-rule"></a>Comment créer cette règle  
Vous créez cette règle à l’aide du langage de règle de revendication ou modèle dans la gestion AD FS composants dans la règle à l’aide de l’appartenance au groupe LDAP envoyer en tant que revendication. Ce modèle de règle fournit les options de configuration suivantes:  
  
-   Spécifiez un nom de règle de revendication  
  
-   Sélectionnez le groupe d’un utilisateur à l’aide du sélecteur d’objet  
  
-   Sélectionnez un type de revendication sortante  
  
-   Sélectionnez un format d’ID de nom sortant \ (qui est disponible uniquement lorsque l’identificateur de nom est choisi à partir de la field\ de type de revendication sortante)  
  
-   Spécifiez une valeur de revendication sortante  
  
Pour plus d’informations sur la création de cette règle, voir [créer une règle pour envoyer l’appartenance comme une revendication](https://technet.microsoft.com/en-us/library/ee913569.aspx).  
  
## <a name="using-the-claim-rule-language"></a>À l’aide du langage de règle de revendication  
Si vous souhaitez émettre des revendications basées sur un SID entrant autre que d’un SID de groupe, vous pouvez utiliser la transformation un modèle de règle de revendication entrante. Si l’administrateur souhaite récupérer les noms de tous les groupes auxquels l’utilisateur est membre, utilisez Envoyer les attributs LDAP en tant que modèle de règle de revendications à la place avec le **tokenGroups** attribut.  
  
### <a name="example-how-to-issue-group-claims-based-on-the-users-group-membership"></a>Exemple: Comment émettre les revendications de groupe selon l’appartenance au groupe de l’utilisateur  
La règle suivante émet des revendications de groupe pour un utilisateur en fonction des SID de groupe entrant:  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-21-397933417-626991126-188441444-512", Issuer == "AD AUTHORITY"]  
=> issue(Type = "http://schemas.xmlsoap.org/claims/Group", Value = "administrators", Issuer = c.Issuer, OriginalIssuer = c.OriginalIssuer, ValueType = c.ValueType);  
```  
  
## <a name="additional-references"></a>Références supplémentaires  
[Créer une règle pour envoyer les attributs LDAP en tant que revendications](https://technet.microsoft.com/library/dd807115.aspx)  
  

