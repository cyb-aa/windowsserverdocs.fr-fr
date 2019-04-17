---
ms.assetid: 85ca191c-0cc7-4453-a72c-42060ddf2ea2
title: "Résumé"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c218a4d66e8208cf627bc93be50bf11ea2fbf862
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="executive-summary"></a>Résumé

>S’applique à: Windows Server2012

>[!IMPORTANT] 
>La documentation suivante a été écrit dans 2013 et est fournie uniquement à des fins d’historique.  Actuellement, nous allons consulter cette documentation et il est susceptible d’être modifiée.  Il peut ne pas refléter les meilleures pratiques en vigueur.

Aucune organisation dotée d’une infrastructure informatique (IT) n’est protégée contre les attaques, mais si les contrôles, les processus et les stratégies appropriées sont implémentés pour protéger les segments de clé de l’infrastructure informatique d’une organisation, il est possible d’empêcher un événement de violation de croître un gros compromission de l’environnement informatique.  
  
Cette synthèse est destinée à être utilisées en tant que document autonome résumant le contenu du document, qui contient des recommandations qui aident les organisations de renforcer la sécurité de leur installation ActiveDirectory. En mettant en œuvre ces recommandations, les organisations sera en mesure d’identifier et hiérarchiser les activités de sécurité, protéger les principales composantes de l’infrastructure informatique de leur organisation et créer des contrôles qui réduisent considérablement la probabilité d’attaques réussies contre les composants essentiels de l’environnement informatique.  
  
Bien que ce document aborde les attaques contre ActiveDirectory et des contre-mesures pour réduire la surface d’attaque les plus courants, il contient également des recommandations pour la récupération en cas de compromission. L’uniquement que récupérer en cas de compromission d’ActiveDirectory consiste à être préparé pour la compromission avant que cela se produit.  
  
Les principales sections de ce document sont:  
  
-   Voies de compromis  
  
-   Ce qui réduit la Surface d’attaque ActiveDirectory  
  
-   Surveillance des signes de compromission d’ActiveDirectory  
  
-   Planification des compromis  
  
## <a name="avenues-to-compromise"></a>Voies de compromis  
Cette section fournit des informations sur certaines des vulnérabilités exploitées plus couramment utilisées par les personnes malveillantes pour compromettre les infrastructures clients. Il contient des catégories générales des vulnérabilités et comment ils sont utilisés initialement malgré les infrastructures clients, se propage compromis sur des systèmes supplémentaires et finalement cible ActiveDirectory et contrôleurs de domaine pour obtenir le contrôle intégral des forêts de l’entreprise. Il ne fournit pas de recommandations détaillées sur l’adressage de chaque type de vulnérabilité, en particulier dans les domaines dans lesquels les vulnérabilités ne servent pas à cibler directement ActiveDirectory. Toutefois, pour chaque type de vulnérabilité, nous avons fourni des liens vers des informations supplémentaires à utiliser pour développer des contre-mesures et réduire la surface d’attaque de l’organisation.  
  
Voici les sujets suivants:  
  
-   **Initiale violation cibles** -la plupart des violations de sécurité démarrent avec la compromission de petites parties d’une organisation infrastructure souvent un ou deux systèmes à la fois. Ces événements initiales, ou les points d’entrée dans le réseau, souvent exploitent des vulnérabilités qui pourraient avoir été résolues, mais qui n’ont pas été. Des vulnérabilités couramment affichées sont:  
  
    -   Lacunes dans les déploiements de logiciels antivirus et anti-programme malveillant  
  
    -   Mise à jour corrective incomplète  
  
    -   Systèmes d’exploitation et les applications obsolètes  
  
    -   Configuration incorrecte  
  
    -   Manque de méthodes de développement d’applications sécurisées  
  
