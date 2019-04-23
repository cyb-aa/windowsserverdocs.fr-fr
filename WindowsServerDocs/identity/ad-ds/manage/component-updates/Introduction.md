---
ms.assetid: 84754c23-f039-4de4-a378-853942e662df
title: Introduction
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 8e2717af6183944b26a71e55b36f31cef51cf2e7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831870"
---
# <a name="introduction"></a>Introduction

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Les attaques contre les infrastructures informatiques, simples ou complexes, aurait tant que disposent d’ordinateurs. Toutefois, au cours de la dernière décennie, un nombre croissant d’organisations de toutes tailles situées dans toutes les régions du monde ont été attaquées et compromises avec des méthodes qui ont considérablement changé le paysage des menaces. La guerre de l’information et le cybercrime ont augmenté à des taux record. « Hacktivism », dans lequel les attaques sont motivées par activistes, a été déclarée comme la motivation pour un nombre de violations prévu pour exposer des informations secrètes des organisations, pour créer des refus de service, ou même de détruire l’infrastructure. Les attaques contre les institutions publiques et privées dans le but de dévoiler les droits de propriété intellectuelle (IP) les organisations sont devenues omniprésentes.  
  
Aucune organisation dotée d’une infrastructure informatique (IT) n’est immune contre les attaques, mais si les contrôles, les processus et les stratégies appropriées sont implémentées pour protéger les principales composantes de l’infrastructure informatique d’une organisation, une augmentation des attaques à partir de pénétration pour terminer la compromission peut être étaient évitable. Étant donné que le nombre et la mise à l’échelle des attaques provenant de l’extérieur d’une organisation a éclipsant menace interne dans ces dernières années, ce document aborde souvent des attaquants externes plutôt que d’une mauvaise utilisation de l’environnement par les utilisateurs autorisés. Néanmoins, les principes et les recommandations fournies dans ce document visent à aider à sécuriser votre environnement contre les intrus externes et le personnel malveillant ou malavisé.  
  
Les informations et les recommandations fournies dans ce document sont tirées de différentes sources et dérivées de pratiques conçues pour protéger les installations Active Directory contre toute compromission. Bien qu’il n’est pas possible d’empêcher les attaques, il est possible pour réduire la surface d’attaque Active Directory et d’implémenter des contrôles facilitant la compromission du répertoire beaucoup plus difficile pour les attaquants. Ce document présente les types courants de vulnérabilités, que nous avons observé dans les environnements compromis et les recommandations les plus courantes que nous avons apporté aux clients pour améliorer la sécurité de leurs installations d’Active Directory.  
  
## <a name="account-and-group-naming-conventions"></a>Conventions d’attribution de noms de groupe et de compte  
Le tableau suivant fournit un guide pour les conventions d’affectation de noms utilisées dans ce document pour les groupes et les comptes référencés dans tout le document. Inclus dans la table est l’emplacement de chaque compte/groupe, son nom, et comment ces comptes/groupes sont référencés dans ce document.  
  


|**Groupe de comptes/emplacement**|**Nom de compte/groupe**|**Comment il est référencé dans ce Document**|
| --- | --- | --- |   
|Active Directory - chaque domaine|Administrateur|Compte administrateur intégré|  
|Active Directory - chaque domaine|Administrateurs|Groupe des administrateurs (BA) intégré|  
|Active Directory - chaque domaine|Administrateurs du domaine|Groupe des administrateurs (DA) de domaine|  
|Active Directory - domaine racine de forêt|Administrateurs de l’entreprise|Groupe d’administrateurs de l’entreprise (EA)|  
|Ordinateur local sécurité comptes SAM (gestionnaire) base de données sur les ordinateurs exécutant Windows Server et les stations de travail qui ne sont pas des contrôleurs de domaine|Administrateur|Compte d’administrateur local|  
|Ordinateur local sécurité comptes SAM (gestionnaire) base de données sur les ordinateurs exécutant Windows Server et les stations de travail qui ne sont pas des contrôleurs de domaine|Administrateurs|Groupe Administrateurs local|  
  
