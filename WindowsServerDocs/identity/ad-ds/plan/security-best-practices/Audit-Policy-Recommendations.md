---
ms.assetid: 0abe0976-4b49-45d6-a7b3-81d28bdb8210
title: Recommandations de stratégie d'audit
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 6cecf2edcb834a963c706fa4a63e7d15b13f7888
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75949625"
---
# <a name="audit-policy-recommendations"></a>Recommandations de stratégie d'audit

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10, Windows 8.1, Windows 7

Cette section traite des paramètres de stratégie d’audit par défaut de Windows, des paramètres de stratégie d’audit recommandés pour la ligne de base et des recommandations plus agressives de Microsoft, pour les produits de station de travail et de serveur.  

Les recommandations de la ligne de base SCM indiquées ici, ainsi que les paramètres que nous recommandons pour aider à détecter les compromissions, sont destinés uniquement au lancement d’un guide de base pour les administrateurs. Chaque organisation doit prendre ses propres décisions concernant les menaces auxquelles elle est confrontée, les tolérances de risque acceptables et les catégories ou sous-catégories de stratégie d’audit qu’elles doivent activer. Pour plus d’informations sur les menaces, reportez-vous au [Guide menaces et contre-mesures](https://technet.microsoft.com/library/hh125921(v=ws.10).aspx). Les administrateurs sans stratégie d’audit réfléchie sont encouragés à commencer par les paramètres recommandés ici, puis à les modifier et à les tester avant de les implémenter dans leur environnement de production.  

Les recommandations concernent les ordinateurs d’entreprise, que Microsoft définit comme des ordinateurs qui ont des exigences de sécurité moyennes et qui nécessitent un haut niveau de fonctionnalité opérationnelle. Les entités nécessitant des exigences de sécurité plus élevées doivent prendre en compte des stratégies d’audit plus agressives.  

> [!NOTE]  
> Les recommandations par défaut et les recommandations de base Microsoft Windows ont été tirées de l' [outil Microsoft Security Compliance Manager](https://technet.microsoft.com/library/cc677002.aspx).  

Les paramètres de stratégie d’audit de base suivants sont recommandés pour les ordinateurs de sécurité normaux qui ne sont pas connus comme étant en cours d’attaque active et réussie par des adversaires ou des programmes malveillants déterminés.  

## <a name="recommended-audit-policies-by-operating-system"></a>Stratégies d’audit recommandées par système d’exploitation  
Cette section contient des tables qui répertorient les recommandations relatives aux paramètres d’audit qui s’appliquent aux systèmes d’exploitation suivants :  

-   Windows Server 2016 

-   Windows Server 2012  

-   R2 Windows Server 2012  

-   Windows Server 2008  

-   Windows 10

-   Windows 8.1  

-   Windows 7  

Ces tables contiennent les paramètres par défaut de Windows, les recommandations de base et les recommandations plus fortes pour ces systèmes d’exploitation.  

**Légende des tables de stratégie d’audit**  

|||  
|-|-|  
|**Conventions**|**Recommandation**|  
|OUI|Activer dans les scénarios généraux|  
|NON|Ne **pas** activer dans les scénarios généraux|  
|IF|Activer si nécessaire pour un scénario spécifique, ou si un rôle ou une fonctionnalité pour lequel l’audit est souhaité est installé sur l’ordinateur|  
|DC|Activer sur les contrôleurs de domaine|  
|Occult|Aucune recommandation|  

**Recommandations relatives aux paramètres d’audit Windows 10, Windows 8 et Windows 7**  

**Stratégie d’audit**  

|Catégorie ou sous-catégorie de stratégie d’audit|Par défaut Windows<br /><br />Échec de la réussite|Recommandation de base<br /><br />Échec de la réussite|Recommandation plus forte<br /><br />Échec de la réussite|  
|----------------------------------------|------------------------------------------|--------------------------------------------------|--------------------------------------------------|  
|**Ouverture de session de compte**||||  
|Auditer la validation des informations d’identification|Non non|Oui non|Oui, oui|  
|Auditer le service d’authentification Kerberos|||Oui, oui|  
|Auditer les opérations de ticket de service Kerberos|||Oui, oui|  
|Auditer d’autres événements d’ouverture de session|||Oui, oui|  
|**Account management (Gestion de compte)**||||  
|Auditer la gestion des groupes d’applications||||  
|Auditer la gestion des comptes d’ordinateur||Oui non|Oui, oui|  
|Auditer la gestion des groupes de distribution||||  
|Auditer d’autres événements de gestion des comptes||Oui non|Oui, oui|  
|Auditer la gestion des groupes de sécurité||Oui non|Oui, oui|  
|Auditer la gestion des comptes d’utilisateurs|Oui non|Oui non|Oui, oui|  
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
|**Ouverture et fermeture de session**||||  
|Auditer le verrouillage du compte|Oui non||Oui non|  
|Auditer les revendications utilisateur/de périphérique||||  
|Auditer le mode étendu IPsec||||  
|Auditer le mode principal IPsec|||Si|  
|Auditer le mode rapide IPsec||||  
|Auditer la fermeture de session|Oui non|Oui non|Oui non|  
|Auditer la connexion <sup>1</sup>|Oui, oui|Oui, oui|Oui, oui|  
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
|**Audit de l’accès global aux objets**||||  
|Auditer le pilote IPSEC||||  
|Auditer d’autres événements système||||  
|Auditer la modification de l’état de la sécurité||||  
|Auditer l’extension du système de sécurité||||  
|Auditer l’intégrité du système||||  

<sup>1</sup> à compter de Windows 10 version 1809, l’audit Logon est activé par défaut pour les opérations de réussite et d’échec. Dans les versions précédentes de Windows, seule la réussite est activée par défaut.

**Recommandations relatives aux paramètres d’audit de Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 et Windows Server 2008**  

|Catégorie ou sous-catégorie de stratégie d’audit|Par défaut Windows<br /><br />Échec de la réussite|Recommandation de base<br /><br />Échec de la réussite|Recommandation plus forte<br /><br />Échec de la réussite|  
|----------------------------------------|------------------------------------------|--------------------------------------------------|--------------------------------------------------|  
|**Ouverture de session de compte**||||  
|Auditer la validation des informations d’identification|Non non|Oui, oui|Oui, oui|  
|Auditer le service d’authentification Kerberos|||Oui, oui|  
|Auditer les opérations de ticket de service Kerberos|||Oui, oui|  
|Auditer d’autres événements d’ouverture de session|||Oui, oui|  
|**Account management (Gestion de compte)**||||  
|Auditer la gestion des groupes d’applications||||  
|Auditer la gestion des comptes d’ordinateur||Oui DC|Oui, oui|  
|Auditer la gestion des groupes de distribution||||  
|Auditer d’autres événements de gestion des comptes||Oui, oui|Oui, oui|  
|Auditer la gestion des groupes de sécurité||Oui, oui|Oui, oui|  
|Auditer la gestion des comptes d’utilisateurs|Oui non|Oui, oui|Oui, oui|  
|**Suivi détaillé**||||  
|Auditer l’activité DPAPI|||Oui, oui|  
|Auditer la création du processus||Oui non|Oui, oui|  
|Auditer la fin du processus||||  
|Auditer les événements RPC||||  
|**Accès DS**||||  
|Auditer la réplication du service d’annuaire détaillé||||  
|Auditer l’accès au service d’annuaire||DC DC|DC DC|  
|Auditer les modifications du service d’annuaire||DC DC|DC DC|  
|Auditer la réplication du service d’annuaire||||  
|**Ouverture et fermeture de session**||||  
|Auditer le verrouillage du compte|Oui non||Oui non|  
|Auditer les revendications utilisateur/de périphérique||||  
|Auditer le mode étendu IPsec||||  
|Auditer le mode principal IPsec|||Si|  
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
|**Audit de l’accès global aux objets**||||  
|Auditer le pilote IPSEC||||  
|Auditer d’autres événements système||||  
|Auditer la modification de l’état de la sécurité||||  
|Auditer l’extension du système de sécurité||||  
|Auditer l’intégrité du système||||  

## <a name="set-audit-policy-on-workstations-and-servers"></a>Définir la stratégie d’audit sur les stations de travail et les serveurs  
Tous les plans de gestion des journaux d’événements doivent analyser les stations de travail et les serveurs. Une erreur courante consiste à analyser uniquement les serveurs ou les contrôleurs de domaine. Étant donné que le piratage malveillant se produit généralement sur les stations de travail, les stations de travail ne sont pas ignorées par la source d’informations la plus récente et la plus ancienne.  

Les administrateurs doivent examiner et tester toute stratégie d’audit avant son implémentation dans son environnement de production.  

## <a name="events-to-monitor"></a>événements à analyser  
Un ID d’événement parfait pour générer une alerte de sécurité doit contenir les attributs suivants :  

-   Probabilité élevée que l’occurrence indique une activité non autorisée  

-   Nombre faible de faux positifs  

-   L’occurrence doit aboutir à une réponse d’investigation/d’investigation  

Deux types d’événements doivent être surveillés et alertés :  

1.  Les événements dans lesquels une seule occurrence indique une activité non autorisée  

2.  Une accumulation d’événements différents de la référence acceptée et attendue  

Voici un exemple de premier événement :  

Si les administrateurs de domaine (DAs) sont interdits de se connecter à des ordinateurs qui ne sont pas des contrôleurs de domaine, une seule occurrence d’un membre DA ouvrant une session sur une station de travail d’utilisateur final doit générer une alerte et être examinée. Ce type d’alerte est facile à générer à l’aide de l’événement d’audit Special Logon 4964 (des groupes spéciaux ont été attribués à une nouvelle ouverture de session). Voici d’autres exemples d’alertes à instance unique :  

-   Si le serveur A ne doit jamais se connecter au serveur B, alerte quand il se connecte.  

-   Alerte si un compte d’utilisateur final normal est ajouté de manière inattendue à un groupe de sécurité sensible.  

-   Si les employés de l’emplacement de la fabrique ne fonctionnent jamais à la nuit, alerte quand un utilisateur ouvre une session à minuit.  

-   Alerte si un service non autorisé est installé sur un contrôleur de domaine.  

-   Examinez si un utilisateur final régulier tente de se connecter directement à un SQL Server pour lequel il n’a pas de raison claire de le faire.  

-   Si vous ne disposez d’aucun membre dans votre groupe DA et que quelqu’un s’y ajoute, vérifiez-le immédiatement.  

Voici un exemple de deuxième événement :  

Un nombre d’échecs d’ouverture de session anormal peut indiquer une attaque par le biais d’un mot de passe. Pour qu’une entreprise fournisse une alerte pour un nombre anormalement élevé d’échecs de connexion, ceux-ci doivent d’abord comprendre les niveaux normaux des échecs de connexion dans leur environnement avant un événement de sécurité malveillant.  

Pour obtenir une liste complète des événements que vous devez inclure lorsque vous surveillez les signes de compromission, consultez [L’annexe L : événements à surveiller](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md).  

## <a name="active-directory-objects-and-attributes-to-monitor"></a>Active Directory les objets et les attributs à analyser  
Voici les comptes, les groupes et les attributs que vous devez surveiller pour vous aider à détecter les tentatives de compromission de votre installation Active Directory Domain Services.  

-   Systèmes pour la désactivation ou la suppression d’un logiciel antivirus et anti-programme malveillant (redémarrer automatiquement la protection lorsqu’elle est désactivée manuellement)  

-   Comptes d’administrateur pour les modifications non autorisées  

-   Activités effectuées à l’aide de comptes privilégiés (supprimer automatiquement le compte quand les activités suspectes sont terminées ou que le temps alloué a expiré)  

-   Comptes privilégiés et VIP dans AD DS. Surveiller les modifications, en particulier les modifications apportées aux attributs de l’onglet compte (par exemple, CN, nom, sAMAccountName, userPrincipalName ou userAccountControl). En plus de surveiller les comptes, limitez les personnes autorisées à modifier les comptes pour un ensemble d’utilisateurs administratifs aussi petit que possible.  

Reportez-vous à [L’annexe L : événements pour surveiller](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md) la liste des événements recommandés à surveiller, leurs évaluations de gravité et un résumé des messages d’événement.  

-   Regroupez les serveurs en fonction de la classification de leurs charges de travail, ce qui vous permet d’identifier rapidement les serveurs qui doivent être les plus étroitement surveillés et les plus rigoureusement configurés.  

-   Modifications apportées aux propriétés et à l’appartenance aux groupes de AD DS suivants : administrateurs de l’entreprise (EA), admins du domaine (DA), administrateurs (BA) et administrateurs du schéma (SA)  

-   Comptes privilégiés désactivés (tels que les comptes d’administrateur intégrés dans Active Directory et sur les systèmes membres) pour l’activation des comptes  

-   Comptes de gestion pour journaliser toutes les écritures dans le compte  

-   Assistant Configuration de sécurité intégré pour configurer les paramètres du service, du Registre, de l’audit et du pare-feu afin de réduire la surface d’attaque du serveur. Utilisez cet Assistant si vous implémentez des serveurs de saut dans le cadre de votre stratégie d’hôte administratif.  

## <a name="additional-information-for-monitoring-active-directory-domain-services"></a>Informations supplémentaires pour l’analyse des Active Directory Domain Services  
Pour plus d’informations sur la surveillance AD DS, consultez les liens suivants :  
  
-   L’audit de l' [accès global aux objets est](https://blogs.technet.com/b/askds/archive/2011/03/10/global-object-access-auditing-is-magic.aspx) un moyen de vous fournir des informations sur la configuration et l’utilisation de la configuration avancée de la stratégie d’audit qui a été ajoutée à Windows 7 et windows Server 2008 R2.  

-   [Présentation des modifications d’audit dans windows 2008](https://blogs.technet.com/b/askds/archive/2007/10/19/introducing-auditing-changes-in-windows-2008.aspx) : présente les modifications apportées à l’audit dans Windows 2008.  

-   [Astuces d’audit intéressantes dans Vista et 2008](https://blogs.technet.com/b/askds/archive/2007/11/16/cool-auditing-tricks-in-vista-and-2008.aspx) : présente les nouvelles fonctionnalités intéressantes d’audit dans Windows Vista et windows Server 2008 qui peuvent être utilisées pour résoudre des problèmes ou voir ce qui se passe dans votre environnement.  

-   [One-Stop pour l’audit dans Windows server 2008 et Windows Vista](https://blogs.technet.com/b/askds/archive/2008/03/27/one-stop-shop-for-auditing-in-windows-server-2008-and-windows-vista.aspx) : contient une compilation des fonctionnalités d’audit et des informations contenues dans windows Server 2008 et Windows Vista.  

-   [Guide pas à pas d’audit de AD DS](https://technet.microsoft.com/library/a9c25483-89e2-4202-881c-ea8e02b4b2a5.aspx) : décrit la nouvelle fonctionnalité d’audit Active Directory Domain Services (AD DS) dans Windows Server 2008. Il fournit également des procédures permettant d’implémenter cette nouvelle fonctionnalité.  

## <a name="general-list-of-security-event-id-recommendation-criticalities"></a>Liste générale des impératifs de recommandation relatives aux ID d’événement de sécurité  
Toutes les recommandations relatives aux ID d’événement sont accompagnées d’une évaluation de la criticité, comme suit :  

**Haute :** Les ID d’événement dont le degré de gravité est élevé doivent toujours et immédiatement être alertés et examinés.  

**Moyenne :** Un ID d’événement avec une évaluation de la gravité moyenne peut indiquer une activité malveillante, mais il doit être accompagné d’une autre anomalie (par exemple, un nombre inhabituel dans un laps de temps donné, des occurrences inattendues ou des occurrences sur un ordinateur qui ne seraient normalement pas censés consigner l’événement). Un événement de gravité moyenne peut également être collecté comme mesure et comparé au fil du temps.  

**Faible :** Et l’ID d’événement avec des événements de faible importance ne doivent pas donner leur attention ou provoquer des alertes, sauf s’ils sont corrélés à des événements de gravité moyenne ou élevée.  

Ces recommandations sont destinées à fournir un guide de référence pour l’administrateur. Toutes les recommandations doivent être examinées minutieusement avant l’implémentation dans un environnement de production.  

Reportez-vous à [L’annexe L : événements pour surveiller](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md) la liste des événements recommandés à surveiller, leurs évaluations de gravité et un résumé des messages d’événement.  
