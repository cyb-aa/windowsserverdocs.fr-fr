---
ms.assetid: 8b15d44e-e4e6-4510-aa91-cc7ec7161b0a
title: Rôle du moteur de revendications
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 4d227be18cd69d79f24edca96bd873afda3f2d2a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385450"
---
# <a name="the-role-of-the-claims-engine"></a>Rôle du moteur de revendications
À son niveau le plus élevé, le moteur de \(revendications\) dans services ADFS AD FS\-est un moteur basé sur des règles qui est dédié au traitement et au traitement des demandes de revendication pour le service FS (Federation Service). Le moteur de revendications est la seule entité du service de fédération qui est chargée de l’exécution de chacun des ensembles de règles sur toutes les relations d’approbation fédérée que vous avez configurées, ainsi que de la transmission du résultat de sortie au pipeline de revendications.  
  
Alors que le pipeline de revendications est davantage un concept logique\-du\-processus de bout en bout pour les revendications de workflow, les règles de revendication sont un élément d’administration réel que vous pouvez utiliser pour personnaliser le workflow des revendications pendant le processus d’exécution des règles de revendication. Pour plus d’informations sur le processus du pipeline, consultez la page [The Role of the Claims Pipeline](The-Role-of-the-Claims-Pipeline.md).  
  
Comme indiqué dans l’illustration suivante, le fait d’accepter des règles \(\)d’acceptation de revendications entrantes, \(d’autoriser des\) demandes de demande de \( revendications et d’émettre des revendications sortantes les règles\) d’émission par le biais de règles de revendication dans toutes les relations d’approbation fédérée de votre organisation sont effectuées par le moteur de revendications.  
  
![Rôles de AD FS](media/adfs2_enginepipeline.gif)  
  
## <a name="claim-rules-execution-process"></a>Processus d’exécution des règles de revendication  
Quand vous configurez une approbation de fournisseur de revendications ou de partie de confiance dans votre organisation avec des règles de\(revendication\) , le jeu de règles de revendication pour cette approbation agit comme un opérateur de niveau de service pour les revendications entrantes en appelant le moteur de revendications pour appliquer les logique dans les règles de revendication pour déterminer s’il faut émettre des revendications et les revendications à émettre.  
  
La section suivante décrit chacune des étapes qui se produisent au niveau du moteur pendant le flux des revendications lors du processus d’exécution des règles de revendication. Chacune des étapes décrites ci-dessous se produit pour chaque phase décrite dans le processus du pipeline de revendications. Ces étapes sont les suivantes :  
  
-   Étape 1 : initialisation  
  
-   Étape 2 : exécution  
  
-   Étape 3 : résultat de l’exécution  
  
Pour plus d’informations sur le processus du pipeline, consultez la page [The Role of the Claims Pipeline](The-Role-of-the-Claims-Pipeline.md).  
  
### <a name="step-1--initialization"></a>Étape 1 : initialisation  
Dans la première étape du processus d’exécution des règles de revendication, le moteur de revendications accepte les revendications entrantes en les ajoutant d’abord au *jeu des revendications d’entrée*. Un jeu de revendications d’entrée est similaire à un cache en mémoire, qui est utilisé pour stocker temporairement des données tant qu’un processus requis a besoin que ces données soient mises à disposition pour la récupération. Les données du jeu de revendications d’entrée sont supprimées une fois que l’exécution de la règle est terminée.  
  
#### <a name="adding-a-claim-to-the-input-claim-set-for-a-rule-set"></a>Ajout d’une revendication au jeu de revendications d’entrée pour un ensemble de règles  
Le jeu de revendications d’entrée est créé par le moteur de revendications lorsqu’il a besoin de stocker temporairement des données de revendication dans la mémoire pendant qu’il traite la logique associée à un ensemble de règles de revendication. Le moteur de revendications copie toutes les revendications entrantes dans le jeu de revendications d’entrée où elles peuvent être récupérées par la première règle de l’ensemble de règles.  
  
Par exemple, dans l’illustration ci-dessous, le moteur de revendications lit les revendications A et B dans les revendications entrantes et les copie dans le jeu de revendications d’entrée. Une fois que les réclamations A et B se trouvent dans le jeu de revendications d’entrée, le moteur de revendications les récupère et les traite comme entrée pour la logique dans la première règle du jeu de règles de revendication.  
  
![Rôles de AD FS](media/adfs2_context1.gif)  
  
Toutes les règles d’un ensemble de règles de revendication partagent le même jeu de revendications d’entrée. Chaque règle de ce jeu peut être ajoutée au jeu de revendications d’entrée partagé, affectant ainsi toutes les règles suivantes du jeu.  
  
