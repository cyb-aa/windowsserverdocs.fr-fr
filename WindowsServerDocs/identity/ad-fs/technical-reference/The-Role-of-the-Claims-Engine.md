---
ms.assetid: 8b15d44e-e4e6-4510-aa91-cc7ec7161b0a
title: "Le rôle du moteur de revendications"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 930b6f8034f17d8902104419042f944b82e90b4f
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

# <a name="the-role-of-the-claims-engine"></a>Le rôle du moteur de revendications
À son niveau le plus élevé, le moteur de revendications dans ActiveDirectory Federation Services \(ADFS\) est un moteur de rule\ dédié au et au traitement des demandes de revendication pour le Service de fédération. Le moteur de revendications est la seule entité du Service de fédération qui est responsable de l’exécution de chacun des ensembles de règles sur toutes les relations d’approbation fédérée que vous avez configuré et la remise du résultat de sortie pour le pipeline de revendications.  
  
Alors que le pipeline de revendications est davantage un concept logique de l’end\ celle-ci-du processus de circulation des revendications, les règles de revendication sont un élément d’administration réel que vous pouvez utiliser pour personnaliser le flux des revendications pendant le processus de l’exécution des règles de revendication. Pour plus d’informations sur le processus du pipeline, consultez [The Role of the Claims Pipeline](The-Role-of-the-Claims-Pipeline.md).  
  
Comme indiqué dans l’illustration suivante, les actes d’acceptation des \(acceptance rules\) revendications entrantes, autorisation de revendications demandeurs \(authorization rules\) et émettre des revendications sortantes \(issuance rules\) par le biais de règles de revendication sur toutes les relations d’approbation fédérée de votre organisation est effectuée par le moteur de revendications.  
  
![Rôles ADFS](media/adfs2_enginepipeline.gif)  
  
## <a name="claim-rules-execution-process"></a>Processus d’exécution des règles de revendication  
Lorsque vous configurez une approbation de fournisseur de revendications ou une partie de confiance confiance dans votre organisation avec les règles de revendication, le set\(s\) de règle de revendication pour qu’act approbation comme un opérateur de contrôle pour les revendications entrantes en appelant le moteur de revendications pour appliquer la logique nécessaire dans les règles de revendication pour déterminer s’il faut émettre les revendications et les revendications à émettre.  
  
La section suivante décrit chacune des étapes qui se produisent par le moteur pendant le flux des revendications à travers le processus de l’exécution des règles de revendication. Chacune des étapes décrites ci-dessous se produit pour chaque phase décrite dans le processus de pipeline de revendications. Ces étapes sont les suivantes:  
  
-   Étape1: initialisation  
  
-   Étape2: exécution  
  
-   Étape3: résultat de l’exécution  
  
Pour plus d’informations sur le processus du pipeline, consultez [The Role of the Claims Pipeline](The-Role-of-the-Claims-Pipeline.md).  
  
### <a name="step-1--initialization"></a>Étape1: initialisation  
Dans la première étape du processus de l’exécution de règles de revendication, le moteur de revendications accepte les revendications entrantes en les ajoutant d’abord le *ensemble de revendications d’entrée*. Un jeu de revendications d’entrée est similaire à un cache en mémoire qui est utilisé pour stocker temporairement des données tant qu’un processus requis a besoin de ces données soient mises à disposition pour la récupération. Le jeu de revendications d’entrée de données sont supprimées une fois terminée l’exécution de la règle.  
  
#### <a name="adding-a-claim-to-the-input-claim-set-for-a-rule-set"></a>Ajout d’une revendication au jeu pour un ensemble de règles de revendications d’entrée  
Le jeu de revendications d’entrée est créé par le moteur de revendications lorsqu’il a besoin de stocker temporairement des données de revendication dans la mémoire pendant qu’il traite la logique associée à un ensemble de règles de revendication. Le moteur de revendications copie toutes les revendications entrantes pour le jeu de revendications d’entrée où elles peuvent être récupérées par la première règle dans l’ensemble de règles.  
  
Par exemple, dans l’illustration ci-dessous, le moteur de revendications lit les revendications A et B à partir de l’entrée de revendications et les copie dans le jeu de revendications d’entrée. Une fois qu’ils se trouvent dans le jeu de revendications d’entrée, le moteur de revendications récupère et traite les revendications A et B en tant qu’entrée pour la logique dans la première règle dans l’ensemble de règles de revendication.  
  
![Rôles ADFS](media/adfs2_context1.gif)  
  
Toutes les règles dans un ensemble de règles de revendication partagent le même jeu de revendications d’entrée. Chaque règle de ce jeu pouvez ajouter à l’ensemble de revendications d’entrée partagé, affectant ainsi toutes les règles suivantes dans le jeu.  
  
