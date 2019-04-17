---
ms.assetid: 65e474b5-3076-4ba3-809d-a09160f7c2bd
title: "Le rôle de règles de revendication"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e47dbecdeee620d3a237ad8d8c41a550d3ef069c
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

# <a name="the-role-of-claim-rules"></a>Le rôle de règles de revendication
La fonction globale du Service de fédération dans ActiveDirectory Federation Services \(ADFS\) consiste à émettre un jeton qui contient un ensemble de revendications. La décision concernant les revendications ADFS, puis émises est régie par les règles de revendication.  
  
## <a name="what-are-claim-rules"></a>Quelles sont les règles de revendication?  
Une règle de revendication représente une instance de logique métier qui prend un ou plusieurs revendications entrantes, leur applique des conditions \ (s’il x y\ puis) et de produire un ou plusieurs revendications sortantes en fonction des paramètres de la condition. Pour plus d’informations sur les revendications entrantes et sortantes, consultez [The Role of Claims ](The-Role-of-Claims.md).  
  
Vous utilisez les règles de revendication lorsque vous avez besoin d’implémenter la logique métier qui contrôle le flux des revendications à travers le pipeline de revendications. Pendant que le pipeline de revendications est davantage un concept logique du end\ celle-ci-du processus de circulation des revendications, les règles de revendication sont un élément d’administration réel que vous pouvez utiliser pour personnaliser le flux des revendications à travers le processus d’émission de revendications.  
  
Pour plus d’informations sur le pipeline de revendications, voir [The Role of the Claims Engine ](The-Role-of-the-Claims-Engine.md).  
  
Règles de revendication présentent les avantages suivants:  
  
-   Fournissent un mécanisme permettant aux administrateurs d’appliquer la logique métier de l’heure de l’exécuter pour approuver les revendications à partir de fournisseurs de revendications  
  
-   Fournissent un mécanisme permettant aux administrateurs de définir les revendications publiées sur les parties de confiance  
  
-   Fournir des fonctionnalités d’autorisation riches et détaillées basées sur les claims\ aux administrateurs qui souhaitent autoriser ou refuser l’accès à des utilisateurs spécifiques  
  
### <a name="how-claim-rules-are-processed"></a>Traitement des règles de revendication  
Les règles de revendication sont traitées via le pipeline de revendications à l’aide de la *moteur de revendications *. Le moteur de revendications est un composant logique du Service de fédération qui examine le jeu des revendications entrantes présenté par un utilisateur et puis, en fonction de la logique de chaque règle, produit un jeu de revendications de sortie.  
  
Ensemble, les règles de revendication moteur et l’ensemble des règles associées à une approbation fédérée donnée déterminent si les revendications entrantes doivent être passées qu’ils sont, filtrées pour répondre aux critères d’une condition spécifique ou transformées en un tout nouveau jeu de revendications avant qu’ils sont émises en tant que revendications sortantes par votre Service de fédération de revendication.  
  
Pour plus d’informations sur ce processus, voir [The Role of the Claims Engine ](The-Role-of-the-Claims-Engine.md).  
  
## <a name="what-are-claim-rule-templates"></a>Quelles sont les modèles de règle de revendication?  
ADFS inclut un ensemble prédéfini de règle de revendication modèles qui sont conçues pour vous aider à facilement sélectionnent et créent les règles de revendication la plus appropriées pour les besoins de votre entreprise particulière. Les modèles sont uniquement utilisés pendant le processus de création de règle de revendication de règle de revendication.  
  
Dans la gestion ADFS de composants, les règles peuvent être créés uniquement à l’aide de modèles de règle de revendication. Une fois que vous utilisez le composant logiciel enfichable pour sélectionner un modèle de règle de revendication, les données nécessaires à la logique de règle d’entrée et enregistrez-le sur la base de données de configuration, il sera \ (à partir de ce forward\ point) visée à l’interface utilisateur en tant qu’une règle de revendication.  
  
### <a name="how-claim-rule-templates-work"></a>Fonctionnement des modèles de règle de revendication  
À première vue, les modèles de règle de revendication semblent forms simplement d’entrée fournies par le composant logiciel enfichable pour collecter des données et processus logique spécifique sur les revendications entrantes. Toutefois, à un niveau plus détaillé, revendication magasin de modèles de règle l’infrastructure du langage de règle qui composent la logique de base nécessaires pour créer rapidement une règle sans avoir à connaître parfaitement le langage de revendication.  
  
