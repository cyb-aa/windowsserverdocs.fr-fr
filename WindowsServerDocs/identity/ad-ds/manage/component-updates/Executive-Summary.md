---
ms.assetid: 85ca191c-0cc7-4453-a72c-42060ddf2ea2
title: Résumé
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: a24c88f5469890f12f821b9f729c7d283b687f43
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71389903"
---
# <a name="executive-summary"></a>Résumé

>S'applique à : Windows Server 2012

>[!IMPORTANT] 
>La documentation suivante a été écrite en 2013 et est fournie à des fins d’historique uniquement.  Actuellement, nous examinons cette documentation et nous sommes susceptibles de changer.  Il peut ne pas refléter les meilleures pratiques actuelles.

Aucune organisation dotée d’une infrastructure informatique n’est protégée contre les attaques, mais si des stratégies, des processus et des contrôles appropriés sont implémentés pour protéger des segments clés de l’infrastructure informatique d’une organisation, il peut être possible de Empêchez un événement de violation de croître vers un compromis de gros de l’environnement informatique.  
  
Ce résumé est destiné à être utile sous la forme d’un document autonome résumant le contenu du document, qui contient des recommandations qui aident les organisations à améliorer la sécurité de leurs installations Active Directory. En implémentant ces recommandations, les organisations sont en mesure d’identifier et de hiérarchiser les activités de sécurité, de protéger les segments clés de l’infrastructure informatique de leur organisation et de créer des contrôles qui réduisent considérablement la probabilité de attaques réussies contre les composants critiques de l’environnement informatique.  
  
Bien que ce document présente les attaques les plus courantes contre les Active Directory et les contre-mesures visant à réduire la surface d’attaque, il contient également des recommandations pour la récupération en cas de compromission complète. La seule façon de procéder à la récupération en cas de compromission complète de Active Directory doit être préparée à la compromission avant qu’elle ne se produise.  
  
Les principales sections de ce document sont les suivantes :  
  
-   Voies de compromis  
  
-   Réduire la Surface d'attaque Active Directory  
  
-   Surveillance des signes de compromission d'Active Directory  
  
-   Planification des compromis  
  
## <a name="avenues-to-compromise"></a>Voies de compromis  
Cette section fournit des informations sur les vulnérabilités les plus couramment utilisées par les attaquants pour compromettre les infrastructures des clients. Elle contient des catégories générales de vulnérabilités et la façon dont elles sont utilisées pour pénétrer initialement les infrastructures des clients, propager la compromission sur des systèmes supplémentaires et finalement cibler Active Directory et les contrôleurs de domaine pour obtenir la totalité contrôle des forêts des organisations. Il ne fournit pas de recommandations détaillées sur la résolution de chaque type de vulnérabilité, en particulier dans les domaines dans lesquels les vulnérabilités ne sont pas utilisées pour cibler directement Active Directory. Toutefois, pour chaque type de vulnérabilité, nous avons fourni des liens vers des informations supplémentaires à utiliser pour développer des contre-mesures et réduire la surface d’attaque de l’organisation.  
  
Les sujets suivants sont inclus :  
  
-   **Cibles de violations initiales** : la plupart des violations de la sécurité des informations commencent avec la compromission de petites parties de l’infrastructure d’une organisation, souvent un ou deux systèmes à la fois. Ces événements initiaux, ou points d’entrée dans le réseau, exploitent souvent des vulnérabilités susceptibles d’avoir été corrigées, mais n’ont pas été résolues. Les vulnérabilités courantes sont les suivantes :  
  
    -   Lacunes dans les déploiements antivirus et anti-programme malveillant  
  
    -   Mise à jour corrective incomplète  
  
    -   Applications et systèmes d’exploitation obsolètes  
  
    -   Configuration incorrecte  
  
    -   Absence de pratiques de développement d’applications sécurisées  
  
