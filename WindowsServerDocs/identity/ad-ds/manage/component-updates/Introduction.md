---
ms.assetid: 84754c23-f039-4de4-a378-853942e662df
title: Introduction
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: dc89afc47eb78a388238e8edf5059b0bec3006ad
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="introduction"></a>Introduction

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Les attaques contre les infrastructures informatiques, simple ou complexe, ont existé tant que les ordinateurs ont. Toutefois, dans la dernière décennie, un nombre croissant d’organisations de toutes tailles, dans toutes les parties du monde ont été attaqué et compromis avec des méthodes qui ont considérablement changé le paysage des menaces. La guerre et le cybercrime ont augmenté à des taux record. «Hacktivism», dans lequel les attaques sont motivées par activistes, a été déclarée comme la motivation principale d’un certain nombre de violations prévu exposer les informations secrètes des organisations, pour créer des refus de service, ou même de détruire l’infrastructure. Les attaques contre les institutions publiques et privées avec l’objectif de dévoiler la propriété intellectuelle des organisations (IP) sont.  
  
Aucune organisation dotée d’une infrastructure informatique (IT) n’est protégée contre les attaques, mais si les contrôles, les processus et les stratégies appropriées sont implémentés pour protéger les segments de clé de l’infrastructure informatique d’une organisation, une augmentation des attaques de pénétration compromission complète peut-être inévitables. Étant donné que le nombre et la montée en puissance parallèle d’attaques provenant de l’extérieur d’une organisation a éclipsant menace dans les dernières années, ce document décrit souvent les intrus externes au lieu d’une mauvaise utilisation de l’environnement par les utilisateurs autorisés. Néanmoins, les principes et les recommandations fournies dans ce document sont destinées à vous aider à sécuriser votre environnement contre les intrus externes et malveillant ou malavisé.  
  
Les informations et les recommandations fournies dans ce document sont tirées de différentes sources et dérivées de pratiques conçues pour protéger les installations ActiveDirectory contre toute compromission. Bien qu’il n’est pas possible d’empêcher les attaques, il est possible pour réduire la surface d’attaque ActiveDirectory et à implémenter des contrôles qui rendent la compromission du répertoire beaucoup plus difficile pour les pirates. Ce document présente les types courants de vulnérabilités, que nous avons observé dans les environnements compromis et les recommandations les plus courantes, que nous avons apporté aux clients afin d’améliorer la sécurité de leur installation ActiveDirectory.  
  
## <a name="account-and-group-naming-conventions"></a>Conventions d’attribution de noms de groupe et de compte  
Le tableau suivant fournit un guide aux conventions d’affectation de noms utilisés dans ce document pour les groupes et les comptes référencés dans le document. Inclus dans le tableau est l’emplacement de chaque groupe/compte, son nom, et comment ces comptes/groupes sont référencés dans ce document.  
  


|**Emplacement/groupe compte**|**Nom du compte/groupe**|**Comment il est référencé dans ce Document**|
| --- | --- | --- |   
|ActiveDirectory - chaque domaine|Administrateur|Compte administrateur intégré|  
|ActiveDirectory - chaque domaine|Administrateurs|Groupe d’administrateurs (BA) intégré|  
|ActiveDirectory - chaque domaine|Admins du domaine|Groupe d’administrateurs (DA) de domaine|  
|ActiveDirectory - domaine racine de forêt|Administrateurs de l’entreprise|Groupe Administrateurs de l’entreprise (EA)|  
|Base de données ordinateur local sécurité comptes manager (SAM) sur les ordinateurs exécutant Windows Server et les stations de travail qui ne sont pas des contrôleurs de domaine|Administrateur|Compte d’administrateur local|  
|Base de données ordinateur local sécurité comptes manager (SAM) sur les ordinateurs exécutant Windows Server et les stations de travail qui ne sont pas des contrôleurs de domaine|Administrateurs|Groupe Administrateurs local|  
  
