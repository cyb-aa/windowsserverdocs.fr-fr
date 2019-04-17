---
ms.assetid: 7f285c9f-c3e8-4aae-9ff4-a9123815114e
title: "Stratégie d’accès centralisée scénario"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 1ec4165209b726609b1f9b2caeab02fb5072c756
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="scenario-central-access-policy"></a>Scénario: Stratégie d’accès centralisée

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Stratégies d’accès central pour les fichiers permettent aux organisations de déployer et gérer des stratégies d’autorisation qui comprennent des expressions conditionnelles qui utilisent des groupes d’utilisateurs, les revendications d’utilisateur, les revendications de périphérique et les propriétés de ressource de manière centralisée. (Les revendications sont des assertions sur les attributs de l’objet auquel elles sont associées). Par exemple, pour accéder à fort impact commercial élevé (données HBI), un utilisateur doit être un employé à plein temps, obtenir l’accès à partir d’un appareil géré et ouvrez une session avec une carte à puce. Ces stratégies sont définies et hébergées dans les Services de domaine ActiveDirectory (ADDS).  
  
Stratégies d’accès de l’organisation visent le respect d’exigences réglementaires et de conformité. Par exemple, si une organisation a un besoin commercial à limiter l’accès aux informations d’identification personnelle (PII) dans les fichiers qu’au propriétaire du fichier et les membres du service ressources humaines (RH) qui sont autorisés à afficher les informations d’identification personnelle, cette stratégie s’applique aux fichiers PII stockés sur les serveurs de fichiers dans l’organisation. Dans cet exemple, vous devez être en mesure de:  
  
-   Identifier et marquer les fichiers qui contiennent des informations d’identification personnelle.  
  
-   Identifier le groupe de ressources humaines les membres qui sont autorisés à afficher les informations d’identification personnelle.  
  
-   Créer une stratégie d’accès centralisée qui s’applique à tous les fichiers qui contiennent des informations d’identification personnelle partout où ils se trouvent sur des serveurs de fichiers dans l’organisation.  
  
L’initiative pour déployer et appliquer une stratégie d’autorisation peut provenir pour de nombreuses raisons et s’appliquent à plusieurs niveaux de l’organisation. Voici quelques exemples de types de stratégie:  
  
-   **Stratégie d’autorisation de l’organisation.** Plus souvent pour origine le Bureau sécurité des informations, cette stratégie d’autorisation est pilotée par une configuration requise générale de l’entreprise ou la conformité, et s’applique à toute l’organisation. Par exemple, les fichiers HBI sont accessibles uniquement employés à plein temps.  
  
-   **Stratégie d’autorisation de service.** Chaque service d’une organisation a des exigences de gestion des données spéciales qu’ils souhaitent mettre en œuvre. Par exemple, le service finance peut souhaiter limiter l’accès aux serveurs de finance aux employés.  
  
-   **Stratégie de gestion de données spécifique.** Cette stratégie a souvent trait aux exigences commerciales et de conformité, et elle est destinée à protéger l’accès aux informations gérées. Par exemple, les établissements financiers peuvent implémenter des murs d’informations afin que les analystes d’accéder aux informations de courtage et aux courtiers d’accéder aux informations analyse.  
  
-   **Besoin de connaître la stratégie.** Ce type de stratégie d’autorisation est généralement utilisé conjointement avec les types de stratégies précédents. Par exemple, les fournisseurs doivent être en mesure d’accéder et modifier uniquement les fichiers appartenant à un projet que sur lesquels ils travaillent.  
  
Les environnements réels nous enseignent également que chaque stratégie d’autorisation doit comporter des exceptions afin que les organisations de réagir rapidement lorsque surviennent de besoins métier importants. Par exemple, les dirigeants qui ne peut pas trouver de leurs cartes à puce et ont besoin d’accéder rapidement aux informations HBI peuvent appeler le support technique pour obtenir une exception temporaire pour accéder à ces informations.  
  
Stratégies d’accès centralisées agissent comme parapluies de sécurité une organisation s’appliquent à ses serveurs. Ces stratégies améliorent (mais ne remplacent pas) les stratégies d’accès local ou les listes de contrôle d’accès discrétionnaire (DACL) qui sont appliquées aux fichiers et dossiers. Par exemple, si une liste DACL sur un fichier autorise l’accès à un utilisateur spécifique, mais une stratégie centralisée qui est appliquée au fichier restreint l’accès au même utilisateur, l’utilisateur ne peut pas obtenir l’accès au fichier. Si la stratégie d’accès centralisée autorise l’accès, mais la liste DACL n’autorise pas l’accès, l’utilisateur ne peut pas obtenir l’accès au fichier.  
  