### <a name="step-2--execution"></a>Étape 2 : exécution  
Dans cette étape du processus de règles de revendication, les règles de revendication sont traitées lorsque le moteur de revendications parcourt une par une et dans l’ordre chronologique toutes les règles d’un ensemble de règles spécifique. Chaque règle d’un ensemble de règles s’exécute une seule fois et est exécutée dans l’ordre dans lequel elles apparaissent de haut en bas, comme indiqué dans la boîte de dialogue Modifier les\-règles de revendication dans le composant logiciel enfichable Gestion des AD FS. La règle de revendication située au début de l’ensemble de règles est traitée en premier, puis les règles suivantes, jusqu’à ce que toutes les règles soient exécutées.  
  
Comme défini dans le langage de règle de revendication, une règle de revendication se compose de deux parties : une condition et une instruction d’émission. Le moteur de revendications traite tout d’abord la partie condition à l’aide des données du jeu de revendications d’entrée pour déterminer si la condition spécifiée dans la règle est vraie pour les revendications contenues dans le jeu de revendications d’entrée définir \(les revendications qui correspondent aux la condition est désignée sous le terme de\)revendications correspondantes. Si des revendications correspondantes sont trouvées, le moteur de revendications exécute l’instruction d’émission de la règle pour chaque jeu des revendications correspondantes. L’instruction d’émission de la règle peut effectuer l’une des tâches suivantes avec les revendications correspondantes :  
  
1.  Copier une revendication correspondante dans le jeu de revendications de sortie.  
  
2.  Transformer les champs de revendication et créer une nouvelle revendication dans le jeu de revendications d’entrée uniquement ou dans les jeux de revendications d’évaluation et de sortie.  
  
