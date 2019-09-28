---
ms.assetid: 606df285-259c-4c6b-8583-9aca1d614c43
title: Quand utiliser un transfert direct ou filtrer la règle de revendication
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 49061aaab2f46d7d3abe80d4fade98c10654fc37
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407291"
---
# <a name="when-to-use-a-pass-through-or-filter-claim-rule"></a>Quand utiliser un transfert direct ou filtrer la règle de revendication
Vous pouvez utiliser cette règle dans services ADFS \(AD FS\) lorsque vous devez prendre un type de revendication entrante spécifique, puis appliquer une action qui détermine la sortie qui doit se produire en fonction des valeurs de la revendication entrante. Lorsque vous utilisez cette règle, vous passez ou filtrez les revendications qui correspondent à la logique de règle dans le tableau ci-dessous, en fonction des options que vous configurez dans la règle.  
  
|Option de règle|Logique de règle|  
|---------------|--------------|  
|Passer toutes les valeurs de revendication|Si le type de revendication entrante est égal à un *type de revendication entrante donné* et que la valeur est égale à *toute valeur*, transmettez la revendication|  
|Transmettre uniquement une valeur de revendication spécifique|Si le type de revendication entrante est égal à un *type de revendication donné* et que la valeur est égale à la *valeur de revendication spécifiée*, transmettez la revendication|  
|Transmettre uniquement les valeurs de revendication qui correspondent à une\-valeur de suffixe de courrier électronique spécifique|Si le type de revendication entrante est égal à un *type de revendication donné* et que la valeur est égale à une *valeur de suffixe spécifiée*, transmettez la revendication|  
|Transmettre uniquement les valeurs de revendication qui commencent par une valeur spécifique|Si le type de revendication entrante est égal au *type de revendication donné* et que la valeur commence par la *valeur de revendication spécifiée*, transmettez la revendication|  
  
Les sections suivantes constituent une introduction aux règles de revendication et fournissent des informations détaillées sur les conditions d’utilisation de cette règle.  
  
## <a name="about-claim-rules"></a>À propos des règles de revendication  
Une règle de revendication représente une instance de logique métier qui prend une revendication entrante, lui applique une condition \(si x Then y\) et génère une revendication sortante basée sur les paramètres de condition. La liste suivante présente d’importantes astuces sur les règles de revendication dont vous devez prendre connaissance avant de poursuivre la lecture de cette rubrique :  
  
-   Dans le composant logiciel enfichable\-gestion des AD FS, les règles de revendication ne peuvent être créées qu’à l’aide de modèles de règles de revendication  
  
-   Les règles de revendication traitent les revendications entrantes soit \(directement à partir d’un fournisseur\) de revendications, par exemple Active Directory ou une autre service FS (Federation Service), soit à partir de la sortie des règles de transformation d’acceptation sur une approbation de fournisseur de revendications.  
  
-   Les règles de revendication sont traitées par le moteur d’émission des revendications au sein d’un ensemble de règles donné et dans l’ordre chronologique. En définissant la hiérarchie des règles, vous pouvez affiner ou filtrer des revendications générées par des règles précédentes dans un ensemble de règles donné.  
  
-   Les modèles de règles de revendication vous obligent toujours à spécifier un type de revendication entrante. Toutefois, vous pouvez traiter plusieurs valeurs de revendication avec le même type de revendication en vous appuyant sur une règle unique.  
  
Pour plus d’informations sur les règles de revendication et les ensembles de règles de revendication, consultez [rôle des règles de revendication](The-Role-of-Claim-Rules.md). Pour plus d’informations sur la façon dont les règles sont traitées, consultez [le rôle du moteur de revendications](The-Role-of-the-Claims-Engine.md). Pour plus d’informations sur le traitement des ensembles de règles de revendication, consultez [le rôle du pipeline de revendications](The-Role-of-the-Claims-Pipeline.md).  
  
## <a name="pass-through-all-claim-values"></a>Passer toutes les valeurs de revendication  
Lorsque vous utilisez cette action, toutes les valeurs de revendication entrante pour le type de revendication spécifiée sont transmises en tant que revendications sortantes. Par exemple, lorsque le type de revendication entrante est spécifié comme le type de revendication de rôle, toutes les valeurs de revendication entrante sont copiées individuellement dans les nouvelles revendications sortantes avec le type de revendication sortante du rôle.  
  
## <a name="filtering-a-claim"></a>Filtrage d’une revendication  
Dans AD FS, le terme *filtrage de revendications* signifie filtrer ou limiter les valeurs de revendication entrante afin que seules certaines valeurs soient transmises ou envoyées en tant que revendications sortantes. Il s’agit du modèle de règle de **transmission ou de filtrage d’une revendication entrante** qui active cette fonction. Dans les propriétés de cette règle, vous pouvez définir des conditions de filtrage des valeurs entrantes de sorte que seules les valeurs répondant à vos critères soient transmises.  
  
