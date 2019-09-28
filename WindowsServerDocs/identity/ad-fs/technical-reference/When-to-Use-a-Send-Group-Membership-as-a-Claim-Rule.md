---
ms.assetid: af16e847-47c2-461e-9df1-cc352a322043
title: Quand utiliser la règle Envoyer l’appartenance à un groupe en tant que revendication
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 82dd9cec2c75a796eb0def508082508a5d0dbf5f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385430"
---
# <a name="when-to-use-a-send-group-membership-as-a-claim-rule"></a>Quand utiliser la règle Envoyer l’appartenance à un groupe en tant que revendication
Vous pouvez utiliser cette règle dans services ADFS \(AD FS\) lorsque vous souhaitez émettre une nouvelle valeur de revendication sortante uniquement pour les utilisateurs qui sont membres d’un groupe de sécurité Active Directory spécifié. Lorsque vous utilisez cette règle, vous émettez une seule revendication pour le groupe que vous spécifiez et qui correspond à la logique de la règle, comme décrit dans le tableau suivant.  
  
|Option de règle|Logique de règle|  
|---------------|--------------|  
|Valeur de revendication sortante|Si l’appartenance à un groupe d’un utilisateur est égale au *groupe spécifié* et que le type de revendication sortante est égal au *type de revendication spécifié*, remplacez la valeur de nom de groupe existante par la *valeur de revendication sortante spécifiée* et émettez la revendication.|  
  
Les sections suivantes présentent brièvement les règles de revendication. Elles expliquent également quand utiliser la règle Envoyer l’appartenance en tant que revendication.  
  
## <a name="about-claim-rules"></a>À propos des règles de revendication  
Une règle de revendication représente une instance de logique métier qui prend une revendication entrante, lui applique une condition \(si x Then y\) et génère une revendication sortante basée sur les paramètres de condition. La liste suivante présente d’importantes astuces sur les règles de revendication dont vous devez prendre connaissance avant de poursuivre la lecture de cette rubrique :  
  
-   Dans le composant logiciel enfichable\-gestion des AD FS, les règles de revendication ne peuvent être créées qu’à l’aide de modèles de règles de revendication  
  
-   Les règles de revendication traitent les revendications entrantes soit \(directement à partir d’un fournisseur\) de revendications, par exemple Active Directory ou une autre service FS (Federation Service), soit à partir de la sortie des règles de transformation d’acceptation sur une approbation de fournisseur de revendications.  
  
-   Les règles de revendication sont traitées par le moteur d’émission des revendications au sein d’un ensemble de règles donné et dans l’ordre chronologique. En définissant la hiérarchie des règles, vous pouvez affiner ou filtrer des revendications générées par des règles précédentes dans un ensemble de règles donné.  
  
-   Les modèles de règles de revendication vous obligent toujours à spécifier un type de revendication entrante. Toutefois, vous pouvez traiter plusieurs valeurs de revendication avec le même type de revendication en vous appuyant sur une règle unique.  
  
Pour plus d’informations sur les règles de revendication et les ensembles de règles de revendication, consultez [rôle des règles de revendication](The-Role-of-Claim-Rules.md). Pour plus d’informations sur la façon dont les règles sont traitées, consultez [le rôle du moteur de revendications](The-Role-of-the-Claims-Engine.md). Pour plus d’informations sur le traitement des ensembles de règles de revendication, consultez [le rôle du pipeline de revendications](The-Role-of-the-Claims-Pipeline.md).  
  
## <a name="outgoing-claim-value"></a>Valeur de revendication sortante  
À l’aide du modèle de règle Envoyer l’appartenance à un groupe en tant que revendication, vous pouvez émettre une revendication en fonction de l’appartenance de l’utilisateur au groupe que vous spécifiez.  
  
En d’autres termes, ce modèle de règle émet une revendication uniquement lorsque l’utilisateur possède l’ID \(de\) sécurité de groupe sid qui correspond au groupe de Active Directory que l’administrateur spécifie. Tous les utilisateurs qui s’authentifient\) auprès d’Active Directory Domain Services \(AD DS auront des revendications de SID de groupe entrantes pour chaque groupe auquel ils appartiennent. Par défaut, les règles de transformation d’acceptation dans l’approbation de fournisseur de revendications Active Directory passent par ces revendications SID de groupe. L’utilisation de ces SID de groupe comme base pour l’émission de revendications est beaucoup plus rapide que la recherche des groupes de l’utilisateur dans AD DS.  
  
Lorsque vous utilisez cette règle, une seule demande est envoyée, en fonction du groupe Active Directory que vous sélectionnez. Par exemple, vous pouvez utiliser ce modèle de règle pour créer une règle qui envoie une revendication de groupe avec la valeur Admin si l’utilisateur est membre du groupe de sécurité Administrateurs du domaine.  
  
## <a name="configuring-this-rule-on-a-claims-provider-trust"></a>Configuration de cette règle sur une approbation de fournisseur de revendications  
Les administrateurs doivent utiliser ce type de règle dans les règles de transformation d’acceptation d’une approbation de fournisseur de revendications uniquement lorsque les SID de groupe sont reçus à partir du fournisseur de revendications. Il s’agit d’un cas de figure rare pour les fournisseurs de revendications, à l’exception d’Active Directory ou des services AD DS.  
  
## <a name="how-to-create-this-rule"></a>Comment créer cette règle  
Vous créez cette règle à l’aide du langage de règle de revendication ou en utilisant le modèle de règle envoyer l’appartenance au groupe LDAP en tant\-que revendication dans le composant logiciel enfichable Gestion des AD FS. Ce modèle de règle fournit les options de configuration suivantes :  
  
-   Spécifier un nom de règle de revendication  
  
-   Sélectionner le groupe d’un utilisateur à l’aide du sélecteur d’objets  
  
-   Sélectionner un type de revendication sortante  
  
-   Sélectionnez un format \(d’ID de nom sortant qui est disponible uniquement lorsque ID de nom est choisi dans le champ type de revendication sortante.\)  
  
-   Spécifier une valeur de revendication sortante  
  
Pour plus d’informations sur la création de cette règle, consultez [créer une règle pour envoyer l’appartenance à un groupe en tant que revendication](https://technet.microsoft.com/library/ee913569.aspx).  
  
## <a name="using-the-claim-rule-language"></a>Utilisation du langage des règles de revendication  
Si vous souhaitez émettre des revendications basées sur un SID entrant autre qu’un SID de groupe, utilisez le modèle de règle Transformer une revendication entrante. Si l’administrateur souhaite récupérer les noms de tous les groupes dont l’utilisateur est membre, utilisez le modèle de règle Envoyer les attributs LDAP en tant que revendication à la place de l’attribut **tokenGroups**.  
  
### <a name="example-how-to-issue-group-claims-based-on-the-users-group-membership"></a>Exemple : Comment émettre des revendications de groupe en fonction de l’appartenance aux groupes de l’utilisateur  
La règle suivante émet des revendications de groupe pour un utilisateur en fonction du SID de groupe entrant :  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-21-397933417-626991126-188441444-512", Issuer == "AD AUTHORITY"]  
=> issue(Type = "http://schemas.xmlsoap.org/claims/Group", Value = "administrators", Issuer = c.Issuer, OriginalIssuer = c.OriginalIssuer, ValueType = c.ValueType);  
```  
  
## <a name="additional-references"></a>Références supplémentaires  
[Créer une règle pour envoyer des attributs LDAP en tant que revendications](https://technet.microsoft.com/library/dd807115.aspx)  
  