-   **Comptes intéressants pour le vol d’informations d’identification** : les attaques par vol d’informations d’identification sont celles dans lesquelles une personne malveillante obtient un accès privilégié à un ordinateur sur un réseau, puis utilise les outils disponibles librement pour extraire les informations d’identification des sessions d’autres comptes connectés.   
    Les éléments suivants sont inclus dans cette section :  
  
    -   **Activités qui augmentent la probabilité de compromission** : étant donné que la cible des vols d’informations d’identification est généralement des comptes de domaine hautement privilégiés et des comptes d’adresse IP virtuelle (VIP), il est important que les administrateurs soient conscients des activités. Cela augmente la probabilité de réussite d’une attaque par vol d’informations d’identification. Ces activités sont les suivantes :  
  
        -   Connexion à des ordinateurs non sécurisés avec des comptes privilégiés  
  
        -   Navigation sur Internet avec un compte doté de privilèges élevés  
  
        -   Configuration de comptes disposant de privilèges locaux avec les mêmes informations d’identification sur les systèmes  
  
        -   Suralimentation et abus des groupes de domaines privilégiés  
  
        -   La gestion de la sécurité des contrôleurs de domaine est insuffisante.  
  
    -   Les comptes, les serveurs et les composants d’infrastructure spécifiques à l' **élévation des privilèges et** à la propagation sont généralement les cibles principales des attaques contre les Active Directory. Ces comptes sont les suivants :  
  
        -   Comptes avec privilèges permanents  
  
        -   Comptes VIP  
  
        -   Comptes de Active Directory « attachés par des privilèges »  
  
        -   Contrôleurs de domaine  
  
        -   Autres services d’infrastructure qui affectent la gestion de l’identité, de l’accès et de la configuration, tels que les serveurs d’infrastructure à clé publique (PKI) et les serveurs de gestion de systèmes  
  
## <a name="reducing-the-active-directory-attack-surface"></a>Réduire la Surface d'attaque Active Directory  
Cette section se concentre sur les contrôles techniques pour réduire la surface d’attaque d’une installation Active Directory. Les sujets suivants sont inclus dans cette section :  
  
-   La section **comptes et groupes privilégiés dans Active Directory** présente les comptes et les groupes privilégiés les plus élevés dans Active Directory et les mécanismes par lesquels les comptes privilégiés sont protégés. Dans Active Directory, trois groupes prédéfinis sont les groupes de privilèges les plus élevés dans l’annuaire (administrateurs de l’entreprise, admins du domaine et administrateurs), bien qu’un certain nombre de groupes et de comptes supplémentaires doivent également être protégés.  
  
-   La section **implémentation de modèles d’administration à privilèges moindres** porte sur l’identification du risque que l’utilisation de comptes à privilèges élevés pour l’administration quotidienne présente, en plus de fournir des recommandations pour réduire ce risque.  
  
Un privilège excessif est introuvable dans les Active Directory dans les environnements compromis. Lorsqu’une organisation a développé l’habitude d’octroyer plus de privilèges que nécessaire, elle se trouve généralement dans toute l’infrastructure :  
  
-   Dans Active Directory  
  
-   Sur les serveurs membres  
  
-   Sur les stations de travail  
  
-   Dans les applications  
  
-   Dans les référentiels de données  
  
-   La section implémentation d’ordinateurs **hôtes d’administration sécurisée** décrit les ordinateurs hôtes d’administration sécurisés, qui sont des ordinateurs configurés pour prendre en charge l’administration de Active Directory et de systèmes connectés. Ces ordinateurs hôtes sont dédiés aux fonctionnalités administratives et n’exécutent pas de logiciels tels que les applications de messagerie, les navigateurs Web ou les logiciels de productivité (tels que les Microsoft Office).  
  
Les éléments suivants sont inclus dans cette section :  
  
-   **Principes de création d’ordinateurs hôtes d’administration sécurisés** : les principes généraux à prendre en compte sont les suivants :  
  
    -   N’administrez jamais un système approuvé à partir d’un hôte moins approuvé.  
  
    -   Ne vous fiez pas à un seul facteur d’authentification lors de l’exécution d’activités privilégiées.  
  
    -   N’oubliez pas la sécurité physique lors de la conception et de l’implémentation d’ordinateurs hôtes d’administration sécurisés.  
  
-   **Sécurisation des contrôleurs de domaine contre les attaques** : si un utilisateur malveillant obtient un accès privilégié à un contrôleur de domaine, il peut modifier, corrompre et détruire la base de données Active Directory et, par extension, tous les systèmes et comptes gérés. par Active Directory.  
  
Les sujets suivants sont inclus dans cette section :  
  
