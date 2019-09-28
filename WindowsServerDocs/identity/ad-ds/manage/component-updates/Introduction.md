---
ms.assetid: 84754c23-f039-4de4-a378-853942e662df
title: Présentation
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: ab21cca727342f6dc69ceecfb0c8991b30b0f227
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71389880"
---
# <a name="introduction"></a>Présentation

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Les attaques contre les infrastructures informatiques, qu’elles soient simples ou complexes, ont existé tant que les ordinateurs disposent de. Toutefois, au cours de la dernière décennie, un nombre croissant d’organisations de toutes tailles situées dans toutes les régions du monde ont été attaquées et compromises avec des méthodes qui ont considérablement changé le paysage des menaces. La guerre de l’information et le cybercrime ont augmenté à des taux record. « Hacktivism », dans laquelle les attaques sont motivées par des positions opinions activistes, a été allégué comme motivation pour un certain nombre de violations destinées à exposer les informations confidentielles des organisations, à créer des refus de service ou même à détruire l’infrastructure. Les attaques contre les institutions publiques et privées avec l’objectif de dévoiler la propriété intellectuelle (IP) de l’organisation sont omniprésentes.  
  
Aucune organisation dotée d’une infrastructure informatique n’est protégée contre les attaques, mais si des stratégies, des processus et des contrôles appropriés sont implémentés pour protéger les segments clés de l’infrastructure informatique d’une organisation, la remontée des attaques à partir de la pénétration pour une compromission complète peut être empêchable. Étant donné que le nombre et la mise à l’échelle des attaques provenant de l’extérieur d’une organisation ont mis en place une menace Insider dans les années récentes, ce document présente souvent des attaquants externes, et non une mauvaise utilisation de l’environnement par des utilisateurs autorisés. Néanmoins, les principes et recommandations fournis dans ce document visent à vous aider à sécuriser votre environnement contre les attaquants externes et mal intentionnés ou malveillants.  
  
Les informations et les recommandations fournies dans ce document sont tirées d’un certain nombre de sources et dérivées de pratiques conçues pour protéger les installations Active Directory contre les compromissions. Bien qu’il ne soit pas possible d’empêcher les attaques, il est possible de réduire la surface d’attaque Active Directory et d’implémenter des contrôles qui rendent la compromission de l’annuaire bien plus difficile pour les attaquants. Ce document présente les types de vulnérabilités les plus courants que nous avons observés dans les environnements compromis et les recommandations les plus courantes que nous avons apportées aux clients pour améliorer la sécurité de leurs installations Active Directory.  
  
## <a name="account-and-group-naming-conventions"></a>Conventions d’affectation des noms de comptes et de groupes  
Le tableau suivant fournit un guide pour les conventions d’affectation de noms utilisées dans ce document pour les groupes et les comptes référencés dans tout le document. L’emplacement de chaque compte/groupe, son nom et la manière dont ces comptes/groupes sont référencés dans ce document sont inclus dans la table.  
  


|**Emplacement du compte/groupe**|**Nom du compte/groupe**|**Comment il est référencé dans ce document**|
| --- | --- | --- |   
|Active Directory-chaque domaine|Administrateur|Compte administrateur intégré|  
|Active Directory-chaque domaine|Administrateurs|Groupe Administrateurs intégré (BA)|  
|Active Directory-chaque domaine|Administrateurs du domaine|Groupe Admins du domaine (DA)|  
|Domaine racine de la forêt Active Directory|Administrateurs de l’entreprise|Groupe administrateurs de l’entreprise (EA)|  
|Base de données du gestionnaire de comptes de sécurité (SAM) de l’ordinateur local sur les ordinateurs exécutant Windows Server et les stations de travail qui ne sont pas des contrôleurs de domaine|Administrateur|Compte d’administrateur local|  
|Base de données du gestionnaire de comptes de sécurité (SAM) de l’ordinateur local sur les ordinateurs exécutant Windows Server et les stations de travail qui ne sont pas des contrôleurs de domaine|Administrateurs|Groupe Administrateurs local|  
  