### <a name="step-2--execution"></a>Étape2: exécution  
Dans cette étape du processus de règles de revendication, les règles de revendication sont traitées lorsque le moteur de revendications ordre chronologique toutes les règles d’un ensemble de règles spécifique, une à la fois. Chaque règle au sein d’un ensemble de règles s’exécute une fois uniquement et est exécutée dans l’ordre dans lequel elles apparaissent de haut en bas comme indiqué dans la boîte de dialogue Modifier les règles de revendication dans la gestion ADFS de composants. La règle de revendication qui est définie en haut de la règle est traitée en premier, puis les règles suivantes sont traitées jusqu'à ce que toutes les règles ont été exécutées.  
  
Comme défini dans le langage de règle de revendication, une règle de revendication se compose de deux parties: une instruction d’émission et la condition. Jeu de revendications les processus de première moteur revendications ensemble pour déterminer si la condition spécifiée dans la règle s’applique aux revendications contenues dans l’entrée de revendications de la partie conditions à l’aide de données dans l’entrée \ (les revendications qui correspondent à la condition de la règle sont appelées une correspondance claims\). Si des revendications correspondantes sont trouvées, le moteur de revendications exécute l’instruction d’émission de la règle pour chaque ensemble de revendications correspondantes. L’instruction d’émission de la règle peut effectuer une des tâches suivantes avec les revendications correspondantes:  
  
1.  Copier une revendication correspondante dans le jeu de revendications de sortie  
  
2.  Transformer les champs de revendication et créer une nouvelle revendication dans le jeu de revendications d’entrée uniquement ou dans l’évaluation et de sortie jeux de revendications.  
  
3.  Utiliser le claim\(s\) correspondant en tant que clé pour rechercher d’autres informations à partir d’un magasin d’attributs pour créer la nouvelle claim\(s\) dans le jeu de revendications d’entrée uniquement ou de jeux de revendications d’entrée et la sortie de.  
  
#### <a name="adding-a-claim-to-the-output-claim-set-for-a-rule-set"></a>Ajout d’une revendication à définir pour un ensemble de règles de revendications de sortie  
Le *jeu de revendications de sortie* est un emplacement de mémoire qui est initialement vide et est importante, car le moteur de revendications renvoie uniquement les revendications qui se trouvent dans le jeu une fois terminé le processus d’exécution de revendications de sortie. Cela signifie que toutes les revendications uniquement présentes dans la revendication d’entrée défini et pas dans le jeu de revendications de sortie seront ignorées lors du calcul le jeu final de revendications sortantes.  
  
#### <a name="adding-a-claim-to-both-claim-sets-for-a-rule-set"></a>Ajout d’une revendication aux deux ensembles de revendication pour un ensemble de règles  
Lorsqu’une règle est traitée, les revendications sont soit ajouté à l’entrée de jeu de revendications ou ensemble de revendications d’entrée et sortie, selon l’instruction qui est utilisée dans l’instruction d’émission de la règle. Le langage de règle de revendication fait référence à ces deux instructions sous la forme *ajouter* ou *problème*.  
  
Si le *ajouter* instruction est utilisée, les revendications sont ajoutées à l’ensemble de revendications d’entrée et les revendications existent alors uniquement dans le cadre de l’exécution et cessent d’exister une fois l’exécution terminée. Si le *problème* instruction est utilisée, les revendications sont ajoutées à l’ensemble de revendications d’entrée et le jeu de revendications de sortie et les revendications seront renvoyées dans les revendications de sortie une fois l’exécution terminée. Pour plus d’informations sur ces instructions, voir [The Role of the Claim Rule Language](The-Role-of-the-Claim-Rule-Language.md).  
  
Si la partie conditions d’une règle au sein d’un ensemble de règles ne correspond pas à toutes les revendications dans le jeu de revendications d’entrée, la partie instruction d’émission de la règle est ignorée et donc aucune revendications ne sont ajoutées à l’ensemble de revendications de sortie ou à l’ensemble de revendications d’entrée. L’illustration suivante et les étapes correspondantes montrent que se passe-t-il lorsque le moteur de revendications exécute une règle de transformation:  
  
![Rôles ADFS](media/adfs2_context2.gif)  
  
1.  Les revendications entrantes sont ajoutées à la revendication d’entrée définie par le moteur de revendications.  
  
2.  Lorsque la première règle s’exécute, il voit les revendications A et B, qui sont à ce moment que les seules revendications présentes dans l’entrée de jeu de revendications, et traite la partie conditions de la logique de règle dans la règle 1.  
  
3.  Dans la mesure où la revendication A est présente dans le jeu de revendications d’entrée, la condition de la règle est déterminée soit vraie \ (correspondant à la revendication de nom) et une nouvelle revendication C est ajoutée à l’entrée de jeu de revendications et de sortie.  
  