-   **Comptes intéressantes pour le vol d’informations d’identification** -attaques de vol d’informations d’identification sont ceux dans lequel une personne malveillante parvient initialement à accès privilégié à un ordinateur sur un réseau, puis utilise outils disponible gratuitement pour extraire des informations d’identification les sessions d’autres comptes connectés.   
    Inclus dans cette section sont les suivants:  
  
    -   **Les activités qui augmente le risque de compromission** - car la cible de vol d’informations d’identification est généralement des comptes de domaine disposant de privilèges élevés et «personne très importante» comptes (VIP), il est important pour les administrateurs à être conscient des activités qui augmentent la probabilité de réussite d’une attaque par usurpation d’informations d’identification. Ces activités sont:  
  
        -   Ouverture de session sur des ordinateurs non sécurisés avec des comptes privilégiés  
  
        -   La navigation sur Internet avec un compte à privilèges élevés  
  
        -   La configuration des comptes privilégiés locales avec les mêmes informations d’identification sur les systèmes  
  
        -   Overpopulation et une utilisation abusive des groupes de domaine privilégiés  
  
        -   Gestion insuffisante de la sécurité des contrôleurs de domaine.  
  
    -   **Élévation et la Propagation de privilège** -des comptes spécifiques, les serveurs et les composants d’infrastructure sont généralement les cibles principales d’attaques par rapport à ActiveDirectory. Ces comptes sont:  
  
        -   Comptes privilégiés définitivement  
  
        -   Comptes VIP  
  
        -   «Attaché au privilège» les comptes ActiveDirectory  
  
        -   Contrôleurs de domaine  
  
        -   Autres services d’infrastructure qui affectent la gestion d’identité, accès et la configuration, tels que les serveurs d’infrastructure à clé publique (PKI) et les serveurs de systèmes de gestion  
  
## <a name="reducing-the-active-directory-attack-surface"></a>Ce qui réduit la Surface d’attaque ActiveDirectory  
Cette section se concentre sur les contrôles techniques pour réduire la surface d’attaque d’une installation d’ActiveDirectory. Inclus dans cette section sont les sujets suivants:  
  
-   Le **comptes privilégiés et groupes dans ActiveDirectory** section décrit les comptes disposant de privilèges plus élevés et les groupes dans ActiveDirectory et les mécanismes par lequel les comptes privilégiés sont protégés. Dans ActiveDirectory, trois groupes intégrés sont les groupes de privilège le plus élevés dans le répertoire (administrateurs de l’entreprise, Admins du domaine et administrateurs), même si un nombre d’autres groupes et comptes doit également être protégé.  
  
-   Le **implémentation de modèles d’administration de moindre privilège** section se concentre sur l’identification le risque que l’utilisation de privilèges très élevée de comptes pour l’administration quotidienne présente, en outre à fournir des recommandations pour réduire ce risque.  
  
Un privilège excessif ne se trouve dans ActiveDirectory uniquement dans les environnements compromis. Lorsqu’une organisation a développé l’habitude d’accorder plus de privilèges que nécessaire, il se trouve généralement dans l’ensemble de l’infrastructure:  
  
-   Dans ActiveDirectory  
  
-   Sur les serveurs membres  
  
-   Sur les stations de travail  
  
-   Dans les applications  
  
-   Dans les référentiels de données  
  
-   Le **implémentation des hôtes d’administration sécurisés** section décrit les hôtes d’administration sécurisés, qui sont des ordinateurs qui sont configurés pour prendre en charge d’administration d’ActiveDirectory et des systèmes connectés. Ces ordinateurs hôtes sont dédiées aux fonctionnalités d’administration et n’exécutent pas les logiciels tels que les applications de messagerie, les navigateurs web ou les logiciels de productivité (par exemple, MicrosoftOffice).  
  
Inclus dans cette section sont les suivants:  
  
-   **Principes de création de sécuriser les ordinateurs hôtes d’administration** -sont les principes généraux à prendre en considération:  
  
    -   Administrer jamais un système approuvé à partir d’un hôte approuvé moins.  
  
    -   Ne comptez pas sur un facteur d’authentification unique lors de l’exécution des activités privilégiées.  
  
    -   N’oubliez pas de sécurité physique lors de la conception et l’implémentation des hôtes d’administration sécurisés.  
  
-   **Sécurisation des contrôleurs de domaine contre les attaques** -si un utilisateur malveillant obtienne l’accès privilégié à un contrôleur de domaine, cet utilisateur peut modifier, endommagées ou détruire la base de données ActiveDirectory et par extension, tous les systèmes et les comptes qui sont gérés par ActiveDirectory.  
  
Inclus dans cette section sont les sujets suivants:  
  
-   **Sécurité physique des contrôleurs de domaine** -contient des recommandations pour fournir une sécurité physique des contrôleurs de domaine dans les centres de données, les succursales et emplacements distants.  
  
