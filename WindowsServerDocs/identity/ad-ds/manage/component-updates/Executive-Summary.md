---
ms.assetid: 85ca191c-0cc7-4453-a72c-42060ddf2ea2
title: Résumé
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: a3eecec9d47f91bb6a9ba549abc3bf62482b2f49
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863280"
---
# <a name="executive-summary"></a>Résumé

>S'applique à : Windows Server 2012

>[!IMPORTANT] 
>La documentation suivante a été écrite en 2013 et est fournie uniquement à des fins d’historique.  Actuellement, nous allons consulter cette documentation et il est susceptible de changer.  Il ne reflète pas les bonnes pratiques en vigueur.

Aucune organisation dotée d’une infrastructure informatique (IT) n’est immune contre les attaques, mais si des contrôles, les processus et les stratégies appropriées sont implémentées pour protéger les principales composantes de l’infrastructure informatique d’une organisation, il est parfois possible pour empêcher un événement de violation d’augmenter pour un compromis en gros de l’environnement informatique.  
  
Ce résumé est destinée à être utile en tant que document autonome récapitulant le contenu du document, qui contient des recommandations qui aideront les organisations dans l’amélioration de la sécurité de leurs installations d’Active Directory. En implémentant ces recommandations, les organisations pourront identifier et hiérarchiser les activités de sécurité, protéger les principales composantes de l’infrastructure informatique de leur organisation et créer des contrôles qui réduisent considérablement la probabilité de attaques réussies contre les composants critiques de l’environnement informatique.  
  
Bien que ce document aborde les attaques courantes contre Active Directory et les contre-mesures pour réduire la surface d’attaque, il contient également des recommandations pour la récupération en cas de compromission complète. La seule façon pour récupérer en cas de compromission complète d’Active Directory doit être préparé pour la compromission avant qu’elle se produise.  
  
Les principales sections de ce document sont :  
  
-   Voies de compromis  
  
-   Réduire la Surface d'attaque Active Directory  
  
-   Surveillance des signes de compromission d'Active Directory  
  
-   Planification des compromis  
  
## <a name="avenues-to-compromise"></a>Voies de compromis  
Cette section fournit des informations sur certaines des vulnérabilités exploitées plus couramment utilisées par les pirates pour compromettre les infrastructures clients. Il contient des catégories générales de vulnérabilités et comment ils sont utilisés pour initialement pénétrer dans les infrastructures clients, propager compromission sur d’autres systèmes et finalement cibler Active Directory et contrôleurs de domaine pour obtenir terminée contrôle des forêts de l’organisation. Il ne fournit pas de recommandations détaillées sur l’adressage de chaque type de vulnérabilité, en particulier dans les domaines dans lequel les vulnérabilités ne sont pas utilisées pour cibler directement Active Directory. Toutefois, pour chaque type de vulnérabilité, nous avons compilé les liens vers des informations supplémentaires à utiliser pour développer des contre-mesures et réduire la surface d’attaque de l’organisation.  
  
Voici les sujets suivants :  
  
-   **Initiale des cibles de violation** -la plupart des violations de sécurité démarrent avec la compromission de petits morceaux de systèmes d’infrastructure souvent un ou deux d’une organisation à la fois. Ces événements initiales, ou les points d’entrée dans le réseau, souvent exploitent des vulnérabilités qui peuvent avoir été résolues, mais n’ont pas été. Des vulnérabilités couramment rencontrées sont :  
  
    -   Écarts dans les déploiements d’antivirus et anti-programme malveillant  
  
    -   Mise à jour corrective incomplète  
  
    -   Les applications sont obsolètes et les systèmes d’exploitation  
  
    -   Misconfiguration  
  
    -   Absence de pratiques de développement d’applications sécurisées  
  