## <a name="about-this-document"></a>À propos de ce Document  
Le MicrosoftInformation Security organisation risque Management (ISRM), qui fait partie de Microsoft informations technologie (MSIT), fonctionne avec des unités commerciales interne, des clients externes et homologues du secteur pour rassembler, diffuser et définir des stratégies, pratiques et des contrôles. Ces informations peuvent être utilisées par Microsoft et de nos clients à renforcer la sécurité et de réduire la surface d’attaque de leurs infrastructures informatiques. Les recommandations fournies dans ce document sont basées sur un nombre de sources d’informations et les pratiques en usage dans MSIT et ISRM. Les sections suivantes présentent plus d’informations sur l’origine de ce document.  
  
### <a name="microsoft-it-and-isrm"></a>MicrosoftIT et ISRM  
Un nombre de pratiques et les contrôles ont été développé dans MSIT et ISRM pour sécuriser les forêts MicrosoftADDS et les domaines. Lorsque ces contrôles sont applicables à grande échelle, ils ont été intégrées à ce document. SAFE-T (accélérateurs de Solution pour les Technologies émergentes) est une équipe au sein de ISRM dont sert à identifier les technologies émergentes et pour définir les exigences de sécurité et des contrôles pour accélérer leur adoption.  
  
### <a name="active-directory-security-assessments"></a>Évaluations de sécurité ActiveDirectory  
Au sein de MicrosoftISRM, l’évaluation, conseils et l’équipe Ingénierie (ACE) fonctionne avec des unités commerciales Microsoft internes et des clients externes pour évaluer la sécurité de l’infrastructure et des applications et de fournir des conseils tactiques et stratégiques pour augmenter la posture de sécurité de l’organisation. Une offre de service ACE est l’ActiveDirectory sécurité évaluation (ADSA), qui est une évaluation globale de l’environnement de domaine ActiveDirectory d’une organisation qui évalue les personnes, des processus et des technologies et génère des recommandations spécifiques au client. Les clients sont fournis avec des recommandations basées sur les caractéristiques uniques de l’organisation, les pratiques et goût du risque. ADSAs ont été effectuées pour les installations ActiveDirectory de Microsoft en plus de celles de nos clients. Au fil du temps, un certain nombre de recommandations ont été trouvé être applicables à des clients de différentes tailles et des secteurs.  
  
### <a name="content-origin-and-organization"></a>Organisation et origine du contenu  
La majorité du contenu de ce document est dérivée de la ADSA et autres évaluations de l’équipe ACE effectuées pour les clients compromis et de clients ayant rencontré pas compromis importantes. Bien que les données de chaque client n’a pas été utilisées pour créer ce document, nous avons recueilli les plus couramment exploitées vulnérabilités que nous avons identifié dans nos évaluations et les recommandations, nous avons apporté aux clients afin d’améliorer la sécurité de leur installation ADDS. Pas de toutes les vulnérabilités sont applicables à tous les environnements, et ne sont pas toutes les recommandations possible d’implémenter dans chaque organisation.  
  
Ce document est organisé comme suit:  
  
## <a name="executive-summary"></a>Résumé  
La direction résumé, qui peut être lu en tant que document autonome ou en combinaison avec le document complet, fournit un résumé de haut niveau de ce document. Inclus dans le résumé exécutif sont les vecteurs d’attaque les plus courants que nous avons observé utilisés pour compromettre des environnements client, résumées recommandations pour la sécurisation des installations d’ActiveDirectory et les objectifs de base pour les clients qui envisagent de déployer de nouvelles forêts ADDS maintenant ou ultérieurement.  
  
### <a name="introduction"></a>Introduction  
Il s’agit de la section que vous lisez.  
  
### <a name="avenues-to-compromise"></a>Voies de compromis  
Cette section fournit des informations sur certaines des plus exploitées couramment vulnérabilités que nous avons identifié pour être utilisées par les personnes malveillantes pour compromettre des infrastructures de clients. Cette section contient des catégories générales des vulnérabilités et comment ils sont optimisés pour initialement malgré les infrastructures clients, se propage compromis sur des systèmes supplémentaires et finalement cibler ADDS et contrôleurs de domaine pour obtenir le contrôle intégral des forêts des organisations.  
  