## <a name="about-this-document"></a>À propos de ce Document  
L’organisation Microsoft Information Security and Risk Management (ISRM), qui fait partie de Microsoft Information Technology (MSIT), fonctionne avec les divisions interne aux clients externes et leurs homologues rassembler, diffuser, et définir des stratégies pratiques et contrôles. Ces informations peuvent être utilisées par Microsoft et nos clients pour renforcer la sécurité et de réduire la surface d’attaque de leurs infrastructures informatiques. Les recommandations fournies dans ce document reposent sur un certain nombre de sources d’informations pratiques en usage dans MSIT et ISRM. Les sections suivantes présentent plus d’informations sur les origines de ce document.  
  
### <a name="microsoft-it-and-isrm"></a>Microsoft IT et ISRM  
Un nombre de pratiques et les contrôles ont été développé dans MSIT et ISRM pour sécuriser les domaines et forêts Microsoft AD DS. Lorsque ces contrôles sont largement applicables, ils ont été intégrées à ce document. SAFE-T (accélérateurs de solutions pour les Technologies émergentes) est une équipe au sein de ISRM dont la tâche consiste à identifier les technologies émergentes et pour définir les exigences de sécurité et contrôles afin d’accélérer leur adoption.  
  
### <a name="active-directory-security-assessments"></a>Évaluations de sécurité Active Directory  
Au sein de Microsoft ISRM, l’évaluation, Consulting et équipe Engineering (ACE) fonctionne avec les divisions Microsoft internes et des clients externes pour évaluer la sécurité d’application et l’infrastructure et de fournir des conseils tactiques et stratégiques pour augmenter le posture de sécurité de l’organisation. Une offre de service ACE est l’Active Directory Security Assessment (ADSA), qui est une évaluation globale de l’environnement de AD DS d’une organisation qui évalue les personnes, processus et technologie et génère des recommandations spécifiques au client. Les clients sont fournis avec des recommandations sont basées sur les caractéristiques uniques de l’organisation, les pratiques et au goût du risque. ADSAs ont été effectuées pour les installations Active Directory chez Microsoft en plus de celles de nos clients. Au fil du temps, un nombre de recommandations ont été trouvé pour être applicable entre les clients de différentes tailles et les secteurs d’activité.  
  
### <a name="content-origin-and-organization"></a>Organisation et origine du contenu  
La majorité du contenu de ce document est dérivée de la ADSA et autres évaluations de l’équipe ACE effectuées pour les clients ayant rencontré pas compromis significatives et de clients compromis. Bien que les données client n’était pas utilisées pour créer ce document, nous avons rassemblé les plus courantes vulnérabilités nous avons identifié dans nos évaluations et les recommandations, nous avons apporté aux clients pour améliorer la sécurité de leurs services AD DS installations. Toutes les vulnérabilités ne sont pas applicables à l’ensemble des environnements et il n’est pas possible d’implémenter tous les recommandations dans chaque organisation.  
  
Ce document est organisé comme suit :  
  
## <a name="executive-summary"></a>Résumé  
L’Executive résumé, qui peut être lu en tant que document autonome ou en association avec le document complet, fournit une synthèse de haut niveau de ce document. Inclus dans le résumé Executive sont des vecteurs d’attaque les plus courants que nous avons observé utilisés pour compromettre des environnements client, résumées recommandations pour sécuriser les installations d’Active Directory, ainsi que les objectifs de base pour les clients qui envisagent de déployer de nouveaux services AD DS forêts maintenant ou à l’avenir.  
  
### <a name="introduction"></a>Introduction  
Il s’agit de la section que vous lisez.  
  