Par exemple, vous pouvez utiliser cette règle pour transmettre uniquement des revendications qui correspondent à la valeur de revendication de l’acheteur lorsque le type de revendication entrante correspond au type de revendication du rôle, ou vous pouvez émettre uniquement des revendications sur le nom de l’utilisateur, mais pas de revendications contenant le numéro de sécurité sociale de l’utilisateur.  
  
Lorsque vous utilisez une condition de filtre avec cette règle, toutes les revendications entrantes sont examinées de manière à identifier les revendications qui correspondent aux critères définis par la règle. Toutes les autres revendications sont ignorées afin que seules les valeurs de revendication spécifiées qui correspondent à un type de revendication sélectionné soient transmises.  
  
Par exemple, comme indiqué dans l’illustration suivante, lorsqu’une règle est définie avec la condition de filtrer uniquement les revendications entrantes qui sont indexées sur le type de revendication UPN @fabrikam.comet qui se terminent par, toutes les autres revendications entrantes sont ignorées, sauf si elles répondent à ces critères. Cela comprend la revendication entrante avec le type de revendication\-adresse de messagerie, même si sa valeur de @fabrikam.comrevendication se termine par. Dans ce cas, seule la revendication contenant la valeur de Nick@fabrikam.com est envoyée à la partie de confiance.  
  
![Quand utiliser le transfert](media/adfs2_filter.gif)  
  
## <a name="configuring-this-rule-on-a-claims-provider-trust"></a>Configuration de cette règle sur une approbation de fournisseur de revendications  
Lorsque vous utilisez une approbation de fournisseur de revendications, cette règle peut être configurée pour transmettre uniquement les revendications entrantes à partir du fournisseur de revendications qui correspondent à certaines contraintes. Par exemple, vous pouvez accepter uniquement les revendications de\-courrier électronique du fournisseur de revendications ; par conséquent, vous devez utiliser ce modèle de règle pour\-accepter les types de revendication de courrier électronique qui se terminent par \(le DNS DNSdufournisseurderevendications.\) nom.  
  
## <a name="configuring-this-rule-on-a-relying-party-trust"></a>Configuration de cette règle sur une approbation de partie de confiance  
Lorsque vous utilisez une approbation de partie de confiance, cette règle peut être configurée pour transmettre ou filtrer les revendications sortantes qui seront envoyées à la partie de confiance. Certaines parties de confiance risquent de ne pas comprendre des types de revendication donnés, ou certaines revendications peuvent contenir des informations sensibles qui ne doivent pas être envoyées à certaines parties de confiance. Ce modèle de règle peut aider à appliquer ces stratégies pour une approbation de partie de confiance particulière.  
  
## <a name="how-to-create-this-rule"></a>Comment créer cette règle  
Vous créez cette règle à l’aide du langage de règle de revendication ou à l’aide du modèle de règle de transmission ou de filtrage d'\-une revendication entrante dans le composant logiciel enfichable Gestion des AD FS. Ce modèle de règle fournit les options de configuration suivantes :  
  
-   Spécifier un nom de règle de revendication  
  
-   Spécifier un type de revendication entrante  
  
-   Passer toutes les valeurs de revendication  
  
-   Transmettre uniquement une valeur de revendication spécifique  
  
-   Transmettre uniquement les valeurs de revendication qui correspondent à une\-valeur de suffixe de courrier électronique spécifique  
  
-   Transmettre uniquement les valeurs de revendication qui commencent par une valeur spécifique  
  
