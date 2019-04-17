---
ms.assetid: 7be1f2cb-02d5-4209-ba79-edf496a88f47
title: "Scénario de l’audit d’accès aux fichiers"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 93d78bbefce38173198f991543fb3a06d145b373
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="scenario-file-access-auditing"></a>Scénario: Audit d’accès du fichier

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

L’audit de sécurité est un des outils plus puissants pour aider à maintenir la sécurité d’une entreprise. Un des objectifs clés des audits de sécurité est la conformité aux normes. Normes industrielles telles que Sarbanes Oxley, d’intégrité Insurance Portability and Accountability Act (HIPAA) et secteur PCI (Payment Card) exigent que les entreprises de suivre un ensemble strict de règles liées à la sécurité des données et de confidentialité. Aide des audits de sécurité établir la présence de telles stratégies et à prouver la conformité à ces normes. En outre, les audits de sécurité détecter un comportement anormal, d’identifier et d’atténuer les lacunes dans les stratégies de sécurité et que de dissuader tout comportement irresponsable en créant un journal d’activité utilisateur qui peut être utilisé pour l’analyse d’investigation.  
  
Spécifications de la stratégie d’audit sont généralement établies aux niveaux suivants:  
  
-   **Sécurité des informations.** Journaux d’audit l’accès aux fichiers est souvent utilisés pour la détection des intrusions et analyse investigation. Les organisations étant en mesure d’obtenir les événements ciblés sur l’accès aux informations précieuses nous allons améliorent considérablement leur exactitude enquête et les temps de réponse.  
  
-   **Stratégie organisationnelle.** Par exemple, les organisations assujetties aux normes PCI peuvent avoir une stratégie centrale pour surveiller l’accès à tous les fichiers qui sont marqués comme contenant des informations de carte de crédit et des informations d’identification personnelle (PII).  
  
-   **Stratégie de service.** Par exemple, du département Finances peut notamment exiger que la capacité à modifier certains documents financiers (tels qu’un rapport trimestriels) soit limitée à du département finance, ainsi que le service faudrait surveiller toutes les autres tentatives de modification de ces documents.  
  
-   **Stratégie d’entreprise.** Par exemple, les propriétaires voudrez surveiller toutes les tentatives non autorisées pour afficher les données qui appartiennent à leurs projets.  
  
Le service de conformité peut également surveiller toutes les stratégies d’autorisation centralisées et des éléments tels que l’utilisateur, d’ordinateur et attributs de ressource de la stratégie.  
  
Une des considérations essentielles des audits de sécurité est le coût de collecte, de stockage et d’analyse des événements d’audit. Si les stratégies d’audit sont trop vagues, le volume d’événements d’audit collectés s’accroît, et cela augmente les coûts. Si les stratégies d’audit sont trop limitées, vous risquez de manquer des événements importants.  
  
Avec Windows Server2012, vous pouvez créer des stratégies d’audit à l’aide de revendications et les propriétés de ressource. Cela conduit à des stratégies d’audit plus riches, davantage ciblées et plus facile à gérer. Il permet des scénarios qui jusqu'à présent étaient impossibles ou trop difficiles à effectuer. Voici quelques exemples de stratégies d’audit que les administrateurs peuvent créer:  
  
-   Auditer tous les utilisateurs qui ne dispose pas d’un jeu de haute sécurité et tente d’accéder aux documents HBI. Par exemple, auditer | Tout le monde | All-Access | Resource.BusinessImpact=HBI et User.SecurityClearance!=High.  
  
-   Auditer tous les fournisseurs lorsqu’ils essaient d’accéder aux documents qui sont associés à des projets qu’ils ne travaillent pas. Par exemple, auditer | Tout le monde | All-Access | User et User.Project Not_AnyOf Resource.Project.  
  
Ces stratégies permettent de normaliser le volume d’événements d’audit et de les limiter uniquement les données les plus importantes ou les utilisateurs.  
  
Une fois que les administrateurs ont créé et appliqué les stratégies d’audit, le point suivant est gleaning des informations significatives à partir des événements d’audit qui ils collectées. Événements d’audit basées sur l’expression de réduire le volume d’audits. Toutefois, les utilisateurs ont besoin d’un moyen pour rechercher des informations significatives dans ces événements et de poser des questions telles que «qui accède à mes données HBI?» Ou «Y A-t-il une tentative non autorisée d’accéder aux données sensibles?»  
  
 Windows Server2012 améliore les événements d’accès aux données existants avec les revendications d’utilisateur, ordinateur et des ressources. Ces événements sont générés par serveur. Pour fournir un affichage complet des événements dans l’organisation, Microsoft collabore avec des partenaires pour fournir la collecte d’événements et des outils d’analyse, tels que les Services ACS dans SystemCenter Operations Manager.  
  
La figure4 montre une vue d’ensemble de la stratégie d’audit centralisée.  
  
![guides de solutions](media/Scenario--File-Access-Auditing/DynamicAccessControl_RevGuide_4.JPG)  
  
**Figure4** expérience d’audit centralisée  
  
Et la consommation d’audits de sécurité généralement impliquent les étapes générales suivantes:  
  
1.  Identifier l’ensemble correct de données et les utilisateurs à surveiller  
  
2.  Créer et appliquer des stratégies d’audit appropriées  
  
3.  Collecter et analyser les événements d’audit  
  
4.  Gérer et surveiller les stratégies qui ont été créés  
  
## <a name="in-this-scenario"></a>Dans ce scénario  
Les rubriques suivantes fournissent des conseils supplémentaires pour ce scénario:  
  
-   [Planifier le fichier de l’audit d’accès](Plan-for-File-Access-Auditing.md)  
  
-   [Déployer l’audit de sécurité avec les stratégies d’Audit centralisées et #40; étapes de démonstration et #41;](Deploy-Security-Auditing-with-Central-Audit-Policies--Demonstration-Steps-.md)  
  
## <a name="BKMK_NEW"></a>Fonctionnalités et rôles inclus dans ce scénario  
Le tableau suivant répertorie les rôles et fonctionnalités qui font partie de ce scénario et décrit la prise en charge.  
  
|Rôle ou une fonctionnalité|Comment il prend en charge ce scénario|  
|-----------------|---------------------------------|  
|Rôle des Services de domaine ActiveDirectory|Les services ADDS dans Windows Server2012 introduit une plateforme d’autorisation basée sur les revendications qui permet de créer des revendications d’utilisateur et revendications de périphérique, une identité composée, (utilisateur plus revendications de périphérique), nouveau modèle accès centralisées (CAP) les stratégies et l’utilisation des informations de classification des fichiers dans les décisions d’autorisation.|  
|Rôle Services de fichiers et stockage|Serveurs de fichiers dans Windows Server2012 fournissent une interface utilisateur où les administrateurs peuvent afficher les autorisations effectives pour les utilisateurs pour un fichier ou dossier et résoudre les problèmes d’accès et accorder l’accès en fonction des besoins.|  
  


