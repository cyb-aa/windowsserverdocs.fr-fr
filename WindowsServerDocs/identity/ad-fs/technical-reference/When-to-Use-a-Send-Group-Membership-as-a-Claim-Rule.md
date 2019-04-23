---
ms.assetid: af16e847-47c2-461e-9df1-cc352a322043
title: Quand utiliser la règle Envoyer l’appartenance à un groupe en tant que revendication
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: dffd886ffd0bedd429918f72408b2d13d9fa1bdf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859170"
---
>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="when-to-use-a-send-group-membership-as-a-claim-rule"></a>Quand utiliser la règle Envoyer l’appartenance à un groupe en tant que revendication
Vous pouvez utiliser cette règle dans Active Directory Federation Services \(AD FS\) lorsque vous souhaitez émettre une nouvelle valeur de revendication sortante uniquement aux utilisateurs qui sont membres d’un groupe de sécurité Active Directory spécifié. Lorsque vous utilisez cette règle, vous émettez une seule revendication pour le groupe que vous spécifiez et qui correspond à la logique de la règle, comme décrit dans le tableau suivant.  
  
|Option de règle|Logique de règle|  
|---------------|--------------|  
|Valeur de revendication sortante|Si l’appartenance au groupe de l’utilisateur est égale au *groupe spécifié* et que le type de revendication sortante est égal au *type de revendication spécifiée*, remplacez la valeur de nom de groupe existante avec la *valeur de revendication sortante spécifiée* et émettez la revendication.|  
  
Les sections suivantes présentent brièvement les règles de revendication. Elles expliquent également quand utiliser la règle Envoyer l’appartenance en tant que revendication.  
  
## <a name="about-claim-rules"></a>À propos des règles de revendication  
Une règle de revendication représente une instance de logique métier qui permettront une revendication entrante, lui appliquer une condition \(if x, alors y\) et génère une revendication sortante selon les paramètres de condition. La liste suivante présente d’importantes astuces sur les règles de revendication dont vous devez prendre connaissance avant de poursuivre la lecture de cette rubrique :  
  
-   Dans le composant logiciel enfichable Gestion AD FS\-, revendication règles peuvent uniquement être créées à l’aide de modèles de règle de revendication  
  
-   Processus de règles de revendication entrante de revendications directement à partir d’un fournisseur de revendications \(comme Active Directory ou un autre Service de fédération\) ou à partir de la sortie de l’acceptation des règles sur une approbation de fournisseur de revendications de transformation.  
  
-   Les règles de revendication sont traitées par le moteur d’émission des revendications au sein d’un ensemble de règles donné et dans l’ordre chronologique. En définissant la hiérarchie des règles, vous pouvez affiner ou filtrer des revendications générées par des règles précédentes dans un ensemble de règles donné.  
  
-   Les modèles de règles de revendication vous obligent toujours à spécifier un type de revendication entrante. Toutefois, vous pouvez traiter plusieurs valeurs de revendication avec le même type de revendication en vous appuyant sur une règle unique.  
  
Pour plus d’informations sur les règles de revendication et des ensembles de règles de revendication, consultez [le rôle des règles de revendication](The-Role-of-Claim-Rules.md). Pour plus d’informations sur le traitement des règles, consultez [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md). Pour plus d’informations mode de traitement des ensembles de règles de revendication, consultez [The Role of the Claims Pipeline](The-Role-of-the-Claims-Pipeline.md).  
  
## <a name="outgoing-claim-value"></a>Valeur de revendication sortante  
À l’aide du modèle de règle Envoyer l’appartenance à un groupe en tant que revendication, vous pouvez émettre une revendication en fonction de l’appartenance de l’utilisateur au groupe que vous spécifiez.  
  
En d’autres termes, ce modèle de règle émet une revendication uniquement lorsque l’utilisateur a l’ID de groupe de sécurité \(SID\) qui correspond au groupe Active Directory spécifié par l’administrateur. Tous les utilisateurs qui s’authentifient auprès des Services de domaine Active Directory \(AD DS\) auront groupe entrant revendications SID pour chaque groupe auquel ils appartiennent. Par défaut, les règles de transformation d’acceptation dans l’approbation de fournisseur de revendications Active Directory passent par ces revendications SID de groupe. L’utilisation de ces SID de groupe comme base pour l’émission de revendications est beaucoup plus rapide que la recherche des groupes de l’utilisateur dans AD DS.  
  
Lorsque vous utilisez cette règle, une seule demande est envoyée, en fonction du groupe Active Directory que vous sélectionnez. Par exemple, vous pouvez utiliser ce modèle de règle pour créer une règle qui envoie une revendication de groupe avec la valeur Admin si l’utilisateur est membre du groupe de sécurité Administrateurs du domaine.  
  
## <a name="configuring-this-rule-on-a-claims-provider-trust"></a>Configuration de cette règle sur une approbation de fournisseur de revendications  
Les administrateurs doivent utiliser ce type de règle dans les règles de transformation d’acceptation d’une approbation de fournisseur de revendications uniquement lorsque les SID de groupe sont reçus à partir du fournisseur de revendications. Il s’agit d’un cas de figure rare pour les fournisseurs de revendications, à l’exception d’Active Directory ou des services AD DS.  
  
## <a name="how-to-create-this-rule"></a>Comment créer cette règle  
Vous créer cette règle à l’aide du langage de règle de revendication ou à l’aide de l’appartenance au groupe LDAP envoyer en tant qu’un modèle de règle de revendication dans la gestion AD FS aligner\-dans. Ce modèle de règle fournit les options de configuration suivantes :  
  
-   Spécifier un nom de règle de revendication  
  
-   Sélectionner le groupe d’un utilisateur à l’aide du sélecteur d’objet  
  
-   Sélectionner un type de revendication sortante  
  
-   Sélectionnez un format d’ID de nom sortant \(qui est disponible uniquement lorsque les ID de nom est choisi en fonction du champ de type de revendication sortante\)  
  
-   Spécifier une valeur de revendication sortante  
  
Pour plus d’informations sur la création de cette règle, consultez [créer une règle pour envoyer l’appartenance au groupe en tant que revendication](https://technet.microsoft.com/library/ee913569.aspx).  
  
## <a name="using-the-claim-rule-language"></a>Utilisation du langage des règles de revendication  
Si vous souhaitez émettre des revendications basées sur un SID entrant autre qu’un SID de groupe, utilisez le modèle de règle Transformer une revendication entrante. Si l’administrateur souhaite récupérer les noms de tous les groupes dont l’utilisateur est membre, utilisez le modèle de règle Envoyer les attributs LDAP en tant que revendication à la place de l’attribut **tokenGroups**.  
  
### <a name="example-how-to-issue-group-claims-based-on-the-users-group-membership"></a>Exemple : Comment émettre les revendications de groupe selon l’appartenance au groupe de l’utilisateur  
La règle suivante émet des revendications de groupe pour un utilisateur en fonction du SID de groupe entrant :  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-21-397933417-626991126-188441444-512", Issuer == "AD AUTHORITY"]  
=> issue(Type = "http://schemas.xmlsoap.org/claims/Group", Value = "administrators", Issuer = c.Issuer, OriginalIssuer = c.OriginalIssuer, ValueType = c.ValueType);  
```  
  
## <a name="additional-references"></a>Références supplémentaires  
[Créer une règle pour envoyer les attributs LDAP en tant que revendications](https://technet.microsoft.com/library/dd807115.aspx)  
  

