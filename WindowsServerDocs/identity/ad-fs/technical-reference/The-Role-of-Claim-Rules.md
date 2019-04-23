---
ms.assetid: 65e474b5-3076-4ba3-809d-a09160f7c2bd
title: Rôle des règles de revendication
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e47dbecdeee620d3a237ad8d8c41a550d3ef069c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860780"
---
>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="the-role-of-claim-rules"></a>Rôle des règles de revendication
La fonction globale du Service de fédération dans Active Directory Federation Services \(AD FS\) consiste à émettre un jeton qui contient un ensemble de revendications. La décision concernant les revendications AD FS accepte et lui envoie ensuite est régie par les règles de revendication.  
  
## <a name="what-are-claim-rules"></a>Définition des règles de revendication  
Une règle de revendication représente une instance de logique métier qui utilisera une ou plusieurs revendications entrantes et applique des conditions \(if x, alors y\) et produire un ou plusieurs revendications sortantes en fonction des paramètres de condition. Pour plus d’informations sur les revendications entrantes et sortantes, consultez [The Role of Claims](The-Role-of-Claims.md).  
  
Vous utilisez les règles de revendication lorsque vous devez implémenter la logique métier qui contrôle le flux des revendications à travers le pipeline de revendications. Bien que le pipeline de revendications est plus un concept logique de la fin\-à\-fin du processus de flux des revendications, les règles sont un élément d’administration réel que vous pouvez utiliser pour personnaliser le flux des revendications à travers le processus d’émission de revendications de la revendication.  
  
Pour plus d’informations sur le pipeline de revendications, consultez [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md).  
  
Les règles de revendication présentent les avantages suivants :  
  
-   Fournissent un mécanisme permettant aux administrateurs d’appliquer l’exécution\-logique métier de temps pour approuver des revendications à partir de fournisseurs de revendications  
  
-   Fournissent un mécanisme permettant aux administrateurs de définir les revendications à publier sur les parties de confiance  
  
-   Fournir des revendications riches et détaillées\-en fonction des fonctionnalités d’autorisation pour les administrateurs qui souhaitent autoriser ou refuser l’accès à des utilisateurs spécifiques  
  
### <a name="how-claim-rules-are-processed"></a>Mode de traitement des règles de revendication  
Les règles de revendication sont traitées via le pipeline de revendications à l’aide du *moteur de revendications*. Le moteur de revendications est un composant logique du service de fédération qui examine le jeu des revendications entrantes présenté par un utilisateur, puis, en fonction de la logique de chaque règle, produit un jeu de revendications de sortie.  
  
Ensemble, le moteur des règles de revendication et le jeu de règles de revendication associé à une approbation fédérée donnée déterminent si les revendications entrantes doivent être transmises telles qu’elles sont, filtrées pour répondre à un critère de condition spécifique ou transformées en un tout nouveau jeu de revendications avant qu’elles ne soient émises en tant que revendications sortantes par votre service de fédération.  
  
Pour plus d’informations sur ce processus, consultez [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md).  
  
## <a name="what-are-claim-rule-templates"></a>Définition des modèles de règles de revendication  
AD FS inclut un ensemble prédéfini de règle de revendication de modèles qui sont conçus pour vous aider à facilement sélectionnent et créent les règles de revendication plus appropriées pour vos besoins professionnels particuliers. Les modèles de règles de revendication sont uniquement utilisés pendant le processus de création des règles de revendication.  
  
Dans le composant logiciel enfichable Gestion AD FS\-, règles peuvent uniquement être créées à l’aide de modèles de règle de revendication. Une fois que vous utilisez le composant logiciel enfichable\-dans pour sélectionner un modèle de règle de revendication, entrez les données nécessaires à la logique de règle et enregistrez-le sur la base de données de configuration, il sera \(à partir de là\) fait référence dans l’interface utilisateur en tant qu’une règle de revendication.  
  
### <a name="how-claim-rule-templates-work"></a>Fonctionnement des modèles de règles de revendication  
À première vue, les modèles de règle semblent être des formulaires de saisie simplement fournis par le composant logiciel enfichable de revendication\-pour collecter des données et processus logique spécifique sur les revendications entrantes. Toutefois, une analyse plus détaillée des modèles de règle de revendication permet de constater qu’ils enregistrent l’infrastructure du langage de règle de revendication utile qui constitue la logique de base nécessaire pour créer rapidement une règle sans avoir à connaître parfaitement le langage.  
  
