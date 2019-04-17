---
ms.assetid: ffb9d63c-ba7c-4ad1-b814-6db67f98c943
title: "Le rôle du Pipeline de revendications"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 5076a686b5d0b9a539f6cad8594aaf84dccc3edb
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

# <a name="the-role-of-the-claims-pipeline"></a>Le rôle du Pipeline de revendications
Le pipeline de revendications dans ActiveDirectory Federation Services \(ADFS\) représente le chemin d’accès revendications doivent respecter via le Service de fédération avant de pouvoir être émises. Le Service de fédération gère l’ensemble du processus end\ celle-ci bout du flux des revendications dans les étapes du pipeline de revendications, qui inclut également le traitement des règles de revendication par le moteur de règles de revendication.  
  
Pour plus d’informations sur les règles de revendication, voir [le rôle des règles de revendication](The-Role-of-Claim-Rules.md). Pour plus d’informations sur la façon dont le moteur de règles de revendication traite les règles, consultez [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md).  
  
La section suivante décrit le processus du Service de fédération supervise plus en détail.  
  
## <a name="claims-pipeline-process"></a>Processus de pipeline de revendications  
Le processus de pipeline de revendications se compose de trois étapes au niveau évolutifs. Chaque étape du processus initialise le moteur de règles de revendication pour exécuter les règles de revendication qui sont spécifiques à ce stade. Ces étapes sont les suivantes \ (dans l’ordre qu’elles ont été occur\):  
  
1.  Accepter les revendications entrantes: cette étape dans le pipeline de revendications est utilisée pour extraire les revendications entrantes du jeton et éliminer les revendications qui ne sont pas prévues ou approuvées. Une fois qu’ils sont extraits, les règles d’acceptation qui composent la règle de transformation d’acceptation définie pour une approbation de fournisseur sont exécutées de revendications. Ces règles peuvent servir à passer ou ajouter de nouvelles revendications qui peuvent ensuite être utilisées dans les étapes suivantes du pipeline de revendications. La sortie de cette étape est utilisée comme entrée pour les deuxième et troisième étapes.  
  
2.  Autoriser le demandeur de revendications: cette étape est utilisée par le moteur de revendications pour émettre d’autorisation ou refuser des revendications basées sur si le demandeur du jeton est autorisé à obtenir un jeton pour la partie de confiance donné ou non. Toutefois, avant que cela peut se produire définissez les règles d’autorisation qui composent la règle d’autorisation d’émission ou la règle d’autorisation de délégation pour une approbation de partie de confiance sont exécutées.  
  
3.  Émettre des revendications sortantes: cette étape est utilisée pour émettre les revendications sortantes et les envoyer dans le pipeline où elles seront empaquetées dans un jeton de sécurité. Toutefois, avant que cela peut se produire les règles d’émission qui composent la règle de transformation d’émission définie pour une approbation de partie de confiance sont exécutées, déterminant ainsi les revendications qui seront émises en tant que revendications sortantes.  
  
Les trois étapes ci-dessus effectuer le traitement des règles de revendications, mais utilisent un autre ensemble de règles. Comme décrit ci-dessus, chaque étape dispose d’un jeu de règles basées sur l’émetteur des revendications entrantes \(the acceptance rules\) ou le service cible pour lequel les claimincludes sont émis \ (rules\ d’autorisation et d’émission).  
  
Revendications sont indépendantes du token\ mais sont transmises sur le réseau encapsulées dans des jetons de sécurité. Les règles de revendication fonctionnent sur les revendications, quelle que soit le format du jeton de sécurité entrant ou sortant.  
  
Revendications règles contiennent la logique définie par l’administrator\ par lequel le moteur de revendications est accepte les revendications entrantes, autorise les revendications en fonction de l’identité du demandeur et émet les revendications sont requis par une partie de confiance. Pour finir, c’est le moteur de revendications qui détermine les revendications passe dans le jeton de sécurité émis après que la revendication a été transmise via le pipeline de revendications.  
  
Comme indiqué dans l’illustration suivante, le pipeline de revendications est responsable de l’ensemble du processus end\ celle-ci bout de circulation d’une revendication via les diverses étapes du pipeline afin de vous retrouver avec une revendication émise qui sera envoyée via une approbation de partie de confiance. La revendication sortante dans l’illustration représente la revendication émise.  
  
![Rôles ADFS](media/adfs2_pipeline.gif)  
  
Bien qu’il n’est pas indiqué dans l’illustration, c’est le moteur de revendications qui effectue le traitement des règles à chaque étape. Pour plus d’informations, voir [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md).  
  