-   **Systèmes d’exploitation de contrôleur de domaine** -contient des recommandations pour la sécurisation des systèmes d’exploitation de contrôleur de domaine.  
  
-   **Configuration des contrôleurs de domaine sécurisés** -paramètres et outils natifs et disponible gratuitement configuration peuvent servir à créer des lignes de base de configuration pour les contrôleurs de domaine qui peuvent ensuite être appliquées par les objets de stratégie de groupe (GPO) de sécurité.  
  
## <a name="monitoring-active-directory-for-signs-of-compromise"></a>Surveillance des signes de compromission d’ActiveDirectory  
Cette section fournit des informations sur les catégories d’audit héritées et sous-catégories de stratégie d’audit (qui ont été introduits dans WindowsVista et Windows Server2008) et avancée de stratégie d’Audit (qui a été introduite dans Windows Server2008R2). Également fourni est d’informations sur les événements et les objets pour contrôler qui peuvent indiquer des tentatives de compromettre l’environnement et certaines références supplémentaires qui peuvent être utilisés pour construire une stratégie d’audit complète pour ActiveDirectory.  
  
Inclus dans cette section sont les sujets suivants:  
  
-   **Stratégie d’Audit de Windows**: journaux d’événements de sécurité de Windows ont des catégories et sous-catégories qui déterminent les événements de sécurité sont suivies et enregistrées.  
  
-   **Auditer les recommandations en matière de stratégie** -cette section décrit les paramètres de stratégie d’audit de Windows par défaut, les paramètres de stratégie qui sont recommandés par Microsoft et des recommandations plus agressives pour les organisations à utiliser pour auditer les stations de travail et serveurs critiques d’audit.  
  
## <a name="planning-for-compromise"></a>Planification des compromis  
Cette section contient des recommandations qui aident les organisations à préparer un compromis avant que cela se produit, instaurez des contrôles qui peuvent détecter un événement de compromission avant une violation complète s’est produite et fournissent des instructions de réponse et de récupération pour les cas dans lesquels une totale compromission de l’annuaire est obtenue par les personnes malveillantes. Inclus dans cette section sont les sujets suivants:  
  
-   **Repenser l’approche** -contient les principes et recommandations pour créer des environnements sécurisés dans lequel une organisation peut placer leurs ressources les plus critiques. Ces recommandations sont les suivantes:  
  
    -   Identification des principes de séparation et la sécurisation des ressources critiques  
  
    -   Définition d’un plan de migration limitée, des risques  
  
    -   Tirant parti des migrations «nonmigratory» le cas échéant  
  
    -   Implémentation de «destruction creative»  
  
    -   Isoler les applications et des systèmes hérités  
  
    -   Simplification de sécurité pour les utilisateurs finaux  
  
-   **Maintenance d’un environnement plus sécurisé** -contient des recommandations de haut niveau destinées à être utilisés en tant que les instructions à utiliser dans le développement non seulement de sécurité efficace, mais de gestion du cycle de vie effective. Inclus dans cette section sont les sujets suivants:  
  
    -   **Créer des pratiques de sécurité centrée sur l’entreprise pour ActiveDirectory** - pour efficacement gérer le cycle de vie des utilisateurs, des données, des applications et des systèmes gérés par ActiveDirectory, suivez ces principes.  
  
        -   **Affecter un propriétaire de l’entreprise pour les données ActiveDirectory** -attribuer la propriété de composants d’infrastructure informatique; pour les données qui sont ajoutées aux Services de domaine ActiveDirectory (ADDS) pour prendre en charge de l’entreprise, par exemple, les nouveaux employés, les nouvelles applications et les nouveaux référentiels d’informations, une unité commerciale désigné ou un utilisateur doit être associé aux données.  
  
        -   **Implémenter la gestion du cycle de vie Business-Driven** -gestion du cycle de vie doit être implémentée pour les données dans ActiveDirectory.  
  
        -   **Classer toutes les données ActiveDirectory** -propriétaires doivent fournir la classification de données dans ActiveDirectory. Dans le modèle de classification des données, la classification de données ActiveDirectory suivantes doit être incluse:  
  
            -   **Systèmes** -classer les applications en cours d’exécution, et de l’informatique de leur rôle, populations de serveur, leur système d’exploitation et les propriétaires d’enregistrement.  
  
            -   **Applications** -classer les applications par la fonctionnalité de base d’utilisateurs et leur système d’exploitation.  
  
            -   **Les utilisateurs** -les comptes dans les installations ActiveDirectory qui sont plus susceptibles d’être ciblés par les personnes malveillantes doivent être marqués et analysés.  
  