Chaque modèle fourni dans l’interface utilisateur \(UI\) représente une syntaxe du langage de règle prédéfinie de revendication, basée sur les tâches administratives les plus couramment requises. Il est toutefois un modèle de règle, qui est l’exception. Ce modèle est appelé le modèle de règle personnalisé. Avec ce modèle, aucune syntaxe n’est prédéfinie. Au lieu de cela vous devez créer directement la syntaxe du langage de règle revendication dans le corps de la forme de modèle de règle de revendication à l’aide de la syntaxe du langage de règle revendication.  
  
Pour plus d’informations sur l’utilisation de la syntaxe du langage de règle revendication, voir [le rôle du langage de règle de revendication](The-Role-of-the-Claim-Rule-Language.md) dans le Guide de déploiement d’ADFS.  
  
> [!TIP]  
> Vous pouvez afficher le langage de règle de revendication associé à une règle à tout moment en cliquant sur le **afficher le langage de règle** bouton des propriétés d’une règle de revendication.  
  
### <a name="how-to-create-a-claim-rule"></a>Comment créer une règle de revendication  
Les règles de revendication sont créées séparément pour chaque relation d’approbation fédérée au sein du Service de fédération et ne sont pas partagées par plusieurs approbations. Vous pouvez créer une règle à partir d’un modèle de règle de revendication, démarrer à partir de zéro en créant la règle à l’aide du langage de règle de revendication ou utiliser Windows PowerShell pour personnaliser une règle.  
  
