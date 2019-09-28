---
ms.assetid: ffb9d63c-ba7c-4ad1-b814-6db67f98c943
title: Rôle du pipeline de revendications
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 6aafa37b06599f4114cf076e87415fece128fb05
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407334"
---
# <a name="the-role-of-the-claims-pipeline"></a>Rôle du pipeline de revendications
Le pipeline de revendications \(dans\) services ADFS AD FS représente le chemin d’accès que les revendications doivent suivre via l’service FS (Federation Service) avant de pouvoir être émises. Le service FS (Federation Service) gère l’ensemble du\-processus\-de bout en bout de la transmission des revendications à travers les différentes étapes du pipeline de revendications, qui incluent également le traitement des règles de revendication par le moteur de règles de revendication.  
  
Pour plus d’informations sur les règles de revendication, consultez [rôle des règles de revendication](The-Role-of-Claim-Rules.md). Pour plus d’informations sur la façon dont le moteur de règles de revendication traite les règles, voir [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md).  
  
La section suivante décrit plus en détail le processus supervisé par le service de fédération.  
  
## <a name="claims-pipeline-process"></a>Processus de pipeline de revendications  
Le processus de pipeline de revendications se\-compose de trois étapes de haut niveau. Chaque étape du processus initialise le moteur de règles de revendication pour exécuter les règles de revendication qui lui sont propres. Ces étapes sont \(les\)suivantes :  
  
1.  Accepter les revendications entrantes : cette étape du pipeline de revendications est utilisée pour extraire les revendications entrantes du jeton et éliminer les revendications qui ne sont pas prévues ou approuvées. Une fois extraites, les règles d’acceptation qui composent le jeu de règles de transformation d’acceptation pour une approbation de fournisseur de revendications sont exécutées. Ces règles servent à passer ou à ajouter de nouvelles revendications qui peuvent être utilisées dans les étapes suivantes du pipeline de revendications. La sortie de cette étape est utilisée comme entrée pour les deuxième et troisième étapes.  
  
2.  Autoriser le demandeur de revendications : cette étape est utilisée par le moteur de revendications pour émettre, autoriser ou refuser des revendications selon que le demandeur du jeton est autorisé ou non à obtenir un jeton pour la partie de confiance donné. Toutefois, avant que cela puisse se produire, les règles d’autorisation qui composent le jeu de règles d’autorisation d’émission ou le jeu de règles d’autorisation de délégation d’une partie de confiance sont exécutées.  
  
3.  Émettre des revendications sortantes : cette étape est utilisée pour émettre les revendications sortantes et les envoyer dans le pipeline où elles seront empaquetées dans un jeton de sécurité. Toutefois, avant que cela puisse se produire, les règles d’émission qui composent la règle de transformation d’émission définie pour une partie de confiance sont exécutées, déterminant ainsi les revendications qui seront émises en tant que revendications sortantes.  
  
Les trois étapes ci-dessus effectuent le traitement des règles de revendication, mais utilisent un autre ensemble de règles. Comme décrit ci-dessus, chaque étape dispose d’un jeu de règles associé basé sur l’émetteur des revendications \(entrantes, les règles\) d’acceptation ou le service cible pour lequel l’autorisation \(claimincludes sont émises et règles\)d’émission.  
  
Les revendications sont\-indépendantes des jetons, mais elles sont transmises sur le réseau encapsulé dans des jetons de sécurité. Les règles de revendication fonctionnent sur les revendications, quel que soit le format du jeton de sécurité entrant ou sortant.  
  
Les règles de revendication contiennent\-la logique définie par l’administrateur par laquelle le moteur de revendications accepte les revendications entrantes, autorise les revendications en fonction de l’identité du demandeur et émet les revendications requises par une partie de confiance. Finalement, c’est le moteur de revendications qui détermine les revendications destinées au jeton de sécurité émis après que la revendication a été transmise via le pipeline de revendications.  
  
Comme indiqué dans l’illustration suivante, le pipeline de revendications est responsable de la\-totalité\-du processus de bout en bout de la transmission d’une revendication via les diverses étapes du pipeline afin d’obtenir une revendication émise qui sera envoyée sur une base de données approbation de tiers. La revendication sortante dans l’illustration représente la revendication émise.  
  
![Rôles de AD FS](media/adfs2_pipeline.gif)  
  
Bien que cela ne soit pas indiqué dans l’illustration, c’est le moteur de revendications qui effectue le traitement des règles à chaque étape. Pour plus d'informations, voir [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md).  
  