Chaque modèle est fourni dans l’interface utilisateur \(l’interface utilisateur\) représente une syntaxe de langage de la règle de revendication préremplies, en fonction des tâches d’administration plus couramment requises. Un modèle de règle fait cependant exception. Il s’agit du modèle de règle personnalisé. Avec ce modèle, aucune syntaxe n’est prédéfinie. Vous devez au contraire la créer directement dans le corps du formulaire du modèle de règle de revendication, à l’aide de la syntaxe du langage de règle de revendication.  
  
Pour plus d’informations sur l’utilisation de la syntaxe de langage de règle de revendication, consultez [The Role of du langage des règles de revendication](The-Role-of-the-Claim-Rule-Language.md) dans le Guide de déploiement d’AD FS.  
  
> [!TIP]  
> Vous pouvez à tout moment afficher le langage de règle de revendication associé à une règle en cliquant sur le bouton **Afficher le langage de la règle** dans les propriétés d’une règle de revendication.  
  
### <a name="how-to-create-a-claim-rule"></a>Création d’une règle de revendication  
Les règles de revendication sont créées séparément pour chaque relation d’approbation fédérée au sein du service de fédération et ne sont pas partagées par plusieurs approbations. Vous pouvez créer une règle à partir d’un modèle de règle de revendication, démarrer à partir de rien en créant la règle à l’aide du langage de règle de revendication ou utiliser Windows PowerShell pour personnaliser une règle.  
  
