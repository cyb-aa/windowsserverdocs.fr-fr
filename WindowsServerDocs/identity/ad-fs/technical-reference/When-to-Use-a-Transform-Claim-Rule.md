---
ms.assetid: 77aa61bf-9c04-4889-a5d2-6f45bc1b8bd2
title: "Quand utiliser une règle de revendication de transformation"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: c7b7ea2c8d9a08a4cbf6c89c2de2482043efe25b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

# <a name="when-to-use-a-transform-claim-rule"></a>Quand utiliser une règle de revendication de transformation
Vous pouvez utiliser cette règle dans Active Directory Federation Services \(AD FS\) lorsque vous devez mapper un type de revendication entrante à un type de revendication sortante, puis appliquez une action qui détermine quelle sortie doit se produire en fonction des valeurs d’origine de la revendication entrante. Lorsque vous utilisez cette règle, vous transmettez ou transformez les revendications qui correspondent à la logique de règle suivante, selon une des options que vous configurez dans la règle, comme décrit dans le tableau suivant.  
  
|Option de règle|Logique de règle|  
|---------------|--------------|  
|Passer toutes les revendications entrantes|Si le type de revendication entrante est égal à *type de revendication spécifiée* et que la valeur est égale à *n’importe quelle valeur*, puis transmettez la revendication avec est égale à des type de revendication sortante *type de revendication spécifiée*|  
|Remplacer une valeur de revendication entrante avec une valeur de revendication sortante|Si le type de revendication entrante est égal à *type de revendication spécifiée* et que la valeur est égale à *valeur de revendication spécifiée*, transformez alors la revendication avec la nouvelle valeur de revendication sortante *valeur de revendication spécifiée* et avec revendication sortante type *type de revendication spécifiée*|  
|Remplacement des revendications de suffixe de messagerie e\ entrantes par un nouveau suffixe de messagerie e\|Si le type de revendication entrante est égal à *type de revendication spécifiée* et que la valeur est égale à *une valeur de suffixe*, transformez alors la revendication avec la nouvelle valeur de revendication sortante *valeur de suffixe spécifiée* et avec revendication sortante type *type de revendication spécifiée*|  
  
Les sections suivantes fournissent une introduction aux règles de revendication et fournissent des informations détaillées sur l’utilisation de cette règle.  
  
## <a name="about-claim-rules"></a>À propos des règles de revendication  
Une règle de revendication représente une instance de logique métier qui prend une revendication entrante, lui appliquer une condition \ (s’il x y\ puis) et génère une revendication sortante basée sur les paramètres de condition. La liste suivante présente d’importantes astuces que vous devez connaître les règles de revendication avant de poursuivre la lecture de cette rubrique:  
  
-   Dans la gestion AD FS de composants, les règles de revendication ne peuvent être créés à l’aide de modèles de règle de revendication  
  
-   Processus de règles de revendication entrante de revendications directement à partir d’un fournisseur de revendications \ (par exemple, Active Directory ou un autre fédération Service\) ou de la sortie de l’acceptation de règles de transformation sur une approbation de fournisseur de revendications.  
  
-   Les règles de revendication sont traitées par le moteur d’émission des revendications dans l’ordre chronologique, dans un ensemble de règles donné. En définissant la hiérarchie des règles, vous pouvez affiner ou filtrer les revendications qui sont générées par les règles précédentes au sein d’un ensemble de règles donné.  
  
-   Modèles de règle de revendication vous obligent toujours à spécifier un type de revendication entrante. Toutefois, vous pouvez traiter plusieurs valeurs de revendication avec le même type de revendication à l’aide d’une règle unique.  
  
Pour plus d’informations sur les règles de revendication et les jeux de règles de revendication, consultez [le rôle des règles de revendication](The-Role-of-Claim-Rules.md). Pour plus d’informations sur le traitement des règles, voir [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md). Pour plus d’informations, mode de traitement des jeux de règles de revendication, voir [The Role of the Claims Pipeline](The-Role-of-the-Claims-Pipeline.md).  
  