Une règle de stratégie d’accès centralisée a les parties logiques suivantes:  
  
-   **Mise en application.** Une condition qui définit les données auxquelles la stratégie s’applique, tels que Resource.businessimpact = High.  
  
-   **Conditions d’accès.** Une liste d’un ou plusieurs entrées contrôle d’accès (ACE) qui définissent qui peut accéder aux données, comme Allow | Contrôle total | User.employeetype = FTE.  
  
-   **Exceptions.** Liste supplémentaire d’un ou plusieurs entrées qui définissent une exception pour la stratégie, tels que memberOf (hbiexceptiongroup).  
  
Les deux figures suivantes montrent le flux de travail dans accès central et stratégies d’audit.  
  
![guides de solutions](media/Scenario--Central-Access-Policy/DynamicAccessControl_RevGuide.JPG)  
  
**Figure1** concepts des stratégies centralisées accès et d’audit  
  
![guides de solutions](media/Scenario--Central-Access-Policy/DynamicAccessControl_RevGuide_2.JPG)  
  
**Figure2** workflow de stratégie d’accès centralisée  
  
La stratégie d’autorisation centralisée combine les composants suivants:  
  
-   Liste des règles d’accès définies de manière centralisée qui ciblent des types spécifiques d’informations, tels que HBI ou PII.  
  
-   Une stratégie définie de manière centralisée qui contient une liste de règles.  
  
-   Un identificateur de stratégie qui est attribué à chaque fichier sur les serveurs de fichiers pour pointer vers une stratégie d’accès centralisée spécifique qui doit être appliquée lors de l’autorisation d’accès.  
  
La figure suivante montre comment vous pouvez combiner des stratégies en listes de stratégie de contrôle de manière centralisée l’accès aux fichiers.  
  
![guides de solutions](media/Scenario--Central-Access-Policy/DynamicAccessControl_RevGuide3.JPG)  
  
**Figure3** combinaison de stratégies  
  
## <a name="in-this-scenario"></a>Dans ce scénario  
Le guide suivant est à votre disposition pour les stratégies d’accès centralisées:  
  
-   [Planifier un déploiement de la stratégie d’accès centralisée](assetId:///0311a76d-d66c-4ddb-ade6-af586a2ad82f)  
  
-   [Déployer une stratégie d’accès centralisée et #40; étapes de démonstration et #41;](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md)  
  
-   [Contrôle d’accès dynamique: Vue d’ensemble du scénario](Dynamic-Access-Control--Scenario-Overview.md)  
  
## <a name="BKMK_NEW"></a>Fonctionnalités et rôles inclus dans ce scénario  
Le tableau suivant répertorie les rôles et fonctionnalités qui font partie de ce scénario et décrit la prise en charge.  
  
|Rôle ou une fonctionnalité|Comment il prend en charge ce scénario|  
|-----------------|---------------------------------|  
|Rôle des Services de domaine ActiveDirectory|Les services ADDS dans Windows Server2012 introduit une plateforme d’autorisation basée sur les revendications qui permet de créer des revendications d’utilisateur et revendications de périphérique, une identité composée, (utilisateur plus revendications de périphérique), nouveaux modèles de stratégie (CAP) d’accès centralisée et l’utilisation des informations de classification des fichiers dans les décisions d’autorisation.|  
|Rôle serveur des Services de stockage et de fichier|Services de fichiers et stockage fournit des technologies qui vous aident à configurer et gérer un ou plusieurs serveurs de fichiers qui constituent des emplacements centralisés sur votre réseau où vous pouvez stocker les fichiers et les partager avec des utilisateurs. Si vos utilisateurs réseau doivent accéder aux mêmes fichiers et applications, ou si la gestion centralisée de sauvegarde et de fichier est importante pour votre organisation, vous devez configurer un ou plusieurs ordinateurs en tant qu’un serveur de fichiers en ajoutant le rôle Services de fichiers et stockage et les services de rôle appropriés sur les ordinateurs.|  
|Ordinateur client Windows|Les utilisateurs peuvent accéder des fichiers et dossiers sur le réseau par le biais de l’ordinateur client.|  
  