## <a name="summary-of-best-practices-for-securing-active-directory-domain-services"></a>Résumé des meilleures pratiques pour la sécurisation des Services de domaine ActiveDirectory  
Le tableau suivant fournit un résumé des recommandations fournies dans ce document pour la sécurisation d’une installation d’ADDS. Certaines meilleures pratiques sont par nature stratégiques et nécessitent une planification complète et projets d’implémentation; d’autres sont tactiques et se concentrent sur des composants spécifiques d’ActiveDirectory et l’infrastructure associée.  
  
Pratiques sont répertoriés dans l’ordre croissant de priorité, qui l’est., les nombres inférieurs indiquent une priorité élevée. Où les meilleures pratiques applicables sont identifiés comme préventives ou détection de nature. Toutes ces recommandations doivent être soigneusement testé et modifié en fonction des besoins pour les caractéristiques et les exigences de votre organisation.  
  
  
|-|-|-|-|  
||**Meilleures pratiques**|**tactique ou stratégique**|**préventives ou détection**|  
| 1 | Applications de correctif. | Tactique | Prévention |  
| 2 | Systèmes d’exploitation de correctif. | Tactique | Prévention |  
| 3 | Déployer et rapidement mettre à jour de logiciels antivirus et anti-programme malveillant sur tous les systèmes et analyse des tentatives de supprimer ou de le désactiver. | Tactique | Les deux |  
| 4 | Analyser les objets ActiveDirectory sensibles pour les tentatives de modification et Windows pour les événements qui peuvent indiquer un abus tentée. | Tactique | Détection |  
| 5 | Protéger et de contrôler les comptes d’utilisateurs qui ont accès aux données sensibles | Tactique | Les deux |  
| 6 | Comptes puissants empêcher l’utilisation sur les systèmes non autorisés. | Tactique | Prévention |  
| 7 | Éliminer l’appartenance permanente aux groupes dotés de privilèges élevés. | Tactique | Prévention |  
| 8 | Implémenter des contrôles pour accorder une appartenance temporaire à des groupes privilégiés en cas de besoin. | Tactique | Prévention |  
| 9 | Implémenter des hôtes d’administration sécurisés. | Tactique | Prévention |  
| 10 | Utilisez la liste verte d’applications sur les contrôleurs de domaine, les ordinateurs hôtes d’administration et les autres systèmes sensibles. | Tactique | Prévention |  
| 11 | Identifier les ressources critiques et de hiérarchiser leur sécurité et de contrôle. | Tactique | Les deux |  
| 12 | Implémenter des contrôles d’accès de privilèges et les rôles pour l’administration des systèmes joints au domaine, son infrastructure prise en charge et le répertoire. | Stratégique | Prévention |  
| 13 | Isoler les applications et des systèmes hérités. | Tactique | Prévention |  
| 14 | Retirer des applications et des systèmes hérités. | Stratégique | Prévention |  
| 15 | Implémenter le développement sécurisées du cycle de vie des programmes pour les applications personnalisées. | Stratégique | Prévention |  
| 16 | Implémenter la gestion de la configuration, examinez la conformité régulièrement et évaluer les paramètres à chaque nouvelle version matérielle ou logicielle. | Stratégique | Prévention |  
| 17 | Migrer les ressources critiques à des forêts inchangés avec sécurité stricts et les exigences de surveillance. | Stratégique | Les deux |  
| 18 | Simplifiez la sécurité pour les utilisateurs finaux. | Stratégique | Prévention |  
| 19 | Utilisez des pare-feu d’hôte pour le contrôle et sécuriser les communications. | Tactique | Prévention |  
| 20 | Appareils de correctif. | Tactique | Prévention |  
| 21 | Implémenter la gestion du cycle de vie centrée sur l’entreprise pour les ressources informatiques. | Stratégique | NON APPLICABLE |  
| 22 | Créer ou mettre à jour des plans de récupération de l’incident. | Stratégique | NON APPLICABLE |  
  