3.  Utilisez\(la revendication\) correspondante comme clé pour rechercher plus d’informations à partir d’un magasin d’attributs\) afin de\(créer de nouvelles revendications dans le jeu de revendications d’entrée uniquement ou dans les jeux de revendications d’entrée et de sortie.  
  
#### <a name="adding-a-claim-to-the-output-claim-set-for-a-rule-set"></a>Ajout d’une revendication dans le jeu de revendications de sortie pour un ensemble de règles  
Le *jeu de revendications de sortie* est un emplacement de la mémoire qui est vide à l’origine. Il est cependant important, car le moteur de revendications renvoie uniquement les revendications présentes dans ce jeu de revendications de sortie lorsque le processus d’exécution est terminé. Cela signifie que toutes les revendications uniquement présentes dans le jeu de revendications d’entrée, et pas dans celui de sortie, sont ignorées lors du calcul du jeu final des revendications de sortie.  
  
#### <a name="adding-a-claim-to-both-claim-sets-for-a-rule-set"></a>Ajout d’une revendication dans les deux jeux de revendications pour un ensemble de règles  
Lorsqu’une règle est traitée, les revendications sont ajoutées dans le jeu de revendications d’entrée ou dans le jeu de revendications d’entrée et le jeu de revendications de sortie en fonction de l’instruction utilisée dans l’instruction d’émission de la règle. Le langage de règle de revendication fait référence à ces deux instructions sous les noms *add* et *issue*.  
  
Si l’instruction *add* est utilisée, les revendications sont uniquement ajoutées au jeu de revendications d’entrée. Les revendications existent alors uniquement dans le cadre de l’exécution et cessent d’exister une fois l’exécution terminée. Si l’instruction *issue* est utilisée, les revendications sont ajoutées au jeu de revendications d’entrée, ainsi qu’au jeu de revendications de sortie. Les revendications sont retournées au jeu de revendications de sortie une fois l’exécution terminée. Pour plus d’informations sur ces instructions, consultez la page [The Role of the Claim Rule Language](The-Role-of-the-Claim-Rule-Language.md).  
  
Si la partie conditions d’une règle au sein d’un ensemble de règles ne correspond à aucune revendication du jeu de revendications d’entrée, la partie instruction d’émission de la règle est ignorée. Par conséquent, aucune revendication n’est ajoutée au jeu de revendications de sortie ou au jeu de revendications d’entrée. L’illustration suivante et les étapes correspondantes représentent le déroulement de la procédure lorsque le moteur de revendications exécute une règle de transformation :  
  
![Rôles de AD FS](media/adfs2_context2.gif)  
  
1.  Des revendications entrantes sont ajoutées au jeu de revendications d’entrée par le moteur de revendications.  
  
2.  Lorsque la première règle s’exécute, elle détecte les revendications A et B qui, à ce moment précis, sont les seules revendications présentes dans le jeu de revendications d’entrée. La règle traite alors la partie conditions de la logique de la règle dans la règle 1.  
  
3.  Étant donné que la revendication a est présente dans le jeu de revendications d’entrée, la condition de la règle est \(déterminée comme ayant la\) valeur true correspondant à la revendication A et une nouvelle revendication C est ajoutée au jeu de revendications d’entrée et au jeu de revendications de sortie.  
  
4.  La règle 2 peut désormais utiliser les revendications A, B et \(C dans le jeu\) de revendications d’entrée comme entrée pour traiter sa logique.  
  
Pour plus d’informations sur la transformation des revendications, consultez la page [When to Use a Transform Claim Rule](When-to-Use-a-Transform-Claim-Rule.md).  
  
### <a name="step-3--execution-result"></a>Étape 3 : résultat de l’exécution  
La dernière étape de l’exécution de l’ensemble de règles de revendication commence une fois que toutes les règles ont été exécutées au sein d’un ensemble de règles spécifique et que le jeu final de revendications est présent dans le jeu de revendications de sortie. Le moteur de revendications renvoie alors le contenu du jeu de revendications de sortie comme résultat de l’exécution de l’ensemble de règles. À ce stade, le pipeline de revendications prend le relais et déplace ce résultat final vers l’étape suivante de son processus.  
  
## <a name="sending-the-execution-output-to-the-claims-pipeline"></a>Envoi de la sortie de l’exécution au pipeline de revendications  
Lorsque le moteur de revendications traite un ensemble de règles, cet ensemble possède ses propres emplacements dans la mémoire, dédiés à ses jeux de réclamations d’entrée et de sortie. Cela signifie que les jeux de réclamation d’entrée et de sortie utilisés par un ensemble de règles sont distincts de ceux utilisés par un autre ensemble de règles.  
  
Une fois que l’ensemble du processus a été exécuté pour \(un ensemble de règles, étapes 1\), 2 et 3, le \(contenu des revendications sortantes récemment\) émis du jeu de revendications de sortie est utilisé comme entrée pour l’ensemble de règles suivant dans le pipeline de revendications. Cela permet aux revendications de circuler de la sortie d’un ensemble de règles à l’entrée d’un autre ensemble de règles, comme indiqué dans l’illustration suivante.  
  
![Rôles de AD FS](media/adfs2_enginecontexts.gif)  
  
> [!NOTE]  
> Bien que l’ensemble de règles d’émission soit également une étape essentielle dans le pipeline, l’illustration ci-dessus n’en tient pas compte afin de simplifier le schéma. Pour consulter une illustration représentant l’ensemble de règles d’émission et la manière dont il s’intègre dans le pipeline de revendications, consultez la page [The Role of the Claims Pipeline](The-Role-of-the-Claims-Pipeline.md).  
  
Dans ce cas, la sortie des règles d’acceptation est utilisée par le pipeline pour faire circuler le jeu final de revendications produit par les règles d’acceptation vers la deuxième étape du pipeline, c’est-à-dire le traitement des règles d’autorisation. À ce stade, le processus \(d’exécution des règles de revendication entières 1, 2 et 3 ci-dessus\) s’exécutera à nouveau pour l’ensemble de règles d’autorisation. Ce cycle se poursuit jusqu’à ce que \(la règle d’émission définie à\) l’étape finale dans le pipeline soit terminée.  
  
Une fois que les revendications sortantes finalisées ont été retournées par le moteur pour l’ensemble de règles d’émission, elles sont rassemblées en un paquet dans un jeton SAML qui est envoyé au client par le service de fédération.  
  
## <a name="processing-authorization-rules"></a>Traitement des règles d’autorisation  
Si le jeu de règles de revendication en cours d’exécution au cours de l’étape 2 du processus d’exécution des \(règles de revendication se compose de règles d’autorisation qui ont un jeu de revendications\)d’entrée et de sortie différent des règles d’acceptation ou d’émission, alors celles-ci les règles d’autorisation s’exécutent pour déterminer si un demandeur de jeton est autorisé à obtenir un jeton de sécurité pour une partie de confiance donnée à partir de la service FS (Federation Service) en fonction des revendications du demandeur.  
  
L’objectif des règles d’autorisation est d’émettre une revendication d’autorisation ou de refus selon que l’utilisateur est autorisé ou non à obtenir un jeton pour la partie de confiance donnée. Comme indiqué dans l’illustration suivante, la sortie de l’exécution de l’autorisation est utilisée par le pipeline pour déterminer si l’ensemble de règles d’émission est exécuté ou non, en fonction de la\/présence ou de l’absence de la revendication d’autorisation ou de refus, mais de l’autorisation la sortie d’exécution elle-même n’est pas utilisée en tant qu’entrée dans le jeu de règles de revendication.  
  
![Rôles de AD FS](media/adfs2_authorization.gif)  
  
Pour plus d’informations à propos de l’autorisation des revendications, consultez la page [When to Use an Authorization Claim Rule](When-to-Use-an-Authorization-Claim-Rule.md).  
  

