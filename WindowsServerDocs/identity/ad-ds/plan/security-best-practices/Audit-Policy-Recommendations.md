---
ms.assetid: 0abe0976-4b49-45d6-a7b3-81d28bdb8210
title: "Recommandations concernant la stratégie d’audit"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d03d38834f89f8cda80b7af147e2bd3e31f4f990
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="audit-policy-recommendations"></a>Recommandations concernant la stratégie d’audit

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Cette section traite les paramètres de stratégie d’audit de Windows par défaut, ligne de base de recommandé de paramètres de stratégie d’audit et les recommandations plus agressives auprès de Microsoft, pour les produits de station de travail et le serveur.  

Les recommandations de ligne de base SCM illustrées ici, ainsi que les paramètres, que nous vous recommandons de détecter toute compromission, sont destinées à être un guide de ligne de base de départ aux administrateurs uniquement. Chaque organisation doit prendre ses propres décisions concernant les attaques, leurs tolérances risque acceptable et les catégories de stratégie d’audit ou des sous-catégories qu’ils doivent activer. Pour plus d’informations sur les menaces, reportez-vous à la [Guide des menaces et contre-mesures](https://technet.microsoft.com/library/hh125921(v=ws.10).aspx). Sans une stratégie d’audit réfléchie en place, les administrateurs sont invités à démarrer avec les paramètres recommandés ici, puis modifier et tester, avant la mise en œuvre dans leur environnement de production.  

Les recommandations sont pour les ordinateurs d’entreprise, qui Microsoft définit en tant qu’ordinateurs qui ont des exigences de sécurité moyenne et nécessitent un haut niveau de fonctionnalité opérationnelle. Entités nécessitant une sécurité accrue, tenez compte plus agressives des exigences des stratégies d’audit.  

> [!NOTE]  
> La valeur par défaut de MicrosoftWindows et recommandations de ligne de base ont été effectuées à partir de la [outil MicrosoftSecurity Compliance Manager](https://technet.microsoft.com/library/cc677002.aspx).  

Les paramètres de stratégie d’audit de ligne de base suivants sont recommandés pour normal de sécurité pour les ordinateurs qui n’est pas connue sous attaque active, réussie par ses adversaires déterminés ou des programmes malveillants.  

## <a name="recommended-audit-policies-by-operating-system"></a>Recommandé de stratégies d’Audit par système d’exploitation  
Cette section contient des tableaux répertoriant les recommandations de paramètre d’audit qui s’appliquent aux systèmes d’exploitation suivants:  

-   Windows Server2012  

-   Windows Server2012R2  

-   Windows Server2008  

-   Windows8  

-   Windows7  

Ces tables contiennent le paramètre par défaut de Windows, les recommandations en matière de ligne de base et les recommandations plus puissante pour ces systèmes d’exploitation.  

**Légende de Tables de stratégie d’audit**  

|||  
|-|-|  
|**Notation**|**Recommandation**|  
|OUI|Activer des scénarios en général|  
|N°|Faire **pas** activer des scénarios en général|  
|IF|Activez si nécessaire pour un scénario spécifique, ou si un rôle ou une fonctionnalité pour laquelle l’audit est souhaitée est installé sur l’ordinateur|  
|CONTRÔLEUR DE DOMAINE|Activer sur les contrôleurs de domaine|  
|[Vide]|Aucune recommandation|  

**Windows8 et Windows7 d’Audit paramètres recommandations**  

**Stratégie d’audit**  

|Catégorie de stratégie d’audit ou la sous-catégorie|Par défaut de Windows<br /><br />Échec de la réussite|Recommandation de ligne de base<br /><br />Échec de la réussite|Recommandation plus puissante<br /><br />Échec de la réussite|  
|----------------------------------------|------------------------------------------|--------------------------------------------------|--------------------------------------------------|  
|**Ouverture de session de compte**||||  
|Validation des informations d’identification d’audit|Non non|Oui non|Oui, oui|  
|Service d’authentification Kerberos d’audit|||Oui, oui|  
|Auditer les opérations de Ticket de Service Kerberos|||Oui, oui|  
|Auditer d’autres événements d’ouverture de session de compte|||Oui, oui|  
|**Gestion des comptes**||||  
|Gestion du groupe d’applications d’audit||||  
|Auditer la gestion de compte d’ordinateur||Oui non|Oui, oui|  
|Gestion de groupe de Distribution d’audit||||  
|Auditer d’autres événements de gestion de compte||Oui non|Oui, oui|  
|Gestion de groupe de sécurité d’audit||Oui non|Oui, oui|  
|Auditer la gestion de compte d’utilisateur|Oui non|Oui non|Oui, oui|  
|**Suivi détaillé**||||  
|Auditer l’activité DPAPI|||Oui, oui|  
|Création du processus d’audit||Oui non|Oui, oui|  
|Fin du processus d’audit||||  
|Auditer les événements RPC||||  
|**Accès DS**||||  
|Auditer la réplication du Service d’annuaire détaillé||||  
|Auditer l’accès au Service d’annuaire||||  
|Auditer les modifications de Service d’annuaire||||  
|Auditer la réplication du Service annuaire||||  
|**Ouverture de session et la fermeture de session**||||  
|Auditer le verrouillage de compte|Oui non||Oui non|  
|Revendications d’utilisateur/appareil d’audit||||  
|Auditer le Mode étendu IPsec||||  
|Auditer le Mode principal IPsec|||IF IF|  
|Auditer le Mode rapide IPsec||||  
|Auditer la fermeture de session|Oui non|Oui non|Oui non|  
|Ouverture de session d’audit|Oui non|Oui non|Oui, oui|  
|Serveur de stratégie d’audit réseau|Oui, oui|||  
|Auditer d’autres événements d’ouverture/fermeture de session||||  
|Ouverture de session spéciale d’audit|Oui non|Oui non|Oui, oui|  
|**Accès aux objets**||||  
|Auditer l’Application générée||||  
|Services de Certification d’audit||||  
|Auditer le partage de fichiers détaillé||||  
|Partage de fichiers d’audit||||  
|Auditer le système de fichiers||||  
|Audit de connexion de la plateforme de filtrage||||  
|Rejet de paquet par plateforme de filtrage d’audit||||  
|Auditer la Manipulation de Handle||||  
|Objet de noyau d’audit||||  
|Auditer d’autres événements d’accès aux objets||||  
|Auditer le Registre||||  
|Stockage amovible d’audit||||  
|Auditer SAM||||  
|Auditer la stratégie d’accès centralisée intermédiaire||||  
|**Modification de la stratégie**||||  
|Auditer la modification de stratégie d’Audit|Oui non|Oui, oui|Oui, oui|  
|Modification de la stratégie d’audit d’authentification|Oui non|Oui non|Oui, oui|  
|Modification de la stratégie d’autorisation d’audit||||  
|Auditer les modifications de stratégie de plateforme de filtrage||||  
|Auditer la modification de stratégie de niveau règle MPSSVC|||Oui  |  
|Auditer d’autres événements de modification de stratégie||||  
|**Utilisation de privilèges**||||  
|Auditer l’utilisation de privilèges Non sensibles||||  
|Auditer d’autres événements d’utilisation de privilèges||||  
|Auditer l’utilisation des privilèges sensibles||||  
|**Système**||||  
|Auditer le pilote IPsec||Oui, oui|Oui, oui|  
|Auditer d’autres événements système|Oui, oui|||  
|Changement d’état de sécurité d’audit|Oui non|Oui, oui|Oui, oui|  
|Extension du système de sécurité d’audit||Oui, oui|Oui, oui|  
|Intégrité du système d’audit|Oui, oui|Oui, oui|Oui, oui|  
|**Accès global aux objets d’audit**||||  
|Auditer le pilote IPsec||||  
|Auditer d’autres événements système||||  
|Changement d’état de sécurité d’audit||||  
|Extension du système de sécurité d’audit||||  
|Intégrité du système d’audit||||  

**Recommandations de paramètres d’Audit Windows Server2008, Windows Server2008R2 et Windows Server2012**  

|Catégorie de stratégie d’audit ou la sous-catégorie|Par défaut de Windows<br /><br />Échec de la réussite|Recommandation de ligne de base<br /><br />Échec de la réussite|Recommandation plus puissante<br /><br />Échec de la réussite|  
|----------------------------------------|------------------------------------------|--------------------------------------------------|--------------------------------------------------|  
|**Ouverture de session de compte**||||  
|Validation des informations d’identification d’audit|Non non|Oui, oui|Oui, oui|  
|Service d’authentification Kerberos d’audit|||Oui, oui|  
|Auditer les opérations de Ticket de Service Kerberos|||Oui, oui|  
|Auditer d’autres événements d’ouverture de session de compte|||Oui, oui|  
|**Gestion des comptes**||||  
|Gestion du groupe d’applications d’audit||||  
|Auditer la gestion de compte d’ordinateur||Oui du contrôleur de domaine|Oui, oui|  
|Gestion de groupe de Distribution d’audit||||  
|Auditer d’autres événements de gestion de compte||Oui, oui|Oui, oui|  
|Gestion de groupe de sécurité d’audit||Oui, oui|Oui, oui|  
|Auditer la gestion de compte d’utilisateur|Oui non|Oui, oui|Oui, oui|  
|**Suivi détaillé**||||  
|Auditer l’activité DPAPI|||Oui, oui|  
|Création du processus d’audit||Oui non|Oui, oui|  
|Fin du processus d’audit||||  
|Auditer les événements RPC||||  
|**Accès DS**||||  
|Auditer la réplication du Service d’annuaire détaillé||||  
|Auditer l’accès au Service d’annuaire||CONTRÔLEUR DE DOMAINE DE CONTRÔLEUR DE DOMAINE|CONTRÔLEUR DE DOMAINE DE CONTRÔLEUR DE DOMAINE|  
|Auditer les modifications de Service d’annuaire||CONTRÔLEUR DE DOMAINE DE CONTRÔLEUR DE DOMAINE|CONTRÔLEUR DE DOMAINE DE CONTRÔLEUR DE DOMAINE|  
|Auditer la réplication du Service annuaire||||  
|**Ouverture de session et la fermeture de session**||||  
|Auditer le verrouillage de compte|Oui non||Oui non|  
|Revendications d’utilisateur/appareil d’audit||||  
|Auditer le Mode étendu IPsec||||  
|Auditer le Mode principal IPsec|||IF IF|  
|Auditer le Mode rapide IPsec||||  
|Auditer la fermeture de session|Oui non|Oui non|Oui non|  
|Ouverture de session d’audit|Oui non|Oui, oui|Oui, oui|  
|Serveur de stratégie d’audit réseau|Oui, oui|||  
|Auditer d’autres événements d’ouverture/fermeture de session|||Oui, oui|  
|Ouverture de session spéciale d’audit|Oui non|Oui non|Oui, oui|  
|**Accès aux objets**||||  
|Auditer l’Application générée||||  
|Services de Certification d’audit||||  
|Auditer le partage de fichiers détaillé||||  
|Partage de fichiers d’audit||||  
|Auditer le système de fichiers||||  
|Audit de connexion de la plateforme de filtrage||||  
|Rejet de paquet par plateforme de filtrage d’audit||||  
|Auditer la Manipulation de Handle||||  
|Objet de noyau d’audit||||  
|Auditer d’autres événements d’accès aux objets||||  
|Auditer le Registre||||  
|Stockage amovible d’audit||||  
|Auditer SAM||||  
|Auditer la stratégie d’accès centralisée intermédiaire||||  
|**Modification de la stratégie**||||  
|Auditer la modification de stratégie d’Audit|Oui non|Oui, oui|Oui, oui|  
|Modification de la stratégie d’audit d’authentification|Oui non|Oui non|Oui, oui|  
|Modification de la stratégie d’autorisation d’audit||||  
|Auditer les modifications de stratégie de plateforme de filtrage||||  
|Auditer la modification de stratégie de niveau règle MPSSVC|||Oui  |  
|Auditer d’autres événements de modification de stratégie||||  
|**Utilisation de privilèges**||||  
|Auditer l’utilisation de privilèges Non sensibles||||  
|Auditer d’autres événements d’utilisation de privilèges||||  
|Auditer l’utilisation des privilèges sensibles||||  
|**Système**||||  
|Auditer le pilote IPsec||Oui, oui|Oui, oui|  
|Auditer d’autres événements système|Oui, oui|||  
|Changement d’état de sécurité d’audit|Oui non|Oui, oui|Oui, oui|  
|Extension du système de sécurité d’audit||Oui, oui|Oui, oui|  
|Intégrité du système d’audit|Oui, oui|Oui, oui|Oui, oui|  
|**Accès global aux objets d’audit**||||  
|Auditer le pilote IPsec||||  
|Auditer d’autres événements système||||  
|Changement d’état de sécurité d’audit||||  
|Extension du système de sécurité d’audit||||  
|Intégrité du système d’audit||||  

## <a name="set-audit-policy-on-workstations-and-servers"></a>Définir la stratégie d’Audit sur les serveurs et stations de travail  
Tous les modes de gestion de journal des événements doivent analyser les serveurs et stations de travail. Une erreur courante consiste à surveiller uniquement les contrôleurs de domaine ou serveurs. Dans la mesure où le piratage malveillants souvent initialement se produit sur les stations de travail, analyse ne pas les stations de travail ignore la plus récent et meilleure source d’informations.  

Les administrateurs doivent efficacement passer en revue et tester une stratégie d’audit avant l’implémentation dans leur environnement de production.  

## <a name="events-to-monitor"></a>Événements à surveiller  
Un ID d’événement parfait pour générer une alerte de sécurité doit contenir les attributs suivants:  

-   Forte probabilité que l’occurrence indique une activité non autorisée  

-   Nombre faible de faux positifs  

-   Occurrence doit aboutir à une réponse sous forme de recherche/analyse  

Deux types d’événements doivent être analysés et recevoir une alerte:  

1.  Ces événements dans lesquels même une seule occurrence indique une activité non autorisée  

2.  Une accumulation d’événements au-dessus d’une ligne de base acceptée et attendue  

Un exemple du premier événement est:  

Si les administrateurs du domaine (DAs) sont interdites à partir de l’ouverture de session sur les ordinateurs qui ne sont pas des contrôleurs de domaine, une seule occurrence d’un membre DA ouverture de session sur une station de travail de l’utilisateur final doit générer une alerte et être analysée. Ce type d’alerte est facile de générer à l’aide de l’événement d’ouverture de session spéciale d’Audit4964 (groupes spéciaux ont reçu une ouverture de session). Autres exemples d’alertes d’instance unique:  

-   Si le serveur A doit se connectent jamais au serveur B, alerte lorsqu’ils se connectent à l’autre.  

-   Alerte si un compte d’utilisateur final normal est inattendu ajouté à un groupe de sécurité sensibles.  

-   Si les employés à l’emplacement usine A jamais fonctionnent pendant la nuit, alerte lorsqu’un utilisateur ouvre une session à minuit.  

-   Alerte si un service non autorisé est installé sur un contrôleur de domaine.  

-   Vérifiez si un utilisateur standard tente de connecter directement à un serveur SQLServer pour lesquels ils ne disposent d’aucune raison clair pour ce faire.  

-   Si vous ne disposez d’aucun membre de votre groupe DA et que quelqu'un ajoute eux-mêmes, vérifiez immédiatement.  

Est un exemple du deuxième événement:  

Un nombre anormal d’ouvertures de session ayant échoué peut indiquer une attaque de recherche de mot de passe. Pour une entreprise fournir une alerte pour un nombre anormalement élevé d’échecs de connexion, ils doivent tout d’abord comprendre les niveaux d’ouvertures de session ayant échoué dans leur environnement avant un événement de sécurité normales.  

Pour obtenir une liste complète des événements que vous devez inclure lors de la surveillance des signes de compromission, voir [annexe l: événements à surveiller](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md).  

## <a name="active-directory-objects-and-attributes-to-monitor"></a>Les objets ActiveDirectory et les attributs à analyser  
Voici les comptes, groupes et les attributs que vous devez surveiller pour vous aider à détecter des tentatives de compromission de l’installation des Services de domaine ActiveDirectory.  

-   Systèmes pour la désactivation ou la suppression du logiciel antivirus et anti-programme malveillant (automatiquement redémarrer la protection lorsqu’il est désactivé manuellement)  

-   Comptes d’administrateur pour les modifications non autorisées  

-   Activités qui sont effectuées à l’aide des comptes privilégiés (automatiquement supprimer un compte lorsque les activités suspectes sont terminées ou période a expiré)  

-   Privilégiés et comptes VIP dans ADDS. Surveillez les modifications apportées, notamment les modifications apportées aux attributs sous l’onglet compte (par exemple, cn, nom, sAMAccountName, userPrincipalName ou userAccountControl). Outre les comptes de surveillance, limiter les personnes peuvent modifier les comptes en tant que petites un ensemble d’utilisateurs administratifs que possible.  

Reportez-vous à [annexe l: événements à surveiller](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md) pour obtenir la liste des événements recommandées pour l’analyse, leur criticité et une synthèse de message d’événement.  

-   Groupes serveurs par la classification de leurs charges de travail, ce qui vous permet d’identifier rapidement les serveurs qui doivent être plus étroitement contrôlée et configuré la plus stricte  

-   Modifications apportées aux propriétés et l’appartenance des groupes de domaine ActiveDirectory suivants: administrateurs de l’entreprise (EA), Admins du domaine (DA), les administrateurs (BA) et administrateurs du schéma (SA)  

-   Comptes privilégiés désactivés (par exemple, les comptes d’administrateur intégrés dans ActiveDirectory et sur des systèmes membres) pour activer les comptes  

-   Comptes de gestion pour enregistrer toutes les écritures dans le compte  

-   Assistant Configuration de sécurité intégrée pour configurer le service, le Registre, d’audit et les paramètres de pare-feu pour réduire la surface d’attaque du serveur. Utilisez cet Assistant si vous implémentez des serveurs de renvoi dans le cadre de votre stratégie d’ordinateur hôte d’administration.  

## <a name="additional-information-for-monitoring-active-directory-domain-services"></a>Informations supplémentaires pour l’analyse des Services de domaine ActiveDirectory  
Passez en revue les liens suivants pour plus d’informations sur l’analyse des services ADDS:  
  
-   [Audit d’accès objet global est magique](http://blogs.technet.com/b/askds/archive/2011/03/10/global-object-access-auditing-is-magic.aspx) -fournit des informations sur la configuration et à l’aide de la Configuration avancée d’Audit stratégie qui a été ajouté à Windows7 et Windows Server2008R2.  

-   [Présentation de l’audit des modifications dans Windows2008](http://blogs.technet.com/b/askds/archive/2007/10/19/introducing-auditing-changes-in-windows-2008.aspx) -introduit l’audit apportées dans Windows2008.  

-   [Refroidir astuces de l’audit dans Vista et 2008](http://blogs.technet.com/b/askds/archive/2007/11/16/cool-auditing-tricks-in-vista-and-2008.aspx) -explique intéressant de nouvelles fonctionnalités d’audit dans WindowsVista et Windows Server2008 qui peut être utilisé pour la résolution des problèmes ou pour voir ce qui se passe dans votre environnement.  

-   [Emplacement centralisé pour l’audit dans Windows Server2008 et WindowsVista](http://blogs.technet.com/b/askds/archive/2008/03/27/one-stop-shop-for-auditing-in-windows-server-2008-and-windows-vista.aspx) -contient une compilation de l’audit des fonctionnalités et les informations contenues dans Windows Server2008 et WindowsVista.  

-   [ADDS audit Step-by-Step Guide](https://technet.microsoft.com/library/a9c25483-89e2-4202-881c-ea8e02b4b2a5.aspx) -décrit la nouvelle fonctionnalité d’audit de Services de domaine ActiveDirectory (ADDS) dans Windows Server2008. Il fournit également des procédures pour implémenter cette nouvelle fonctionnalité.  

## <a name="general-list-of-security-event-id-recommendation-criticalities"></a>Liste générale de sécurité événement ID recommandation les points critiques  
Toutes les recommandations de l’ID d’événement sont accompagnées d’une importance évaluation comme suit:  

**Haute:** ID d’événement avec un niveau de gravité élevée doit toujours immédiatement être alertés et examinés.  

**Moyenne:** un ID d’événement avec un niveau de gravité moyenne peut indiquer une activité malveillante, mais il doit être livré avec certains autres anomalies (par exemple, un nombre inhabituel qui se produisent dans une période donnée, occurrences inattendues ou occurrences d’un ordinateur qui normalement ne devrait pas à consigner l’événement.). Un événement de l’importance de support peut également r être collecté en tant qu’une mesure et par rapport au fil du temps.  

**Faible:** et ID d’événement avec les événements d’une importance faible ne doit pas récupérer une attention particulière ou l’affichage des alertes, sauf en corrélation avec les événements de gravité moyenne ou élevée.  

Ces recommandations sont destinées à fournir un guide de ligne de base pour l’administrateur. Toutes les recommandations doivent être minutieusement examinées avant l’implémentation dans un environnement de production.  

Reportez-vous à [annexe l: événements à surveiller](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md) pour obtenir la liste des événements recommandées pour l’analyse, leur criticité et une synthèse de message d’événement.  
