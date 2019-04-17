---
ms.assetid: ceb9ce18-5a94-4166-9edd-2685b81fc15f
title: "Déployer des revendications dans les forêts"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 7d78258d8f1db9889b6d2db8c497780940ed35a1
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="deploy-claims-across-forests"></a>Déployer des revendications dans les forêts

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Dans Windows Server2012, un type de revendication est une assertion sur l’objet avec lequel il est associé. Types de revendications sont définis par forêt dans ActiveDirectory. Il existe plusieurs scénarios où un principal de sécurité doit pouvoir franchir une limite d’approbation pour accéder aux ressources dans une forêt approuvée. Transformation de revendications inter-forêts dans Windows Server2012vous permet de transformer les revendications entrantes et sortantes qui franchissent les forêts afin que les revendications soient reconnues et acceptées dans les forêts d’approbation et approuvées. Certains scénarios réels de transformation des revendications sont:  
  
-   Les forêts de confiance peuvent utiliser la transformation comme protection contre l’élévation de privilèges en filtrant les revendications entrantes ayant des valeurs spécifiques.  
  
    Les forêts de confiance peuvent également émettre des revendications pour les principaux franchissant une limite d’approbation si la forêt approuvée ne pas prendre en charge ou émettre des revendications.  
  
-   Forêts approuvées peuvent utiliser la transformation de revendication pour empêcher certains types de revendications et les revendications avec certaines valeurs de sortir de la forêt d’approbation.  
  
-   Vous pouvez également utiliser la revendication de transformation pour mapper différents types de revendications entre forêts d’approbation et approuvées. Cela peut servir pour généraliser le type de revendication, la valeur de revendication ou les deux. Sans cela, vous devez normaliser les données entre les forêts avant de pouvoir utiliser les revendications. La généralisation des revendications entre les forêts d’approbation et approuvées réduit les coûts informatiques.  
  
## <a name="claim-transformation-rules"></a>Les règles de transformation de revendication  
La syntaxe du langage de règle transformation divise une règle en deux parties principales: une série d’instructions de condition et l’instruction d’émission. Chaque instruction de condition a deux sous-composants: l’identificateur de revendication et la condition. L’instruction d’émission contient les mots clés, les séparateurs et une expression d’émission. L’instruction de condition peut commence avec une variable d’identificateur de revendication, qui représente la revendication d’entrée correspondante. La condition vérifie l’expression. Si la revendication d’entrée ne correspond pas à la condition, le moteur de transformation ignore l’instruction d’émission et évalue la revendication d’entrée suivante par rapport à la règle de transformation. Si toutes les conditions correspondent à la revendication d’entrée, il traite l’instruction d’émission.  
  
Pour plus d’informations sur le langage des règles de revendication, voir [Claims Transformation Rules Language](Claims-Transformation-Rules-Language.md).  
  
## <a name="linking-claim-transformation-policies-to-forests"></a>Liaison des stratégies de transformation de revendication à des forêts  
Il existe deux composants impliqués dans la définition des stratégies de transformation de revendication: les objets de stratégie de transformation et le lien de transformation de revendication. Les objets de stratégie live dans le contexte d’appellation de configuration dans une forêt et qu’ils contiennent des informations de mappage pour les revendications. Le lien spécifie les approuver et des forêts approuvées le mappage s’applique.  
  
Il est important de comprendre si la forêt est l’approbation ou approuvée, car c’est pour lier les objets de stratégie de transformation. Par exemple, la forêt approuvée est la forêt qui contient les comptes d’utilisateurs qui nécessitent un accès. La forêt d’approbation est la forêt qui contient les ressources que vous souhaitez autoriser les utilisateurs à. Les revendications se déplacent dans le même sens que l’entité qui requiert l’accès de sécurité. Par exemple, s’il existe une approbation à sens unique à partir de la forêt contoso.com à la forêt adatum.com, les revendications seront acheminées depuis adatum.com à contoso.com, qui permet aux utilisateurs de adatum.com d’accéder aux ressources dans contoso.com.  
  
Par défaut, une forêt approuvée autorise tous les revendications sortantes et une forêt de confiance rejette toutes les revendications entrantes qu’il reçoit.  
  
## <a name="in-this-scenario"></a>Dans ce scénario  
Le guide suivant est disponible pour ce scénario:  
  
-   [Déployer des revendications dans les forêts et #40; étapes de démonstration et #41;](Deploy-Claims-Across-Forests--Demonstration-Steps-.md)  
  
-   [Langage de règles de Transformation de revendications](Claims-Transformation-Rules-Language.md)  
  
## <a name="BKMK_NEW"></a>Fonctionnalités et rôles inclus dans ce scénario  
Le tableau suivant répertorie les rôles et fonctionnalités qui font partie de ce scénario et décrit la prise en charge.  
  
|Rôle ou une fonctionnalité|Comment il prend en charge ce scénario|  
|-----------------|---------------------------------|  
|Services de domaine ActiveDirectory|Dans ce scénario, vous devez configurer deux forêts ActiveDirectory avec une approbation bidirectionnelle. Vous avez des revendications dans les deux forêts. Vous permet également de définir des stratégies d’accès central sur la forêt d’approbation dans lequel se trouvent les ressources.|  
|Rôle Services de fichiers et stockage|Dans ce scénario, la classification des données est appliquée aux ressources sur les serveurs de fichiers. La stratégie d’accès centralisée est appliquée au dossier dans lequel vous souhaitez accorder l’accès utilisateur. Après la transformation, la revendication accorde l’accès des utilisateurs aux ressources en fonction de la stratégie d’accès centralisée qui est appliquée au dossier sur le serveur de fichiers.|  
  