## <a name="pass-through-all-claim-values"></a>Passer toutes les valeurs de revendication  
Lorsque vous utilisez cette action, toutes les valeurs de revendication entrante qui correspondent à un type de revendication entrante spécifiée sont mappées à un type de revendication sortante spécifiée avant qu’ils sont envoyés en tant que revendications sortantes en jetons signés par votre Service de fédération.  
  
Par exemple, lorsqu’une règle est définie avec la **passer toutes les valeurs de revendication** logique d’option et entrants de revendication de groupe et le type de revendication sortante du rôle est spécifié, toutes les valeurs de revendication entrante qui proviennent de l’émetteur sont copiées individuellement dans les nouvelles revendications sortantes avec le type de revendication de rôle.  
  
## <a name="transforming-a-claim"></a>Transformation d’une revendication  
Dans AD FS, le terme *transformation des revendications* valeur avec une valeur de revendication sortante de revendication implique le remplacement d’une entrée. Il s’agit de la transformation une règle de revendication entrante qui permet cette fonction. Dans les propriétés de cette règle, vous pouvez définir des conditions pour transformer les valeurs entrantes avec une valeur de revendication sortante selon le type de revendication entrante spécifiée.  
  
Par exemple, comme indiqué dans l’illustration suivante, lorsqu’une règle est définie avec la condition pour remplacer une valeur entrante par une valeur de revendication sortante, tous les types de revendication entrante de groupe sont mappés aux nouveaux types de revendication sortante du rôle. Dans ce cas, la valeur de revendication entrante de l’acheteur est remplacée par la nouvelle valeur de revendication sortante d’administrateur.  
  
![Quand utiliser une transformation](media/adfs2_transform.gif)  
  
Vous pouvez également utiliser cette règle pour appliquer une condition qui remplace toutes les revendications entrantes avec une valeur de suffixe de messagerie e\ spécifié avec une nouvelle valeur. Par exemple, vous pouvez définir une condition dans cette règle pour modifier toutes les valeurs de revendication avec le suffixe de Sales.corp.fabrikam.com par Fabrikam.com.  
  
## <a name="configuring-this-rule-on-a-claims-provider-trust"></a>Configuration de cette règle sur une approbation de fournisseur de revendications  
Lorsque vous utilisez une approbation de fournisseur de revendications, cette règle peut être configurée pour transformer les revendications entrantes du fournisseur de revendications en équivalents dignes de confiance. Types de revendication ou les valeurs peuvent avoir une signification différente dans votre entreprise et dans les organisations de fournisseur de revendications de revendication. Vous pouvez utiliser cette règle afin de normaliser les types de revendications et les valeurs qui proviennent du fournisseur de revendications afin que leurs équivalents de revendication sortante puissent être reconnus par la partie de confiance.  
  
## <a name="configuring-this-rule-on-a-relying-party-trust"></a>Configuration de cette règle sur une approbation de partie de confiance  
Lorsque vous utilisez une approbation de partie de confiance, cette règle peut être configurée pour transformer les revendications pour la partie de confiance spécifique. Types de revendication ou les valeurs peuvent avoir une signification différente pour une partie de confiance spécifique, et cette règle permet de modifier les types de revendications sortantes et les valeurs pour une seule partie de confiance de revendication.  
  
## <a name="how-to-create-this-rule"></a>Comment créer cette règle  
Vous créez cette règle à l’aide du langage de règle de revendication ou les **transformer une revendication entrante** modèle de règle dans la gestion AD FS de composants. Ce modèle de règle fournit les options de configuration suivantes:  
  
-   Spécifiez un nom de règle de revendication  
  
-   Transformer un type de revendication entrante spécifique pour un type de revendication sortante spécifiée  
  
-   Passer toutes les valeurs de revendication  
  
-   Remplacer une valeur de revendication entrante avec une valeur de revendication sortante  
  
-   Remplacer les revendications de suffixe e\ de messagerie entrantes par un nouveau suffixe de messagerie e\  
  
