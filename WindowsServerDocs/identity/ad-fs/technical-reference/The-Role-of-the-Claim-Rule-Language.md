---
title: Rôle du langage de règle de revendication
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: dda9d148-d72f-4bff-aa2a-f2249fa47e4c
ms.technology: identity-adfs
ms.openlocfilehash: a5f7f4435fd465f0794ff5cd5757677af899b1f6
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444156"
---
# <a name="the-role-of-the-claim-rule-language"></a>Rôle du langage de règle de revendication
Le bloc de construction d’administration pour le comportement des revendications entrantes et sortantes, tandis que le moteur de revendications agit comme moteur de traitement de la logique dans le langage de règle de revendication joue le langage de règle de revendication Active Directory Federation Services (ADFS) qui définit la règle personnalisée. Pour plus d’informations sur la façon toutes les règles sont traitées par le moteur de revendications, consultez [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md).  

## <a name="creating-custom-claim-rules-using-the-claim-rule-language"></a>Création de règles de revendication personnalisées à l’aide du langage de règle de revendication  
AD FS fournit aux administrateurs la possibilité de définir des règles personnalisées qu’ils peuvent utiliser pour déterminer le comportement de revendications d’identité avec le langage de règle de revendication. Vous pouvez utiliser les exemples de syntaxe du langage de règle de revendication de cette rubrique pour créer une règle personnalisée qui énumère, ajoute, supprime et modifie les revendications afin de satisfaire aux besoins de votre organisation. Vous pouvez créer des règles personnalisées en saisissant la syntaxe du langage de règle de revendication dans le modèle de règle **Envoyer des revendications à l’aide d’une revendication personnalisée**.  

Les règles sont séparées par des points-virgules.  

Pour plus d’informations sur quand utiliser des règles personnalisées, consultez [quand utiliser une règle de revendication personnalisée](When-to-Use-a-Custom-Claim-Rule.md).  

## <a name="using-claim-rule-templates-to-learn-about-the-claim-rule-language-syntax"></a>Utilisation de modèles de règle de revendication pour en savoir plus sur la syntaxe du langage de règle de revendication  
AD FS fournit également un ensemble de d’émission de revendication prédéfinis et des modèles de règle d’acceptation que vous pouvez utiliser pour implémenter courantes des règles de revendication de revendication. Dans la boîte de dialogue **Modifier les règles de revendication** pour une approbation donnée, vous pouvez créer une règle prédéfinie et en afficher la syntaxe du langage de règle de revendication en cliquant sur l’onglet **Afficher le langage de la règle** correspondant à cette règle. L’utilisation des informations contenues dans cette section et de la technique **Afficher le langage de règle** peut vous aider à élaborer vos propres règles personnalisées.  

Pour plus d’informations sur les règles de revendication et des modèles de règle de revendication, consultez [le rôle des règles de revendication](The-Role-of-Claim-Rules.md).  

## <a name="understanding-the-components-of-the-claim-rule-language"></a>Présentation des composants du langage de règle de revendication  
Le langage de règle de revendication se compose des éléments suivants, séparés par le « = > « opérateur :  

-   Une condition  

-   Une instruction d’émission  

### <a name="conditions"></a>Conditions  
Vous pouvez utiliser des conditions dans une règle permettant de vérifier les revendications d’entrée et de déterminer si l’instruction d’émission de la règle doit être exécutée. Une condition est une expression logique qui doit être évaluée comme « true » pour que la partie du corps de la règle s’exécute. Si cette partie est manquante, une valeur logique true est supposée et le corps de la règle est toujours exécuté. La partie conditions contient une liste des conditions qui sont combinées avec l’opérateur de conjonction logique (« & & »). Toutes les conditions de la liste doivent être évaluées comme « true » pour que la totalité de la partie conditionnelle soit évaluée comme « true ». La condition peut être un opérateur de sélection de revendication ou un appel de fonction d’agrégation. Ces deux conditions s’excluent mutuellement, ce qui signifie que les sélecteurs de revendication et les fonctions d’agrégation ne peuvent pas être combinés dans la partie conditions d’une règle unique.  

Les conditions sont facultatives dans les règles. Par exemple, la règle suivante ne contient pas de condition :  

```  
=> issue(type = "http://test/role", value = "employee");  
```  

Il existe trois types de condition :  

-   Condition unique : c’est la forme la plus simple d’une condition. Vérifications sont effectuées pour qu’une seule expression ; par exemple, nom du compte windows = utilisateur de domaine.  