Cette section ne fournit pas de recommandations détaillées sur l’adressage de chaque type de vulnérabilité, en particulier dans les domaines dans lesquels les vulnérabilités ne servent pas à cibler directement ActiveDirectory. Toutefois, pour chaque type de vulnérabilité, nous avons fourni des liens vers des informations supplémentaires que vous pouvez utiliser pour développer des contre-mesures et réduire la surface d’attaque de votre organisation.  
  
### <a name="reducing-the-active-directory-attack-surface"></a>Ce qui réduit la Surface d’attaque ActiveDirectory  
Cette section commence en fournissant des informations générales sur les comptes privilégiés et groupes dans ActiveDirectory pour fournir les informations qui vous permettent de préciser les raisons pour les recommandations suivantes pour la sécurisation et la gestion des comptes et groupes privilégiés. Ensuite, nous abordons les approches pour réduire la nécessité d’utiliser des comptes dotés de privilèges élevés pour l’administration quotidienne, ne requiert pas le niveau de privilège est accordé aux groupes tels que les groupes Administrateurs de l’entreprise (EA), Admins du domaine (DA) et les administrateurs intégrés (BA) dans ActiveDirectory. Ensuite, nous fournissons des indications pour sécuriser les comptes et groupes privilégiés et pour l’implémentation de systèmes et des pratiques d’administration sécurisées.  
  
Bien que cette section fournit des informations détaillées sur ces paramètres de configuration, nous avons également inclus annexes pour chaque recommandation qui fournissent des instructions pas à pas de configuration qui peuvent être utilisés» en tant qu’est» ou peuvent être modifiées pour les besoins de l’organisation. Cette section se termine en fournissant des informations pour déployer et gérer les contrôleurs de domaine, qui doivent être entre les systèmes plus stricte sécurisés dans l’infrastructure en toute sécurité.  
  
### <a name="monitoring-active-directory-for-signs-of-compromise"></a>Surveillance des signes de compromission d’ActiveDirectory  
Si vous avez implémenté des événements (SIEM) dans votre environnement d’analyse et les informations de sécurité fiable ou sont à l’aide des autres mécanismes pour surveiller la sécurité de l’infrastructure, cette section fournit des informations qui peuvent être utilisées pour identifier les événements sur les systèmes Windows peuvent indiquer qu’une organisation est attaquée. Nous abordons les stratégies d’audit traditionnelles et avancées, notamment la configuration des sous-catégories d’audit en vigueur dans les systèmes d’exploitation Windows7 et WindowsVista. Cette section comprend la liste complète des objets et les systèmes d’auditer et une annexe associée répertorie les événements pour lesquels vous devez analyser si l’objectif est de détecter les tentatives de compromission.  
  
### <a name="planning-for-compromise"></a>Planification des compromis  
Cette section commence par «recul» à partir des détails techniques pour vous concentrer sur les principes et les processus qui peuvent être implémentées pour identifier les utilisateurs, les applications et les systèmes qui ne sont plus critiques non seulement pour l’infrastructure informatique, mais à l’entreprise. Après avoir identifié ce qui est essentiel à la stabilité et les opérations de votre organisation, vous concentrer sur séparation et la sécurisation de ces ressources, qu’ils soient des systèmes, des personnes ou des droits de propriété intellectuelle. Dans certains cas, la séparation et la sécurisation des ressources peuvent être effectuées dans votre environnement de domaine ActiveDirectory existant, alors que dans d’autres cas, vous devez envisager de «cellules «petite taille, distincts qui permettent d’établir une limite sécurisée autour de ressources critiques et d’analyser ces ressources plus stricte que les composants moins critiques. Un concept appelé «destruction creative», qui est un mécanisme par lequel des systèmes et applications héritées peuvent être éliminés en créant de nouvelles solutions est abordé, et à la fin de la section contenant des recommandations qui permettent de gérer un environnement plus sécurisé en combinant des informations commerciales et informatiques pour construire une image détaillée de ce qui est un état de fonctionnement normal. Grâce à ce qui est normal d’une organisation, les anomalies susceptibles d’indiquer les attaques et les compromis peuvent être identifiés plus facilement.  
  