Pour plus d’informations sur la création de ce modèle, voir [créer une règle pour transformer une revendication entrante](https://technet.microsoft.com/library/dd807068.aspx) dans le Guide de déploiement d’AD FS.  
  
## <a name="using-the-claim-rule-language"></a>À l’aide du langage de règle de revendication  
Si la revendication sortante doit être construite à partir du contenu de plusieurs revendications entrantes, vous devez utiliser une règle personnalisée à la place. Si la valeur de revendication de la revendication sortante doit être basée sur la valeur de la revendication entrante, mais avec du contenu supplémentaire, vous devez également utiliser une règle personnalisée dans ce contexte. Pour plus d’informations, voir [quand utiliser une règle de revendication personnalisée](When-to-Use-a-Custom-Claim-Rule.md).  
  
### <a name="examples-of-how-to-construct-a-transform-rule-syntax"></a>Exemples montrant comment construire une syntaxe de règle de transformation  
Lorsque vous utilisez la syntaxe du langage de règle revendication pour transformer les revendications, vous pouvez définir une propriété de la revendication transformée sur une nouvelle valeur littérale. Par exemple, la règle suivante modifie la valeur des revendications de rôle de «Administrateurs» en «root» tout en conservant le même type de revendication:  
  
```  
c:[type == “https://schemas.microsoft.com/ws/2008/06/identity/claims/role”, value == “Administrators”]  => issue(type = c.type, value = “root”);  
```  
  
Les expressions régulières peuvent également servir aux transformations de revendication. Par exemple, la règle suivante définit le domaine dans les revendications de nom d’utilisateur windows au format de l’option Domaine\\Nom d’utilisateur pour FABRIKAM:  
  
```  
c:[type == "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name"] => issue(type = c.type, value = regexreplace(c.value, "(?<domain>[^\\]+)\\(?<user>.+)", "FABRIKAM\${user}"));  
```  
  
### <a name="best-practices-for-creating-custom-rules"></a>Meilleures pratiques pour la création de règles personnalisées  
Les transformations de revendications peuvent être appliquées de façon sélective aux revendications sélectionnées à l’aide des fonctionnalités de filtrage de base. Chacun des propriétés de revendication utilisées pour le filtrage peut être affecté à valeurs, avec les caractéristiques suivantes:  
  
|Propriété de revendication|Description|  
|------------------|---------------|  
|Type de valeur, le type de valeur|Ces propriétés plus fréquemment servira des affectations. Sur le type et la valeur minimum doivent être spécifiés pour la revendication transformée résultante.|  
|Émetteur|Le langage de règle de revendication permet la définition de l’émetteur d’une revendication, cela n’est généralement pas recommandé. L’émetteur d’une revendication n’est pas sérialisé dans le jeton. Lors de la réception d’un jeton de la propriété Issuer de toutes les revendications est définie à l’identificateur du serveur de fédération qui a signé le jeton. Par conséquent, la définition de l’émetteur d’une revendication dans les règles n’aura pas effet sur le contenu du jeton et le paramètre seront perdu une fois que la revendication est empaquetée dans un jeton. Le seul scénario où la définition de l’émetteur d’une revendication est justifié est si elle est définie sur une valeur spécifique dans l’ensemble de règles de fournisseur de revendications et une partie de confiance de règles de jeu a été créé avec des règles qui font référence à cette valeur spécifique. Si la propriété Issuer n’est pas explicitement définie sur une valeur dans une règle de revendication que le moteur d’émission des revendications lui affecte la valeur «LOCAL AUTHORITY».|  
|OriginalIssuer|De même pour Issuer, OriginalIssuer doit généralement pas être explicitement affecté une valeur. Contrairement à l’émetteur, la propriété OriginalIssuer est sérialisée dans le jeton, mais les attentes des consommateurs de jeton sont que, si la valeur, elle contient l’identificateur du serveur de fédération qui a émis la revendication.|  
|Propriétés|Comme indiqué dans la section précédente, le conteneur des propriétés de la revendication n’est pas conservé dans le jeton, afin des affectations aux propriétés ne doivent être effectuées que si des stratégies locales sous-jacentes vont référencer les informations stockées dans la propriété.|  
  
Pour plus d’informations sur l’utilisation du langage de règle de revendication, voir [le rôle du langage de règle de revendication](The-Role-of-the-Claim-Rule-Language.md).  
  

