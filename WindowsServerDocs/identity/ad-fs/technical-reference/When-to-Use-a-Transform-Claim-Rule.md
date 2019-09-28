---
ms.assetid: 77aa61bf-9c04-4889-a5d2-6f45bc1b8bd2
title: Quand utiliser une règle de revendication de transformation
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: b7cdf68783db1b6b775209e4e42dc6b6ccf0e1b8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385421"
---
# <a name="when-to-use-a-transform-claim-rule"></a>Quand utiliser une règle de revendication de transformation
Vous pouvez utiliser cette règle dans Services ADFS \(AD FS @ no__t-1 lorsque vous devez mapper un type de revendication entrante à un type de revendication sortante, puis appliquer une action qui détermine la sortie qui doit se produire en fonction des valeurs provenant de la revendication entrante. Lorsque vous utilisez cette règle, vous transmettez ou transformez les revendications qui correspondent à la logique de règle suivante, en fonction des options que vous configurez dans la règle, comme décrit dans le tableau suivant :  
  
|Option de règle|Logique de règle|  
|---------------|--------------|  
|Passer toutes les revendications entrantes|Si le type de revendication entrante est égal au *type de revendication spécifiée* et que la valeur est égale à une *valeur quelconque*, passez alors la revendication avec le type de revendication sortante égal au *type de revendication spécifiée*|  
|Remplacer une valeur de revendication entrante avec une valeur de revendication sortante|Si le type de revendication entrante est égal au *type de revendication spécifiée* et que la valeur est égale à la *valeur de revendication spécifiée*, transformez alors la revendication avec la nouvelle valeur de revendication sortante *valeur de revendication spécifiée* et avec le type de revendication sortante *type de revendication spécifiée*|  
|Remplacement des revendications e @ no__t-0mail entrantes par un nouveau suffixe e @ no__t-1mail|Si le type de revendication entrante est égal au *type de revendication spécifiée* et que la valeur est égale à une *valeur quelconque de suffixe*, transformez alors la revendication avec la nouvelle valeur de revendication sortante *valeur de suffixe spécifiée* et avec le type de revendication sortante *type de revendication spécifiée*|  
  
Les sections suivantes constituent une introduction aux règles de revendication et fournissent des informations détaillées sur les conditions d’utilisation de cette règle.  
  
## <a name="about-claim-rules"></a>À propos des règles de revendication  
Une règle de revendication représente une instance de logique métier qui prend une revendication entrante, lui applique une condition \(si x Then y\) et génère une revendication sortante basée sur les paramètres de condition. La liste suivante présente d’importantes astuces sur les règles de revendication dont vous devez prendre connaissance avant de poursuivre la lecture de cette rubrique :  
  
-   Dans le composant logiciel enfichable\-gestion des AD FS, les règles de revendication ne peuvent être créées qu’à l’aide de modèles de règles de revendication  
  
-   Les règles de revendication traitent les revendications entrantes soit \(directement à partir d’un fournisseur\) de revendications, par exemple Active Directory ou une autre service FS (Federation Service), soit à partir de la sortie des règles de transformation d’acceptation sur une approbation de fournisseur de revendications.  
  
-   Les règles de revendication sont traitées par le moteur d’émission des revendications au sein d’un ensemble de règles donné et dans l’ordre chronologique. En définissant la hiérarchie des règles, vous pouvez affiner ou filtrer des revendications générées par des règles précédentes dans un ensemble de règles donné.  
  
-   Les modèles de règles de revendication vous obligent toujours à spécifier un type de revendication entrante. Toutefois, vous pouvez traiter plusieurs valeurs de revendication avec le même type de revendication en vous appuyant sur une règle unique.  
  
Pour plus d’informations sur les règles de revendication et les ensembles de règles de revendication, consultez [rôle des règles de revendication](The-Role-of-Claim-Rules.md). Pour plus d’informations sur la façon dont les règles sont traitées, consultez [le rôle du moteur de revendications](The-Role-of-the-Claims-Engine.md). Pour plus d’informations sur le traitement des ensembles de règles de revendication, consultez [le rôle du pipeline de revendications](The-Role-of-the-Claims-Pipeline.md).  
  
## <a name="pass-through-all-claim-values"></a>Passer toutes les valeurs de revendication  
Lorsque vous utilisez cette action, toutes les valeurs de revendication entrante qui correspondent à un type de revendication entrante spécifiée sont mappées à un type de revendication sortante spécifiée avant d’être envoyées en tant que revendications sortantes en jetons signés par votre service FS (Federation Service).  
  
Par exemple, lorsqu’une règle est définie avec la logique d’option **Passer toutes les valeurs de revendication** et le type de revendication entrante de groupe, et que le type de revendication sortante du rôle est spécifié, toutes les valeurs de revendication entrante dans un flux de l’émetteur sont copiées individuellement dans les nouvelles revendications sortantes avec le type de revendication de rôle.  
  
## <a name="transforming-a-claim"></a>Transformation d’une revendication  
Dans AD FS, le terme *transformation des revendications* signifie remplacer une valeur de revendication entrante par une autre valeur de revendication sortante. C’est la règle de transformation d’une revendication entrante qui permet cette fonction de transformation. Dans les propriétés de cette règle, vous pouvez définir des conditions pour transformer les valeurs entrantes en une valeur de revendication sortante selon le type de revendication entrante spécifiée.  
  
Par exemple, comme indiqué dans l’illustration suivante, lorsqu’une règle est définie avec la condition permettant de remplacer une valeur entrante par une valeur de revendication sortante, tous les types de revendication entrante de groupe sont mappés aux nouveaux types de revendication sortante de rôle. Dans ce cas, la valeur de revendication entrante de l’acheteur est remplacée par la nouvelle valeur de revendication sortante de l’administrateur.  
  
![Quand utiliser une transformation](media/adfs2_transform.gif)  
  
Vous pouvez également utiliser cette règle pour appliquer une condition qui remplace toutes les revendications entrantes par une valeur de suffixe e @ no__t-0mail spécifiée par une nouvelle valeur. Par exemple, vous pouvez définir une condition dans cette règle pour modifier toutes les valeurs de revendication de suffixe de sales.corp.fabrikam.com par fabrikam.com.  
  
## <a name="configuring-this-rule-on-a-claims-provider-trust"></a>Configuration de cette règle sur une approbation de fournisseur de revendications  
Lorsque vous utilisez une approbation de fournisseur de revendications, cette règle peut être configurée pour transformer les revendications entrantes du fournisseur de revendications en équivalents dignes de confiance. Les types de revendication ou les valeurs de revendication peuvent avoir une signification dans votre entreprise et en avoir une autre dans les organisations du fournisseur de revendications. Cette règle permet de normaliser les types de revendications et les valeurs provenant du fournisseur de revendications afin que leurs équivalents de revendication sortante puissent être reconnus par la partie de confiance.  
  
## <a name="configuring-this-rule-on-a-relying-party-trust"></a>Configuration de cette règle sur une approbation de partie de confiance  
Lorsque vous utilisez une approbation de partie de confiance, cette règle peut être configurée pour transformer des revendications pour la partie de confiance spécifique. Les types de revendication ou les valeurs de revendication peuvent avoir une signification différente pour une partie de confiance spécifique et cette règle vous permet de modifier les types de revendications sortantes et les valeurs pour une seule partie de confiance.  
  
## <a name="how-to-create-this-rule"></a>Comment créer cette règle  
Vous créez cette règle à l’aide du langage de règle de revendication ou à l’aide du modèle de règle **transformer une revendication entrante** dans le composant logiciel enfichable de gestion AD FS @ no__t-1Dans. Ce modèle de règle fournit les options de configuration suivantes :  
  
-   Spécifier un nom de règle de revendication  
  
-   Transformer un type de revendication entrante spécifique en un type de revendication sortante spécifiée  
  
-   Passer toutes les valeurs de revendication  
  
-   Remplacer une valeur de revendication entrante avec une valeur de revendication sortante  
  
-   Remplacement des revendications de suffixe e @ no__t-0mail entrantes par un nouveau suffixe e @ no__t-1mail  
  
Pour plus d’instructions sur la création de ce modèle, consultez [créer une règle pour transformer une revendication entrante](https://technet.microsoft.com/library/dd807068.aspx) dans le Guide de déploiement AD FS.  
  
## <a name="using-the-claim-rule-language"></a>Utilisation du langage des règles de revendication  
Si la revendication sortante doit être construite à partir du contenu de plusieurs revendications entrantes, vous devez utiliser une règle personnalisée à la place. Si la valeur de revendication de la revendication sortante doit être basée sur la valeur de la revendication entrante, mais avec du contenu supplémentaire, vous devez également utiliser une règle personnalisée dans ce contexte. Pour plus d'informations, voir [When to Use a Custom Claim Rule](When-to-Use-a-Custom-Claim-Rule.md).  
  
### <a name="examples-of-how-to-construct-a-transform-rule-syntax"></a>Exemples montrant comment construire une syntaxe de règle de transformation  
Lorsque vous utilisez la syntaxe du langage de règles de revendication pour transformer les revendications, vous pouvez définir une propriété de la revendication transformée sur une nouvelle valeur littérale. Par exemple, la règle suivante modifie la valeur des revendications de rôle de « Administrateurs » en « root » tout en conservant le même type de revendication :  
  
```  
c:[type == “https://schemas.microsoft.com/ws/2008/06/identity/claims/role”, value == “Administrators”]  => issue(type = c.type, value = “root”);  
```  
  
Les expressions régulières peuvent également servir aux transformations de revendication. Par exemple, la règle suivante définit le domaine dans les revendications de nom d’utilisateur Windows au format domaine @ no__t-0USER sur FABRIKAM :  
  
```  
c:[type == "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name"] => issue(type = c.type, value = regexreplace(c.value, "(?<domain>[^\\]+)\\(?<user>.+)", "FABRIKAM\${user}"));  
```  
  
### <a name="best-practices-for-creating-custom-rules"></a>Meilleures pratiques pour la création de règles personnalisées  
Les transformations de revendications peuvent être appliquées de façon sélective aux revendications sélectionnées à l’aide des fonctionnalités de filtrage de base. Chacune des propriétés de revendication utilisées pour le filtrage peut être une valeur affectée, avec les caractéristiques suivantes :  
  
|Propriété de revendication|Description|  
|------------------|---------------|  
|Type, Value, ValueType|Ces propriétés servent fréquemment aux affectations. Au minimum, le type et la valeur doivent être spécifiés pour la revendication transformée résultante.|  
|Émetteur|Tandis que le langage des règles de revendication permet la définition de l’émetteur (Issuer) de la demande, cela n’est généralement pas recommandé. L’émetteur de la revendication n’est pas sérialisé dans le jeton. À la réception d’un jeton, la propriété Issuer de toutes les revendications prend la valeur de l’identificateur du serveur FS (Federation Server) qui a signé le jeton. Par conséquent, la définition de l’émetteur d’une revendication dans les règles n’aura aucun effet sur le contenu du jeton et le paramètre est perdu une fois que la revendication est empaquetée dans un jeton. Le seul scénario qui justifie la définition de l’émetteur d’une revendication est le suivant : si elle prend une valeur spécifique dans l’ensemble de règles du fournisseur de revendications et que l’ensemble de règles de la partie de confiance a été créé avec des règles faisant référence à cette valeur spécifique. Si la propriété Issuer n’est pas définie explicitement dans une règle de revendication, le moteur d’émission des revendications lui affecte la valeur « LOCAL AUTHORITY ».|  
|Émetteur d’origine|Comme pour la propriété Issuer, OriginalIssuer ne doit généralement pas avoir une valeur explicitement affectée. Contrairement à la propriété Issuer, OriginalIssuer est sérialisée dans le jeton, mais les attentes des consommateurs de jeton impliquent que, si une valeur lui est affectée, elle contient l’identificateur du serveur FS (Federation Server) qui a émis la revendication initiale.|  
|Properties|Comme indiqué dans la section précédente, le jeu de propriétés de la revendication n’est pas conservé dans le jeton. Ainsi, les affectations aux propriétés doivent être effectuées uniquement si des stratégies locales sous-jacentes vont référencer les informations stockées dans la propriété.|  
  
Pour plus d’informations sur l’utilisation du langage de règle de revendication, consultez [le rôle du langage de règle de revendication](The-Role-of-the-Claim-Rule-Language.md).  
  

