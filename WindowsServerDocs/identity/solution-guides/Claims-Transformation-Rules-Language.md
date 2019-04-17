---
ms.assetid: e831f781-3c45-4d44-b411-160d121d1324
title: "Langage de règles de Transformation de revendications"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 4a6b378bc4aef180ebedd260008febaa2f2a76ae
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="claims-transformation-rules-language"></a>Langage de règles de Transformation de revendications

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

La forêt sur des revendications grâce à transformation vous pont les revendications pour le contrôle d’accès dynamique dans les limites de la forêt en définissant des stratégies de transformation de revendications sur les approbations entre forêts. Le composant principal de toutes les stratégies est règles qui sont écrits en langage de règles de transformation de revendications. Cette rubrique fournit des détails sur ce langage et fournit des conseils sur la création de règles de transformation des revendications.  
  
Les applets de commande Windows PowerShell pour les stratégies de transformation sur inter-forêts approuve les options permettant de définir des stratégies simples qui sont requis en commun scénarios. Ces applets de commande traduire l’entrée d’utilisateur dans les stratégies et les règles dans le langage de règles de transformation de revendications et les stockent dans ActiveDirectory dans le format prescrit. Pour plus d’informations sur les applets de commande pour la transformation des revendications, voir la [applets de commande ADDS pour le contrôle d’accès dynamique](https://go.microsoft.com/fwlink/?LinkId=243150).  
  
En fonction de la configuration des revendications et la configuration requise placée sur l’approbation entre forêts dans vos forêts ActiveDirectory, vos stratégies de transformation des revendications peut-être être plus complexes que les stratégies prises en charge par les applets de commande Windows PowerShell pour ActiveDirectory. Pour créer des efficacement ces stratégies, il est essentiel de comprendre la syntaxe du langage de règles revendications transformation et la sémantique. Cette revendications langage des règles de transformation («la langue») dans ActiveDirectory est un sous-ensemble de la langue utilisée par [ActiveDirectory Federation Services](https://go.microsoft.com/fwlink/?LinkId=243982) de similaires à des fins et il présente une syntaxe et la sémantique similaire. Toutefois, il existe moins d’opérations autorisées et restrictions de syntaxe supplémentaires sont placées dans la version d’ActiveDirectory de la langue.  
  
Cette rubrique explique brièvement la syntaxe et la sémantique du langage de règles de transformation de revendications dans ActiveDirectory et les considérations à effectuer lors de la création de stratégies. Il fournit plusieurs ensembles de règles de l’exemple pour vous aider à démarrer et des exemples de syntaxe incorrecte et les messages qu’ils génèrent, pour vous aider à déchiffrer les messages d’erreur lorsque vous créez les règles.  
  
## <a name="tools-for-authoring-claims-transformation-policies"></a>Outils pour la création de stratégies de transformation des revendications  
**Applets de commande Windows PowerShell pour ActiveDirectory**: c’est le moyen par défaut et recommandé pour créer et définir des stratégies de transformation de revendications. Ces applets de commande fournissent des commutateurs pour les stratégies simples et vérifiez les règles qui sont définies pour les stratégies plus complexes.  
  
**LDAP**: stratégies de transformation des revendications peuvent être modifiées dans ActiveDirectory par le biais de répertoire accès protocole LDAP (Lightweight). Toutefois, cela est déconseillé, car les stratégies ont plusieurs composants complexes, et les outils que vous utilisez ne peuvent pas valider la stratégie avant d’écrire dans ActiveDirectory. Cela peut nécessiter par la suite d’une quantité considérable de temps pour diagnostiquer les problèmes.  
  
## <a name="active-directory-claims-transformation-rules-language"></a>Langage des règles de transformation des revendications ActiveDirectory  
  
### <a name="syntax-overview"></a>Vue d’ensemble de la syntaxe  
Voici une brève présentation de la syntaxe et la sémantique de la langue:  
  
-   L’ensemble de règles de transformation de revendications se compose de zéro ou plusieurs règles. Chaque règle comporte deux parties actives: **sélectionner une liste de conditions** et **Action de règle**. Si le **sélectionner une liste de conditions** a la valeur TRUE, l’action de règle correspondante est exécutée.  
  
-   **Sélectionnez la liste de conditions** a zéro ou plusieurs **Conditions sélectionnez**. Toutes les **sélectionnez des Conditions** doit avoir la valeur TRUE pour le **sélectionner une liste de conditions** soit évaluée comme TRUE.  
  
-   Chaque **sélectionner une Condition** dispose d’un ensemble de zéro ou plusieurs **Conditions de correspondance**. Tous les **Conditions de correspondance** doit avoir la valeur TRUE pour la Condition sélectionnez soit évaluée comme TRUE. Toutes ces conditions sont évaluées par rapport à une seule revendication. Une revendication qui correspond à un **sélectionner une Condition** peuvent être identifiés par un **identificateur** et en le **Action de règle**.  
  
-   Chaque **une Condition de correspondance** spécifie la condition pour faire correspondre les **Type** ou **valeur** ou **ValueType** d’une revendication à l’aide de différents **opérateurs de Condition** et **opérateurs de chaîne**.  
  
    -   Lorsque vous spécifiez un **une Condition de correspondance** pour un **valeur**, vous devez également spécifier un **une Condition de correspondance** pour un spécifique **ValueType** et vice versa. Ces conditions doivent être en regard de chacun des autres dans la syntaxe.  
  
    -   **Type de valeur** correspondance conditions doit utiliser spécifique **ValueType** littéraux uniquement.  
  
-   Un **Action de règle** permet de copier une revendication qui est marquée avec une **identificateur** ou d’émettre une revendication basée sur une revendication qui est marquée avec un identificateur et/ou donné opérateurs de chaîne.  
  
**Exemple de règle**  
  
Cet exemple montre une règle qui peut être utilisée pour convertir les Type de revendications entre deux forêts, autant qu’elles utilisent les revendications mêmes types valeur et ont la même interprétation des valeurs de revendications pour ce type. La règle est une condition de correspondance et une instruction d’émission qui utilise les opérateurs de chaîne et une référence de revendications correspondant.  
  
```  
C1: [TYPE=="EmployeeType"]    
                 => ISSUE (TYPE= "EmpType", VALUE = C1.VALUE, VALUETYPE = C1.VALUETYPE);  
[TYPE=="EmployeeType"] == Select Condition List with one Matching Condition for claims Type.  
ISSUE (TYPE= "EmpType", VALUE = C1.VALUE, VALUETYPE = C1.VALUETYPE) == Rule Action that issues a claims using string literal and matching claim referred with the Identifier.  
  
```  
  
### <a name="runtime-operation"></a>Opération Runtime  
Il est important de comprendre le fonctionnement de l’exécution de transformations de revendications pour créer les règles de manière efficace. L’opération de runtime utilise trois ensembles de revendications:  
  
1.  **Jeu de revendications d’entrée**: le jeu de revendications qui figurent à l’opération de transformation des revendications d’entrée.  
  
2.  **Utilisation de revendications ensemble**: intermédiaire des revendications qui sont lues et écrites dans lors de la transformation des revendications.  
  
3.  **Jeu de revendications de sortie**: résultat de l’opération de transformation des revendications.  
  
Voici un bref aperçu de l’opération de transformation des revendications de runtime:  
  
1.  Revendications d’entrée pour la transformation des revendications sont utilisées pour initialiser le jeu de revendications de travail.  
  
    1.  Lors du traitement de chaque règle, le jeu de revendications de travail est utilisé pour les revendications d’entrée.  
  
    2.  La liste de conditions de sélection dans une règle est comparée à tous les jeux possible des revendications à partir du jeu de revendications de travail.  
  
    3.  Chaque ensemble de revendications est utilisé pour exécuter l’action dans cette règle.  
  
    4.  Exécution d’une règle résultats de l’action dans une seule revendication, qui est ajouté à la sortie des revendications ensemble et le jeu de revendications de travail. Par conséquent, la sortie d’une règle est utilisée comme entrée pour les règles suivantes dans l’ensemble de règles.  
  
2.  Les règles dans l’ensemble de règles sont traités dans un ordre séquentiel à partir de la première règle.  
  
3.  Lorsque l’ensemble de règles entière est traitée, l’ensemble de revendications de sortie est traitée pour supprimer les doublons de revendications et autres issues.The sont la sortie du processus de transformation de revendications.  
  
Il est possible d’écrire des transformations de revendications complexe basé sur le comportement d’exécution précédente.  
  
**Exemple: Opération de Runtime**  
  
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
Syntaxe spéciale pour les règles sont les suivantes:  
  
1.  Vider l’ensemble de règles ne == aucune revendications de sortie  
  
2.  Sélectionnez la liste de conditions de vider == chaque revendication correspond à la liste de Condition sélectionnez  
  
    **Exemple: Liste de Condition sélectionnez vide**  
  
    La règle suivante correspond à chaque revendication dans la plage de travail.  
  
    ```  
    => Issue (Type = "UserType", Value = "External", ValueType = "string")  
    ```  
  
3.  Vider la liste de mise en correspondance sélectionnez == chaque revendication, correspond à la liste de Condition sélectionnez  
  
    **Exemple: Conditions de correspondance vide**  
  
    La règle suivante correspond à chaque revendication dans la plage de travail. Il s’agit de la règle de base «autoriser tout» si elle est utilisée seule.  
  
    ```  
    C1:[] => Issule (claim = C1);  
    ```  
  
## <a name="security-considerations"></a>Considérations de sécurité  
**Revendications qui permet d’entrer une forêt**  
  
Les revendications présentées par des entités de sécurité qui sont entrantes vers une forêt doivent être inspectés soigneusement pour vous assurer que nous autoriser ou émettre uniquement les revendications correctes. Revendications incorrectes peuvent compromettre la sécurité de la forêt, et il doit s’agir d’une considération supérieure lors de la création des stratégies de transformation des revendications qui permet d’entrer une forêt.  
  
ActiveDirectory comprend les fonctionnalités suivantes afin d’éviter une mauvaise configuration des revendications qui permet d’entrer une forêt:  
  
-   Si aucune stratégie de transformation des revendications définie pour les revendications qui permet d’entrer une forêt, pour des raisons de sécurité, dispose d’une approbation de forêt ActiveDirectory supprime toutes les revendications principales, entrez la forêt.  
  
-   Si vous exécutez la règle définie sur les revendications qui passe les résultats de la forêt dans les revendications qui ne sont pas définies dans la forêt, les revendications undefined sont supprimées des revendications de sortie.  
  
**Revendications qui quittent une forêt**  
  
Revendications qui quittent une forêt présente un problème de sécurité inférieur pour la forêt que les revendications qui permet d’entrer la forêt. Revendications sont autorisées à quitter la forêt en tant que-est la même lorsqu’il n’existe aucune revendication correspondante stratégie de transformation en place. Il est également possible d’émettre des revendications qui ne sont pas définies dans la forêt dans le cadre de la transformation des revendications qui quittent la forêt. Il s’agit à configurer facilement des approbations inter-forêts avec les revendications. Un administrateur peut déterminer si les revendications qui permet d’entrer la forêt doivent être transformé et configurer la stratégie appropriée. Par exemple, un administrateur peut définir une stratégie s’il est nécessaire pour masquer une revendication pour empêcher la divulgation d’informations.  
  
**Erreurs de syntaxe dans les règles de transformation des revendications**  
  
Si une stratégie de transformation des revendications donné dispose d’un ensemble de règles est incorrect ou s’il existe des autres problèmes de syntaxe ou de stockage, la stratégie est considéré comme non valide. Cela est traité différemment des conditions par défaut mentionnées précédemment.  
  
ActiveDirectory ne peut pas déterminer l’intention dans ce cas et passe en mode de sécurité intégrée, où aucun revendications de sortie ne sont générées sur cette approbation + la direction de parcours. Intervention de l’administrateur est requis pour corriger le problème. Cela peut se produire si LDAP est utilisé pour modifier la stratégie de transformation des revendications. Applets de commande Windows PowerShell pour ActiveDirectory ont validation en place pour empêcher l’écriture d’une stratégie avec les problèmes de syntaxe.  
  
## <a name="other-language-considerations"></a>Autres considérations relatives à la langue  
  
1.  Il existe plusieurs mots clés ou des caractères spéciaux dans cette langue (appelée terminaux). Celles-ci sont présentées dans le [terminaux langue](Claims-Transformation-Rules-Language.md#BKMK_LT) table plus loin dans cette rubrique. Les messages d’erreur utilisent les balises pour ces terminaux de levée d’ambiguïté.  
  
2.  Terminaux peuvent parfois être utilisés en tant qu’opérateurs de chaîne. Toutefois, l’utilisation de ce type peut avoir ou en conflit avec la définition du langage des conséquences inattendues. Ce type d’utilisation n’est pas recommandé.  
  
3.  L’action de règle ne peut pas effectuer des conversions de types de valeurs de revendication, et un ensemble de règles qui contient cette action de règle est considéré comme non valide. Cela peut provoquer une erreur d’exécution, et aucune revendications de sortie ne sont produites.  
  
4.  Si une action de règle fait référence à un identificateur qui n’a pas été utilisé dans la partie de la liste Sélectionnez la Condition de la règle, il est une utilisation non valide. Cela peut provoquer une erreur de syntaxe.  
  
    **Exemple: Référence d’identificateur Incorrect**  
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
  
    Utilisation du moteur Regex  
  
    ```  
    C1: [type =~ "XYZ*"] => Issue (claim = C1);  
    ```  
  
-   **Interdire un certain type de revendication**  
    Type exact  
  
    ```  
    C1:[type != "XYZ"] => Issue (claim=C1);  
    ```  
  
    Utilisation du moteur Regex  
  
    ```  
    C1:[Type !~ "XYZ?"] => Issue (claim=C1);  
    ```  
  
## <a name="examples-of-rules-parser-errors"></a>Exemples de règles d’erreurs d’analyse  
Règles de transformation des revendications sont analysées par un analyseur personnalisé pour détecter les erreurs de syntaxe. Cet analyseur est exécuté par les applets de commande Windows PowerShell connexes avant de stocker des règles dans ActiveDirectory. Les erreurs dans les règles, y compris les erreurs de syntaxe, d’analyse sont imprimés sur la console. Contrôleurs de domaine également exécutent l’analyseur avant d’utiliser les règles de transformation des revendications, et qu’ils connectent à des erreurs dans le journal des événements (ajouter des numéros de journal des événements).  
  
Cette section illustre quelques exemples de règles qui sont écrits avec une syntaxe incorrecte et la syntaxe correspondante des erreurs générées par l’analyseur.  
  
1.  Exemple:  
  
    ```  
    c1;[]=>Issue(claim=c1);  
    ```  
  
    Cet exemple a un point-virgule utilisée de manière incorrecte à la place d’un signe deux-points.   
    **Message d’erreur:**  
    *POLICY0002: N’a pas pu analyser les données de la stratégie.*  
    *Numéro de ligne: 1, numéro de colonne: 2, erreur jeton:;. Ligne: «c1; [] = > issue(claim=c1);».*  
    *Erreur d’analyse: ' POLICY0030: erreur de syntaxe, inattendue «;», attendue une des opérations suivantes: «:».»*  
  
2.  Exemple:  
  
    ```  
    c1:[]=>Issue(claim=c2);  
    ```  
  
    Dans cet exemple, la balise d’identificateur dans l’instruction d’émission de copie n’est pas définie.   
    **Message d’erreur**:   
    *POLICY0011: Aucune condition dans la règle de revendication correspond à la balise de condition spécifiée dans le CopyIssuanceStatement: 'c2'.*  
  
3.  Exemple:  
  
    ```  
    c1:[type=="x1", value=="1", valuetype=="bool"]=>Issue(claim=c1)  
    ```  
  
    «bool» n’est pas un Terminal dans la langue, et il n’est pas un type de valeur valide. Terminaux valides sont répertoriés dans le message d’erreur suivant.   
    **Message d’erreur:**  
    *POLICY0002: N’a pas pu analyser les données de la stratégie.*  
    Numéro de ligne: 1, numéro de colonne: 39, erreur jeton: «bool». Ligne: ' c1: [type == «x1», valeur = «1», valuetype == «bool»] = > issue(claim=c1);».   
    *Erreur d’analyse: «POLICY0030: erreur de syntaxe, inattendue «chaîne», qui attend une des opérations suivantes: «INT64_TYPE» 'UINT64_TYPE' 'Exemple', 'BOOLEAN_TYPE', 'Identificateur'*  
  
4.  Exemple:  
  
    ```  
    c1:[type=="x1", value==1, valuetype=="boolean"]=>Issue(claim=c1);  
    ```  
  
    Le chiffre **1** dans cet exemple n’est pas un jeton valide dans la langue et l’utilisation de ce type n’est pas autorisée dans une condition de correspondance. Il doit être entouré de guillemets doubles pour rendre une chaîne.   
    **Message d’erreur:**  
    *POLICY0002: N’a pas pu analyser les données de la stratégie.*  
    *Numéro de ligne: 1, numéro de colonne: 23, erreur jeton: 1. ligne: «c1: [type == «x1», valeur == 1, valuetype == «bool»] = > issue(claim=c1);». **Erreur d’analyse: ' POLICY0029: entrée inattendue.*  
  
5.  Exemple:  
  
    ```  
    c1:[type == "x1", value == "1", valuetype == "boolean"] =>   
  
         Issue(type = c1.type, value="0", valuetype == "boolean");  
    ```  
  
    Cet exemple montre comment utilise un signe égal double (==) au lieu d’un seul signe égal (=).   
    **Message d’erreur:**  
    *POLICY0002: N’a pas pu analyser les données de la stratégie.*  
    *Numéro de ligne: 1, numéro de colonne: 91, erreur jeton: ==. Ligne: «c1: [type == «x1», valeur = «1»,*  
    *ValueType == «booléen»] = > problème (type=c1.type, valeur = «0», valuetype == «booléen»);».*  
    *Erreur d’analyse: «POLICY0030: erreur de syntaxe, inattendue '==', attendez une des opérations suivantes: '='*  
  
6.  Exemple:  
  
    ```  
    c1:[type=="x1", value=="boolean", valuetype=="string"] =>   
  
          Issue(type=c1.type, value=c1.value, valuetype = "string");  
    ```  
  
    Cet exemple est correct de syntaxe et de la sémantique. Toutefois, l’utilisation «booléenne» comme une valeur de chaîne est liée à confusion, et il doit être évitée. Comme mentionné précédemment, à l’aide de terminaux langue comme valeurs de revendications doivent donc être évitées lorsque cela est possible.  
  
## <a name="BKMK_LT"></a>Terminaux de langue  
Le tableau suivant répertorie l’ensemble complet des chaînes de Terminal Server et les bornes langue associée qui sont utilisés dans le langage de règles de transformation de revendications. Ces définitions utilisent des chaînes de UTF-16pas la casse.  
  
|Chaîne|Terminal Server|  
|----------|------------|  
|"=>"|IMPLIQUE|  
|";"|POINT-VIRGULE|  
|":"|SIGNE DEUX-POINTS|  
|","|VIRGULE|  
|"."|POINT|  
|"["|O_SQ_BRACKET|  
|"]"|C_SQ_BRACKET|  
|"("|O_BRACKET|  
|")"|C_BRACKET|  
|"=="|EQ|  
|"!="|NEQ|  
|"=~"|REGEXP_MATCH|  
|"!~"|REGEXP_NOT_MATCH|  
|"="|ATTRIBUER|  
|"&&"|ET|  
|«problème»|PROBLÈME|  
|«type»|TYPE|  
|«value»|VALEUR|  
|«valuetype»|VALUE_TYPE|  
|«revendications»|REVENDICATION|  
|«[_A-Za-z][_A-Za-z0-9]*»|IDENTIFICATEUR|  
|"\\"[^\\"\n]*\\""|CHAÎNE|  
|«uint64»|UINT64_TYPE|  
|«int64»|INT64_TYPE|  
|«chaîne»|EXEMPLE|  
|«booléenne»|BOOLEAN_TYPE|  
  
## <a name="language-syntax"></a>Syntaxe du langage  
Le langage des règles de transformation revendications suivant est spécifié sous forme de l’extension ABNF. Cette définition utilise les bornes sont spécifiés dans le tableau précédent, en plus de la production d’extension ABNF défini ici. Les règles doivent être codés en UTF-16, et les comparaisons de chaînes doivent être traités comme de la casse.  
  
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
  