## <a name="about-this-document"></a>À propos de ce document  
L’organisation Microsoft Information Security and Risk Management (ISRM), qui fait partie de Microsoft Information Technology (MSIT), travaille avec les entités internes, les clients externes et les pairs de l’industrie pour rassembler, diffuser et définir des stratégies. pratiques et contrôles. Ces informations peuvent être utilisées par Microsoft et nos clients pour accroître la sécurité et réduire la surface d’attaque de leurs infrastructures informatiques. Les recommandations fournies dans ce document sont basées sur un certain nombre de sources d’informations et de pratiques utilisées dans MSIT et ISRM. Les sections suivantes présentent des informations supplémentaires sur les origines de ce document.  
  
### <a name="microsoft-it-and-isrm"></a>Microsoft IT et ISRM  
Un certain nombre de pratiques et de contrôles ont été développés dans MSIT et ISRM pour sécuriser les forêts et les domaines Microsoft AD DS. Lorsque ces contrôles sont largement applicables, ils ont été intégrés dans ce document. SAFE-T (accélérateurs de solution pour les technologies émergentes) est une équipe de ISRM dont la Charte est d’identifier des technologies émergentes et de définir des exigences et des contrôles de sécurité pour accélérer leur adoption.  
  
### <a name="active-directory-security-assessments"></a>Évaluations de la sécurité Active Directory  
Au sein de Microsoft ISRM, l’équipe d’évaluation, de Conseil et d’ingénierie travaille avec les entités Microsoft internes et les clients externes pour évaluer la sécurité des applications et de l’infrastructure et fournir des conseils tactiques et stratégiques pour augmenter la position de sécurité de l’organisation. Une offre de service ACE est l’évaluation de la sécurité Active Directory (ADSA), qui est une évaluation holistique de l’environnement AD DS d’une organisation qui évalue les personnes, les processus et la technologie et qui produit des recommandations spécifiques au client. Les clients possèdent des recommandations basées sur les caractéristiques, les pratiques et l’appétit des risques propres à l’organisation. Les ADSAs ont été effectuées pour les installations Active Directory chez Microsoft, en plus de celles de nos clients. Au fil du temps, un certain nombre de recommandations ont été détectées pour les clients de tailles et d’industries variables.  
  
### <a name="content-origin-and-organization"></a>Organisation et origine du contenu  
La majeure partie du contenu de ce document est dérivée de la ADSA et d’autres évaluations d’équipe ACE effectuées pour les clients compromis et les clients qui n’ont pas subi de compromission importante. Bien que les données client individuelles n’aient pas été utilisées pour créer ce document, nous avons collecté les vulnérabilités les plus couramment exploitées que nous avons identifiées dans nos évaluations et les recommandations que nous avons apportées aux clients pour améliorer la sécurité de leurs AD DS installation. Toutes les vulnérabilités ne sont pas applicables à l’ensemble des environnements et il n’est pas possible d’implémenter tous les recommandations dans chaque organisation.  
  
Ce document est organisé comme suit :  
  
## <a name="executive-summary"></a>Résumé  
Le résumé, qui peut être lu en tant que document autonome ou en combinaison avec le document complet, fournit une synthèse générale de ce document. Les vecteurs d’attaque les plus courants que nous avons observés pour compromettre les environnements clients, les recommandations récapitulatives pour la sécurisation des installations Active Directory et les objectifs de base pour les clients qui envisagent de déployer de nouveaux AD DS forêts maintenant ou à l’avenir.  
  
### <a name="introduction"></a>Présentation  
Il s’agit de la section que vous lisez maintenant.  
  
