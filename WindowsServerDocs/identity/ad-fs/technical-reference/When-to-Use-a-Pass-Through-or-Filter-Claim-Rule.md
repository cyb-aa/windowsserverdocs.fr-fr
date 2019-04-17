---
ms.assetid: 606df285-259c-4c6b-8583-9aca1d614c43
title: "Quand utiliser un transfert direct ou filtrer la règle de revendication"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: aa205c46bf67dc25a55232b799bdd39fee4ac3c6
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

# <a name="when-to-use-a-pass-through-or-filter-claim-rule"></a>Quand utiliser un transfert direct ou filtrer la règle de revendication
Vous pouvez utiliser cette règle dans Active Directory Federation Services \(AD FS\) lorsque vous avez besoin prendre un type de revendication entrante spécifique, puis appliquez une action qui détermine quelle sortie basée sur les valeurs de la revendication entrante. Lorsque vous utilisez cette règle, vous transmettez ou filtrez les revendications qui correspondent à la logique de règle dans le tableau suivant, en fonction des options que vous configurez dans la règle.  
  
|Option de règle|Logique de règle|  
|---------------|--------------|  
|Passer toutes les valeurs de revendication|Si le type de revendication entrante est égal à *type de revendication spécifiée* et que la valeur est égale à *n’importe quelle valeur*, puis transmettez la revendication|  
|Transmettre uniquement la valeur de revendication spécifique|Si le type de revendication entrante est égal à *type de revendication spécifiée* et que la valeur est égale à *valeur de revendication spécifiée*, puis transmettez la revendication|  
|Transmettre uniquement les valeurs de revendication qui correspondent à une valeur de suffixe de messagerie e\ spécifique|Si le type de revendication entrante est égal à *type de revendication spécifiée* et que la valeur est égale à *valeur de suffixe spécifiée*, puis transmettez la revendication|  
|Transmettre uniquement les valeurs de revendication qui commencent par une valeur spécifique|Si le type de revendication entrante est égal à *type de revendication spécifiée* et la valeur commence par *valeur de revendication spécifiée*, puis transmettez la revendication|  
  
Les sections suivantes fournissent une introduction aux règles de revendication et fournissent des informations détaillées sur l’utilisation de cette règle.  
  
## <a name="about-claim-rules"></a>À propos des règles de revendication  
Une règle de revendication représente une instance de logique métier qui prend une revendication entrante, lui appliquer une condition \ (s’il x y\ puis) et génère une revendication sortante basée sur les paramètres de condition. La liste suivante présente d’importantes astuces que vous devez connaître les règles de revendication avant de poursuivre la lecture de cette rubrique:  
  
-   Dans la gestion AD FS de composants, les règles de revendication ne peuvent être créés à l’aide de modèles de règle de revendication  
  
-   Processus de règles de revendication entrante de revendications directement à partir d’un fournisseur de revendications \ (par exemple, Active Directory ou un autre fédération Service\) ou de la sortie de l’acceptation de règles de transformation sur une approbation de fournisseur de revendications.  
  
-   Les règles de revendication sont traitées par le moteur d’émission des revendications dans l’ordre chronologique, dans un ensemble de règles donné. En définissant la hiérarchie des règles, vous pouvez affiner ou filtrer les revendications qui sont générées par les règles précédentes au sein d’un ensemble de règles donné.  
  
-   Modèles de règle de revendication vous obligent toujours à spécifier un type de revendication entrante. Toutefois, vous pouvez traiter plusieurs valeurs de revendication avec le même type de revendication à l’aide d’une règle unique.  
  
Pour plus d’informations sur les règles de revendication et les jeux de règles de revendication, consultez [le rôle des règles de revendication](The-Role-of-Claim-Rules.md). Pour plus d’informations sur le traitement des règles, voir [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md). Pour plus d’informations, mode de traitement des jeux de règles de revendication, voir [The Role of the Claims Pipeline](The-Role-of-the-Claims-Pipeline.md).  
  
## <a name="pass-through-all-claim-values"></a>Passer toutes les valeurs de revendication  
Lorsque vous utilisez cette action, toutes les valeurs de revendication entrante pour le type de revendication spécifiée sont transmises en tant que revendications sortantes. Par exemple, lorsque le type de revendication entrante est spécifié comme le type de revendication de rôle, toutes les valeurs de revendication entrante sont copiées individuellement dans les nouvelles revendications sortantes avec le type de revendication sortante du rôle.  
  
