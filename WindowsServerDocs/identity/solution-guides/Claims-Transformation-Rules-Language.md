---
ms.assetid: e831f781-3c45-4d44-b411-160d121d1324
title: Langage de règles de Transformation de revendications
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 4a6b378bc4aef180ebedd260008febaa2f2a76ae
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856210"
---
# <a name="claims-transformation-rules-language"></a>Langage de règles de Transformation de revendications

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La forêt sur des revendications permet de fonctionnalité de transformation vous permettent de pont des revendications pour le contrôle d’accès dynamique au-delà des limites de la forêt en définissant des stratégies de transformation de revendications sur les approbations entre forêts. Le composant principal de toutes les stratégies est de règles qui sont écrits en langage de règles de transformation de revendications. Cette rubrique fournit des détails sur ce langage et fournit des conseils sur la création de règles de transformation des revendications.  
  
Les applets de commande Windows PowerShell pour les stratégies de transformation sur entre forêts approuve ont options pour définir des stratégies simples qui sont nécessaires en commun des scénarios. Ces applets de commande traduire l’entrée utilisateur dans les stratégies et les règles dans le langage de règles de transformation de revendications et les stockent dans Active Directory dans le format prescrit. Pour plus d’informations sur les applets de commande pour la transformation des revendications, consultez la [applets de commande AD DS pour le contrôle d’accès dynamique](https://go.microsoft.com/fwlink/?LinkId=243150).  
  
Selon la configuration des revendications et les respect des exigences concernant l’approbation entre forêts dans vos forêts Active Directory, vos stratégies de transformation de revendications peuvent avoir à être plus complexe que les stratégies prises en charge par les applets de commande Windows PowerShell pour Active Répertoire. Pour créer efficacement ces stratégies, il est essentiel de comprendre la syntaxe de langage de règles de transformation revendications et la sémantique. Ce langage des règles de transformation (le « langue ») dans Active Directory est un sous-ensemble du langage qui est utilisé par des revendications [Active Directory Federation Services](https://go.microsoft.com/fwlink/?LinkId=243982) pour similaires à des fins et il a une syntaxe et une sémantique similaire. Toutefois, il existe moins d’opérations autorisées, et les restrictions de syntaxe supplémentaires sont placées dans la version d’Active Directory de la langue.  
  
Cette rubrique explique brièvement la syntaxe et la sémantique du langage de règles de transformation de revendications dans Active Directory et les considérations à être apportées lors de la création de stratégies. Il fournit plusieurs ensembles de règles de l’exemple pour vous aider à démarrer et des exemples de syntaxe incorrecte et les messages qu’ils génèrent, pour vous aider à déchiffrer les messages d’erreur lorsque vous créez les règles.  
  
## <a name="tools-for-authoring-claims-transformation-policies"></a>Outils de création de stratégies de transformation des revendications  
**Applets de commande Windows PowerShell pour Active Directory**: Il s’agit de la façon par défaut et recommandée pour créer et définir des stratégies de transformation de revendications. Ces applets de commande fournissent des commutateurs pour des stratégies simples et vérifiez les règles qui sont définies pour les stratégies plus complexes.  
  
**LDAP**: Stratégies de transformation des revendications peuvent être modifiées dans Active Directory via Lightweight Directory Access Protocol (LDAP). Toutefois, cela est déconseillé, car les stratégies ont plusieurs composants complexes, et les outils que vous utilisez ne peuvent pas valider la stratégie avant leur écriture dans Active Directory. Cela peut nécessiter par la suite d’une quantité considérable de temps pour diagnostiquer les problèmes.  
  
## <a name="active-directory-claims-transformation-rules-language"></a>Langage des règles de transformation de revendications Active Directory  
  
### <a name="syntax-overview"></a>Vue d’ensemble de la syntaxe  
Voici un bref aperçu de la syntaxe et sémantique du langage :  
  
-   L’ensemble de règles de transformation de revendications se compose de zéro ou plusieurs règles. Chaque règle comporte deux parties actives : **Sélectionnez la liste de conditions** et **Rule Action**. Si le **sélectionner une liste de conditions** a la valeur TRUE, l’action de règle correspondante est exécutée.  
  
-   **Sélectionnez la liste de conditions** a zéro ou plusieurs **Conditions sélectionnez**. Tous les **sélectionner les Conditions** doit avoir la valeur TRUE pour le **sélectionner une liste de conditions** soit évaluée à TRUE.  
  
-   Chaque **sélectionner la Condition** possède un ensemble de zéro ou plusieurs **Conditions de correspondance**. Tous les **Conditions de correspondance** doit avoir la valeur TRUE pour le sélectionner la Condition soit évaluée à TRUE. Toutes ces conditions sont évaluées par rapport à une seule revendication. Une revendication qui correspond à un **sélectionner la Condition** peuvent être identifiés par un **identificateur** et mentionnées dans le **Action de règle**.  
  
-   Chaque **une Condition de correspondance** spécifie la condition de correspondance la **Type** ou **valeur** ou **ValueType** d’une revendication à l’aide de différents  **Opérateurs de condition** et **littéraux de chaîne**.  
  
    -   Lorsque vous spécifiez un **une Condition de correspondance** pour un **valeur**, vous devez également spécifier un **une Condition de correspondance** pour un spécifique **ValueType** et vice versa. Ces conditions doivent être en regard de chacun des autres dans la syntaxe.  
  
    -   **ValueType** conditions de correspondance doit utiliser spécifique **ValueType** littéraux uniquement.  
  
-   Un **Action de règle** pouvez copier une revendication qui est marquée avec un **identificateur** ou émettre une revendication basée sur une revendication qui est marquée avec un identificateur et/ou donné des littéraux de chaîne.  
  
**Exemple de règle**  
  
Cet exemple montre une règle qui peut être utilisée pour convertir les revendications de Type entre deux forêts, autant qu’elles utilisent les mêmes revendications types valeur et ont les interprétations de mêmes pour les valeurs de revendications pour ce type. La règle a une condition de correspondance et une instruction d’émission qui utilise des littéraux de chaîne et une référence de revendications correspondant.  
  
```  
C1: [TYPE=="EmployeeType"]    
                 => ISSUE (TYPE= "EmpType", VALUE = C1.VALUE, VALUETYPE = C1.VALUETYPE);  
[TYPE=="EmployeeType"] == Select Condition List with one Matching Condition for claims Type.  
ISSUE (TYPE= "EmpType", VALUE = C1.VALUE, VALUETYPE = C1.VALUETYPE) == Rule Action that issues a claims using string literal and matching claim referred with the Identifier.  
  
```  
  
### <a name="runtime-operation"></a>Opération d’exécution  
Il est important de comprendre le fonctionnement de l’exécution de transformations de revendications pour créer les règles de manière efficace. L’opération d’exécution utilise trois ensembles de revendications :  
  
1.  **Ensemble de revendications d’entrée**: Ensemble de revendications qui sont fournies à l’opération de transformation des revendications d’entrée.  
  
2.  **Jeu de revendications de travail**: Revendications intermédiaires qui sont lus à partir d’et écrites à lors de la transformation de revendications.  
  
3.  **Ensemble de revendications de sortie**: Sortie de l’opération de transformation de revendications.  
  
Voici une brève présentation de l’opération de transformation de revendications de runtime :  
  
1.  Les revendications d’entrée pour la transformation des revendications sont utilisées pour initialiser l’ensemble de revendications de travail.  
  
    1.  Lors du traitement de chaque règle, l’ensemble de revendications de travail est utilisé pour les revendications d’entrée.  
  
    2.  La liste de conditions de sélection dans une règle est comparée à tous les ensembles possibles de revendications à partir de l’ensemble de revendications de travail.  
  
    3.  Chaque jeu de revendications correspondantes est utilisé pour exécuter l’action dans cette règle.  
  
    4.  Exécution d’une règle résultats d’action dans une revendication, qui est ajouté à la sortie des revendications ensemble et l’ensemble de revendications de travail. Par conséquent, la sortie à partir d’une règle est utilisée comme entrée pour les règles suivantes dans l’ensemble de règles.  
  
2.  Les règles dans l’ensemble de règles sont traitées dans l’ordre séquentiel, en commençant par la première règle.  
  
3.  Lors du traitement de l’ensemble de règles entier, l’ensemble de revendications de sortie est traitée pour supprimer les doublons de revendications et pour d’autres problèmes de sécurité. Les revendications qui en résulte sont le résultat du processus de transformation de revendications.  
  
Il est possible d’écrire des transformations de revendications complexes basés sur le comportement d’exécution précédente.  
  
**Exemple : Opération d’exécution**  
  
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
Une syntaxe spéciale pour les règles sont les suivantes :  
  
1.  Vider l’ensemble de règles ne == aucune revendication de sortie  
  
2.  Sélectionner la liste de conditions de vide == chaque revendication correspond à la liste Select de Condition  
  
    **Exemple : Sélectionnez la Condition vide liste**  
  
    La règle suivante correspond à chaque revendication dans le jeu de travail.  
  
    ```  
    => Issue (Type = "UserType", Value = "External", ValueType = "string")  
    ```  
  
3.  Vide la liste Select de mise en correspondance == chaque revendication, correspond à la liste Select de Condition  
  
    **Exemple : Conditions de correspondance vide**  
  
    La règle suivante correspond à chaque revendication dans le jeu de travail. Il s’agit de la règle de base « Allow-all » s’il est utilisé seul.  
  
    ```  
    C1:[] => Issule (claim = C1);  
    ```  
  
## <a name="security-considerations"></a>Considérations relatives à la sécurité  
**Revendications qui permet d’entrer une forêt**  
  
Les revendications présentées par des principaux qui entrent dans une forêt doivent être inspectés de soigneusement pour vous assurer que nous autoriser ou émettre uniquement les revendications appropriées. Revendications incorrectes peuvent compromettre la sécurité de la forêt, et cela doit être une considération supérieure lors de la création de stratégies de transformation des revendications qui permet d’entrer une forêt.  
  
Active Directory présente les caractéristiques suivantes afin d’éviter une mauvaise configuration des revendications qui permet d’entrer une forêt :  
  
-   Si une approbation de forêt n’a aucune stratégie de transformation de revendications défini pour les revendications qui permet d’entrer une forêt, pour des raisons de sécurité, Active Directory supprime toutes les revendications principal qui permet d’entrer la forêt.  
  
-   Si la règle définie sur les revendications qu’entre les résultats de la forêt dans les revendications qui ne sont pas définies dans la forêt en cours d’exécution, les revendications non définies sont supprimées à partir des revendications de sortie.  
  
**Revendications de sortie d’une forêt**  
  
Revendications qui laissent une forêt présente un problème de sécurité moindres pour la forêt que les revendications qui permet d’entrer la forêt. Revendications sont autorisées à laisser de la forêt en tant que-est du même si aucune revendication correspondante stratégie de transformation en place. Il est également possible d’émettre des revendications qui ne sont pas définies dans la forêt dans le cadre de la transformation des revendications qui laissent la forêt. Cela consiste à configurer facilement des approbations inter-forêts avec les revendications. Un administrateur peut déterminer si les revendications qui permet d’entrer la forêt doivent être transformées et configurer la stratégie appropriée. Par exemple, un administrateur peut définir une stratégie s’il est nécessaire pour masquer une revendication pour empêcher la divulgation d’informations.  
  
**Erreurs de syntaxe dans les règles de transformation de revendications**  
  
Si une stratégie de transformation de revendications donné possède un ensemble de règles qui a une syntaxe incorrect ou s’il existe des autres problèmes de syntaxe ou de stockage, la stratégie est considérée comme non valide. Celle-ci est traitée différemment les conditions par défaut mentionnées précédemment.  
  
Active Directory ne peut pas déterminer l’intention dans ce cas et entre dans un mode de prévention de défaillance, où aucune revendication de sortie n’est générées sur cette approbation + la direction de parcours. Intervention de l’administrateur est nécessaire pour corriger le problème. Cela peut se produire si LDAP est utilisé pour modifier la stratégie de transformation de revendications. Applets de commande Windows PowerShell pour Active Directory ont validation en place pour empêcher l’écriture d’une stratégie avec les problèmes de syntaxe.  
  
## <a name="other-language-considerations"></a>Autres considérations relatives à la langue  
  
1.  Il existe plusieurs mots clés ou des caractères spécifiques dans cette langue (dénommée terminaux). Celles-ci sont présentées dans le [terminaux de langage](Claims-Transformation-Rules-Language.md#BKMK_LT) tableau plus loin dans cette rubrique. Les messages d’erreur recourir aux balises pour ces terminaux pour éviter les ambiguïtés.  
  
2.  Les terminaux peuvent parfois être utilisés en tant que littéraux de chaîne. Toutefois, une telle utilisation peut entrer en conflit avec la définition de langage ou avoir des conséquences inattendues. Ce genre d’utilisation n’est pas recommandé.  
  
3.  L’action de règle ne peut pas effectuer de conversion de type sur les valeurs de revendication et un ensemble de règles qui contient cette action de règle est considérée comme non valide. Cela entraînerait une erreur d’exécution, et aucune revendication de sortie n’est produites.  
  
4.  Si une action de règle fait référence à un identificateur qui n’a pas été utilisé dans la partie de sélectionner une liste de conditions de la règle, il est une utilisation non valide. Cela entraînerait une erreur de syntaxe.  
  
    **Exemple : Référence d’identificateur incorrecte**  
    La règle suivante illustre un identificateur incorrect utilisé dans une action de règle.  
  
    ```  
    C1:[] => Issue (claim = C2);  
    ```  
  
## <a name="sample-transformation-rules"></a>Exemples de règles de transformation  
  
-   **Autoriser toutes les revendications d’un certain type**  
  
    Type exact  
  
    ```  
    C1:[type=="XYZ"] => Issue (claim = C1);  
    ```  
  
    À l’aide de Regex  
  
    ```  
    C1: [type =~ "XYZ*"] => Issue (claim = C1);  
    ```  
  
-   **Interdire un certain type de revendication**  
    Type exact  
  
    ```  
    C1:[type != "XYZ"] => Issue (claim=C1);  
    ```  
  
    À l’aide de Regex  
  
    ```  
    C1:[Type !~ "XYZ?"] => Issue (claim=C1);  
    ```  
  
## <a name="examples-of-rules-parser-errors"></a>Exemples de règles d’erreurs d’analyse  
Règles de transformation des revendications sont analysées par un analyseur personnalisé pour rechercher les erreurs de syntaxe. Cet analyseur est exécuté par les applets de commande Windows PowerShell associées avant de stocker des règles dans Active Directory. Toutes les erreurs dans les règles, notamment les erreurs de syntaxe, d’analyse sont imprimés sur la console. Contrôleurs de domaine exécutent également l’analyseur avant d’utiliser les règles pour transformer les revendications et consigner les erreurs dans le journal des événements (ajouter des numéros de journal des événements).  
  
Cette section illustre quelques exemples de règles qui sont écrites avec une syntaxe incorrecte et la syntaxe correspondante erreurs générées par l’analyseur.  
  
1.  Exemple :  
  
    ```  
    c1;[]=>Issue(claim=c1);  
    ```  
  
    Cet exemple comprend un point-virgule utilisée de manière incorrecte à la place d’un signe deux-points.   
    **Message d’erreur :**  
    *POLICY0002 : N’a pas pu analyser les données de stratégie.*  
    *Numéro de ligne : 1, numéro de colonne : 2, erreur jeton : ;. Line: 'c1;[]=>Issue(claim=c1);'.*  
    *Erreur d’analyse : ' POLICY0030 : Erreur de syntaxe inattendue ';', l’une des suivantes attendue : ' :'.'*  
  
2.  Exemple :  
  
    ```  
    c1:[]=>Issue(claim=c2);  
    ```  
  
    Dans cet exemple, la balise de l’identificateur dans l’instruction d’émission de copie n’est pas définie.   
    **Message d’erreur**:   
    *POLICY0011 : Aucune condition dans la règle de revendication correspond à la balise de la condition spécifiée dans le CopyIssuanceStatement : « c2 ».*  
  
3.  Exemple :  
  
    ```  
    c1:[type=="x1", value=="1", valuetype=="bool"]=>Issue(claim=c1)  
    ```  
  
    « bool » n’est pas un Terminal dans le langage, et il n’est pas un type de valeur valide. Les terminaux valides sont répertoriées dans le message d’erreur suivant.   
    **Message d’erreur :**  
    *POLICY0002 : N’a pas pu analyser les données de stratégie.*  
    Numéro de ligne : 1, numéro de colonne : 39, jeton d’erreur : « bool ». Line: 'c1:[type=="x1", value=="1",valuetype=="bool"]=>Issue(claim=c1);'.   
    *Erreur d’analyse : ' POLICY0030 : Erreur de syntaxe, inattendue 'STRING', sauf pour un des éléments suivants : 'INT64_TYPE' 'UINT64_TYPE' 'STRING_TYPE' 'BOOLEAN_TYPE' 'IDENTIFIER'*  
  
4.  Exemple :  
  
    ```  
    c1:[type=="x1", value==1, valuetype=="boolean"]=>Issue(claim=c1);  
    ```  
  
    Le chiffre **1** dans cet exemple n’est pas un jeton valide dans la langue, et cette utilisation n’est pas autorisée dans une condition de correspondance. Il doit être encadré de guillemets doubles pour sous forme de chaîne.   
    **Message d’erreur :**  
    *POLICY0002 : N’a pas pu analyser les données de stratégie.*  
    *Numéro de ligne : 1, numéro de colonne : 23, erreur jeton : 1. Line: 'c1:[type=="x1", value==1, valuetype=="bool"]=>Issue(claim=c1);'.**Parser error: ' POLICY0029 : Entrée inattendue.*  
  
5.  Exemple :  
  
    ```  
    c1:[type == "x1", value == "1", valuetype == "boolean"] =>   
  
         Issue(type = c1.type, value="0", valuetype == "boolean");  
    ```  
  
    Cet exemple utilise un double signe égal (==) au lieu d’un seul signe égal (=).   
    **Message d’erreur :**  
    *POLICY0002 : N’a pas pu analyser les données de stratégie.*  
    *Numéro de ligne : 1, numéro de colonne : 91, jeton d’erreur : ==. Line: 'c1:[type=="x1", value=="1",*  
    *valuetype=="boolean"]=>Issue(type=c1.type, value="0", valuetype=="boolean");'.*  
    *Erreur d’analyse : ' POLICY0030 : Erreur de syntaxe, inattendue '==', sauf pour un des éléments suivants : '='*  
  
6.  Exemple :  
  
    ```  
    c1:[type=="x1", value=="boolean", valuetype=="string"] =>   
  
          Issue(type=c1.type, value=c1.value, valuetype = "string");  
    ```  
  
    Cet exemple est syntaxiquement et sémantiquement correct. Toutefois, l’utilisation de « boolean » comme valeur de chaîne est liée à confusion, et il doit être évitée. Comme mentionné précédemment, à l’aide de terminaux de langage comme valeurs de revendications doivent être évitées si possible.  
  
## <a name="BKMK_LT"></a>Terminaux de langage  
Le tableau suivant répertorie l’ensemble complet des chaînes de terminal et les terminaux de langage associé sont utilisés dans le langage de règles de transformation de revendications. Ces définitions utilisent des chaînes UTF-16 non-respect de la casse.  
  
|Chaîne|Terminal Server|  
|----------|------------|  
|"=>"|INSINUER|  
|";"|POINT-VIRGULE|  
|":"|SIGNE DEUX-POINTS|  
|","|COMMA|  
|"."|DOT|  
|"["|O_SQ_BRACKET|  
|"]"|C_SQ_BRACKET|  
|"("|O_BRACKET|  
|")"|C_BRACKET|  
|"=="|EQ|  
|"!="|NEQ|  
|"=~"|REGEXP_MATCH|  
|"!~"|REGEXP_NOT_MATCH|  
|"="|AFFECTER|  
|"&&"|ET|  
|« problème »|PROBLÈME|  
|« type »|TYPE|  
|« valeur »|VALEUR|  
|"valuetype"|VALUE_TYPE|  
|« revendication »|REVENDICATION|  
|"[_A-Za-z][_A-Za-z0-9]*"|IDENTIFICATEUR|  
|"\\"[^\\"\n]*\\""|CHAÎNE|  
|"uint64"|UINT64_TYPE|  
|"int64"|INT64_TYPE|  
|« chaîne »|STRING_TYPE|  
|« boolean »|BOOLEAN_TYPE|  
  
## <a name="language-syntax"></a>Syntaxe du langage  
Le langage de règles de transformation revendications suivant est spécifié sous forme ABNF. Cette définition utilise les terminaux sont spécifiés dans le tableau précédent, outre les productions ABNF définis ici. Les règles doivent être encodées en UTF-16, et les comparaisons de chaînes doivent être traitées comme respectent pas la casse.  
  
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
  