### <a name="avenues-to-compromise"></a>Voies de compromis  
Cette section fournit des informations sur les vulnérabilités les plus couramment utilisées par les attaquants pour compromettre les infrastructures des clients. Cette section commence par des catégories générales de vulnérabilités et la façon dont elles sont exploitées pour pénétrer initialement les infrastructures des clients, propager la compromission sur des systèmes supplémentaires et finalement cibler AD DS et les contrôleurs de domaine pour obtenir la totalité contrôle des forêts des organisations.  
  
Cette section ne fournit pas de recommandations détaillées sur la résolution de chaque type de vulnérabilité, en particulier dans les domaines dans lesquels les vulnérabilités ne sont pas utilisées pour cibler directement Active Directory. Toutefois, pour chaque type de vulnérabilité, nous avons fourni des liens vers des informations supplémentaires que vous pouvez utiliser pour développer des contre-mesures et réduire la surface d’attaque de votre organisation.  
  
### <a name="reducing-the-active-directory-attack-surface"></a>Réduire la Surface d'attaque Active Directory  
Cette section commence par fournir des informations générales sur les comptes et les groupes privilégiés dans Active Directory pour fournir les informations qui permettent de clarifier les raisons des recommandations suivantes pour la sécurisation et la gestion des groupes privilégiés et clients. Nous discutons ensuite des approches pour réduire le besoin d’utiliser des comptes à privilèges élevés pour l’administration quotidienne, ce qui ne nécessite pas le niveau de privilège accordé aux groupes tels que les administrateurs de l’entreprise (EA), les administrateurs de domaine (DA) et les Administrateurs (BA) groupes dans Active Directory. Ensuite, nous fournissons des conseils pour sécuriser les groupes et les comptes privilégiés, et pour implémenter des systèmes et des pratiques d’administration sécurisés.  
  
Bien que cette section fournit des informations détaillées sur ces paramètres de configuration, nous avons également inclus des annexes pour chaque recommandation fournissant des instructions de configuration pas à pas qui peuvent être utilisées « en l’or » ou qui peuvent être modifiées pour le besoins de l’organisation. Cette section se termine en fournissant des informations pour déployer et gérer en toute sécurité les contrôleurs de domaine, qui doivent figurer parmi les systèmes les plus sécurisés dans l’infrastructure.  
  
### <a name="monitoring-active-directory-for-signs-of-compromise"></a>Surveillance des signes de compromission d'Active Directory  
Que vous ayez implémenté des informations de sécurité et une surveillance des événements (SIEM) robustes dans votre environnement ou que vous utilisiez d’autres mécanismes pour surveiller la sécurité de l’infrastructure, cette section fournit des informations qui peuvent être utilisées pour identifier les événements sur Windows. systèmes pouvant indiquer qu’une organisation est attaquée. Nous abordons les stratégies d’audit traditionnelles et avancées, notamment la configuration efficace des sous-catégories d’audit dans les systèmes d’exploitation Windows 7 et Windows Vista. Cette section comprend des listes exhaustives d’objets et de systèmes à auditer, et une annexe associée répertorie les événements pour lesquels vous devez surveiller si l’objectif est de détecter les tentatives de compromission.  
  
### <a name="planning-for-compromise"></a>Planification des compromis  
Cette section commence par « pas à pas principal » du détail technique pour se concentrer sur les principes et les processus qui peuvent être implémentés pour identifier les utilisateurs, les applications et les systèmes les plus importants non seulement pour l’infrastructure informatique, mais aussi pour l’entreprise. Après avoir identifié ce qui est le plus important pour la stabilité et les opérations de votre organisation, vous pouvez vous concentrer sur la protection et la sécurisation de ces ressources, qu’il s’agisse de propriété intellectuelle, de personnes ou de systèmes. Dans certains cas, la séparation et la sécurisation des ressources peuvent être effectuées dans votre environnement de AD DS existant. dans d’autres cas, vous devez envisager d’implémenter de petites « cellules » distinctes qui vous permettent d’établir une limite sécurisée autour des ressources critiques et de les surveiller ressources plus rigoureusement que les composants moins critiques. Un concept appelé « destruction créative », qui est un mécanisme qui permet d’éliminer les applications et les systèmes hérités en créant de nouvelles solutions, et la section se termine par des recommandations qui peuvent aider à maintenir un environnement plus sécurisé en combinaison d’informations commerciales et informatiques pour créer une image détaillée de ce qu’est un état opérationnel normal. En sachant ce qui est normal pour une organisation, les anomalies pouvant indiquer des attaques et des compromis peuvent être identifiées plus facilement.  
  
