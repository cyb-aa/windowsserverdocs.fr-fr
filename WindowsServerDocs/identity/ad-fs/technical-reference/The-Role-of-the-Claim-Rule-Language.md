---
title: "Le rôle du langage de règle de revendication"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: dda9d148-d72f-4bff-aa2a-f2249fa47e4c
ms.technology: identity-adfs
ms.openlocfilehash: 206da33d33cbded0450db29615dc74eb1161cbce
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/07/2017
---
>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

# <a name="the-role-of-the-claim-rule-language"></a>Le rôle du langage de règle de revendication
Les Services de fédération ActiveDirectory (ADFS) de revendication langage de règle joue le bloc de construction administratif pour le comportement des revendications entrantes et sortantes, tandis que le moteur de revendications agit comme moteur de traitement de la logique dans le langage de règle de revendication qui définit la règle personnalisée. Pour plus d’informations sur la façon de toutes les règles sont traitées par le moteur de revendications, voir [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md).  
  
## <a name="creating-custom-claim-rules-using-the-claim-rule-language"></a>Création de règles de revendication personnalisée à l’aide du langage de règle de revendication  
ADFS fournit aux administrateurs la possibilité de définir des règles personnalisées qu’ils peuvent utiliser pour déterminer le comportement des revendications d’identité avec le langage de règle de revendication. Vous pouvez utiliser les exemples de syntaxe du langage de règle revendication dans cette rubrique pour créer une règle personnalisée qui énumère, ajoute, supprime et modifie les revendications pour répondre aux besoins de votre organisation. Vous pouvez créer des règles personnalisées en tapant dans la syntaxe du langage de règle revendication dans le **envoyer des revendications à l’aide d’une revendication personnalisée** modèle de règle.  
  
Les règles sont séparés par des points-virgules.  
  
Pour plus d’informations sur l’utilisation des règles personnalisées, voir [quand utiliser une règle de revendication personnalisée](When-to-Use-a-Custom-Claim-Rule.md).  
  
## <a name="using-claim-rule-templates-to-learn-about-the-claim-rule-language-syntax"></a>Pour en savoir plus sur la syntaxe du langage de règle revendication à l’aide de modèles de règle de revendication  
ADFS fournit également un ensemble d’émission de revendication prédéfinis et modèles de règles d’acceptation que vous pouvez utiliser pour implémenter courantes des règles de revendication de revendication. Dans le **modifier les règles de revendication** boîte de dialogue pour une approbation donnée, vous pouvez créer une règle prédéfinie — et afficher la syntaxe du langage de règle revendication qui rend cette règle, en cliquant sur le **afficher le langage de règle** onglet correspondant à cette règle. En utilisant les informations de cette section et la **afficher le langage de règle** technique peut vous aider à élaborer vos propres règles personnalisées.  
  
Pour plus d’informations sur les règles de revendication et les modèles de règle de revendication, consultez [le rôle des règles de revendication](The-Role-of-Claim-Rules.md).  
  
## <a name="understanding-the-components-of-the-claim-rule-language"></a>Présentation des composants du langage de règle de revendication  
Le langage de règle de revendication se compose des éléments suivants, séparés par le «= >» opérateur:  
  
-   Une condition  
  
-   Une instruction d’émission  
  
### <a name="conditions"></a>Conditions  
Vous pouvez utiliser des conditions dans une règle pour vérifier les revendications d’entrée et de déterminer si l’instruction d’émission de la règle doit être exécutée. Une condition est une expression logique qui doit être évaluée sur true pour exécuter la partie du corps de la règle. Si cette partie est manquante, une valeur logique true est supposée; autrement dit, les corps de la règle est toujours exécuté. La partie conditions contient une liste des conditions qui sont associées à l’opérateur de conjonction logique («& &»). Toutes les conditions de la liste doivent être évaluées à vrai pour la partie entière conditionnelle soit évaluée sur true. La condition peut être un opérateur de sélection de revendications ou un appel de fonction d’agrégation. Ces deux s’excluent mutuellement, ce qui signifie que les sélecteurs de revendication et les fonctions d’agrégation ne peuvent pas être combinées dans une partie de conditions de règle unique.  
  
Les conditions sont facultatives dans les règles. Par exemple, la règle suivante ne dispose pas d’une condition:  
  
```  
=> issue(type = "http://test/role", value = "employee");  
```  
  
Il existe trois types de conditions:  
  
-   Condition unique: il s’agit d’une condition de la forme la plus simple. Vérifications sont effectuées pour une seule expression; par exemple, nom du compte windows = utilisateur du domaine.  
  