-   **Comptes attrayants pour le vol d’informations d’identification** -vol de ces informations d’identification est celles dans lesquelles un attaquant initialement Obtient un accès privilégié à un ordinateur sur un réseau et utilise ensuite les outils disponibles gratuitement pour extraire des informations d’identification depuis les sessions des autres comptes connectés.   
    Inclus dans cette section sont les suivantes :  
  
    -   **Les activités qui augmentent la probabilité de compromission** - parce que la cible de vol d’informations d’identification est généralement des comptes de domaine disposant de privilèges élevés et « person très important » comptes (VIP), il est important que les administrateurs à être attentif activités qui augmentent la probabilité de réussite d’une attaque de vol d’informations d’identification. Ces activités sont :  
  
        -   Ouvrir une session sur des ordinateurs non sécurisés avec des comptes privilégiés  
  
        -   Naviguer sur Internet avec un compte disposant de privilèges élevés  
  
        -   Configuration des comptes privilégiés locaux avec les mêmes informations d’identification dans les systèmes  
  
        -   Overpopulation et l’utilisation abusive des groupes de domaine avec privilèges  
  
        -   Gestion insuffisante de la sécurité des contrôleurs de domaine.  
  
    -   **Privilèges d’élévation et la Propagation** -comptes spécifiques, les serveurs et les composants d’infrastructure sont généralement les cibles principales d’attaques par rapport à Active Directory. Ces comptes sont :  
  
        -   Comptes privilégiés définitivement  
  
        -   Comptes d’adresse IP virtuelle  
  
        -   « Attaché au privilège » des comptes Active Directory  
  
        -   Contrôleurs de domaine  
  
        -   Autres services d’infrastructure qui affectent la gestion des identités, les accès et les configuration, tels que les serveurs d’infrastructure à clé publique (PKI) et les serveurs d’administration de systèmes  
  
## <a name="reducing-the-active-directory-attack-surface"></a>Réduire la Surface d'attaque Active Directory  
Cette section se concentre sur les contrôles techniques pour réduire la surface d’attaque d’une installation d’Active Directory. Inclus dans cette section sont les sujets suivants :  
  
-   Le **comptes privilégiés et groupes dans Active Directory** section décrit les comptes à privilèges plus élevés et les groupes dans Active Directory et les mécanismes par lequel les comptes privilégiés sont protégés. Dans Active Directory, trois groupes intégrés sont les groupes de privilège le plus élevés dans le répertoire (administrateurs de l’entreprise, Admins du domaine et administrateurs), même si un nombre de comptes et des groupes supplémentaires doit également être protégé.  
  
-   Le **implémentation de modèles d’administration doté des privilèges minimaux** section se concentre sur l’identification des risques et présentant l’utilisation de comptes à privilèges élevés pour l’administration quotidienne, en plus de fournir des recommandations pour réduire ce risque.  
  
Un privilège excessif n’est pas uniquement trouvé dans Active Directory dans les environnements compromis. Lorsqu’une organisation a développé l’habitude d’accorder le privilège plus élevé que nécessaire, il se trouve généralement dans l’infrastructure :  
  
-   Dans Active Directory  
  
-   Sur les serveurs membres  
  
-   Sur les stations de travail  
  
-   Dans les applications  
  
-   Dans les référentiels de données  
  
-   Le **implémentation des hôtes d’administration sécurisés** section décrit les hôtes d’administration sécurisés, qui sont des ordinateurs qui sont configurés pour prendre en charge d’administration d’Active Directory et des systèmes connectés. Ces hôtes sont dédiées à des fonctionnalités d’administration et n’exécutent pas de logiciels tels que les applications de messagerie, des navigateurs web ou des logiciels de productivité (telles que Microsoft Office).  
  
Inclus dans cette section sont les suivantes :  
  
-   **Principes de création de sécuriser les ordinateurs hôtes d’administration** -les principes généraux à prendre en compte sont :  
  
    -   Administrer jamais un système approuvé à partir d’un hôte de niveau de confiance moindre.  
  
    -   Ne comptez pas sur un facteur d’authentification unique lors de l’exécution des activités privilégiées.  
  
    -   N’oubliez pas de sécurité physique lors de la conception et implémentation des hôtes d’administration sécurisés.  
  
-   **Sécurisation des contrôleurs de domaine contre les attaques** -si un utilisateur malveillant obtient un accès privilégié à un contrôleur de domaine, cet utilisateur peut modifier endommagé et détruire la base de données Active Directory et par extension, tous les systèmes et les comptes géré par Active Directory.  
  
Inclus dans cette section sont les sujets suivants :  
  
-   **Sécurité physique des contrôleurs de domaine** -contient des recommandations pour apporter une sécurité physique pour les contrôleurs de domaine dans les centres de données, les succursales et les emplacements distants.  
  