### <a name="avenues-to-compromise"></a>Voies de compromis  
Cette section fournit des informations sur certaines des exploité couramment des vulnérabilités que nous ayons trouvée pour être utilisé par les pirates pour compromettre les infrastructures clients. Cette section commence par catégories générales de vulnérabilités et comment ils sont optimisés pour pénétrer initialement des infrastructures clients, propager compromission sur d’autres systèmes et finalement cibles AD DS et contrôleurs de domaine pour obtenir complète contrôle des forêts de l’organisation.  
  
Cette section ne fournit pas de recommandations détaillées sur l’adressage de chaque type de vulnérabilité, en particulier dans les domaines dans lequel les vulnérabilités ne sont pas utilisées pour cibler directement Active Directory. Toutefois, pour chaque type de vulnérabilité, nous avons compilé les liens vers des informations supplémentaires que vous pouvez utiliser pour développer des contre-mesures et réduire la surface d’attaque de votre organisation.  
  
### <a name="reducing-the-active-directory-attack-surface"></a>Réduire la Surface d'attaque Active Directory  
Cette section commence en fournissant des informations générales sur les comptes privilégiés et groupes dans Active Directory pour fournir les informations que vous aide à clarifier les raisons pour les recommandations suivantes pour la sécurisation et la gestion des groupes privilégiés et comptes. Nous présentons ensuite les approches afin de réduire la nécessité d’utiliser des comptes à privilèges élevés pour l’administration quotidienne, ce qui ne nécessite pas le niveau de privilège est accordé aux groupes tels que les administrateurs d’entreprise (EA), Admins du domaine (DA) et intégré Groupes d’administrateurs (BA) dans Active Directory. Ensuite, nous vous fournissons des conseils pour sécuriser les comptes et groupes disposant de privilèges et pour l’implémentation des systèmes et des méthodes d’administration sécurisées.  
  
Bien que cette section fournit des informations détaillées sur ces paramètres de configuration, nous avons également inclus les annexes de chaque recommandation qui fournissent des instructions pas à pas de configuration qui peuvent être utilisé « en l’état », ou peuvent être modifiées pour le besoins de l’organisation. Cette section se termine en fournissant des informations pour déployer et gérer les contrôleurs de domaine, qui doivent être entre les systèmes plus stricte sécurisés dans l’infrastructure en toute sécurité.  
  
### <a name="monitoring-active-directory-for-signs-of-compromise"></a>Surveillance des signes de compromission d'Active Directory  
Si vous avez implémenté des événements dans votre environnement de surveillance (SIEM) et des informations de sécurité robuste ou autres mécanismes pour surveiller la sécurité de l’infrastructure, cette section fournit des informations qui peuvent être utilisées pour identifier les événements sur Windows systèmes susceptibles d’indiquer qu’une organisation est attaquée. Nous abordons les stratégies audit traditionnelles et avancées, notamment une configuration efficace des sous-catégories d’audit dans les systèmes d’exploitation Windows 7 et Windows Vista. Cette section inclut une liste complète des objets et des systèmes à auditer, et une annexe associée répertorie les événements pour lesquels vous devez surveiller si l’objectif est de détecter les tentatives de compromission.  
  
### <a name="planning-for-compromise"></a>Planification des compromis  
Cette section commence par « recul » à partir des détails techniques pour vous concentrer sur les principes et les processus qui peuvent être implémentées pour identifier les utilisateurs, les applications et les systèmes qui ne sont plus critiques non seulement pour l’infrastructure informatique, mais pour l’entreprise. Après avoir identifié ce qui est essentiel pour la stabilité et les opérations de votre organisation, vous concentrer sur séparant et sécurisation de ces ressources, qu’ils soient des droits de propriété intellectuelle, des personnes ou des systèmes. Dans certains cas, séparant les sécurisation des ressources ne peuvent être exécutés dans votre environnement AD DS existant, tandis que dans d’autres cas, vous devez envisager d’implémenter « cells » petits, distincts qui vous permettent d’établir une limite sécurisée autour des ressources critiques et les surveiller ressources plus stricte que les composants moins critiques. Un concept appelé « destruction creative », qui est un mécanisme par lequel les systèmes et applications héritées peuvent être éliminées en créant de nouvelles solutions est décrite et met fin à la section avec des recommandations qui peuvent contribuer à maintenir un environnement plus sécurisé par combinaison des informations commerciales et informatiques pour construire une vue détaillée de ce qui est un état de fonctionnement normal. En sachant que ce qui est normal pour une organisation, les anomalies pouvant indiquer des attaques et des compromis peuvent être plus facilement identifiés.  
  