-   **Sécurité physique pour les contrôleurs de domaine** : contient des recommandations pour assurer la sécurité physique des contrôleurs de domaine dans les centres de donnes, les filiales et les emplacements distants.  
  
-   **Systèmes d’exploitation de contrôleur de domaine** : contient des recommandations pour la sécurisation des systèmes d’exploitation de contrôleur de domaine.  
  
-   **Configuration sécurisée des contrôleurs de domaine** : les outils et paramètres de configuration natifs et disponibles librement peuvent être utilisés pour créer des lignes de base de configuration de sécurité pour les contrôleurs de domaine qui peuvent être appliqués par la suite par des objets stratégie de groupe (GPO).  
  
## <a name="monitoring-active-directory-for-signs-of-compromise"></a>Surveillance des signes de compromission d'Active Directory  
Cette section fournit des informations sur les catégories d’audit héritées et les sous-catégories de stratégie d’audit (qui ont été introduites dans Windows Vista et Windows Server 2008), ainsi que sur la stratégie d’audit avancée (qui a été introduite dans Windows Server 2008 R2). Vous trouverez également des informations sur les événements et les objets à surveiller qui peuvent indiquer des tentatives de compromission de l’environnement et des références supplémentaires qui peuvent être utilisées pour construire une stratégie d’audit complète pour Active Directory.  
  
Les sujets suivants sont inclus dans cette section :  
  
-   **Stratégie d’audit Windows** : les journaux des événements de sécurité Windows ont des catégories et des sous-catégories qui déterminent les événements de sécurité suivis et enregistrés.  
  
-   **Recommandations de stratégie d’audit** : cette section décrit les paramètres de stratégie d’audit par défaut de Windows, les paramètres de stratégie d’audit recommandés par Microsoft et des recommandations plus agressives pour les organisations à utiliser pour auditer les serveurs critiques et stations.  
  
## <a name="planning-for-compromise"></a>Planification des compromis  
Cette section contient des recommandations qui aident les organisations à se préparer à un compromis avant qu’elles ne se produisent, à implémenter des contrôles capables de détecter un événement compromis avant qu’une violation complète ne se produise, et à fournir des instructions de réponse et de récupération pour les cas où une compromission complète de l’annuaire est effectuée par des attaquants. Les sujets suivants sont inclus dans cette section :  
  
-   **Repenser l’approche** : contient des principes et des recommandations pour créer des environnements sécurisés dans lesquels une organisation peut placer les ressources les plus critiques. Ces instructions sont les suivantes :  
  
    -   Identification des principes pour isoler et sécuriser les ressources critiques  
  
    -   Définition d’un plan de migration limité et basé sur les risques  
  
    -   Tirer parti des migrations « migratoires » lorsque cela est nécessaire  
  
    -   Implémentation de la « destruction créative »  
  
    -   Isolation des systèmes et applications héritées  
  
    -   Simplification de la sécurité pour les utilisateurs finaux  
  
-   **Maintenance d’un environnement plus sécurisé** : contient des recommandations de haut niveau destinées à être utilisées en tant que recommandations pour le développement d’une sécurité efficace, mais d’une gestion du cycle de vie efficace. Les sujets suivants sont inclus dans cette section :  
  
    -   **Création de pratiques de sécurité axées sur l’entreprise pour Active Directory** pour gérer efficacement le cycle de vie des utilisateurs, des données, des applications et des systèmes gérés par Active Directory, suivez ces principes.  
  
        -   **Affecter une propriété de l’entreprise à Active Directory données** -affecter la propriété des composants d’infrastructure à celle-ci ; pour les données ajoutées à Active Directory Domain Services (AD DS) pour la prise en charge de l’activité, par exemple, de nouveaux employés, de nouvelles applications et de nouveaux référentiels d’informations, une unité commerciale ou un utilisateur désigné doit être associé aux données.  
  
        -   **Implémenter la gestion du cycle** de vie piloté par l’entreprise-la gestion du cycle de vie doit être implémentée pour les données dans Active Directory.  
  
        -   **Classifier tous les Active Directory les** propriétaires d’entreprise doivent fournir une classification des données dans Active Directory. Dans le modèle de classification des données, la classification des données de Active Directory suivantes doit être incluse :  
  
            -   **Systèmes** : classification des populations de serveurs, du rôle de leur système d’exploitation, des applications qui s’y exécutent et des propriétaires de l’informatique et des professionnels de l’enregistrement.  
  
            -   **Applications** : classifier les applications par fonctionnalité, base d’utilisateurs et leur système d’exploitation.  
  
            -   **Utilisateurs** : les comptes des installations Active Directory les plus susceptibles d’être ciblés par les attaquants doivent être marqués et surveillés.  
  