-   **Systèmes d’exploitation de contrôleur de domaine** -contient des recommandations pour la sécurisation des systèmes d’exploitation de contrôleur de domaine.  
  
-   **Configuration de contrôleurs de domaine sécurisés** -paramètres et outils de configuration Native et disponibles gratuitement peuvent être utilisés pour créer des configurations de référence pour les contrôleurs de domaine qui peuvent être appliquées par la suite par des objets de stratégie de groupe (sécurité Stratégie de groupe).  
  
## <a name="monitoring-active-directory-for-signs-of-compromise"></a>Surveillance des signes de compromission d'Active Directory  
Cette section fournit des informations sur les catégories d’audit héritées et des sous-catégories de stratégie d’audit (qui ont été introduits dans Windows Vista et Windows Server 2008) et stratégie d’Audit avancée (qui a été introduite dans Windows Server 2008 R2). Également fourni d’informations sur les événements et les objets pour surveiller qui peuvent indiquer des tentatives de compromettre l’environnement et des références supplémentaires qui peuvent être utilisées pour construire une stratégie d’audit complète pour Active Directory.  
  
Inclus dans cette section sont les sujets suivants :  
  
-   **Stratégie d’Audit de Windows** - journaux d’événements de sécurité de Windows ont les catégories et sous-catégories qui déterminent les événements de sécurité sont suivies et enregistrées.  
  
-   **Recommandations de stratégie d’audit** -cette section décrit les paramètres de stratégie d’audit de Windows par défaut, les paramètres de stratégie qui sont recommandées par Microsoft et des recommandations plus agressives pour les organisations à utiliser pour auditer les serveurs critiques d’audit et stations de travail.  
  
## <a name="planning-for-compromise"></a>Planification des compromis  
Cette section contient des recommandations qui permettent aux organisations de préparer un compromis avant qu’elle se produise, instaurez des contrôles qui peuvent détecter un événement de compromission avant une infraction complète et fournissent des instructions de réponse et de récupération pour les cas dans lesquels une compromission complète du répertoire est obtenue par des attaquants. Inclus dans cette section sont les sujets suivants :  
  
-   **Repenser l’approche** -contient des principes et des instructions pour créer des environnements sécurisés dans lequel une organisation peut placer leurs ressources les plus critiques. Ces directives sont les suivantes :  
  
    -   Identification des principes de séparation et de sécurisation des ressources critiques  
  
    -   Définition d’un plan de migration limitées, en fonction des risques  
  
    -   En tirant parti des migrations « nonmigratory » lorsque cela est nécessaire  
  
    -   Implémentation de « destruction creative »  
  
    -   Isoler les systèmes hérités et les applications  
  
    -   Simplifiez-vous la sécurité pour les utilisateurs finaux  
  
-   **Maintenance d’un environnement plus sécurisé** -contient des recommandations de haut niveau destinées à être utilisés comme des lignes directrices à utiliser dans le développement de sécurité effectives non seulement, mais gestion du cycle de vie effective. Inclus dans cette section sont les sujets suivants :  
  
    -   **Création des pratiques de sécurité centrée sur l’entreprise pour Active Directory** - pour efficacement gérer le cycle de vie des utilisateurs, des données, des applications et des systèmes gérés par Active Directory, appliquez ces principes.  
  
        -   **Attribuer une propriété de données Active Directory** -attribuer la propriété de composants d’infrastructure informatique ; pour les données qui sont ajoutées à Active Directory Domain Services (AD DS) pour prendre en charge de l’entreprise, par exemple, nouveaux employés, nouvelles applications, et de nouveaux référentiels d’informations, une unité commerciale désigné ou un utilisateur doit être associé aux données.  
  
        -   **Implémenter la gestion du cycle de vie Business-Driven** -gestion du cycle de vie doit être implémentée pour les données dans Active Directory.  
  
        -   **Classer toutes les données Active Directory** -chefs d’entreprise doivent fournir la classification des données dans Active Directory. Dans le modèle de classification, la classification pour les données d’Active Directory suivantes doit être incluse :  
  
            -   **Systèmes** -classer les applications s’exécutant sur ceux-ci et de l’informatique de leur rôle, alimentations du serveur, leur système d’exploitation et les propriétaires de l’enregistrement.  
  
            -   **Applications** -classer les applications par la fonctionnalité de base d’utilisateurs et leur système d’exploitation.  
  
            -   **Les utilisateurs** -les comptes dans les installations Active Directory qui sont susceptibles d’être la cible d’attaquants doivent être balisés et surveillées.  
  