Pour plus d’instructions sur la création de ce modèle, consultez [créer une règle pour passer ou filtrer une revendication entrante](https://technet.microsoft.com/library/dd807060.aspx) dans le Guide de déploiement AD FS.  
  
## <a name="using-the-claim-rule-language"></a>Utilisation du langage des règles de revendication  
Si une revendication doit être envoyée uniquement lorsque la valeur de revendication correspond à un modèle personnalisé, vous devez utiliser une règle personnalisée. Pour plus d’informations, consultez la section Quand utiliser une règle personnalisée.  
  
### <a name="examples-of-how-to-construct-a-pass-through-or-filter-rule-syntax"></a>Exemples de syntaxe d’une règle de transmission ou de filtrage  
Une règle de filtrage simple permet de filtrer les revendications selon l’une des propriétés présentées ci-dessus. Par exemple, la règle suivante transmet toutes les revendications de\-courrier électronique :  
  
```  
c:[type == “http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress”]  => issue(claim  = c);  
```  
  
Les filtres peuvent être logiquement\-et Ed ensemble. Par exemple, la règle suivante accepte toutes les revendications\-de courrier électronique avec la valeurjohndoe@fabrikam.com:  
  
```  
c:[type == “http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress”, value == “johndoe@fabrikam.com “]  => issue(claim  = c);  
```  
  
Dans les exemples ci-dessus, les filtres utilisent toujours un opérateur d’égalité. Le langage des règles de revendication prend en charge les opérateurs suivants :  
  
-   \=\=est égal à\-respect de la casse \( \-\)  
  
-   \!\=n’est pas égal à la casse\- \( \-\)  
  
-   \=~\-correspondance d’expression régulière  
  
-   \!~ \-non\--correspondance d’expression régulière  
  
Par exemple, la règle suivante accepte toutes les revendications\-de courrier électronique non émises par le serveur de Fédération local qui ont le suffixe Boeing.com :  
  
```  
c:[type == “http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress”, value =~ “^.*@boeing\.com$” , issuer != “LOCAL AUTHORITY”]  => issue(claim  = c);  
```  
  
### <a name="best-practices-for-creating-custom-rules"></a>Meilleures pratiques pour la création de règles personnalisées  
Un filtre peut être appliqué à une ou plusieurs des propriétés de chaque revendication, comme décrit dans le tableau suivant.  
  

| Propriété de revendication |                                                                                                                                                                                                                                                                                                                                                  Description                                                                                                                                                                                                                                                                                                                                                  |
|----------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      Type      |                                                                                                                                                                                        Le type \(de revendication généralement représenté sous la\) forme d’un URI reflète un accord implicite entre partenaires dans une Fédération sur le type d’informations transmises dans la revendication. Par exemple, les revendications de type http\/:\/\/schemas.xmlsoap.org\/WS\/2005\/05\/revendications\/d’identité EmailAddress contiennent l’e\-adresse de messagerie de l’utilisateur.                                                                                                                                                                                         |
|     Value      |                                                                                                                                                                                                                                                                   Valeur de la revendication. Par exemple, une revendication de type http :\/\/schemas.xmlsoap.org\/WS\/2005\/05\/revendications\/\/d’identité EmailAddress peut avoir la valeurjohndoe@fabrikam.com                                                                                                                                                                                                                                                                    |
|   ValueType    |                                                                                                                                                                                                  ValueType représente la manière dont les informations contenues dans la valeur de la revendication doivent être interprétées. En général, ValueType est défini sur http :\/\/www.w3.org\/2001\/XmlSchema\#String, mais la valeur de la revendication peut contenir des données \(encodées à la valeur base64Binary, par exemple, une image \) ou une date, une valeur booléenne, etc.                                                                                                                                                                                                  |
|     Émetteur     | L’émetteur représente la partie qui a émis pour la dernière fois des revendications relatives à l’utilisateur. Si les revendications sont obtenues sur un serveur de fédération du fournisseur de revendications, l’émetteur de toutes les revendications va être associé à « Autorité locale ». Si les revendications ont été reçues par un serveur de fédération du fournisseur de fédération, l’émetteur des revendications va être configuré comme l’identifiant du fournisseur de revendications qui a signé le jeton. Par conséquent, lors du traitement des règles sur les revendications en provenance du fournisseur de revendications, l’émetteur de toutes les revendications va être associé à la même valeur. Lorsque vous créez des règles pour une partie de confiance, la propriété de l’émetteur peut être utilisée pour distinguer les revendications issues des différents fournisseurs de revendications. |
| Émetteur d’origine |                                                                                                   Cette propriété de revendication est destinée à transmettre le serveur de fédération qui a émis initialement la revendication. Étant donné que la propriété de l’émetteur des revendications est définie sur le dernier serveur de Fédération qui a signé le jeton, l’émetteur d’origine est utile dans les scénarios où une revendication est passée par \(plusieurs serveurs de Fédération, par exemple, une partie de confiance qui reçoit un jeton. un serveur de Fédération du fournisseur de Fédération peut être intéressé par le serveur de Fédération du fournisseur de revendications qui a authentifié l’utilisateur.\)                                                                                                   |
|   Properties   |                                                                                                                             Outre les cinq propriétés présentées ci-dessus, chaque revendication possède également un jeu de propriétés dans lequel peuvent être stockées les propriétés nommées. Ces propriétés ne sont pas sérialisées dans le jeton et ne sont pertinentes pour le transfert d’informations entre les composants du pipeline d’émission de revendications qu’au niveau d’un serveur de fédération unique. Par exemple, la configuration d’une propriété dans le cadre du traitement des règles du fournisseur de revendications, puis la référence à cette propriété dans les règles de la partie de confiance.                                                                                                                              |