## <a name="summary-of-best-practices-for-securing-active-directory-domain-services"></a>Résumé des meilleures pratiques pour la sécurisation des Active Directory Domain Services  
Le tableau suivant fournit un résumé des recommandations fournies dans ce document pour sécuriser une installation AD DS. Certaines meilleures pratiques sont de nature stratégique et nécessitent des projets de planification et d’implémentation complets. d’autres sont tactiques et se concentrent sur des composants spécifiques de Active Directory et de l’infrastructure associée.  
  
Les pratiques sont répertoriées dans l’ordre de priorité approximatif, autrement dit, les nombres inférieurs indiquent une priorité plus élevée. Le cas échéant, les meilleures pratiques sont identifiées comme préventives ou détectives par nature. Toutes ces recommandations doivent être rigoureusement testées et modifiées en fonction des besoins et des exigences de votre organisation.  
  
  
||**Bonne pratique**|**Tactique ou stratégique**|**Préventive ou détective**|  
|-|-|-|-|  
|1|Application de correctifs.|Tactique|Intervenir|  
|2|Corriger les systèmes d’exploitation.|Tactique|Intervenir|  
|3|Déployez et mettez rapidement à jour les logiciels antivirus et anti-programme malveillant sur tous les systèmes et surveillez les tentatives de suppression ou de désactivation.|Tactique|Les deux|  
|4|Surveiller les objets de Active Directory sensibles pour les tentatives de modification et Windows pour les événements qui peuvent indiquer une tentative de compromission.|Tactique|Simplifié|  
|5|Protéger et surveiller les comptes pour les utilisateurs qui ont accès à des données sensibles|Tactique|Les deux|  
|6\.|Empêchez l’utilisation de comptes puissants sur des systèmes non autorisés.|Tactique|Intervenir|  
|7|Éliminez l’appartenance permanente aux groupes à privilèges élevés.|Tactique|Intervenir|  
|8|Implémentez des contrôles pour accorder une appartenance temporaire à des groupes privilégiés si nécessaire.|Tactique|Intervenir|  
|9|Implémentez des hôtes d’administration sécurisés.|Tactique|Intervenir|  
|10|Utilisez la liste verte d’applications sur les contrôleurs de domaine, les hôtes administratifs et les autres systèmes sensibles.|Tactique|Intervenir|  
|11|Identifiez les ressources critiques et hiérarchisez leur sécurité et leur surveillance.|Tactique|Les deux|  
|12|Implémentez des contrôles d’accès en fonction du rôle et des privilèges minimaux pour l’administration de l’annuaire, son infrastructure de prise en charge et les systèmes joints à un domaine.|Stratégique|Intervenir|  
|13|Isolez les systèmes et les applications héritées.|Tactique|Intervenir|  
|14|Désaffecter des systèmes et des applications héritées.|Stratégique|Intervenir|  
|15|Implémentez des programmes de cycle de vie de développement sécurisés pour les applications personnalisées.|Stratégique|Intervenir|  
|16|Implémentez la gestion de la configuration, examinez la conformité régulièrement et évaluez les paramètres avec chaque nouvelle version matérielle ou logicielle.|Stratégique|Intervenir|  
|17|Migrez les ressources critiques vers les forêts de grande valeur avec des exigences strictes en matière de sécurité et de surveillance.|Stratégique|Les deux|  
|18|Simplifiez la sécurité pour les utilisateurs finaux.|Stratégique|Intervenir|  
|19|Utilisez des pare-feu basés sur l’hôte pour contrôler et sécuriser les communications.|Tactique|Intervenir|  
|20|Appareils à correctifs.|Tactique|Intervenir|  
|21|Implémentez une gestion du cycle de vie centrée sur l’entreprise pour les ressources informatiques.|Stratégique|N/A|  
|22|Créer ou mettre à jour des plans de récupération d’incident.|Stratégique|N/A|  
  