-   Conditions multiples : cette condition requiert des vérifications supplémentaires pour traiter plusieurs expressions dans le corps de la règle ; par exemple, nom du compte windows = utilisateur de domaine et groupe = contosopurchasers.  

> [!NOTE]  
> Une autre condition existe, mais il s’agit d’un sous-ensemble d’une condition unique ou de conditions multiples. Il est appelé une condition d’expression régulière (Regex). Elle est utilisée pour prendre une expression d’entrée et la faire correspondre au modèle donné. Un exemple d’utilisation est présenté ci-dessous.  

Les exemples suivants montrent quelques-unes des constructions de syntaxe, basées sur les types de condition, que vous pouvez utiliser pour créer des règles personnalisées.  

#### <a name="single--condition-examples"></a>Unique - exemples de condition  
Unique - conditions de l’expression sont décrites dans le tableau suivant. Elles sont construites pour simplement vérifier une revendication avec un type spécifique ou une revendication avec un type et une valeur spécifiques.  


|                                                                                                                   Description de la condition                                                                                                                    |                           Exemple de syntaxe d’une condition                            |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------|
|               Cette règle comporte une condition pour vérifier une revendication d’entrée avec un type de revendication spécifiée («<http://test/name>»). Si une revendication correspondante est présente dans les revendications d’entrée, la règle copie la ou les revendications correspondantes vers le jeu des revendications de sortie.               |         ``` c: [type == "http://test/name"] => issue(claim = c );```          |
| Cette règle comporte une condition pour vérifier une revendication d’entrée avec un type de revendication spécifiée («<http://test/name>») et la valeur (« Terry ») de la demande. Si une revendication correspondante est présente dans les revendications d’entrée, la règle copie la ou les revendications correspondantes vers le jeu des revendications de sortie. | ``` c: [type == "http://test/name", value == "Terry"] => issue(claim = c);``` |

Plus - des conditions complexes sont affichées dans la section suivante, y compris les conditions pour vérifier plusieurs revendications, les conditions à vérifier l’émetteur de revendication et conditions vérifier les valeurs qui correspondent à un modèle d’expression régulière.  

#### <a name="multiple--condition-examples"></a>Exemples de conditions multiples-  
Le tableau suivant fournit un exemple de plusieurs - conditions d’expression.  


|                                                                                                                   Description de la condition                                                                                                                    |                                        Exemple de syntaxe d’une condition                                        |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------|
| Cette règle contient une condition pour vérifier des deux revendications, chacun avec un type de revendication spécifié d’entrée («<http://test/name>«  et  »<http://test/email>»). Si les deux revendications correspondantes sont présentes dans les revendications d’entrée, la règle copie le nom de la revendication vers le jeu des revendications de sortie. | ``` c1: [type  == "http://test/name"] && c2: [type == "http://test/email"] => issue (claim  = c1 );``` |

#### <a name="regular--condition-examples"></a>Standard - exemples de condition  
Le tableau suivant fournit un exemple d’une expression régulière,-en fonction de condition.  

|Description de la condition|Exemple de syntaxe d’une condition|  
|-------------------------|----------------------------|  
|Cette règle comporte une condition qui utilise une expression régulière pour vérifier un e-revendication de messagerie se terminant par «@fabrikam.com». Si une revendication correspondante est trouvée dans les revendications d’entrée, la règle copie la revendication correspondante vers le jeu des revendications de sortie.|```c: [type  == "http://test/email", value  =~ "^. +@fabrikam.com$" ] => issue (claim  = c );```|  

### <a name="issuance-statements"></a>Instructions d’émission  
Règles personnalisées sont traitées selon les instructions d’émission (*problème* ou *ajouter* ) que vous programmez dans la règle de revendication. Selon le résultat souhaité, l’instruction issue ou add peut être écrite dans la règle pour remplir le jeu de revendications d’entrée ou de sortie. Une règle personnalisée qui utilise l’instruction add remplit explicitement et uniquement les valeurs de revendication pour le jeu de revendications d’entrée, alors qu’une règle de revendication personnalisée qui utilise l’instruction issue remplit les valeurs de revendication des jeux de revendications d’entrée et de sortie. Cela peut être utile lorsqu’une valeur de revendication est destinée à être utilisée uniquement par les futures règles dans le jeu des règles de revendication.  

Par exemple, dans l’illustration suivante, la revendication entrante est ajoutée au jeu de revendications d’entrée par le moteur d’émission de revendications. Lorsque la première règle de revendication personnalisée s’exécute et les critères d’utilisateur de domaine est remplie, le moteur d’émission de revendications traite la logique de la règle à l’aide de l’instruction add et la valeur de **éditeur** est ajouté à l’ensemble de revendications d’entrée. Étant donné que la valeur de l’éditeur est présente dans l’ensemble de revendications d’entrée, règle 2 peut correctement traiter l’instruction issue dans sa logique et générer une nouvelle valeur de **Hello**, qui est ajouté à la sortie de deux jeux de revendications et à l’entrée de jeu pour une utilisation par revendication la règle suivante dans l’ensemble de règles. Règle 3 peut désormais utiliser toutes les valeurs qui sont présents dans la revendication d’entrée défini en tant qu’entrée pour traiter sa logique.  

![Rôles d’AD FS](media/adfs2_customrule.gif)  

#### <a name="claim-issuance-actions"></a>Actions d’émission de revendication  
Le corps de la règle représente une action d’émission de revendication. Le langage reconnaît deux actions d’émission de revendication :  

-   **Instruction issue :** l’instruction issue crée une revendication qui est redirigée vers les jeux de revendications d’entrée et de sortie. Par exemple, l’instruction suivante émet une nouvelle revendication basée sur son jeu de revendications d’entrée :  

    ```c:[type == "Name"] => issue(type = "Greeting", value = "Hello " + c.value);```  

-   **Instruction add :** l’instruction add crée une nouvelle revendication qui est uniquement ajoutée au jeu de revendications d’entrée. Par exemple, l’instruction suivante ajoute une nouvelle revendication au jeu de revendications d’entrée :  

    ```c:[type == "Name", value == "domain user"] => add(type = "Role", value = "Editor");``` 

L’instruction d’émission d’une règle définit les revendications qui seront émises par la règle lorsque les conditions seront remplies. Il existe deux formes d’instructions d’émission en ce qui concerne les arguments et le comportement de l’instruction :  

-   **Normal** : ces instructions d’émission peuvent émettre des revendications à l’aide des valeurs littérales de la règle ou à l’aide des valeurs des revendications correspondant aux conditions. Une instruction d’émission Normal peut être constituée de l’un des formats suivants ou des deux :  

    -   *Copie de la revendication* : ce format crée une copie de la revendication existante dans le jeu de revendications de sortie. Cette forme d’émission est uniquement applicable lorsqu’elle est combinée avec l’instruction d’émission « issue ». Lorsqu’elle est associée avec l’instruction d’émission « add », elle n’a aucun effet.  

    -   *Nouvelle réclamation* : Ce format crée une nouvelle revendication, selon les valeurs pour les différentes propriétés de revendication. La propriété Claim.Type doit être spécifiée ; toutes les autres propriétés de revendication sont facultatives. Pour ce format, l’ordre des arguments n’a pas d’importance.  

-   **Magasin d’attributs** : cette instruction crée des revendications avec des valeurs récupérées dans le magasin d’attributs. Il est possible de créer plusieurs types de revendications à l’aide d’une instruction d’émission unique, ce qui est importante pour les magasins d’attributs qui rendent les opérations (e/s) d’entrée/sortie réseau ou disque pendant la récupération de l’attribut. Par conséquent, il est préférable de limiter le nombre d’allers-retours entre le moteur de stratégie et le magasin d’attributs. Vous pouvez également créer plusieurs revendications pour un type de revendication donné. Lorsque le magasin d’attributs renvoie plusieurs valeurs pour un type de revendication donné, l’instruction d’émission crée automatiquement une revendication pour chaque valeur de revendication retournée. Une implémentation du magasin d’attributs utilise les arguments param pour remplacer les espaces réservés dans l’argument de requête par des valeurs qui sont fournies dans les arguments param. Les espaces réservés utilisent la même syntaxe que la .NET String.Format (), fonction (par exemple, {1}, {2}, et ainsi de suite). Pour cette forme d’émission, l’ordre des arguments est important. Il doit s’agir de l’ordre indiqué dans la grammaire suivante.  

Le tableau suivant décrit certaines constructions de syntaxe courantes pour les deux types d’instruction d’émission dans les règles de revendication.  

|Type d’instruction d’émission|Description de l’instruction d’émission|Exemple de syntaxe de l’instruction d’émission|  
|---------------------------|----------------------------------|-------------------------------------|  
|Normal|La règle suivante émet toujours la même revendication chaque fois qu’un utilisateur présente la valeur et le type de revendication spécifiés :|```c: [type  == "http://test/employee", value  == "true"] => issue (type = "http://test/role", value = "employee");```|  
|Normal|La règle suivante convertit un type de revendication en un autre. Notez que la valeur de la revendication qui correspond à la condition « c » est utilisée dans l’instruction d’émission.|```c: [type  == "http://test/group" ] => issue (type  = "http://test/role", value  = c.Value );```|  
|Magasin d’attributs|La règle suivante utilise la valeur d’une revendication entrante pour demander le magasin d’attributs Active Directory :|```c: [Type  == "http://test/name" ] => issue (store  = "Enterprise AD Attribute Store", types  =  ("http://test/email" ), query  = ";mail;{0}", param  = c.Value )```|  
|Magasin d’attributs|La règle suivante utilise la valeur d’une revendication entrante pour interroger un magasin d’attributs de langage SQL (Structured Query) précédemment configurés :|```c: [type  == "http://test/name"] => issue (store  = "Custom SQL store", types  =  ("http://test/email","http://test/displayname" ), query  = "SELECT mail, displayname FROM users WHERE name ={0}", param  = c.value );```|  

#### <a name="expressions"></a>Expressions  
Les expressions sont utilisées sur le côté droit pour les contraintes de sélecteur de revendications et les paramètres de l’instruction d’émission. Il existe divers types d’expression pris en charge par le langage. Toutes les expressions du langage sont basées sur des chaînes, ce qui signifie qu’elles utilisent des chaînes en entrée et produisent des chaînes. Les nombres ou autres types de données, comme la date/l’heure, utilisés dans les expressions ne sont pas pris en charge. Les types d’expression suivants sont pris en charge par le langage :  

-   Opérateur sur chaîne : Valeur, délimitée par le caractère de guillemet (") des deux côtés de la chaîne.  

-   Concaténation d’expressions en une chaîne : le résultat est une chaîne générée par la concaténation des valeurs gauche et droite.  

-   Appel de fonction : La fonction est identifiée par un identificateur et les paramètres sont passés en tant qu’une virgule-liste délimitée d’expressions entourés parenthèses (« () »).  

-   Accès à la propriété de la revendication sous la forme d’un nom de variable, suivi d’un point et du nom de la propriété : le résultat de la valeur de la propriété de la revendication identifiée pour une évaluation de la variable donnée. La variable doit d’abord être liée à un sélecteur de revendication avant de pouvoir être utilisée de cette façon. Vous ne pouvez pas utiliser la variable liée à un sélecteur de revendication dans les contraintes de ce même sélecteur de revendication.  

L’accès aux propriétés de revendication suivantes est disponible :  

-   Claim.Type  

-   Claim.Value  

-   Claim.Issuer  

-   Claim.OriginalIssuer  

-   Claim.ValueType  

-   Claim.Properties\[propriété\_nom\] (cette propriété retourne une chaîne vide si la propriété _nom est introuvable dans la collection de propriétés de la revendication. )  

Vous pouvez utiliser la fonction RegexReplace pour appeler un élément dans un expression. Cette fonction prend une expression d’entrée et la fait correspondre avec le modèle donné. Si le modèle correspond, la sortie de la correspondance est remplacée par la valeur de remplacement.  

#### <a name="exists-functions"></a>Fonctions « Exists »  
La fonction Exists peut être utilisée dans une condition pour déterminer si une revendication qui correspond à la condition existe dans le jeu de revendications d’entrée. Si aucune revendication correspondante n’existe, l’instruction d’émission n’est appelée qu’une seule fois. Dans l’exemple suivant, la revendication « origine » est émise une seule fois s’il existe au moins une revendication dans la collection de jeu de revendications d’entrée dont l’émetteur est « MSFT », quel que soit le nombre de revendications de l’émetteur dont la valeur est « MSFT ». Cette fonction permet d’empêcher l’émission de doublons de revendications.  

```  
exists([issuer == "MSFT"])  
   => issue(type = "origin", value = "Microsoft");  
```  

## <a name="rule-body"></a>Corps de la règle  
Le corps de la règle peut contenir une seule instruction d’émission. Si les conditions sont utilisées sans la fonction Exists, le corps de la règle est exécuté une fois chaque fois que la partie conditions est mise en correspondance.  

## <a name="additional-references"></a>Références supplémentaires  
[Créer une règle pour envoyer des revendications à l’aide d’une règle personnalisée](https://technet.microsoft.com/library/dd807049.aspx)  