### <a name="summary-of-best-practice-recommendations"></a>Résumé des recommandations de meilleures pratiques  
Cette section fournit une table qui résume les recommandations formulées dans ce document et les trie par priorité relative, en plus de fournir des liens vers où vous trouverez plus d’informations sur chaque recommandation dans le document et ses annexes.  
  
### <a name="appendices"></a>Annexes  
Annexes sont inclus dans ce document pour transférer les informations contenues dans le corps du document. La liste des annexes et une brève description de chacun d’eux est incluse le tableau suivant.  
  
 
|**Annexe**|**Description**|
| --- | --- | 
|[Annexe b : Comptes privilégiés et groupes dans Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-B--Privileged-Accounts-and-Groups-in-Active-Directory.md)|Fournit des informations générales qui vous aident à identifier les utilisateurs et groupes, que vous devez vous concentrer sur la sécurisation, car ils peuvent être exploitées par des attaquants de compromettre et même détruire votre installation Active Directory.|  
|[Annexe C : Comptes protégés et groupes dans Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)|Contient des informations sur les groupes protégés dans Active Directory. Il contient également des informations pour une personnalisation limitée (suppression) des groupes qui sont considérés comme des groupes protégés et sont affectés par AdminSDHolder et SDProp.|  
|[Annexe d : Sécurisation des comptes d’administrateur intégré dans Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)|Contient des instructions pour aider à sécuriser le compte d’administrateur dans chaque domaine dans la forêt.|  
|[Annexe e : Sécurisation des groupes administrateurs d’entreprise dans Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory.md)|Contient des instructions pour aider à sécuriser le groupe Administrateurs de l’entreprise dans la forêt.|  
|[Annexe f : Sécurisation des groupes d’administrateurs de domaine dans Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory.md)|Contient des instructions pour aider à sécuriser le groupe Admins du domaine dans chaque domaine dans la forêt.|  
|[Annexe g : Sécurisation des groupes d’administrateurs dans Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-G--Securing-Administrators-Groups-in-Active-Directory.md)|Contient des instructions pour aider à sécuriser le groupe Administrateurs intégré dans chaque domaine dans la forêt.|  
|[Appendix h : Sécurisation des comptes des administrateurs locaux et groupes](../../../ad-ds/plan/security-best-practices/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups.md)|Contient des instructions pour aider à sécuriser les comptes administrateur locaux et groupes d’administrateurs sur les stations de travail et les serveurs joints au domaine.|  
|[Annexe i : Création de gestion de comptes pour les comptes protégés et groupes dans Active Directory](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md)|Fournit des informations pour créer des comptes disposant de privilèges limités et peuvent être rigoureusement contrôlées, mais peuvent être utilisées pour remplir des groupes privilégiés dans Active Directory lors de l’élévation temporaire est nécessaire.|  
|[Annexe l : Événements à surveiller](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md)|Répertorie les événements pour lesquels vous devez surveiller dans votre environnement.|  
|[Annexe m : Liens vers des documents et lecture recommandée](../../../ad-ds/manage/Appendix-M--Document-Links-and-Recommended-Reading.md)|Contient une liste de lecture recommandée. Contient également une liste de liens vers des documents externes et leurs URL afin que les lecteurs de copies papier de ce document peuvent accéder à ces informations.|  
  


