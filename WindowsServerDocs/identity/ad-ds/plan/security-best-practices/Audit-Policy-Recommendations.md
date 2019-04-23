---
ms.assetid: 0abe0976-4b49-45d6-a7b3-81d28bdb8210
title: Recommandations en matière de stratégie d’audit
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 343a9a7aedf22e9c021249f00fb628f871a2ce1f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835750"
---
# <a name="audit-policy-recommendations"></a>Recommandations en matière de stratégie d’audit

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10, Windows 8.1, Windows 7

Cette section traite les paramètres de stratégie d’audit de Windows par défaut, de base recommandées des paramètres de stratégie d’audit et les recommandations plus agressives de Microsoft, pour les produits de station de travail et serveur.  

Les recommandations de base SCM illustrées ici, ainsi que les paramètres que nous vous recommandons de détecter toute compromission, sont destinées uniquement à servir de départ de base aux administrateurs. Chaque organisation doit effectuer ses propres décisions concernant les attaques, leurs tolérances au risque acceptable, et les catégories de stratégie d’audit ou des sous-catégories, ils doivent activer. Pour plus d’informations sur les menaces, reportez-vous à la [Guide des menaces et contre-mesures](https://technet.microsoft.com/library/hh125921(v=ws.10).aspx). Sans une stratégie d’audit réfléchie en place, les administrateurs sont invités à démarrer avec les paramètres recommandés ici, puis de modifier et de test, avant d’implémenter dans leur environnement de production.  

Les recommandations sont pour les ordinateurs de classe entreprise, Microsoft définit en tant qu’ordinateurs qui ont des exigences de sécurité moyenne et nécessitent un niveau élevé de bon fonctionnement. Entités nécessitant une sécurité accrue exigences doivent prendre en compte plus agressives des stratégies d’audit.  

> [!NOTE]  
> Paramètres par défaut de Microsoft Windows et de recommandations de base ont été effectuées à partir de la [Microsoft Security Compliance Manager outil](https://technet.microsoft.com/library/cc677002.aspx).  

Les paramètres de stratégie d’audit de base suivantes sont recommandées pour les ordinateurs de sécurité normale qui ne sont pas connus pour être attaqué active, réussie par des adversaires déterminés ou des programmes malveillants.  

## <a name="recommended-audit-policies-by-operating-system"></a>Stratégies d’Audit par système d’exploitation recommandées  
Cette section contient des tables qui répertorient les recommandations de paramètre d’audit qui s’appliquent aux systèmes d’exploitation suivants :  

-   Windows Server 2016 

-   Windows Server 2012  

-   Windows Server 2012 R2  

-   Windows Server 2008  

-   Windows 10

-   Windows 8.1  

-   Windows 7  

Ces tables contiennent le paramètre par défaut de Windows, les recommandations de base et les recommandations plus fortes pour ces systèmes d’exploitation.  

**Légende de Tables de stratégie d’audit**  

|||  
|-|-|  
|**Notation**|**Recommandation**|  
|OUI|Activer des scénarios en général|  
|NON|Faire **pas** activer des scénarios en général|  
|IF|Activer si nécessaire pour un scénario spécifique, ou si un rôle ou une fonctionnalité pour laquelle l’audit est souhaitée est installé sur l’ordinateur|  
|DC|Activer sur les contrôleurs de domaine|  
|[Vide]|Aucune recommandation|  

**Windows 10, Windows 8 et Windows 7 d’Audit paramètres recommandations**  

**Stratégie d’audit**  

|Catégorie de stratégie d’audit ou une sous-catégorie|Par défaut de Windows<br /><br />Échec de la réussite|Recommandation de la ligne de base<br /><br />Échec de la réussite|Recommandation plus forte<br /><br />Échec de la réussite|  
|----------------------------------------|------------------------------------------|--------------------------------------------------|--------------------------------------------------|  
|**Connexion au compte**||||  
|Auditer la validation des informations d’identification|Non non|Oui non|Oui, oui|  
|Auditer le service d’authentification Kerberos|||Oui, oui|  
|Auditer les opérations de ticket du service Kerberos|||Oui, oui|  
|Auditer d’autres événements d’ouverture de session|||Oui, oui|  
|**Gestion des comptes**||||  
|Auditer la gestion des groupes d’applications||||  
|Auditer la gestion des comptes d’ordinateur||Oui non|Oui, oui|  
|Auditer la gestion des groupes de distribution||||  
|Auditer d’autres événements de gestion des comptes||Oui non|Oui, oui|  
|Auditer la gestion des groupes de sécurité||Oui non|Oui, oui|  
|Auditer la gestion des comptes d’utilisateur|Oui non|Oui non|Oui, oui|  
|**Suivi détaillé**||||  
|Auditer l’activité DPAPI|||Oui, oui|  
|Auditer la création du processus||Oui non|Oui, oui|  
|Auditer la fin du processus||||  
|Auditer les événements RPC||||  
|**Accès DS**||||  
|Auditer la réplication du service d’annuaire détaillé||||  
|Auditer l’accès au service d’annuaire||||  
|Auditer les modifications du service d’annuaire||||  
|Auditer la réplication du service d’annuaire||||  
|**Ouverture de session et de fermeture de session**||||  
|Auditer le verrouillage du compte|Oui non||Oui non|  
|Auditer les revendications utilisateur/de périphérique||||  
|Auditer le mode étendu IPsec||||  
|Auditer le mode principal IPsec|||IF     IF|  
|Auditer le mode rapide IPsec||||  
|Auditer la fermeture de session|Oui non|Oui non|Oui non|  
|Audit d’ouverture de session <sup>1</sup>|Oui, oui|Oui, oui|Oui, oui|  
|Auditer le serveur NPS (Network Policy Server)|Oui, oui|||  
|Auditer d’autres événements d’ouverture/fermeture de session||||  
|Auditer l’ouverture de session spéciale|Oui non|Oui non|Oui, oui|  
|**Accès aux objets**||||  
|Auditer l’application générée||||  
|Auditer les services de certification||||  
|Auditer le partage de fichiers détaillé||||  
|Auditer le partage de fichiers||||  
|Auditer le système de fichiers||||  
|Auditer la connexion de la plateforme de filtrage||||  
|Auditer le rejet de paquet par la plateforme de filtrage||||  
|Auditer la manipulation de handle||||  
|Auditer l’objet de noyau||||  
|Auditer d’autres événements d’accès à l’objet||||  
|Auditer le Registre||||  
|Auditer le stockage amovible||||  
|Auditer SAM||||  
|Auditer la stratégie d’accès centralisée intermédiaire||||  
|**Modification de la stratégie**||||  
|Auditer la modification de la stratégie d’audit|Oui non|Oui, oui|Oui, oui|  
|Auditer la modification de la stratégie d’authentification|Oui non|Oui non|Oui, oui|  
|Auditer la modification de la stratégie d’autorisation||||  
|Auditer la modification de la stratégie de plateforme de filtrage||||  
|Auditer la modification de la stratégie de niveau règle MPSSVC|||Oui  |  
|Auditer d’autres événements de modification de stratégie||||  
|**Utilisation des privilèges**||||  
|Auditer l’utilisation de privilèges non sensibles||||  
|Auditer d’autres événements d’utilisation de privilèges||||  
|Auditer l’utilisation de privilèges sensibles||||  
|**Système**||||  
|Auditer le pilote IPSEC||Oui, oui|Oui, oui|  
|Auditer d’autres événements système|Oui, oui|||  
|Auditer la modification de l’état de la sécurité|Oui non|Oui, oui|Oui, oui|  
|Auditer l’extension du système de sécurité||Oui, oui|Oui, oui|  
|Auditer l’intégrité du système|Oui, oui|Oui, oui|Oui, oui|  
|**Accès global aux objets d’audit**||||  
|Auditer le pilote IPSEC||||  
|Auditer d’autres événements système||||  
|Auditer la modification de l’état de la sécurité||||  
|Auditer l’extension du système de sécurité||||  
|Auditer l’intégrité du système||||  

<sup>1</sup> à compter de Windows 10 version 1809, ouverture de session d’Audit est activé par défaut pour la réussite et échec. Dans les versions précédentes de Windows, réussite uniquement est activée par défaut.

**Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 et recommandations de paramètres d’Audit Windows Server 2008**  

|Catégorie de stratégie d’audit ou une sous-catégorie|Par défaut de Windows<br /><br />Échec de la réussite|Recommandation de la ligne de base<br /><br />Échec de la réussite|Recommandation plus forte<br /><br />Échec de la réussite|  
|----------------------------------------|------------------------------------------|--------------------------------------------------|--------------------------------------------------|  
|**Connexion au compte**||||  
|Auditer la validation des informations d’identification|Non non|Oui, oui|Oui, oui|  
|Auditer le service d’authentification Kerberos|||Oui, oui|  
|Auditer les opérations de ticket du service Kerberos|||Oui, oui|  
|Auditer d’autres événements d’ouverture de session|||Oui, oui|  
|**Gestion des comptes**||||  
|Auditer la gestion des groupes d’applications||||  
|Auditer la gestion des comptes d’ordinateur||Oui le contrôleur de domaine|Oui, oui|  
|Auditer la gestion des groupes de distribution||||  
|Auditer d’autres événements de gestion des comptes||Oui, oui|Oui, oui|  
|Auditer la gestion des groupes de sécurité||Oui, oui|Oui, oui|  
|Auditer la gestion des comptes d’utilisateur|Oui non|Oui, oui|Oui, oui|  
|**Suivi détaillé**||||  
|Auditer l’activité DPAPI|||Oui, oui|  
|Auditer la création du processus||Oui non|Oui, oui|  
|Auditer la fin du processus||||  
|Auditer les événements RPC||||  
|**Accès DS**||||  
|Auditer la réplication du service d’annuaire détaillé||||  
|Auditer l’accès au service d’annuaire||DC    DC|DC    DC|  
|Auditer les modifications du service d’annuaire||DC    DC|DC    DC|  
|Auditer la réplication du service d’annuaire||||  
|**Ouverture de session et de fermeture de session**||||  
|Auditer le verrouillage du compte|Oui non||Oui non|  
|Auditer les revendications utilisateur/de périphérique||||  
|Auditer le mode étendu IPsec||||  
|Auditer le mode principal IPsec|||IF     IF|  
|Auditer le mode rapide IPsec||||  
|Auditer la fermeture de session|Oui non|Oui non|Oui non|  
|Auditer l’ouverture de session|Oui, oui|Oui, oui|Oui, oui|  
|Auditer le serveur NPS (Network Policy Server)|Oui, oui|||  
|Auditer d’autres événements d’ouverture/fermeture de session|||Oui, oui|  
|Auditer l’ouverture de session spéciale|Oui non|Oui non|Oui, oui|  
|**Accès aux objets**||||  
|Auditer l’application générée||||  
|Auditer les services de certification||||  
|Auditer le partage de fichiers détaillé||||  
|Auditer le partage de fichiers||||  
|Auditer le système de fichiers||||  
|Auditer la connexion de la plateforme de filtrage||||  
|Auditer le rejet de paquet par la plateforme de filtrage||||  
|Auditer la manipulation de handle||||  
|Auditer l’objet de noyau||||  
|Auditer d’autres événements d’accès à l’objet||||  
|Auditer le Registre||||  
|Auditer le stockage amovible||||  
|Auditer SAM||||  
|Auditer la stratégie d’accès centralisée intermédiaire||||  
|**Modification de la stratégie**||||  
|Auditer la modification de la stratégie d’audit|Oui non|Oui, oui|Oui, oui|  
|Auditer la modification de la stratégie d’authentification|Oui non|Oui non|Oui, oui|  
|Auditer la modification de la stratégie d’autorisation||||  
|Auditer la modification de la stratégie de plateforme de filtrage||||  
|Auditer la modification de la stratégie de niveau règle MPSSVC|||Oui  |  
|Auditer d’autres événements de modification de stratégie||||  
|**Utilisation des privilèges**||||  
|Auditer l’utilisation de privilèges non sensibles||||  
|Auditer d’autres événements d’utilisation de privilèges||||  
|Auditer l’utilisation de privilèges sensibles||||  
|**Système**||||  
|Auditer le pilote IPSEC||Oui, oui|Oui, oui|  
|Auditer d’autres événements système|Oui, oui|||  
|Auditer la modification de l’état de la sécurité|Oui non|Oui, oui|Oui, oui|  
|Auditer l’extension du système de sécurité||Oui, oui|Oui, oui|  
|Auditer l’intégrité du système|Oui, oui|Oui, oui|Oui, oui|  
|**Accès global aux objets d’audit**||||  
|Auditer le pilote IPSEC||||  
|Auditer d’autres événements système||||  
|Auditer la modification de l’état de la sécurité||||  
|Auditer l’extension du système de sécurité||||  
|Auditer l’intégrité du système||||  

## <a name="set-audit-policy-on-workstations-and-servers"></a>Définir une stratégie d’Audit sur les serveurs et stations de travail  
Tous les plans de gestion du journal des événements doivent surveiller les serveurs et stations de travail. Une erreur courante consiste à surveiller uniquement les contrôleurs de domaine ou serveurs. Comme souvent initialement de piratage malveillant se produit sur les stations de travail, ne pas les stations de travail de surveillance ignore la meilleure et plus ancienne source d’informations.  

Les administrateurs doivent réfléchie passez en revue et tester une stratégie d’audit avant l’implémentation dans leur environnement de production.  

## <a name="events-to-monitor"></a>événements à analyser  
Un ID d’événement parfait pour générer une alerte de sécurité doit contenir les attributs suivants :  

-   Forte probabilité que l’occurrence indique une activité non autorisée  

-   Nombre faible de faux positifs  

-   Occurrence doit provoquer une réponse de l’investigation/analyse  

Deux types d’événements doivent être contrôlés et recevoir des alertes :  

1.  Ces événements dans lequel même une seule occurrence indique une activité non autorisée  

2.  Une accumulation d’événements différents de la référence acceptée et attendue  

Un exemple du premier événement est :  

Si les administrateurs de domaine (DAs) sont interdites à partir de l’ouverture de session sur des ordinateurs qui ne sont pas des contrôleurs de domaine, une seule occurrence d’un membre de DA ouvrir une session sur une station de travail de l’utilisateur final doit générer une alerte et être examinée. Ce type d’alerte est facile de générer à l’aide de l’événement d’ouverture de session spéciale d’Audit 4964 (groupes spéciaux affectées à une nouvelle ouverture de session). Autres exemples d’alertes d’instance unique :  

-   Si le serveur A ne doit jamais se connecter vers le serveur B, alerte lorsqu’ils se connectent entre eux.  

-   Une alerte si un compte normal de l’utilisateur final est inattendu ajouté à un groupe de sécurité sensibles.  

-   Si les employés dans l’emplacement de la fabrique A jamais fonctionneront pendant la nuit, alerte quand un utilisateur se connecte à minuit.  

-   Une alerte si un service non autorisé est installé sur un contrôleur de domaine.  

-   Vérifiez si un utilisateur final régulière tente de connecter directement à un serveur SQL pour lesquels ils ne disposent d’aucune raison clair pour effectuer cette opération.  

-   Si vous n’avez aucun membre dans votre groupe DA et que quelqu'un ajoute eux-mêmes, vérifiez immédiatement.  

Est un exemple du deuxième événement :  

Un nombre anormal d’échecs de connexion peut indiquer une attaque de recherche de mot de passe. Pour une entreprise de fournir une alerte pour un nombre anormalement élevé d’échecs de connexion, ils doivent d’abord comprendre les niveaux normales d’ouvertures de session ayant échoué dans leur environnement avant un événement de sécurité malveillant.  

Pour obtenir la liste complète des événements que vous devez inclure lors de l’analyse pour rechercher des signes de compromission, consultez [annexe L: Événements à surveiller](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md).  

## <a name="active-directory-objects-and-attributes-to-monitor"></a>Objets Active Directory et les attributs à analyser  
Voici les comptes, les groupes et les attributs que vous devez surveiller pour vous aider à détecter les tentatives visant à compromettre votre installation Active Directory Domain Services.  

-   Systèmes pour la désactivation ou la suppression du logiciel antivirus et anti-programme malveillant (automatiquement redémarrer la protection lorsqu’il est désactivé manuellement)  

-   Comptes d’administrateur pour les modifications non autorisées  

-   Activités sont effectuées en utilisant des comptes privilégiés (automatiquement compte de suppression lorsque les activités suspectes sont terminées ou heure dont dispose le processus a expiré)  

-   Privilégiés et comptes d’adresse IP virtuelle dans les services AD DS. Surveillez les modifications apportées, en particulier les modifications apportées aux attributs sur l’onglet compte (par exemple, cn, nom, sAMAccountName, userPrincipalName ou userAccountControl). Outre la surveillance des comptes, restreindre les personnes autorisées à modifier les comptes à faibles un ensemble d’utilisateurs administratifs que possible.  

Reportez-vous à [annexe l : Événements à surveiller](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md) pour obtenir la liste des événements recommandées pour surveiller, leur criticité et une synthèse de message d’événement.  

-   Groupe serveurs par la classification de leurs charges de travail, ce qui vous permet d’identifier rapidement les serveurs qui doivent être le plus étroitement surveillés et plus stricte configuré  

-   Modifications apportées aux propriétés et l’appartenance de groupes des services AD DS suivants : Administrateurs de l’entreprise (EA), Admins du domaine (DA), les administrateurs (BA) et schéma (SA)  

-   Comptes privilégiés désactivés (par exemple, les comptes d’administrateur intégrés dans Active Directory et sur les systèmes de membre) pour activer les comptes  

-   Comptes de gestion pour consigner toutes les écritures dans le compte  

-   Assistant de Configuration de sécurité intégrée pour configurer le service, le Registre, d’audit et les paramètres de pare-feu pour réduire la surface d’attaque du serveur. Utilisez cet Assistant si vous implémentez des serveurs jump dans le cadre de votre stratégie d’administration hôte.  

## <a name="additional-information-for-monitoring-active-directory-domain-services"></a>Informations supplémentaires pour la surveillance des Services de domaine Active Directory  
Passez en revue les liens suivants pour plus d’informations sur la surveillance des services AD DS :  
  
-   [Audit d’accès objet global est magique](http://blogs.technet.com/b/askds/archive/2011/03/10/global-object-access-auditing-is-magic.aspx) -fournit des informations sur la configuration et à l’aide de la Configuration avancée d’Audit stratégie qui a été ajouté à Windows 7 et Windows Server 2008 R2.  

-   [Présentation de l’audit des modifications dans Windows 2008](http://blogs.technet.com/b/askds/archive/2007/10/19/introducing-auditing-changes-in-windows-2008.aspx) -introduit les modifications audit apportées dans Windows 2008.  

-   [Refroidir l’audit des astuces dans Vista et 2008](http://blogs.technet.com/b/askds/archive/2007/11/16/cool-auditing-tricks-in-vista-and-2008.aspx) -explique les nouvelles fonctionnalités intéressantes de l’audit dans Windows Vista et Windows Server 2008 qui peut être utilisé pour la résolution des problèmes ou pour voir ce qui se passe dans votre environnement.  

-   [Guichet unique pour l’audit dans Windows Server 2008 et Windows Vista](http://blogs.technet.com/b/askds/archive/2008/03/27/one-stop-shop-for-auditing-in-windows-server-2008-and-windows-vista.aspx) -contient une compilation de l’audit des fonctionnalités et les informations contenues dans Windows Server 2008 et Windows Vista.  

-   [Guide pas à pas d’audit d’AD DS](https://technet.microsoft.com/library/a9c25483-89e2-4202-881c-ea8e02b4b2a5.aspx) -décrit la nouvelle fonctionnalité audit de Services de domaine Active Directory (AD DS) dans Windows Server 2008. Il fournit également des procédures pour implémenter cette nouvelle fonctionnalité.  

## <a name="general-list-of-security-event-id-recommendation-criticalities"></a>Liste générale de sécurité Event ID recommandation selon l’importance  
Toutes les recommandations de l’ID d’événement sont accompagnées d’une importance rating comme suit :  

**Élevée :** ID d’événement avec une évaluation plus critiques doivent immédiatement et toujours être alertés et examinés.  

**Moyenne :** Un ID d’événement avec un niveau de gravité moyenne peut indiquer une activité malveillante, mais elle doit être accompagnée de certains autres anomalies (par exemple, un nombre inhabituel qui se produisent dans une période donnée, les occurrences inattendues ou les occurrences d’un ordinateur qui serait pas normalement dans le journal.). Un événement de l’importance de support peut également r être regroupé en une mesure et par rapport au fil du temps.  

**Faible :** Et l’ID d’événement avec un événement de priorité basse ne doit pas recueillir votre attention ou génèrent des alertes, sauf si la corrélation avec les événements de gravité moyen ou élevé.  

Ces recommandations sont destinées à fournir un guide de référence de l’administrateur. Toutes les recommandations doivent être minutieusement contrôlées avant l’implémentation dans un environnement de production.  

Reportez-vous à [annexe l : Événements à surveiller](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md) pour obtenir la liste des événements recommandées pour surveiller, leur criticité et une synthèse de message d’événement.  