### <a name="summary-of-best-practice-recommendations"></a>Résumé des recommandations  
Cette section fournit une table qui récapitule les recommandations formulées dans ce document et les commandes par priorité relative, en plus de fournir des liens vers où plus d’informations sur chaque recommandation peut être trouvé dans le document et ses annexes.  
  
### <a name="appendices"></a>Annexes  
Annexes sont inclus dans ce document pour compléter les informations contenues dans le corps du document. La liste des annexes et une brève description de chaque est incluse le tableau suivant.  
  
 
|**Annexe**|**Description**|
| --- | --- | 
|[Annexe b: comptes privilégiés et groupes dans ActiveDirectory](../../../ad-ds/plan/security-best-practices/Appendix-B--Privileged-Accounts-and-Groups-in-Active-Directory.md)|Fournit des informations en arrière-plan qui vous aide à identifier les utilisateurs et groupes que vous devez vous concentrer sur la sécurisation, car ils peuvent être exploitées par des personnes malveillantes de compromettre et même détruire votre installation d’Active Directory.|  
|[Annexe c: comptes protégés et groupes dans ActiveDirectory](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)|Contient des informations sur les groupes protégés dans Active Directory. Il contient également des informations pour la personnalisation limitée (suppression) des groupes qui sont considérés comme des groupes protégés et sont affectés par AdminSDHolder et SDProp.|  
|[Annexe d: sécurisation des comptes d’administrateur intégrés dans ActiveDirectory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)|Contient des instructions pour vous aider à sécuriser le compte d’administrateur dans chaque domaine dans la forêt.|  
|[Annexe e: sécurisation des groupes d’administrateurs de l’entreprise dans ActiveDirectory](../../../ad-ds/plan/security-best-practices/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory.md)|Contient des instructions pour vous aider à sécuriser le groupe Administrateurs de l’entreprise dans la forêt.|  
|[Annexe f: sécurisation des groupes d’administrateurs de domaine dans ActiveDirectory](../../../ad-ds/plan/security-best-practices/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory.md)|Contient des instructions pour vous aider à sécuriser le groupe Admins du domaine dans chaque domaine dans la forêt.|  
|[Annexeg: Sécurisation des groupes d’administrateurs dans ActiveDirectory](../../../ad-ds/plan/security-best-practices/Appendix-G--Securing-Administrators-Groups-in-Active-Directory.md)|Contient des instructions pour vous aider à sécuriser le groupe Administrateurs intégré dans chaque domaine dans la forêt.|  
|[Annexeh: Sécurisation des comptes administrateur locaux et groupes](../../../ad-ds/plan/security-best-practices/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups.md)|Contient des instructions pour aider à sécuriser les comptes administrateur locaux et groupes d’administrateurs sur les stations de travail et serveurs joints au domaine.|  
|[AnnexeI Création de gestion des comptes pour les comptes protégés et groupes dans ActiveDirectory](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md)|Fournit des informations pour créer des comptes qui disposent de privilèges limités et peuvent être contrôlés stricte, mais peuvent être utilisées pour remplir des groupes privilégiés dans Active Directory quand l’élévation temporaire est requise.|  
|[Annexel: Événements à surveiller](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md)|Répertorie les événements pour lesquels vous devez surveiller dans votre environnement.|  
|[Annexe m: liens vers des documents et lecture recommandée](../../../ad-ds/manage/Appendix-M--Document-Links-and-Recommended-Reading.md)|Contient une liste de lecture recommandée. Contient également une liste de liens vers des documents externes et leurs URL afin que les lecteurs de papier de ce document peuvent accéder à ces informations.|  
  


