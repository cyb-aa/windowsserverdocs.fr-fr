---
ms.assetid: 7f285c9f-c3e8-4aae-9ff4-a9123815114e
title: Stratégie d’accès centralisée du scénario
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: a22592e5c8af9fa23725de90a14a9a8a46c286d7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861152"
---
# <a name="scenario-central-access-policy"></a>Scénario : Stratégie d’accès centralisée

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Les stratégies d’accès centralisées pour les fichiers permettent aux organisations de déployer et gérer de manière centrale des stratégies d’autorisation qui comprennent des expressions conditionnelles utilisant des groupes d’utilisateurs, des revendications d’utilisateur, de revendications de périphérique et des propriétés de ressource. (Les revendications sont des assertions sur les attributs de l’objet auquel elles sont associées). Par exemple, pour accéder aux données HBI (high-business-impact), un utilisateur doit être un employé à plein-temps, obtenir l’accès à partir d’un périphérique géré et se connecter avec une carte à puce. Ces stratégies sont définies et hébergées dans les services de domaine Active Directory (AD DS).  
  
Les stratégies d’accès de l’organisation visent le respect d’exigences réglementaires et de conformité. Prenons l’exemple d’une organisation qui se conforme à une exigence professionnelle selon laquelle les informations d’identification personnelle (PII) figurant dans les fichiers ne doivent être accessibles qu’au propriétaire du fichier et aux employés du service des ressources humaines habilités à afficher ce type d’information. Cette stratégie s’applique à l’ensemble des fichiers PII stockés sur les serveurs de fichiers dans l’organisation. Dans cet exemple, vous devez être en mesure d’effectuer les opérations suivantes :  
  
-   identifier et marquer les fichiers contenant des informations d’identification personnelle ;  
  
-   identifier le groupe d’employés du service des ressources humaines habilités à afficher ce type d’information ;  
  
-   créer une stratégie d’accès centralisée qui s’applique à tous les fichiers contenant des informations d’identification personnelle stockées sur les serveurs de fichiers dans l’organisation.  
  
La décision de déployer et de mettre en application une stratégie d’autorisation peut s’appliquer à différents niveaux de l’organisation et être motivée par de nombreuses raisons : Voici quelques exemples de types de stratégies :  
  
-   **Stratégie d’autorisation au niveau de l’organisation** : ayant le plus souvent pour origine le bureau Sécurité des informations, cette stratégie d’autorisation vise le respect de la conformité ou des exigences d’organisation de haut niveau et s’applique à toute l’organisation. Par exemple, les fichiers HBI sont accessibles uniquement aux employés à plein-temps.  
  
-   **Stratégie d’autorisation de service** : : chaque service d’une organisation a des exigences particulières en matière de gestion des données qui doivent être respectées. Par exemple : le service Finance peut souhaiter limiter l’accès des serveurs de finance aux employés de ce même service.  
  
-   **Stratégie de gestion des données spécifique** : : cette stratégie a souvent trait aux exigences commerciales et de conformité. Elle a pour objectif de protéger l’accès aux informations gérées. Par exemple, les établissements financiers peuvent implémenter des murs d’informations visant à empêcher les analystes d’accéder aux informations de courtage et aux courtiers d’accéder aux informations d’analyse.  
  
-   **Stratégie basée sur le besoin de connaître.** Ce type de stratégie d’autorisation est généralement utilisé conjointement avec les types de stratégies précédents. Par exemple, les fournisseurs doivent pouvoir accéder et modifier uniquement les fichiers en rapport avec un projet sur lequel ils travaillent.  
  
Les environnements réels nous enseignent également que chaque stratégie d’autorisation doit comporter des exceptions pour permettre aux organisations de réagir rapidement en cas de survenue de besoins métier importants. Par exemple, les dirigeants qui ne trouvent pas leurs cartes à puce et ont besoin d’un accès rapide aux informations HBI peuvent appeler le support technique pour obtenir une exception temporaire d’accès à ces informations.  
  
Les stratégies d’accès centralisées agissent comme un processus de sécurité « parapluie » que l’organisation déploie sur tous ses serveurs. Ces stratégies améliorent les stratégies d’accès locales ou les listes de contrôle d’accès discrétionnaire (DACL, Discretionary Access Control List) qui sont appliquées aux fichiers et aux dossiers. Par exemple, si une liste de contrôle d’accès discrétionnaire (DACL) sur un fichier autorise l’accès à un utilisateur spécifique, mais qu’une stratégie centrale appliquée au fichier restreint l’accès au même utilisateur, l’utilisateur ne peut pas obtenir l’accès au fichier. Si la stratégie d’accès centralisée autorise l’accès, mais pas la liste de contrôle d’accès discrétionnaire (DACL), l’utilisateur ne peut pas obtenir l’accès au fichier.  
  