Toutes ces options coexistent pour vous offrir la possibilité de choisir la méthode adaptée à un scénario donné. Pour plus d’informations sur la création d’une règle de revendication, consultez [configuration des règles de revendication](https://technet.microsoft.com/library/ee913571.aspx) dans le Guide de FSDeployment AD.  
  
#### <a name="using-claim-rule-templates"></a>Utilisation de modèles de règles de revendication  
Les modèles de règles de revendication sont uniquement utilisés pendant le processus de création des règles de revendication. Vous pouvez utiliser l’un des modèles suivants pour créer une règle de revendication :  
  
-   Passer ou filtrer une revendication entrante  
  
-   Transformer une revendication entrante  
  
-   Envoyer les attributs LDAP en tant que revendications  
  
-   Envoyer l’appartenance à un groupe en tant que revendication  
  
-   Envoyer des revendications à l’aide d’une règle personnalisée  
  
-   Autoriser ou refuser des utilisateurs en fonction d'une revendication entrante  
  
-   Autoriser tous les utilisateurs  
  
Pour plus d’informations sur chacun de ces modèles de règle de revendication, consultez [déterminer le Type de revendication modèle de règle à utiliser](Determine-the-Type-of-Claim-Rule-Template-to-Use.md).  
  
#### <a name="using-the-claim-rule-language"></a>Utilisation du langage des règles de revendication  
Pour les règles d’entreprise qui dépassent le cadre des modèles de règles de revendication standard, vous pouvez utiliser un modèle de règle personnalisé pour exprimer une série de conditions logiques complexes à l’aide du langage de règle de revendication. Pour plus d’informations sur l’utilisation d’une règle personnalisée, consultez [quand utiliser une règle de revendication personnalisée](When-to-Use-a-Custom-Claim-Rule.md).  
  
#### <a name="using-windowspowershell"></a>Utilisation de Windows PowerShell  
Vous pouvez également utiliser l’objet d’applet de commande ADFSClaimRuleSet avec Windows PowerShell pour créer ou gérer des règles dans AD FS. Pour plus d’informations sur la façon dont vous pouvez utiliser Windows PowerShell avec cette applet de commande, consultez la page [Administration d’AD FS avec Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=179634).  
  
## <a name="what-is-a-claim-rule-set"></a>Définition d’un jeu de règles de revendication  
Comme indiqué dans l’illustration suivante, un jeu de règles de revendication est un regroupement d’une ou plusieurs règles pour une approbation fédérée donnée qui définit le traitement des revendications par le moteur de règles de revendication. Lorsqu’une revendication entrante est reçue par le service de fédération, le moteur de règles de revendication applique la logique définie par le jeu de règles de revendication approprié. La somme finale de la logique de chaque règle du jeu détermine la méthode d’émission des revendications pour une approbation donnée, dans son intégralité.  
  
![Rôles d’AD FS](media/adfs2_claimruleset.gif)  
  
Les règles de revendication sont traitées par le moteur de revendications dans l’ordre chronologique, dans un jeu de règles donné. Cet ordre est important, car la sortie d’une règle peut être utilisée comme entrée pour la règle suivante du jeu.  
  
## <a name="what-are-claim-rule-set-types"></a>Définition des types de jeux de règles de revendication  
Un type de jeu de règles de revendication est un segment logique d’une approbation fédérée qui détermine si le jeu de règles de revendication associé à l’approbation sera utilisé pour l’émission, l’autorisation ou l’acceptation des revendications. Chaque approbation fédérée peut être associée à un ou plusieurs types de jeux de règles de revendication, selon le type d’approbation utilisé.  
  
Le tableau suivant décrit les différents types de jeux de règles de revendication et explique la relation de ce type de jeu avec une approbation du fournisseur de revendications ou de la partie de confiance.  
  
|Type de jeu de règles de revendication|Description|Utilisé sur|  
|-----------------------|---------------|-----------|  
|Jeu de règles de transformation d’acceptation|Jeu de règles de revendication que vous utilisez sur une approbation particulière du fournisseur de revendication pour spécifier les revendications entrantes qui vont être acceptées par l’organisation du fournisseur de revendications et les revendications sortantes qui seront envoyées à l’approbation de la partie de confiance.<br /><br />Les revendications entrantes qui seront utilisées pour se procurer ce jeu de règles seront les revendications générées par le jeu de règles de transformation d’émission, tel que défini dans l’organisation du fournisseur de revendications.<br /><br />Par défaut, le nœud de l’approbation du fournisseur de revendications contient une approbation du fournisseur de revendications nommée **Active Directory**, qui est utilisée pour représenter le magasin d’attributs source pour le jeu de règles de transformation d’acceptation. Cet objet d’approbation est utilisé pour représenter la connexion de votre service de fédération à une base de données Active Directory sur votre réseau. Cette approbation par défaut permet de traiter les revendications pour les utilisateurs qui ont été authentifiés par Active Directory ; elle ne peut pas être supprimée.|Approbations du fournisseur de revendications|  
|Jeu de règles de transformation d’émission|Jeu de règles de revendication que vous utilisez sur une approbation de la partie de confiance pour spécifier les revendications qui seront émises vers la partie de confiance.<br /><br />Les revendications entrantes qui seront utilisées pour se procurer ce jeu de règles seront initialement les revendications générées par les règles de transformation d’émission.|Approbations de la partie de confiance|  
|Jeu de règles d’autorisation d’émission|Jeu de règles de revendication que vous utilisez sur une approbation de la partie de confiance pour spécifier les utilisateurs qui seront autorisés à recevoir un jeton pour la partie de confiance.<br /><br />Ces règles déterminent si un utilisateur peut recevoir des revendications pour une partie de confiance et, par conséquent, accéder à la partie de confiance.<br /><br />À moins que vous ne spécifiiez une règle d’autorisation d’émission, l’accès est refusé par défaut à tous les utilisateurs.|Approbations de la partie de confiance|  
|Jeu de règles d’autorisation de délégation|Jeu de règles de revendication que vous utilisez sur une approbation de la partie de confiance pour spécifier les utilisateurs qui seront autorisés à agir comme délégués au nom d’autres utilisateurs pour la partie de confiance.<br /><br />Ces règles déterminent si le demandeur est autorisé à emprunter l’identité d’un utilisateur tout en identifiant toujours le demandeur du jeton qui est envoyé à la partie de confiance.<br /><br />À moins que vous ne spécifiiez une règle d’autorisation d’émission, par défaut, aucun utilisateur ne peut agir comme délégué.|Approbations de la partie de confiance|  
|Jeu de règles d’autorisation de substitution d’identité|Jeu de règles de revendication que vous configurez à l’aide de Windows PowerShell pour déterminer si un utilisateur peut totalement se substituer à un autre pour la partie de confiance.<br /><br />Ces règles déterminent si le demandeur est autorisé à emprunter l’identité d’un utilisateur sans identifier le demandeur du jeton qui est envoyé à la partie de confiance.<br /><br />La substitution de l’identité d’un autre utilisateur est, dans ce sens, une fonction très puissante, car la partie de confiance ne saura pas que l’identité de l’utilisateur a été substituée.|Approbation de partie de confiance|  
  
Pour plus d’informations sur, activez les règles de revendication approprié à utiliser dans votre organisation, consultez [déterminer le Type de revendication modèle de règle à utiliser](Determine-the-Type-of-Claim-Rule-Template-to-Use.md).  
  