## <a name="filtering-a-claim"></a>Filtrage d’une revendication  
Dans AD FS, le terme *filtrage de revendications* signifie filtrer ou limiter en entrant les valeurs de revendication afin que seules certaines valeurs soient transmises ou envoyées par le biais d’en tant que revendications sortantes. Il s’agit du **passer ou filtrer une revendication entrante** modèle de règle qui permet cette fonction. Dans les propriétés de cette règle, vous pouvez définir des conditions de filtrage afin que seules les valeurs qui répondent aux critères spécifiés sont transmises par le biais des valeurs entrantes.  
  
Par exemple, vous pouvez utiliser cette règle pour transmettre uniquement des revendications qui correspondent à la valeur de revendication de l’acheteur lorsque le type de revendication de rôle, ou vous souhaiterez peut-être émettre uniquement des revendications sur le nom de l’utilisateur de correspondances de types de revendication entrants, mais pas de revendications contenant le numéro de sécurité sociale de l’utilisateur.  
  
Lorsque vous utilisez une condition de filtre avec cette règle, toutes les revendications entrantes sont examinées pour déterminer les revendications qui correspondent aux critères définis par la règle. Toutes les autres revendications sont ignorées afin qu’uniquement les valeurs de revendication spécifiées qui correspondent à un type de revendication sélectionné transmettre.  
  
Par exemple, comme indiqué dans l’illustration suivante, lorsqu’une règle est définie avec la condition pour filtrer les revendications entrantes uniquement qui correspondent à l’UPN type de revendication et également se terminer par @fabrikam.com, tous les autres revendications entrantes sont ignorées, sauf si elles répondent à ces critères. Cela inclut la revendication entrante avec le type de revendication d’adresse de messagerie E\ même si sa valeur de revendication se termine par @fabrikam.com. Dans ce cas, seule la revendication contenant la valeur de Nick@fabrikam.com est envoyé à la partie de confiance.  
  
![Quand utiliser passe par le biais de](media/adfs2_filter.gif)  
  
## <a name="configuring-this-rule-on-a-claims-provider-trust"></a>Configuration de cette règle sur une approbation de fournisseur de revendications  
Lorsque vous utilisez une approbation de fournisseur de revendications, cette règle peut être configurée pour transmettre entrant uniquement les revendications du fournisseur de revendications qui correspondent à certaines contraintes. Par exemple, vous souhaiterez peut-être accepter uniquement les revendications de courrier e\ du fournisseur de revendications; Par conséquent, vous utiliserez ce modèle de règle pour accepter les types de revendications de courrier e\ qui se terminent par nom de système de nom de domaine \(DNS\) du fournisseur de revendications.  
  
## <a name="configuring-this-rule-on-a-relying-party-trust"></a>Configuration de cette règle sur une approbation de partie de confiance  
Lorsque vous utilisez une approbation de partie de confiance, cette règle peut être configurée pour transmettre ou filtrer les revendications sortantes qui seront envoyées à la partie de confiance. Certaines parties de confiance peut ne pas comprennent certains types de revendication ou certaines revendications peuvent contenir des informations sensibles qui ne doivent pas être envoyées à certaines parties de confiance. Ce modèle de règle peut aider à appliquer ces stratégies pour une approbation de partie de confiance particulier.  
  
## <a name="how-to-create-this-rule"></a>Comment créer cette règle  
Vous créez cette règle à l’aide du langage de règle de revendication ou passez à l’aide d’ou filtrez un modèle de règle de revendication entrante dans la gestion AD FS de composants. Ce modèle de règle fournit les options de configuration suivantes:  
  
-   Spécifiez un nom de règle de revendication  
  
-   Spécifiez un type de revendication entrante  
  
-   Passer toutes les valeurs de revendication  
  
-   Transmettre uniquement la valeur de revendication spécifique  
  
-   Transmettre uniquement les valeurs de revendication qui correspondent à une valeur de suffixe de messagerie e\ spécifique  
  
-   Transmettre uniquement les valeurs de revendication qui commencent par une valeur spécifique  
  