Une règle de stratégie d’accès centralisée est composée des parties logiques suivantes :  
  
-   **Applicabilité.** Condition qui définit les données auxquelles la stratégie s’applique, telle que Resource.BusinessImpact=High.  
  
-   **Conditions d’accès.** liste d’une ou plusieurs entrées de contrôle d’accès (ACE) qui définissent qui peut accéder aux données, comme Allow | Full Control | User.EmployeeType=FTE.  
  
-   **Exceptions.** liste supplémentaire d’une ou plusieurs entrées de contrôle d’accès (ACE) qui définissent une exception pour la stratégie, telle que MemberOf(HBIExceptionGroup).  
  
Les deux figures suivantes montrent le flux de travail des stratégies d’audit et d’accès centralisées.  
  
![guides de solutions](media/Scenario--Central-Access-Policy/DynamicAccessControl_RevGuide.JPG)  
  
**Figure 1** Concepts des stratégies d’audit et d’accès centralisées  
  
![guides de solutions](media/Scenario--Central-Access-Policy/DynamicAccessControl_RevGuide_2.JPG)  
  
**Figure 2** Flux de travail de la stratégie d’accès centralisée  
  
La stratégie d’autorisation centralisée combine les composants suivants :  
  
-   Une liste des règles d’accès définies de manière centralisée qui ciblent des types spécifiques d’informations, tels que HBI ou PII.  
  
-   Une stratégie définie de manière centralisée qui contient une liste de règles.  
  
-   Un identificateur de stratégie affecté à chaque fichier sur les serveurs de fichiers pour pointer vers une stratégie d’accès centralisée spécifique qui doit être appliquée pendant l’autorisation d’accès.  
  
La figure suivante montre comment associer des stratégies en listes de stratégie pour contrôler de manière centralisée l’accès aux fichiers.  
  
![guides de solutions](media/Scenario--Central-Access-Policy/DynamicAccessControl_RevGuide3.JPG)  
  
**Figure 3** Combinaison de stratégies  
  
## <a name="in-this-scenario"></a>Dans ce scénario  
Le guide suivant est à votre disposition pour les stratégies d’accès centralisée :  
  
-   [Planifier le déploiement d’une stratégie d’accès centralisée](assetId:///0311a76d-d66c-4ddb-ade6-af586a2ad82f)  
  
-   [Déployer une procédure de démonstration &#40;de stratégie d’accès centralisée&#41;](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md)  
  
-   [Access Control dynamique : vue d’ensemble du scénario](Dynamic-Access-Control--Scenario-Overview.md)  
  
## <a name="roles-and-features-included-in-this-scenario"></a><a name="BKMK_NEW"></a>Rôles et fonctionnalités inclus dans ce scénario  
Le tableau qui suit décrit les rôles et les fonctionnalités inclus dans ce scénario et détaille la manière dont ils prennent en charge ce dernier.  
  
|Rôle/fonctionnalité|Prise en charge de ce scénario|  
|-----------------|---------------------------------|  
|Rôle Services de domaine Active Directory|AD DS dans Windows Server 2012 introduit une plateforme d’autorisation basée sur les revendications qui permet la création de revendications d’utilisateur et d’appareils, l’identité composée, (utilisateurs plus revendications de périphérique), les nouveaux modèles de stratégie d’accès centralisée et l’utilisation des informations de classification de fichiers dans les décisions d’autorisation.|  
|Rôle de serveur Services de fichiers et de stockage|Les services de fichiers et de stockage fournissent des technologies qui vous permettent de configurer et de gérer un ou plusieurs serveurs de fichiers qui constituent sur votre réseau des emplacements centralisés où vous pouvez stocker des fichiers et les partager avec d’autres utilisateurs. Si vos utilisateurs réseau doivent accéder aux mêmes fichiers et applications, ou si la sauvegarde et la gestion des fichiers centralisées sont des éléments importants pour votre organisation, configurez un ou plusieurs ordinateurs en tant que serveurs de fichiers en ajoutant le rôle Services de fichiers et de stockage et les services de rôle appropriés aux ordinateurs.|  
|Ordinateur client Windows|Les utilisateurs peuvent accéder aux fichiers et dossiers sur le réseau via l’ordinateur client.|  
  