4.  Règle 2 peut désormais utiliser les revendications A, B et C \ (toutes les revendications d’entrée revendication Set) en tant qu’entrée pour traiter sa logique.  
  
Pour plus d’informations sur la transformation des revendications, voir [When to Use a Transform Claim Rule](When-to-Use-a-Transform-Claim-Rule.md).  
  
### <a name="step-3--execution-result"></a>Étape3: résultat de l’exécution  
La dernière étape de l’exécution de l’ensemble de règle revendication commence une fois que toutes les règles ont été exécutés au sein d’un ensemble de règles donné et le jeu final de revendications est présent dans le jeu de revendications de sortie. À ce stade, le moteur de revendications renvoie le contexte de la définir en tant que la sortie de l’exécution du jeu de règles de revendications de sortie. À ce stade, il est le pipeline de revendications qui prend le relais et déplace ce résultat final à l’étape suivante dans son processus.  
  
## <a name="sending-the-execution-output-to-the-claims-pipeline"></a>Envoyer la sortie de l’exécution pour le pipeline de revendications  
Lorsque le moteur de revendications processus un ensemble de règles, ensemble possède ses propres emplacements dédiés dans la mémoire pour l’entrée et les jeux de revendications de sortie. Cela signifie que l’entrée et la sortie de revendication jeux utilisés par une règle ensemble est distinctes à partir de l’entrée et de jeux utilisé dans un autre ensemble de règles de revendications de sortie.  
  
Après l’exécution de l’ensemble du processus pour un ensemble de règles donné \ (étapes1, 2 et 3\), les revendications sortantes nouvellement émises \ (contenu de la Set de revendications de sortie) sera utilisé comme entrée pour la règle suivante définie dans le pipeline de revendications. Cela permet aux revendications de circuler de la sortie d’un ensemble de règles à l’entrée pour un autre ensemble de règles, comme illustré dans l’illustration suivante.  
  
![Rôles ADFS](media/adfs2_enginecontexts.gif)  
  
> [!NOTE]  
> Bien que l’ensemble de règles d’émission soit également une étape essentielle dans le pipeline, l’illustration ci-dessus ne sera pas affichée uniquement à des fins de simplification de l’illustration. Pour consulter une illustration représentant l’ensemble de règles d’émission et la façon dont il s’intègre dans le pipeline de revendications, voir [The Role of the Claims Pipeline](The-Role-of-the-Claims-Pipeline.md).  
  
Dans ce cas, la sortie des règles d’acceptation est utilisée par le pipeline pour faire circuler le jeu final de revendications produit par les règles d’acceptation vers la deuxième étape dans le pipeline, c'est-à-dire le traitement des règles d’autorisation. À ce stade, l’intégralité de la revendication des processus d’exécution des règles \ (étapes1, 2 et 3 above\) s’exécute de nouveau pour l’ensemble de règles d’autorisation. Ce cycle se poursuit jusqu'à ce que la règle d’émission définie \ (la dernière étape de la pipeline\) a été effectuée.  
  
Une fois que les revendications sortantes finalisées ont été retournées par le moteur pour l’ensemble de règles d’émission, elles seront empaquetées dans un jeton SAML et le Service de fédération envoie le jeton au client.  
  
## <a name="processing-authorization-rules"></a>Traitement des règles d’autorisation  
Si l’ensemble de règles de revendication qui est en cours d’exécution pendant l’étape2 du processus de l’exécution de règles de revendication se compose de règles d’autorisation \ (qui ont une autre entrée et les jeux que rules\ acceptation ou d’émission de revendications de sortie), puis les règles d’autorisation seront exécute pour déterminer si un demandeur du jeton est autorisé à obtenir un jeton de sécurité pour une partie de confiance donné à partir du Service de fédération basé sur les revendications du demandeur.  
  
L’objectif des règles d’autorisation consiste à émettre une autorisation ou refuser des revendications selon si l’utilisateur est autorisé à obtenir un jeton pour la partie de confiance donné ou non. Comme indiqué dans l’illustration suivante, la sortie de l’exécution de l’autorisation est utilisée par le pipeline pour déterminer si l’ensemble de règles d’émission est exécuté ou non: basée sur la présence ou l’absence de l’autorisation and\ / ou refuser la demande, mais la sortie de l’exécution d’autorisation elle-même n’est pas utilisée comme entrée pour l’ensemble de règles de revendication.  
  
![Rôles ADFS](media/adfs2_authorization.gif)  
  
Pour plus d’informations sur l’autorisation de revendications, voir [quand utiliser une règle de revendication d’autorisation](When-to-Use-an-Authorization-Claim-Rule.md).  
  