Pour plus d’informations sur la création de ce modèle, voir [créer une règle pour passer ou filtrer une revendication entrante](https://technet.microsoft.com/library/dd807060.aspx) dans le Guide de déploiement d’AD FS.  
  
## <a name="using-the-claim-rule-language"></a>À l’aide du langage de règle de revendication  
Si une revendication doit être envoyée uniquement lorsque la valeur de revendication correspond à un modèle personnalisé, vous devez utiliser une règle personnalisée. Pour plus d’informations, consultez la section quand utiliser une règle personnalisée.  
  
### <a name="examples-of-how-to-construct-a-pass-through-or-filter-rule-syntax"></a>Exemples de comment construire un transfert direct ou la syntaxe de règle de filtre  
Une règle de filtrage simple permet de filtrer les revendications selon une des propriétés présentées ci-dessus. Par exemple, la règle suivante passera par le biais de toutes les revendications de courrier e\:  
  
```  
c:[type == “http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress”]  => issue(claim  = c);  
```  
  
Les filtres peuvent être logiquement AND\-ed ensemble. Par exemple, la règle suivante accepte toutes les revendications de courrier e\ avec la valeurjohndoe@fabrikam.com:  
  
```  
c:[type == “http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress”, value == “johndoe@fabrikam.com “]  => issue(claim  = c);  
```  
  
Dans les exemples ci-dessus, les filtres utilisés toujours un opérateur d’égalité. Le langage de règle de revendication prend en charge les opérateurs suivants:  
  
-   \ = \ = \-égal à \(case\-sensitive\)  
  
-   \! \ = \-n’est pas égal à \(case\-sensitive\)  
  
-   \ = ~ \-correspondance d’expression régulière  
  
-   \! ~ \-autre que celle-correspondance d’expression régulière  
  
Par exemple, la règle suivante accepte toutes les revendications de courrier e\ non émises par le serveur de fédération local qui ont le suffixe Boeing.com:  
  
```  
c:[type == “http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress”, value =~ “^.*@boeing\.com$” , issuer != “LOCAL AUTHORITY”]  => issue(claim  = c);  
```  
  
### <a name="best-practices-for-creating-custom-rules"></a>Meilleures pratiques pour la création de règles personnalisées  
Un filtre peut être appliqué à un ou plusieurs des propriétés de chaque revendication, comme décrit dans le tableau suivant.  
  
|Propriété de revendication|Description|  
|------------------|---------------|  
|Type|Le type de revendication \ (généralement représenté par un Uri\) reflète un accord implicite entre partenaires dans une fédération sur le type d’informations transmis dans la revendication. Par exemple, les revendications de type http:///\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/emailaddress contiendra l’adresse de messagerie e\ de l’utilisateur.|  
|Valeur|La valeur de la revendication. Par exemple, une revendication de type http:///\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/emailaddress peut avoir une valeur dejohndoe@fabrikam.com|  
|Type de valeur|Le type de valeur représente la façon dont les informations contenues dans la valeur de la revendication doit être interprété. En général le type de valeur sera définie à http:///\/ www.w3.org \/2001\/XMLSchema\#string, mais la valeur de revendication peut contenir des données Base64Binary codé \ (par exemple, un image\) ou une date, booléen et ainsi de suite.|  
|Émetteur|L’émetteur représente la partie qui dernière émis les revendications relatives à l’utilisateur. Si les revendications sont obtenues sur un serveur de fédération du fournisseur de revendications l’émetteur de toutes les revendications va être défini sur «Autorité locale». Si les revendications reçues par un serveur de fédération du fournisseur de fédération, l’émetteur des revendications va être associé à l’identificateur du fournisseur de revendications de fournisseur de revendications qui a signé le jeton. Par conséquent, lors du traitement des règles sur les revendications reçues à partir d’un fournisseur de revendications l’émetteur de toutes les revendications va être définie avec la même valeur. Lors de la création de règles pour une partie de confiance, la propriété issuer peut être utilisée pour faire la distinction entre les revendications issues des différents fournisseurs de revendications.|  
|OriginalIssuer|Cette propriété de revendication est destinée à transmettre le serveur de fédération qui a émis initialement la revendication. Dans la mesure où la propriété issuer de revendications est définie au dernier serveur de fédération qui a signé le jeton, l’émetteur d’origine est utile dans les scénarios où une revendication a transité par plusieurs serveurs de fédération \ (par exemple, une partie de confiance qui reçoit un jeton à partir d’un serveur de fédération de fournisseur de fédération intéresser particulier revendications serveur de fédération du fournisseur authentifié le par l’utilisateur)|  
|Propriétés|Outre les cinq propriétés présentées ci-dessus, chaque revendication possède également un conteneur des propriétés où les propriétés nommées peuvent être stockées. Ces propriétés ne sont pas sérialisées dans le jeton et ne pour passer les informations entre les composants du pipeline d’émission revendications au sein d’un serveur de fédération unique. Par exemple, si une propriété au cours des revendications des règles de fournisseur de traitement, puis la référence à celui-ci dans les règles de partie de confiance.|  
  

