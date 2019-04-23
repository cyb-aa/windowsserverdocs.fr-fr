---
ms.assetid: 7be1f2cb-02d5-4209-ba79-edf496a88f47
title: Audit d’accès des fichiers de scénario
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 93d78bbefce38173198f991543fb3a06d145b373
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867680"
---
# <a name="scenario-file-access-auditing"></a>Scénario : Audit d'accès aux fichiers

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

L’audit de sécurité est l’un des outils les plus puissants de gestion de la sécurité d’une entreprise. La conformité aux normes fait notamment partie de ses objectifs clés. Des normes industrielles telles que Sarbanes Oxley, HIPAA (Health Insurance Portability and Accountability Act) et PCI (Payment Card Industry) exigent que les entreprises se conforment à un ensemble strict de règles liées à la sécurité et à la confidentialité des données. Les audits de sécurité visent à établir la présence de ce type de stratégies et à prouver la conformité à ces normes. De plus, les audits de sécurité permettent de détecter un comportement anormal, d’identifier et d’atténuer les lacunes dans les stratégies de sécurité, ainsi que de dissuader tout comportement irresponsable en créant un journal de l’activité utilisateur qui peut être utilisé pour une analyse d’investigation.  
  
Les exigences en matière de stratégie d’audit sont généralement établies à différents niveaux :  
  
-   **Sécurité des informations.** Les journaux d’audit sur l’accès aux fichiers sont souvent utilisés pour des analyses d’investigation et de détection d’intrusion. Les organisations peuvent désormais largement améliorer leurs temps de réponse et leur précision en termes d’investigation car elles disposent d’événements ciblés en matière d’accès aux informations importantes.  
  
-   **Stratégie organisationnelle.** À titre d’exemple, les organisations assujetties aux normes PCI pourraient disposer d’une stratégie centrale pour surveiller l’accès à tous les fichiers qui sont marqués comme contenant des informations de carte de crédit et des informations d’identification personnelle (PII, Personally Identifiable Information).  
  
-   **Stratégie de service.** Le service des finances peut notamment exiger que la capacité à modifier certains documents financiers (tels qu’un rapport sur les revenus trimestriels) soit limitée aux membres de son service. Il serait donc légitime que ce dernier souhaite surveiller toutes les autres tentatives de modification de ces documents.  
  
-   **Stratégie métier.** Les propriétaires des projets pourraient par exemple surveiller toutes les tentatives non autorisées d’affichage des données qui appartiennent à leurs projets.  
  
En outre, le service de conformité peut souhaiter surveiller toutes les stratégies d’autorisation centralisées et les éléments de la stratégie tels que les attributs utilisateur, d’ordinateur et de ressource.  
  
Le coût de collecte, de stockage et d’analyse des événements d’audit de sécurité représente l’une des considérations essentielles des audits de sécurité. Si les stratégies d’audit ne sont pas suffisamment spécifiques, le volume d’événements d’audit collectés s’accroît, ce qui augmente les coûts. Si les stratégies d’audit sont trop limitées, vous risquez de manquer des événements importants.  
  
Avec Windows Server 2012, vous pouvez créer des stratégies d’audit à l’aide de revendications et propriétés de la ressource. Vous générez ainsi des stratégies d’audit plus riches, davantage ciblées et faciles à utiliser, et faites exister des scénarios qui jusqu’à présent étaient impossibles ou trop difficiles à effectuer. Voici des exemples de stratégies d’audit que les administrateurs peuvent créer :  
  
-   Auditer tous les utilisateurs qui ne bénéficient pas d’habilitations sécuritaires élevées et qui tentent d’accéder à des documents HBI. Par exemple, Auditer | Tout le monde | All-Access | Resource.BusinessImpact=HBI AND User.SecurityClearance!=High.  
  
-   Auditer tous les fournisseurs lorsqu’ils tentent d’accéder à des documents liés à des projets sur lesquels ils ne travaillent pas. Par exemple, Auditer | Tout le monde | All-Access | User.EmploymentStatus=Vendor AND User.Project Not_AnyOf Resource.Project.  
  
Ces stratégies permettent de normaliser le volume des événements d’audit et de les limiter aux données ou aux utilisateurs les plus pertinents.  
  
Une fois que les administrateurs ont créé et appliqué les stratégies d’audit, le point suivant qu’ils doivent prendre en considération est la collecte d’informations significatives à partir des événements d’audit collectés. Les événements d’audit basés sur des expressions peuvent aider à réduire le volume des audits. Toutefois, les utilisateurs doivent pouvoir interroger ces événements pour des informations pertinentes et poser des questions telles que « qui accède à mes données HBI ? » ou « Y A-t-il une tentative non autorisée d’accéder aux données sensibles ? »  
  
 Windows Server 2012 améliore les événements d’accès aux données existantes avec les revendications d’utilisateur, ordinateur et des ressources. Ces événements sont générés par serveur. Afin de fournir une vue globale des événements dans l’organisation, Microsoft s’emploie aux côtés de partenaires à fournir des collections d’événements et des outils d’analyse, tels que les services ACS dans Microsoft System Center Operations Manager.  
  
La Figure 4 présente une vue d’ensemble de la stratégie d’audit centralisée.  
  
![guides de solutions](media/Scenario--File-Access-Auditing/DynamicAccessControl_RevGuide_4.JPG)  
  
**Figure 4** Expérience d’audit centralisée  
  
La mise en place et la consommation d’audits de sécurité implique normalement les étapes générales suivantes :  
  
1.  Identifier l’ensemble correct de données et d’utilisateurs à surveiller  
  
2.  Créer et appliquer les stratégies d’audit appropriées  
  
3.  Collecter et analyser les événements d’audit  
  
4.  Gérer et surveiller les stratégies qui ont été créées  
  
## <a name="in-this-scenario"></a>Dans ce scénario  
Les rubriques suivantes fournissent des conseils supplémentaires pour ce scénario :  
  
-   [Fichier planifier l’audit d’accès](Plan-for-File-Access-Auditing.md)  
  
-   [Déployer l’audit de sécurité avec les stratégies d’Audit centralisées &#40;étapes de démonstration&#41;](Deploy-Security-Auditing-with-Central-Audit-Policies--Demonstration-Steps-.md)  
  
## <a name="BKMK_NEW"></a>Fonctionnalités et rôles inclus dans ce scénario  
Le tableau qui suit décrit les rôles et les fonctionnalités inclus dans ce scénario et détaille la manière dont ils prennent en charge ce dernier.  
  
|Rôle/fonctionnalité|Prise en charge de ce scénario|  
|-----------------|---------------------------------|  
|Rôle des Services de domaine Active Directory|Les services AD DS dans Windows Server 2012 introduit une plateforme d’autorisation basée sur les revendications qui permet de créer des revendications d’utilisateur et revendications de périphérique, une identité composée, (utilisateur plus revendications de périphérique), une nouveau modèle de stratégies (CAP) accès centralisée et l’utilisation de la classification des fichiers informations dans les décisions d’autorisation.|  
|Rôle des Services de fichiers et de stockage|Serveurs de fichiers dans Windows Server 2012 fournissent une interface utilisateur où les administrateurs peuvent afficher les autorisations effectives pour les utilisateurs d’un fichier ou dossier et résoudre les problèmes d’accès et accorder l’accès en fonction des besoins.|  
  


