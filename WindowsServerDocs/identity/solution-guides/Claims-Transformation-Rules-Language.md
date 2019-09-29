---
ms.assetid: e831f781-3c45-4d44-b411-160d121d1324
title: Langage de règles de Transformation de revendications
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 200d592bc68562856bbdee623e70d73d41457c15
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357580"
---
# <a name="claims-transformation-rules-language"></a>Langage de règles de Transformation de revendications

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La fonctionnalité de transformation des revendications sur plusieurs forêts vous permet de relier des revendications pour des Access Control dynamiques entre les limites de la forêt en définissant des stratégies de transformation des revendications sur les approbations sur plusieurs forêts. Le composant principal de toutes les stratégies est celui qui est écrit dans le langage des règles de transformation des revendications. Cette rubrique fournit des informations détaillées sur ce langage et fournit des conseils sur la création de règles de transformation de revendications.  
  
Les applets de commande Windows PowerShell pour les stratégies de transformation sur des approbations sur plusieurs forêts disposent d’options permettant de définir des stratégies simples qui sont requises dans les scénarios courants. Ces applets de commande traduisent la saisie utilisateur dans des stratégies et des règles dans le langage de règles de transformation des revendications, puis les stockent dans Active Directory dans le format prescrit. Pour plus d’informations sur les applets de commande pour la transformation des revendications, consultez les [applets de commande AD DS pour les Access Control dynamiques](https://go.microsoft.com/fwlink/?LinkId=243150).  
  
Selon la configuration des revendications et les exigences placées sur l’approbation entre forêts dans vos forêts Active Directory, vos stratégies de transformation des revendications peuvent être plus complexes que les stratégies prises en charge par les applets de commande Windows PowerShell pour active Directory. Pour créer efficacement de telles stratégies, il est essentiel de comprendre la syntaxe et la sémantique du langage de transformation des revendications. Ce langage de règles de transformation des revendications (« langage ») dans Active Directory est un sous-ensemble du langage utilisé par [services ADFS](https://go.microsoft.com/fwlink/?LinkId=243982) à des fins similaires, et il a une syntaxe et une sémantique très similaires. Toutefois, il y a moins d’opérations autorisées et des restrictions de syntaxe supplémentaires sont placées dans la version Active Directory du langage.  
  
Cette rubrique explique brièvement la syntaxe et la sémantique du langage de règles de transformation des revendications dans Active Directory et les éléments à prendre en considération lors de la création de stratégies. Il fournit plusieurs ensembles d’exemples de règles pour vous aider à démarrer, ainsi que des exemples de syntaxe incorrecte et les messages qu’ils génèrent, pour vous aider à déchiffrer les messages d’erreur lorsque vous créez les règles.  
  
## <a name="tools-for-authoring-claims-transformation-policies"></a>Outils de création de stratégies de transformation des revendications  
**Applets de commande Windows PowerShell pour Active Directory**: Il s’agit de la méthode recommandée et recommandée pour créer et définir des stratégies de transformation des revendications. Ces applets de commande fournissent des commutateurs pour les stratégies simples et vérifient les règles qui sont définies pour les stratégies plus complexes.  
  
**LDAP**: Les stratégies de transformation des revendications peuvent être modifiées dans Active Directory par le biais du protocole LDAP (Lightweight Directory Access Protocol). Toutefois, cela n’est pas recommandé, car les stratégies comportent plusieurs composants complexes, et les outils que vous utilisez peuvent ne pas valider la stratégie avant de l’écrire dans Active Directory. Cela peut nécessiter un temps considérable pour diagnostiquer les problèmes.  
  
## <a name="active-directory-claims-transformation-rules-language"></a>Langage de règles de transformation des revendications Active Directory  
  
### <a name="syntax-overview"></a>Vue d’ensemble de la syntaxe  
Voici une brève vue d’ensemble de la syntaxe et de la sémantique du langage :  
  
-   L’ensemble de règles de transformation des revendications se compose de zéro ou plusieurs règles. Chaque règle a deux parties actives : **Sélectionnez la liste des conditions** et l’action de la **règle**. Si la **liste Sélectionner la condition** prend la valeur true, l’action de règle correspondante est exécutée.  
  
-   La sélection de la **liste** de conditions comporte zéro ou plusieurs **conditions Select**. Toutes les **conditions Select** doivent avoir la valeur true pour que la liste de conditions **Select** soit évaluée à true.  
  
-   Chaque **condition Select** a un ensemble de zéro ou plusieurs **conditions de correspondance**. Toutes les **conditions de correspondance** doivent avoir la valeur true pour que la condition Select soit évaluée à true. Toutes ces conditions sont évaluées par rapport à une seule revendication. Une revendication qui correspond à une **condition Select** peut être référencée par un **identificateur** et référencée dans l’action de la **règle**.  
  
-   Chaque **condition de correspondance** spécifie la condition qui correspond au **type** ou à la **valeur** ou au **ValueType** d’une revendication à l’aide de différents **opérateurs de condition** et **littéraux de chaîne**.  
  
    -   Lorsque vous spécifiez **une condition de correspondance** pour une **valeur**, vous devez également spécifier une **condition de correspondance** pour un **ValueType** spécifique, et vice versa. Ces conditions doivent être les unes à côté des autres dans la syntaxe.  
  
    -   Les conditions de correspondance **ValueType** doivent uniquement **utiliser des** littéraux spécifiques.  
  
-   Une **action de règle** peut copier une revendication qui est marquée avec un **identificateur** ou émettre une revendication basée sur une revendication qui est marquée avec un identificateur et/ou des littéraux de chaîne donnés.  
  
**Exemple de règle**  
  
Cet exemple montre une règle qui peut être utilisée pour traduire le type de revendications entre deux forêts, à condition qu’elles utilisent les mêmes types de revendications et aient les mêmes interprétations pour les valeurs de revendication pour ce type. La règle a une condition de correspondance et une instruction issue qui utilise des littéraux de chaîne et une référence de revendications correspondante.  
  
```  
C1: [TYPE=="EmployeeType"]    
                 => ISSUE (TYPE= "EmpType", VALUE = C1.VALUE, VALUETYPE = C1.VALUETYPE);  
[TYPE=="EmployeeType"] == Select Condition List with one Matching Condition for claims Type.  
ISSUE (TYPE= "EmpType", VALUE = C1.VALUE, VALUETYPE = C1.VALUETYPE) == Rule Action that issues a claims using string literal and matching claim referred with the Identifier.  
  
```  
  
### <a name="runtime-operation"></a>Opération d’exécution  
Il est important de comprendre l’opération d’exécution des transformations de revendications pour créer efficacement les règles. L’opération d’exécution utilise trois ensembles de revendications :  
  
1.  **Jeu de revendications d’entrée**: Jeu d’entrée des revendications accordées à l’opération de transformation des revendications.  
  
2.  **Ensemble de revendications de travail**: Revendications intermédiaires lues et écrites dans pendant la transformation des revendications.  
  
3.  **Jeu de revendications de sortie**: Résultat de l’opération de transformation des revendications.  
  
Voici une brève vue d’ensemble de l’opération de transformation des revendications du Runtime :  
  
1.  Les revendications d’entrée pour la transformation de revendications sont utilisées pour initialiser le jeu de revendications de travail.  
  
    1.  Lors du traitement de chaque règle, le jeu de revendications de travail est utilisé pour les revendications d’entrée.  
  
    2.  La liste des conditions de sélection d’une règle est mise en correspondance avec tous les jeux de revendications possibles du jeu de revendications de travail.  
  
    3.  Chaque ensemble de revendications correspondantes est utilisé pour exécuter l’action dans cette règle.  
  
    4.  L’exécution d’une action de règle génère une revendication, qui est ajoutée au jeu de revendications de sortie et au jeu de revendications de travail. Ainsi, la sortie d’une règle est utilisée comme entrée pour les règles suivantes dans l’ensemble de règles.  
  
2.  Les règles de l’ensemble de règles sont traitées dans l’ordre séquentiel, en commençant par la première règle.  
  
3.  Lorsque l’ensemble de règles entier est traité, le jeu de revendications de sortie est traité pour supprimer les revendications dupliquées et pour d’autres problèmes de sécurité. Les revendications résultantes sont la sortie du processus de transformation des revendications.  
  
Il est possible d’écrire des transformations de revendications complexes basées sur le comportement d’exécution précédent.  
  
**Tels Opération d’exécution @ no__t-0  
  
Cet exemple montre l’opération d’exécution d’une transformation de revendications qui utilise deux règles.  
  
```  
  
     C1:[Type=="EmpType", Value=="FullTime",ValueType=="string"] =>  
                Issue(Type=="EmployeeType", Value=="FullTime",ValueType=="string");  
     [Type=="EmployeeType"] =>   
               Issue(Type=="AccessType", Value=="Privileged", ValueType=="string");  
Input claims and Initial Evaluation Context:  
  {(Type= "EmpType"),(Value="FullTime"),(ValueType="String")}  
{(Type= "Organization"),(Value="Marketing"),(ValueType="String")}  
After Processing Rule 1:  
 Evaluation Context:  
  {(Type= "EmpType"),(Value="FullTime"),(ValueType="String")}  
{(Type= "Organization"), (Value="Marketing"),(ValueType="String")}  
  {(Type= "EmployeeType"),(Value="FullTime"),(ValueType="String")}  
Output Context:  
  {(Type= "EmployeeType"),(Value="FullTime"),(ValueType="String")}  
  
After Processing Rule 2:  
Evaluation Context:  
  {(Type= "EmpType"),(Value="FullTime"),(ValueType="String")}  
{(Type= "Organization"),(Value="Marketing"),(ValueType="String")}  
  {(Type= "EmployeeType"),(Value="FullTime"),(ValueType="String")}  
  {(Type= "AccessType"),(Value="Privileged"),(ValueType="String")}  
Output Context:  
  {(Type= "EmployeeType"),(Value="FullTime"),(ValueType="String")}  
  {(Type= "AccessType"),(Value="Privileged"),(ValueType="String")}  
  
Final Output:  
  {(Type= "EmployeeType"),(Value="FullTime"),(ValueType="String")}  
  {(Type= "AccessType"),(Value="Privileged"),(ValueType="String")}  
  
```  
  
### <a name="special-rules-semantics"></a>Sémantique des règles spéciales  
Voici une syntaxe spéciale pour les règles :  
  
1.  Ensemble de règles vide = = aucune revendication de sortie  
  
2.  Liste de conditions Select vide = = chaque revendication correspond à la liste de conditions Select  
  
    **Tels Liste de conditions Select vide @ no__t-0  
  
    La règle suivante correspond à chaque revendication de la plage de travail.  
  
    ```  
    => Issue (Type = "UserType", Value = "External", ValueType = "string")  
    ```  
  
3.  Empty Select Matching List = = chaque revendication correspond à la liste de conditions Select  
  
    **Tels Conditions de correspondance vides @ no__t-0  
  
    La règle suivante correspond à chaque revendication de la plage de travail. Il s’agit de la règle de base « autoriser tout » si elle est utilisée seule.  
  
    ```  
    C1:[] => Issule (claim = C1);  
    ```  
  
## <a name="security-considerations"></a>Considérations relatives à la sécurité  
**Revendications qui entrent dans une forêt**  
  
Les revendications présentées par les principaux qui sont entrants dans une forêt doivent être soigneusement inspectées pour garantir que nous autorisons ou émettent uniquement les revendications appropriées. Des revendications incorrectes peuvent compromettre la sécurité de la forêt, ce qui doit être une préoccupation principale lors de la création de stratégies de transformation pour les revendications qui entrent dans une forêt.  
  
Active Directory présente les fonctionnalités suivantes pour empêcher une configuration Inpossible des revendications qui entrent dans une forêt :  
  
-   Si aucune stratégie de transformation de revendication n’est définie pour une approbation de forêt pour les revendications qui entrent dans une forêt, pour des raisons de sécurité, Active Directory supprime toutes les revendications principales qui entrent dans la forêt.  
  
-   Si l’exécution de l’ensemble de règles sur les revendications qui pénètrent dans une forêt entraîne des revendications qui ne sont pas définies dans la forêt, les revendications non définies sont supprimées des revendications de sortie.  
  
**Revendications qui laissent une forêt**  
  
Les revendications qui laissent une forêt présentent un problème de sécurité moins important pour la forêt que les revendications qui entrent dans la forêt. Les revendications sont autorisées à sortir de la forêt, même lorsqu’aucune stratégie de transformation de revendication correspondante n’est en place. Il est également possible d’émettre des revendications qui ne sont pas définies dans la forêt dans le cadre de la transformation des revendications qui laissent la forêt. Cela permet de configurer facilement des approbations sur plusieurs forêts avec des revendications. Un administrateur peut déterminer si les revendications qui entrent dans la forêt doivent être transformées et configurer la stratégie appropriée. Par exemple, un administrateur peut définir une stratégie s’il est nécessaire de masquer une revendication pour empêcher la divulgation d’informations.  
  
**Erreurs de syntaxe dans les règles de transformation des revendications**  
  
Si une stratégie de transformation de revendications donnée a un ensemble de règles qui est syntaxiquement incorrect ou s’il existe d’autres problèmes de syntaxe ou de stockage, la stratégie est considérée comme non valide. Elle est traitée différemment des conditions par défaut mentionnées précédemment.  
  
Active Directory ne parvient pas à déterminer l’intention dans ce cas et passe en mode de prévention de défaillance, où aucune revendication de sortie n’est générée sur cette relation d’approbation + Direction de parcours. L’intervention de l’administrateur est requise pour corriger le problème. Cela peut se produire si le protocole LDAP est utilisé pour modifier la stratégie de transformation des revendications. Les applets de commande Windows PowerShell pour Active Directory ont une validation en place pour empêcher l’écriture d’une stratégie avec des problèmes de syntaxe.  
  
## <a name="other-language-considerations"></a>Autres considérations relatives au langage  
  
1.  Il existe plusieurs mots clés ou caractères spéciaux dans cette langue (appelés terminaux). Celles-ci sont présentées dans le tableau des [terminaux de langue](Claims-Transformation-Rules-Language.md#BKMK_LT) plus loin dans cette rubrique. Les messages d’erreur utilisent les balises pour ces terminaux pour la désambiguïsation.  
  
2.  Les terminaux peuvent parfois être utilisés en tant que littéraux de chaîne. Toutefois, une telle utilisation peut être en conflit avec la définition de langage ou avoir des conséquences inattendues. Ce type d’utilisation n’est pas recommandé.  
  
3.  L’action de règle ne peut effectuer aucune conversion de type sur des valeurs de revendication, et un ensemble de règles qui contient une telle action de règle est considéré comme non valide. Cela provoque une erreur d’exécution et aucune revendication de sortie n’est générée.  
  
4.  Si une action de règle fait référence à un identificateur qui n’a pas été utilisé dans la partie sélectionner la liste de conditions de la règle, il s’agit d’une utilisation non valide. Cela entraînerait une erreur de syntaxe.  
  
    **Tels Référence d’identificateur incorrecte @ no__t-0  
    La règle suivante illustre un identificateur incorrect utilisé dans l’action de la règle.  
  
    ```  
    C1:[] => Issue (claim = C2);  
    ```  
  
## <a name="sample-transformation-rules"></a>Exemples de règles de transformation  
  
-   **Autoriser toutes les revendications d’un certain type**  
  
    Type exact  
  
    ```  
    C1:[type=="XYZ"] => Issue (claim = C1);  
    ```  
  
    Utilisation d’Regex  
  
    ```  
    C1: [type =~ "XYZ*"] => Issue (claim = C1);  
    ```  
  
-   **Interdire un certain type de revendication**  
    Type exact  
  
    ```  
    C1:[type != "XYZ"] => Issue (claim=C1);  
    ```  
  
    Utilisation d’Regex  
  
    ```  
    C1:[Type !~ "XYZ?"] => Issue (claim=C1);  
    ```  
  
## <a name="examples-of-rules-parser-errors"></a>Exemples d’erreurs de l’analyseur de règles  
Les règles de transformation des revendications sont analysées par un analyseur personnalisé pour rechercher les erreurs de syntaxe. Cet analyseur est exécuté par les applets de commande Windows PowerShell associées avant de stocker des règles dans Active Directory. Toutes les erreurs dans l’analyse des règles, y compris les erreurs de syntaxe, sont imprimées sur la console. Les contrôleurs de domaine exécutent également l’analyseur avant d’utiliser les règles de transformation des revendications, et consignent les erreurs dans le journal des événements (ajouter les numéros des journaux des événements).  
  
Cette section illustre quelques exemples de règles écrites avec une syntaxe incorrecte et les erreurs de syntaxe correspondantes qui sont générées par l’analyseur.  
  
1. Exemple :  
  
   ```  
   c1;[]=>Issue(claim=c1);  
   ```  
  
   Cet exemple présente un point-virgule incorrectement utilisé à la place d’un signe deux-points.   
   **Message d’erreur :**  
   @NO__T 0POLICY0002 : Impossible d’analyser les données de stratégie. *  
   Numéro de @no__t 0Line : 1, numéro de colonne : 2, jeton d’erreur :;. Ligne : 'C1 ; [] = > problème (revendication = C1); '. *  
   erreur @no__t 0Parser : 'POLICY0030: Erreur de syntaxe, '; 'inattendu, qui attendait l’un des éléments suivants : ' : '. ' *  
  
2. Exemple :  
  
   ```  
   c1:[]=>Issue(claim=c2);  
   ```  
  
   Dans cet exemple, la balise d’identificateur dans l’instruction de copie d’émission n’est pas définie.   
   **Message d’erreur**:   
   @NO__T 0POLICY0011 : Aucune condition de la règle de revendication ne correspond à la balise de condition spécifiée dans CopyIssuanceStatement : 'C2 '. *  
  
3. Exemple :  
  
   ```  
   c1:[type=="x1", value=="1", valuetype=="bool"]=>Issue(claim=c1)  
   ```  
  
   « bool » n’est pas un terminal dans la langue et il ne s’agit pas d’un ValueType valide. Les terminaux valides sont répertoriés dans le message d’erreur suivant.   
   **Message d’erreur :**  
   @NO__T 0POLICY0002 : Impossible d’analyser les données de stratégie. *  
   Numéro de ligne : 1, numéro de colonne : 39, jeton d’erreur : "bool". Ligne : 'C1 : [type = = "X1", valeur = = "1", ValueType = = "bool"] = > problème (revendication = C1); '.   
   erreur @no__t 0Parser : 'POLICY0030: Erreur de syntaxe, 'STRING’inattendu, qui attendait l’un des éléments suivants : 'INT64_TYPE' 'UINT64_TYPE' 'STRING_TYPE' 'BOOLEAN_TYPE' 'IDENTIFICATEUR' *  
  
4. Exemple :  
  
   ```  
   c1:[type=="x1", value==1, valuetype=="boolean"]=>Issue(claim=c1);  
   ```  
  
   Le chiffre **1** de cet exemple n’est pas un jeton valide dans le langage, et ce type d’utilisation n’est pas autorisé dans une condition de correspondance. Elle doit être placée entre guillemets doubles pour en faire une chaîne.   
   **Message d’erreur :**  
   @NO__T 0POLICY0002 : Impossible d’analyser les données de stratégie. *  
   Numéro de @no__t 0Line : 1, numéro de colonne : 23, jeton d’erreur : 1. Ligne : 'C1 : [type = = "X1", valeur = = 1, ValueType = = "bool"] = > problème (revendication = C1); '. * @ no__t-1Parser erreur : 'POLICY0029: Entrée inattendue. </em>  
  
5. Exemple :  
  
   ```  
   c1:[type == "x1", value == "1", valuetype == "boolean"] =>   
  
        Issue(type = c1.type, value="0", valuetype == "boolean");  
   ```  
  
   Cet exemple utilise un double signe égal (= =) au lieu d’un seul signe égal (=).   
   **Message d’erreur :**  
   @NO__T 0POLICY0002 : Impossible d’analyser les données de stratégie. *  
   Numéro de @no__t 0Line : 1, numéro de colonne : 91, jeton d’erreur : = =. Ligne : 'C1 : [type = = "X1", valeur = = "1", *  
   *ValueType = = "Boolean"] = > problème (type = C1. type, valeur = "0", ValueType = = "Boolean"); '.*  
   erreur @no__t 0Parser : 'POLICY0030: Erreur de syntaxe, ' = 'inattendu, qui attendait l’un des éléments suivants : ' = ' *  
  
6. Exemple :  
  
   ```  
   c1:[type=="x1", value=="boolean", valuetype=="string"] =>   
  
         Issue(type=c1.type, value=c1.value, valuetype = "string");  
   ```  
  
   Cet exemple est syntaxiquement et sémantiquement correct. Toutefois, l’utilisation de « booléen » comme valeur de chaîne est liée à la confusion et doit être évitée. Comme mentionné précédemment, l’utilisation de terminaux de langage comme valeurs de revendications doit être évitée dans la mesure du possible.  
  
## <a name="BKMK_LT"></a>Terminaux de langue  
Le tableau suivant répertorie l’ensemble complet des chaînes de terminal et les terminaux de langue associés utilisés dans le langage de règles de transformation des revendications. Ces définitions utilisent des chaînes UTF-16 qui ne respectent pas la casse.  
  
|Chaîne|Terminaux|  
|----------|------------|  
|« = > »|ENTRAÎN|  
|";"|VIRGULE|  
|":"|SIGNE|  
|","|POINT|  
|"."|CÉDÉ|  
|"["|O_SQ_BRACKET|  
|"]"|C_SQ_BRACKET|  
|"("|O_BRACKET|  
|")"|C_BRACKET|  
|"=="|EQ|  
|"!="|NEQ|  
|"=~"|REGEXP_MATCH|  
|"!~"|REGEXP_NOT_MATCH|  
|"="|ASSIGNÉS|  
|« & & »|ET|  
|problématique|PROBLÈME|  
|entrer|TYPE|  
|ajoutée|AJOUTÉE|  
|ValueType|VALUE_TYPE|  
|revendiqu|REVENDIQU|  
|« [A-za-z] [a-zA-z0-9] * »|IDENTIFICATEUR|  
|« \\ » [^ \\» \n] * \\ «»|CHAÎNE|  
|UInt64|UINT64_TYPE|  
|Int64|INT64_TYPE|  
|chaîne|STRING_TYPE|  
|expression|BOOLEAN_TYPE|  
  
## <a name="language-syntax"></a>Syntaxe du langage  
Le langage de règles de transformation des revendications suivant est spécifié sous forme de ABNF. Cette définition utilise les terminaux spécifiés dans le tableau précédent en plus des productions ABNF définies ici. Les règles doivent être encodées au format UTF-16, et les comparaisons de chaînes doivent être traitées comme non sensibles à la casse.  
  
```  
Rule_set        = ;/*Empty*/  
             / Rules  
Rules         = Rule  
             / Rule Rules  
Rule          = Rule_body  
Rule_body       = (Conditions IMPLY Rule_action SEMICOLON)  
Conditions       = ;/*Empty*/  
             / Sel_condition_list  
Sel_condition_list   = Sel_condition  
             / (Sel_condition_list AND Sel_condition)  
Sel_condition     = Sel_condition_body  
             / (IDENTIFIER COLON Sel_condition_body)  
Sel_condition_body   = O_SQ_BRACKET Opt_cond_list C_SQ_BRACKET  
Opt_cond_list     = /*Empty*/  
             / Cond_list  
Cond_list       = Cond  
             / (Cond_list COMMA Cond)  
Cond          = Value_cond  
             / Type_cond  
Type_cond       = TYPE Cond_oper Literal_expr  
Value_cond       = (Val_cond COMMA Val_type_cond)  
             /(Val_type_cond COMMA Val_cond)  
Val_cond        = VALUE Cond_oper Literal_expr  
Val_type_cond     = VALUE_TYPE Cond_oper Value_type_literal  
claim_prop       = TYPE  
             / VALUE  
Cond_oper       = EQ  
             / NEQ  
             / REGEXP_MATCH  
             / REGEXP_NOT_MATCH  
Literal_expr      = Literal  
             / Value_type_literal  
  
Expr          = Literal  
             / Value_type_expr  
             / (IDENTIFIER DOT claim_prop)  
Value_type_expr    = Value_type_literal  
             /(IDENTIFIER DOT VALUE_TYPE)  
Value_type_literal   = INT64_TYPE  
             / UINT64_TYPE  
             / STRING_TYPE  
             / BOOLEAN_TYPE  
Literal        = STRING  
Rule_action      = ISSUE O_BRACKET Issue_params C_BRACKET  
Issue_params      = claim_copy  
             / claim_new  
claim_copy       = CLAIM ASSIGN IDENTIFIER  
claim_new       = claim_prop_assign_list  
claim_prop_assign_list = (claim_value_assign COMMA claim_type_assign)  
             /(claim_type_assign COMMA claim_value_assign)  
claim_value_assign   = (claim_val_assign COMMA claim_val_type_assign)  
             /(claim_val_type_assign COMMA claim_val_assign)  
claim_val_assign    = VALUE ASSIGN Expr  
claim_val_type_assign = VALUE_TYPE ASSIGN Value_type_expr  
Claim_type_assign   = TYPE ASSIGN Expr  
  
```  
  