-   Conditions multiples: cette condition requiert des vérifications supplémentaires pour traiter plusieurs expressions dans le corps de la règle; par exemple, nom du compte windows = utilisateur de domaine et groupe = contosopurchasers.  
  
> [!NOTE]  
> Une autre condition existe, mais il est un sous-ensemble de condition unique ou de conditions multiples. Il est appelé une condition d’expression régulière (Regex). Il est utilisé pour prendre une expression d’entrée et correspond à l’expression avec un modèle donné. Un exemple de la façon dont elle est peut être utilisé est présentée ci-dessous.  
  
Les exemples suivants montrent quelques-unes des constructions de syntaxe, qui sont basées sur les types de conditions, que vous pouvez utiliser pour créer des règles personnalisées.  
  
#### <a name="single--condition-examples"></a>Seul - exemples de condition  
Single - conditions d’expression sont décrites dans le tableau suivant. Elles sont construites pour simplement vérifier une revendication avec un type de revendication spécifiée ou une revendication avec un type de revendication spécifiée et la valeur de revendication.  
  
|Description de la condition|Exemple de syntaxe de condition|  
|-------------------------|----------------------------|  
|Cette règle contient une condition pour vérifier une revendication d’entrée avec un type de revendication spécifique («http://test/name»). Si une revendication correspondante est présente dans les revendications d’entrée, la règle copie la revendication correspondante ou des revendications à l’ensemble de revendications de sortie.|``` c: [type == "http://test/name"] => issue(claim = c );```|  
|Cette règle contient une condition pour vérifier une revendication d’entrée avec un type de revendication spécifique («http://test/name») et la valeur («Terry») de revendication. Si une revendication correspondante est présente dans les revendications d’entrée, la règle copie la revendication correspondante ou des revendications à l’ensemble de revendications de sortie.|``` c: [type == "http://test/name", value == "Terry"] => issue(claim = c);```|  
  
Plus - conditions complexes sont affichés dans la section suivante, y compris les conditions pour vérifier plusieurs revendications, les conditions pour vérifier l’émetteur d’une revendication et conditions vérifier les valeurs qui correspondent à un modèle d’expression régulière.  
  
#### <a name="multiple--condition-examples"></a>Plusieurs - exemples de condition  
Le tableau suivant fournit un exemple de plusieurs - conditions d’expression.  
  
|Description de la condition|Exemple de syntaxe de condition|  
|-------------------------|----------------------------|  
|Cette règle contient une condition pour vérifier deux revendications d’entrée, chacun avec un type de revendication spécifiée («http://test/name» et «http://test/email»). Si les deux revendications correspondantes sont présentes dans les revendications d’entrée, la règle copie la revendication de nom vers le jeu de revendications de sortie.|``` c1: [type  == "http://test/name"] && c2: [type == "http://test/email"] => issue (claim  = c1 );```|  
  
#### <a name="regular--condition-examples"></a>Normal - exemples de condition  
Le tableau suivant fournit un exemple d’une expression régulière,-en fonction de condition.  
  
|Description de la condition|Exemple de syntaxe de condition|  
|-------------------------|----------------------------|  
|Cette règle contient une condition qui utilise une expression régulière pour vérifier un e-revendication de courrier se terminant par «@fabrikam.com». Si une revendication correspondante est trouvée dans les revendications d’entrée, la règle copie la revendication correspondante vers le jeu de revendications de sortie.|```c: [type  == "http://test/email", value  =~ "^. +@fabrikam.com$" ] => issue (claim  = c );```|  
  
### <a name="issuance-statements"></a>Instructions d’émission  
Règles personnalisées sont traitées selon les instructions d’émission (*problème* ou *ajouter* ) que vous programmez dans la règle de revendication. En fonction du résultat souhaité, l’instruction issue ou ajouter peut être écrite dans la règle pour remplir le jeu de revendications d’entrée ou de sortie. Une règle personnalisée qui utilise l’instruction add explicitement remplit revendication valeurs uniquement à l’entrée de jeu de revendications pendant une règle de revendication personnalisée qui utilise l’instruction issue remplit revendication valeurs entrée jeu de revendications et dans la sortie de jeu de revendications. Cela peut être utile lorsqu’une valeur de revendication est destinée à être utilisée uniquement par les futures règles dans l’ensemble de règles de revendication.  
  
Par exemple, dans l’illustration suivante, la revendication entrante est ajoutée au jeu par le moteur d’émission des revendications de revendications d’entrée. Lorsque la première règle de revendication personnalisée s’exécute et les critères d’utilisateur de domaine est rempli, le moteur d’émission des revendications traite la logique de la règle à l’aide de l’instruction add et la valeur de **éditeur** est ajoutée au jeu de revendications d’entrée. Étant donné que la valeur de l’éditeur est présente dans le jeu de revendications d’entrée, règle 2 peut correctement traiter l’instruction issue dans sa logique et générer une nouvelle valeur de **Hello**, qui est ajoutée à la sortie de ces deux jeu de revendications et définir la revendication d’entrée définie pour une utilisation par la règle suivante dans la règle. Règle 3 peut désormais utiliser toutes les valeurs qui sont présents dans le jeu en tant qu’entrée pour traiter sa logique de revendications d’entrée.  
  
![Rôles ADFS](media/adfs2_customrule.gif)  
  
#### <a name="claim-issuance-actions"></a>Actions d’émission de revendication  
Le corps de la règle représente une action d’émission de revendication. Il existe deux actions d’émission de revendication reconnaît la langue:  
  
-   **Instruction issue:** l’instruction issue crée une revendication qui est redirigée vers l’entrée et la sortie des jeux de revendications. Par exemple, l’instruction suivante émet une nouvelle revendication basée sur son jeu de revendications d’entrée:  
  
    ```c:[type == "Name"] => issue(type = "Greeting", value = "Hello " + c.value);```  
  
-   **Instruction Add:** l’instruction add crée une nouvelle revendication qui est ajoutée uniquement à la collection de jeu de revendications d’entrée. Par exemple, l’instruction suivante ajoute une nouvelle revendication au jeu de revendications d’entrée:  
  
    ```c:[type == "Name", value == "domain user"] => add(type = "Role", value = "Editor");``` 
  
L’instruction d’émission d’une règle définit les revendications qui seront émises par la règle lorsque les conditions sont remplies. Il existe deux formes d’instructions d’émission en ce qui concerne les arguments et le comportement de l’instruction:  
  
-   **Normal**: ces instructions d’émission peuvent émettre des revendications à l’aide des valeurs littérales dans la règle ou les valeurs des revendications qui correspondent aux conditions. Une instruction d’émission normal peut être constitué d’au moins des formats suivants:  
  
    -   *Copie de la revendication*: ce format crée une copie de la revendication existante dans le jeu de revendications de sortie. Cette forme d’émission est uniquement applicable lorsqu’elle est combinée avec l’instruction d’émission «issue». Lorsqu’elle est combinée avec l’instruction d’émission «ajouter», il n’a pas d’effet.  
  
    -   *Nouvelle revendication*: ce format crée une nouvelle revendication d’après les valeurs des diverses propriétés de revendication. Claim.Type doit être spécifié; toutes les autres propriétés de revendication sont facultatives. L’ordre des arguments pour cet écran est ignoré.  
  
-   **Magasin d’attributs**: cette instruction crée des revendications avec des valeurs qui sont récupérés à partir d’un magasin d’attributs. Il est possible de créer plusieurs types de revendications en utilisant une instruction d’émission unique, ce qui est importante pour les magasins d’attributs qui rendent les opérations de (e/s) d’entrée/sortie réseau ou disque pendant la récupération de l’attribut. Par conséquent, il est préférable de limiter le nombre d’allers-retours entre le moteur de stratégie et le magasin d’attributs. Vous pouvez également créer plusieurs revendications pour un type de revendication donné. Lorsque le magasin d’attributs renvoie plusieurs valeurs pour un type de revendication donné, l’instruction d’émission crée automatiquement une revendication pour chaque valeur de revendication retournée. Une implémentation du magasin d’attributs utilise les arguments param pour remplacer les espaces réservés dans l’argument de requête avec les valeurs qui sont fournies dans les arguments param. Les espaces réservés utilisent la même syntaxe comme fonction String.Format .NET () (par exemple, {1}, {2} et ainsi de suite). L’ordre des arguments pour cette forme d’émission est important, et il doit être l’ordre indiqué dans la grammaire suivante.  
  
Le tableau suivant décrit certaines constructions de syntaxe courantes pour les deux types d’instructions d’émission dans les règles de revendication.  
  
|Type d’instruction d’émission|Description de l’instruction d’émission|Exemple de syntaxe d’instruction d’émission|  
|---------------------------|----------------------------------|-------------------------------------|  
|Normal|La règle suivante émet toujours la même revendication chaque fois qu’un utilisateur a le type de revendication spécifiée et la valeur:|```c: [type  == "http://test/employee", value  == "true"] => issue (type = "http://test/role", value = "employee");```|  
|Normal|La règle suivante convertit un type de revendication en un autre. Notez que la valeur de la revendication qui correspond à la condition «c» est utilisée dans l’instruction d’émission.|```c: [type  == "http://test/group" ] => issue (type  = "http://test/role", value  = c.Value );```|  
|Magasin d’attributs|La règle suivante utilise la valeur d’une revendication entrante pour demander le magasin d’attributs ActiveDirectory:|```c: [Type  == "http://test/name" ] => issue (store  = "Enterprise AD Attribute Store", types  =  ("http://test/email" ), query  = ";mail;{0}", param  = c.Value )```|  
|Magasin d’attributs|La règle suivante utilise la valeur d’une revendication entrante pour interroger un magasin d’attributs de langage SQL (Structured Query) précédemment configuré:|```c: [type  == "http://test/name"] => issue (store  = "Custom SQL store", types  =  ("http://test/email","http://test/displayname" ), query  = "SELECT mail, displayname FROM users WHERE name ={0}", param  = c.value );```|  
  
#### <a name="expressions"></a>Expressions  
Expressions sont utilisées sur le côté droit pour les contraintes de sélecteur de revendications et les paramètres de l’instruction d’émission. Il existe différents types d’expressions qui prend en charge de la langue. Toutes les expressions du langage sont basées sur des chaînes, ce qui signifie qu’ils utilisent des chaînes en entrée et produisent des chaînes. Nombres ou autres types de données, comme la date/heure, dans les expressions ne sont pas pris en charge. Voici les types d’expressions qui prend en charge de la langue:  
  
-   Littéral de chaîne: valeur, délimitée par le caractère de guillemet (") aux deux extrémités de chaîne.  
  
-   Concaténation d’expressions: le résultat est une chaîne générée par la concaténation des valeurs gauche et droite.  
  
-   Appel de fonction: la fonction est identifiée par un identificateur et les paramètres sont transmis en tant qu’une virgule-liste délimitée par des expressions entourés parenthèses («()»).  
  
-   Accès à la propriété de la revendication sous la forme d’un nom de propriété de point de nom de variable: le résultat de la valeur de propriété de la revendication identifiée pour une variable donnée. La variable doit tout d’abord être liée à un sélecteur de revendication avant de pouvoir être utilisé de cette façon. Vous ne pouvez pas utiliser la variable qui est liée à un sélecteur de revendication dans les contraintes de ce même sélecteur de revendications.  
  
Les propriétés de revendication suivantes sont disponibles pour l’accès:  
  
-   Claim.Type  
  
-   Claim.Value  
  
-   Claim.Issuer  
  
-   Claim.OriginalIssuer  
  
-   Claim.ValueType  
  
-   Claim.Properties\[property\_name\] (cette propriété renvoie une chaîne vide si la propriété _ordinateur est introuvable dans la collection de propriétés de la revendication. )  
  
Vous pouvez utiliser la fonction RegexReplace pour appeler à l’intérieur d’une expression. Cette fonction prend une expression d’entrée et met en correspondance avec le modèle donné. Si le modèle correspond, la sortie de la correspondance est remplacée par la valeur de remplacement.  
  
#### <a name="exists-functions"></a>Fonctions «EXISTS»  
La fonction Exists peut être utilisée dans une condition pour déterminer si une revendication qui correspond à que la condition existe dans l’entrée revendications ensemble. Si aucune revendication correspondante n’existe, l’instruction d’émission est appelée une seule fois. Dans l’exemple suivant, la revendication «origine» est émise une seule fois, s’il existe au moins une revendication dans la collection de jeu de revendications d’entrée dispose l’émetteur est «MSFT», quel que soit le nombre de revendications ont l’émetteur dont la valeur «Msft». À l’aide de cette fonction empêche l’émission de doublons de revendications.  
  
```  
exists([issuer == "MSFT"])  
   => issue(type = "origin", value = "Microsoft");  
```  
  
## <a name="rule-body"></a>Corps de la règle  
Le corps de la règle peut contenir seulement une seule instruction d’émission. Si les conditions sont utilisées sans la fonction Exists, le corps de la règle est exécuté une fois pour chaque fois que la partie conditions est mise en correspondance.  
  
## <a name="additional-references"></a>Références supplémentaires  
[Créer une règle pour envoyer des revendications à l’aide d’une règle personnalisée](https://technet.microsoft.com/library/dd807049.aspx)  
  