Toutes ces options coexistent pour vous offrir la possibilité de choisir la méthode appropriée pour un scénario donné. Pour plus d’informations sur la création d’une règle de revendication, voir [configuration des règles de revendication](https://technet.microsoft.com/library/ee913571.aspx) dans le Guide de FSDeployment AD.  
  
#### <a name="using-claim-rule-templates"></a>À l’aide de modèles de règle de revendication  
Les modèles sont uniquement utilisés pendant le processus de création de règle de revendication de règle de revendication. Vous pouvez utiliser un des modèles suivants pour créer une règle de revendication:  
  
-   Passer ou filtrer une revendication entrante  
  
-   Transformer une revendication entrante  
  
-   Envoyer les attributs LDAP en tant que revendications  
  
-   Envoyer l’appartenance au groupe sous la forme d’une revendication  
  
-   Envoyer des revendications à l’aide d’une règle personnalisée  
  
-   Autoriser ou refuser des utilisateurs en fonction d’une revendication entrante  
  
-   Autoriser tous les utilisateurs  
  
Pour plus d’informations sur chacun de ces modèles de règle de revendication, consultez [déterminer le Type de revendication modèle de règle à utiliser](Determine-the-Type-of-Claim-Rule-Template-to-Use.md).  
  
#### <a name="using-the-claim-rule-language"></a>À l’aide du langage de règle de revendication  
Règles d’entreprise qui dépassent le cadre des modèles de règle de revendication standard, vous pouvez utiliser un modèle de règle personnalisé pour exprimer une série de conditions logiques complexes à l’aide du langage de règle de revendication. Pour plus d’informations sur l’utilisation d’une règle personnalisée, consultez [quand utiliser une règle de revendication personnalisée](When-to-Use-a-Custom-Claim-Rule.md).  
  
#### <a name="using-windows-powershell"></a>À l’aide de Windows PowerShell  
Vous pouvez également utiliser l’objet d’applet de commande ADFSClaimRuleSet avec Windows PowerShell pour créer ou gérer des règles dans ADFS. Pour plus d’informations sur la façon dont vous pouvez utiliser Windows PowerShell avec cette applet de commande, voir [Administration d’ADFS avec Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=179634).  
  
## <a name="what-is-a-claim-rule-set"></a>Qu’est un ensemble de règles de revendication?  
Comme indiqué dans l’illustration suivante, un ensemble de règles de revendication est un regroupement d’une ou plusieurs règles pour une approbation fédérée donnée qui définit comment revendications seront traitées par le moteur de règles de revendications. Lorsqu’une revendication entrante est reçue par le Service de fédération le moteur de règles de revendication applique la logique définie par l’ensemble de règles de revendication approprié. C’est la somme finale de la logique de chaque règle du jeu qui détermine comment d’émission des revendications pour une approbation donnée dans son intégralité.  
  
![Rôles ADFS](media/adfs2_claimruleset.gif)  
  
Les règles de revendication sont traitées par le moteur de revendications dans l’ordre chronologique, dans un ensemble de règles donné. Cet ordre est important, car la sortie d’une règle peut être utilisée comme entrée pour la règle suivante dans le jeu.  
  
## <a name="what-are-claim-rule-set-types"></a>Règles de revendication définition des types  
Une règle de revendication définie le type est un segment logique d’une approbation fédérée qui détermine si l’ensemble de règles de revendication associé à l’approbation sera utilisé pour l’émission de revendications, d’autorisation ou l’acceptation. Chaque approbation fédérée peut comporter un ou plusieurs règles de revendication définir les types de lui est associés, selon le type d’approbation est utilisé.  
  
Le tableau suivant décrit les différents types de jeux de règles de revendication et explique sa relation avec un fournisseur de revendications ou partie de confiance.  
  
|Type de jeu de règles de revendication|Description|Utilisé sur|  
|-----------------------|---------------|-----------|  
|Jeu de règles de transformation acceptation|Un ensemble de règles de revendication que vous utilisez sur une notamment des revendications approbation de fournisseur pour spécifier les revendications entrantes qui vont être acceptées par l’organisation du fournisseur de revendications et les revendications sortantes qui seront envoyées à la partie de confiance.<br /><br />Les revendications entrantes qui seront utilisées pour la source de ce jeu de règles seront les revendications générées par la règle de transformation d’émission définie comme spécifié dans l’organisation du fournisseur de revendications.<br /><br />Par défaut, le nœud d’approbation de fournisseur de revendications contient une approbation de fournisseur de revendications nommée **ActiveDirectory** qui est utilisé pour représenter le magasin d’attributs source pour l’ensemble de règles de transformation acceptation. Cet objet d’approbation est utilisé pour représenter la connexion à partir de votre Service de fédération à une base de données ActiveDirectory sur votre réseau. Cette approbation par défaut est de traiter les revendications pour les utilisateurs qui ont été authentifiés par ActiveDirectory et il ne peut pas être supprimé.|Approbations de fournisseur de revendications|  
|Ensemble de règles de transformation d’émission|Un ensemble de règles de revendication que vous utilisez sur une approbation de partie de confiance pour spécifier les revendications qui seront émises vers la partie de confiance.<br /><br />Les revendications entrantes qui seront utilisées pour la source de ce jeu de règles seront initialement les revendications générées par les règles de transformation d’acceptation.|Approbations de partie de confiance|  
|Ensemble de règles d’autorisation d’émission|Un ensemble de règles de revendication que vous utilisez sur une approbation de partie de confiance pour spécifier les utilisateurs qui seront autorisés à recevoir un jeton pour la partie de confiance.<br /><br />Ces règles déterminent si un utilisateur peut recevoir des revendications pour une partie de confiance et, par conséquent, l’accès à la partie de confiance.<br /><br />Sauf si vous spécifiez une règle d’autorisation d’émission, tous les utilisateurs seront refusées accès par défaut.|Approbations de partie de confiance|  
|Ensemble de règles d’autorisation de délégation|Un ensemble de règles de revendication que vous utilisez sur une approbation de partie de confiance pour spécifier les utilisateurs qui seront autorisés à agir comme délégués pour d’autres utilisateurs à la partie de confiance.<br /><br />Ces règles déterminent si le demandeur est autorisé à emprunter l’identité d’un utilisateur tout en identifiant toujours le demandeur du jeton qui est envoyé à la partie de confiance.<br /><br />Sauf si vous spécifiez une règle d’autorisation d’émission, aucun utilisateur ne peut agir comme délégués par défaut.|Approbations de partie de confiance|  
|Ensemble de règles d’autorisation d’emprunt d’identité|Un ensemble de règles que vous configurez à l’aide de Windows PowerShell pour déterminer si un utilisateur peuvent totalement de revendication emprunter l’identité d’un autre utilisateur à la partie de confiance.<br /><br />Ces règles déterminent si le demandeur est autorisé à emprunter l’identité d’un utilisateur sans identifier le demandeur du jeton qui est envoyé à la partie de confiance.<br /><br />L’emprunt d’identité d’un autre utilisateur de cette façon d’est une fonctionnalité très puissante, car la partie de confiance ne sait pas que l’utilisateur est empruntée.|Approbation de partie de confiance|  
  
Pour plus d’informations sur Sélectionner les règles de revendication approprié à utiliser dans votre organisation, voir [déterminer le Type de revendication modèle de règle à utiliser](Determine-the-Type-of-Claim-Rule-Template-to-Use.md).  
  