## <a name="summary-of-best-practices-for-securing-active-directory-domain-services"></a>Résumé des bonnes pratiques pour la sécurisation des Services de domaine Active Directory  
Le tableau suivant fournit un résumé des recommandations fournies dans ce document pour la sécurisation d’une installation AD DS. Quelques-unes des meilleures pratiques sont par nature stratégiques et nécessitent une planification complète et projets d’implémentation ; d’autres sont tactiques et se concentrent sur des composants spécifiques d’Active Directory et l’infrastructure associée.  
  
Pratiques sont répertoriés dans l’ordre croissant de priorité, qui l’est., les nombres inférieurs indiquent une priorité plus élevée. Où applicable aux meilleures pratiques sont identifiés comme préventives ou détective par nature. Toutes ces recommandations doivent être soigneusement testé et modifié en fonction des besoins pour les caractéristiques et les exigences de votre organisation.  
  
  
||**Il est recommandé**|**Tactique ou stratégique**|**Préventives ou détective**|  
|-|-|-|-|  
|1|Applications de correctifs.|Tactique|Préventives|  
|2|Correctifs des systèmes d’exploitation.|Tactique|Préventives|  
|3|Déployez et rapidement mettre à jour des logiciels antivirus et anti-programme malveillant sur tous les systèmes et surveiller les tentatives de supprimer ou désactiver.|Tactique|Les deux|  
|4|Surveiller les objets Active Directory sensibles pour les tentatives de modification et Windows pour les événements qui peuvent indiquer un abus tentée.|Tactique|Détective|  
|5|Protéger et contrôler les comptes pour les utilisateurs qui ont accès aux données sensibles|Tactique|Les deux|  
|6|Comptes puissants empêchent l’utilisation sur les systèmes non autorisés.|Tactique|Préventives|  
|7|Éliminer l’appartenance permanente aux groupes disposant de privilèges élevés.|Tactique|Préventives|  
|8|Implémenter des contrôles pour accorder une appartenance temporaire à des groupes privilégiés en cas de besoin.|Tactique|Préventives|  
|9|Implémenter des hôtes d’administration sécurisés.|Tactique|Préventives|  
|10|Utilisez la liste verte d’applications sur les contrôleurs de domaine, les hôtes d’administration et les autres systèmes sensibles.|Tactique|Préventives|  
|11|Identifier les ressources critiques et de hiérarchiser leur sécurité et de surveillance.|Tactique|Les deux|  
|12|Implémentez des contrôles d’accès de moindre privilège, en fonction du rôle pour l’administration de l’annuaire, son infrastructure de prise en charge et des systèmes joints à un domaine.|Stratégique|Préventives|  
|13|Isoler les applications et les systèmes hérités.|Tactique|Préventives|  
|14|Mettre hors service des applications et les systèmes hérités.|Stratégique|Préventives|  
|15|Implémenter des programmes de cycle de vie de développement sécurisé pour les applications personnalisées.|Stratégique|Préventives|  
|16|Implémenter la gestion de la configuration, vérifiez régulièrement la conformité et évaluer les paramètres avec chaque nouvelle version de matériel ou logiciel.|Stratégique|Préventives|  
|17|Faites migrer des ressources critiques à des forêts vierge avec les exigences de surveillance et de sécurité strictes.|Stratégique|Les deux|  
|18|Simplifiez la sécurité pour les utilisateurs finaux.|Stratégique|Préventives|  
|19|Utilisez des pare-feu d’hôte pour le contrôle et de sécuriser les communications.|Tactique|Préventives|  
|20|Appareils de correctif.|Tactique|Préventives|  
|21|Implémenter la gestion du cycle de vie centrée sur l’entreprise pour les ressources informatiques.|Stratégique|N/A|  
|22|Créer ou mettre à jour des plans de récupération de l’incident.|Stratégique|N/A|  
  