### <a name="summary-of-best-practice-recommendations"></a>Résumé des recommandations relatives aux meilleures pratiques  
Cette section fournit un tableau qui résume les recommandations présentées dans ce document et les classe par priorité relative, en plus de fournir des liens vers les emplacements où se trouvent d’autres informations sur chaque recommandation dans le document et ses annexes.  
  
### <a name="appendices"></a>Annexes  
Les annexes sont incluses dans ce document pour compléter les informations contenues dans le corps du document. La liste des annexes et une brève description de chacune d’elles sont incluses dans le tableau suivant.  
  
 
|**Annexe**|**Description**|
| --- | --- | 
|[Annexe B : Groupes et comptes privilégiés dans Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-B--Privileged-Accounts-and-Groups-in-Active-Directory.md)|Fournit des informations générales qui vous aident à identifier les utilisateurs et les groupes que vous devez vous concentrer sur la sécurisation, car ils peuvent être exploités par des personnes malveillantes pour compromettre et même détruire votre installation Active Directory.|  
|[Annexe C : Groupes et comptes protégés dans Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)|Contient des informations sur les groupes protégés dans Active Directory. Il contient également des informations pour une personnalisation limitée (suppression) des groupes considérés comme des groupes protégés et sont affectés par AdminSDHolder et SDProp.|  
|[Annexe D : Sécurisation des comptes d’administrateurs intégrés dans Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)|Contient des indications pour aider à sécuriser le compte administrateur dans chaque domaine de la forêt.|  
|[Annexe E : Sécurisation des groupes d’administrateurs de l’entreprise dans Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory.md)|Contient des indications pour aider à sécuriser le groupe administrateurs de l’entreprise dans la forêt.|  
|[Annexe F : Sécurisation des groupes d’administrateurs de domaine dans Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory.md)|Contient des indications pour aider à sécuriser le groupe administrateurs du domaine dans chaque domaine de la forêt.|  
|[Annexe G : Sécurisation des groupes d’administrateurs dans Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-G--Securing-Administrators-Groups-in-Active-Directory.md)|Contient des indications pour aider à sécuriser le groupe Administrateurs intégré dans chaque domaine de la forêt.|  
|[Annexe H : Sécurisation des groupes et des comptes des administrateurs locaux](../../../ad-ds/plan/security-best-practices/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups.md)|Contient des instructions pour sécuriser les comptes d’administrateur local et les groupes d’administrateurs sur les serveurs et stations de travail joints à un domaine.|  
|[Annexe I : Création de comptes de gestion pour les groupes et les comptes protégés dans Active Directory](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md)|Fournit des informations pour créer des comptes disposant de privilèges limités et pouvant être contrôlés de façon stricte, mais qui peuvent être utilisés pour remplir des groupes privilégiés dans Active Directory lorsque l’élévation temporaire est nécessaire.|  
|[Annexe L : événements à analyser](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md)|Répertorie les événements que vous devez surveiller dans votre environnement.|  
|[Annexe M : Liens vers des documents et lecture recommandée](../../../ad-ds/manage/Appendix-M--Document-Links-and-Recommended-Reading.md)|Contient une liste de lectures recommandées. Contient également une liste de liens vers des documents externes et leurs URL afin que les lecteurs des copies papier de ce document puissent accéder à ces informations.|  
  


