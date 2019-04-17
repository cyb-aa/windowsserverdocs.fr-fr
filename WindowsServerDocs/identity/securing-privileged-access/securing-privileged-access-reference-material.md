---
title: "Sécurisation de l’accès privilégié des documents de référence"
description: "Sécurité de Windows Server"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 22ee9a77-4872-4c54-82d9-98fc73a378c0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: ee5769a3ed0225ebdd31645027bace38f0bff1b9
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="securing-privileged-access-reference-material"></a>Sécurisation de l’accès privilégié des documents de référence

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012


Cette section contient des informations de référence pour la sécurisation de l’accès privilégié, notamment:

-   [Modèle de niveau d’administration ActiveDirectory](#ADATM_BM)

-   [Principe de source propre](#CSP_BM)

-   [Approche de conception de forêt d’administration ESAE](#ESAE_BM)

-   [Équivalence de niveau 0](#T0E_BM)

-   [Outils d’administration et Types d’ouverture de session](#ATLT_BM)

## <a name="ADATM_BM"></a>Modèle de niveau d’administration ActiveDirectory
L’objectif de ce modèle de niveau est de protéger les systèmes d’identité à l’aide d’un ensemble de zones de mémoire tampon entre un contrôle total de l’environnement (niveau 0) et le risque élevé pour la station de travail qui nuisent souvent.

![Diagramme montrant les trois couches du modèle de niveau](../media/securing-privileged-access-reference-material/PAW_RM_Fig1.JPG)

Le modèle de niveau se compose de trois niveaux et inclut uniquement les comptes d’administration, les comptes d’utilisateur standard pas:

-   **Niveau 0** -contrôle Direct des identités d’entreprise dans l’environnement. Niveau 0 inclut les comptes, groupes et autres ressources qui ont un contrôle administratif direct ou indirect de la forêt ActiveDirectory, des domaines ou contrôleurs de domaine et toutes les ressources qu’il contient. Le degré de sécurité de toutes les ressources de niveau 0 est équivalent qu’ils sont toutes efficacement dans le contrôle de l’autre.

-   **Niveau 1** -contrôle des serveurs d’entreprise et des applications. Ressources de niveau 1 incluent des systèmes d’exploitation serveur, les services cloud et les applications d’entreprise. Comptes de niveau administrateur 1 ont un contrôle administratif d’une quantité importante de la valeur commerciale est hébergé sur ces ressources. Un exemple de rôle courant est Administrateurs de serveur qui gèrent ces systèmes d’exploitation avec la possibilité d’avoir un impact sur tous les services d’entreprise.

-   **Niveau 2** -contrôle des stations de travail utilisateur et des périphériques. Comptes de niveau administrateur 2 ont un contrôle administratif d’une quantité importante de la valeur commerciale est hébergé sur leurs stations de travail et des périphériques. Exemples du support technique et ordinateur prend en charge les administrateurs dans la mesure où ils peuvent avoir un impact sur l’intégrité de presque toutes les données utilisateur.

> [!NOTE]
> Les niveaux offrent également une base des priorités pour protéger les ressources administratives, mais il est important de prendre en compte qu’un intrus avec le contrôle de toutes les ressources de tout niveau peut accéder à la plupart ou toutes les ressources d’entreprise. La raison pour laquelle qu'il est utile comme un mécanisme de priorité de base est la personne malveillante et plus coûteuse. Il est plus facile pour un attaquant d’opérer avec un contrôle total de toutes les identités (niveau 0) ou les serveurs et services cloud (niveau 1) plutôt que d’accéder à chaque station de travail ou un périphérique de l’utilisateur (niveau 2) pour obtenir les données de votre organisation.

### <a name="containment-and-security-zones"></a>Confinement et zones de sécurité
Les niveaux sont par rapport à une zone de sécurité spécifique. Bien qu’elles ont porté de nombreux noms, zones de sécurité sont une approche bien établie qui assure le confinement des menaces de sécurité par le biais d’isolation de la couche réseau entre eux. Le modèle de niveau complète l’isolation en assurant le confinement des adversaires au sein d’une zone de sécurité où l’isolement réseau n’est pas efficace. Zones de sécurité peuvent s’étendre à la fois locaux et cloud infrastructure, comme dans l’exemple où les contrôleurs de domaine et les membres du domaine dans le même domaine sont hébergés localement et dans Azure.

![Diagramme montrant comment les zones de sécurité peuvent couvrir à la fois local et infrastructure de cloud](../media/securing-privileged-access-reference-material/PAW_RM_Fig2.JPG)

Le modèle de niveau empêche l’escalade de privilèges en limitant les administrateurs peuvent contrôler et où ils peuvent se connecter (étant donné que l’ouverture de session sur un ordinateur accorde le contrôle de ces informations d’identification et de toutes les ressources gérées par ces informations d’identification).

### <a name="control-restrictions"></a>Restrictions de contrôle
Restrictions de contrôle sont indiquées dans la figure ci-dessous:

![Diagramme des restrictions de contrôle](../media/securing-privileged-access-reference-material/PAW_RM_Fig3.JPG)

### <a name="primary-responsibilities-and-critical-restrictions"></a>Principales responsabilités et restrictions critiques
**Administrateur de niveau 0** -gérer le magasin d’identités et un petit nombre de systèmes qui se trouvent dans le contrôle effectif, et:

-   Peut gérer et contrôler les ressources à tout niveau si nécessaire

-   Peut uniquement session interactive ou accéder aux ressources approuvées au niveau du niveau 0

**Administrateur de niveau 1** -gérer les serveurs d’entreprise, services et applications, et:

-   Peut uniquement gérer et contrôler les ressources au niveau du niveau 1 ou 2

-   Peut uniquement accéder aux ressources (via le type de connexion réseau) ce qui sont approuvées au niveau 1 ou des niveaux de niveau 0

-   Peut uniquement connecter de façon interactive aux ressources approuvées au niveau 1

**Administrateur de niveau 2** -gérer les bureaux d’entreprise, les ordinateurs portables, des imprimantes et autres périphériques d’utilisateur, et:

-   Peut uniquement gérer et contrôler les ressources au niveau 2

-   Peut accéder aux ressources (via le type de connexion réseau) à tout niveau si nécessaire

-   Peut uniquement connecter de façon interactive aux ressources approuvées au niveau 2

### <a name="logon-restrictions"></a>Restrictions d’ouverture de session
Restrictions d’ouverture de session sont indiquées dans la figure ci-dessous:

![Diagramme des restrictions d’ouverture de session](../media/securing-privileged-access-reference-material/PAW_RM_Fig4.JPG)

> [!NOTE]
> Notez que certaines ressources peuvent avoir l’impact de niveau 0 à la disponibilité de l’environnement, mais pas directement impact protéger la confidentialité et intégrité des actifs. Ceux-ci incluent le service serveur DNS et les périphériques réseau critiques tels que les serveurs proxy Internet.

## <a name="CSP_BM"></a>Principe de source propre
Le principe de source propre nécessite toutes les dépendances de sécurité de confiance en tant que l’objet sécurisé.

![Diagramme montrant comment un sujet contrôlant un objet est une dépendance de sécurité de cet objet](../media/securing-privileged-access-reference-material/PAW_RM_Fig5.JPG)

N’importe quel sujet ayant le contrôle d’un objet est une dépendance de sécurité de cet objet. Si un pirate peut contrôler n’importe où dans le contrôle effectif d’un objet cible, il peut contrôler cet objet cible. Pour cette raison, vous devez vous assurer que les garanties de toutes les dépendances de sécurité sont à ou au-dessus du niveau de sécurité souhaité de l’objet lui-même.

Lors de la simple en principe, son application exige de comprendre les relations de contrôle d’une ressource d’intérêt (objet) et l’exécution d’une analyse de dépendance à découvrir toutes les dépendances de sécurité (sujets).

Étant donné que le contrôle est transitif, ce principe doit être répété de façon récursive. Pour exemple, si A contrôle B et B contrôle C, puis A également c indirectement.

![Diagramme montrant comment si A contrôle B et B contrôle C, puis A également C indirectement](../media/securing-privileged-access-reference-material/PAW_RM_Fig6.JPG)

Une personne malveillante qui compromet A l’accès à tous les éléments A contrôle (y compris B) et tout ce que B contrôle (y compris C). À l’aide de la langue des dépendances de sécurité sur ce même exemple, B et A sont les dépendances de sécurité de C et doivent être sécurisés au niveau de garantie souhaité de C afin que C ait ce niveau d’assurance.

Pour les systèmes d’infrastructure et l’identité de l’informatique, ce principe doit être appliqué aux moyens les plus courantes de contrôle, notamment au matériel sur lequel sont installés les systèmes, le support d’installation pour les systèmes, l’architecture et la configuration du système et les opérations quotidiennes.

### <a name="clean-source-for-installation-media"></a>Source propre pour support d’installation
![Diagramme montrant une source propre pour support d’installation](../media/securing-privileged-access-reference-material/PAW_RM_CSIM.JPG)

Appliquer le principe de source propre pour support d’installation, vous devez vous assurer que le support d’installation n’a pas été falsifié depuis que par le fabricant (mieux vous pouvez déterminer). Cette figure illustre une personne malveillante de compromettre un ordinateur à l’aide de ce chemin d’accès:

![Figure montrant une personne malveillante à l’aide d’un chemin d’accès pour compromettre un ordinateur](../media/securing-privileged-access-reference-material/PAW_RM_Fig8.JPG)

Appliquer le principe de source propre pour support d’installation, vous devez valider l’intégrité des logiciels tout au long du cycle de possession, depuis leur au cours de l’acquisition, leur stockage et de transfert jusqu'à ce qu’il est utilisé.

#### <a name="software-acquisition"></a>Acquisition des logiciels
La source du logiciel doit être validée par un des moyens suivants:

-   Logiciels sont obtenus à partir d’un support physique provenant du fabricant ou d’une source digne de confiance, support généralement fabriqué est envoyé à partir d’un fournisseur.

-   Logiciel est obtenue à partir d’Internet et validé avec les hachages de fichiers fournis par le fournisseur.

-   Logiciel est obtenue à partir d’Internet et validé en téléchargeant et en comparant deux copies indépendantes:

    -   Téléchargez sur deux hôtes sans aucune relation de sécurité (pas dans le même domaine et non gérés par les mêmes outils), de préférence à partir de connexions Internet distinctes.

    -   Comparez les fichiers téléchargés à l’aide d’un utilitaire comme certutil:

        `certutil -hashfile <filename>`

Lorsque possible, tous les logiciels d’application, telles que des outils et des programmes d’installation de l’application doivent être signés numériquement et vérifiés à l’aide de Windows Authenticode avec le [Windows Sysinternals](https://www.microsoft.com/sysinternals)outil s, *sigcheck.exe*, avec vérification de la révocation. Certains logiciels peuvent être nécessaires dans lequel le fournisseur ne donne pas ce type de signature numérique.

#### <a name="software-storage-and-transfer"></a>Transfert et stockage de logiciel
Après avoir obtenu le logiciel, il doit être stocké dans un emplacement protégé de toute modification, en particulier par les hôtes connectés à internet ou du personnel de confiance à un niveau inférieur à celui des systèmes où sera installé le logiciel ou le système d’exploitation. Ce stockage peut être un média physique ou un emplacement électronique sécurisé.

#### <a name="software-usage"></a>Utilisation des logiciels
Dans l’idéal, le logiciel doit être validé au moment qu'il est utilisé, par exemple, lorsqu’il est installé manuellement, empaqueté pour un outil de gestion de configuration ou importé dans un outil de gestion de configuration.

### <a name="clean-source-for-architecture-and-design"></a>Source propre pour l’architecture et la conception
Appliquer le principe de source propre à l’architecture système, vous devez vous assurer que le système n’est pas dépendant des systèmes de confiance inférieurs. Un système peut être dépendant sur un système de confiance plus élevé, mais pas sur un système de confiance inférieur avec des normes de sécurité inférieures.

Par exemple, son acceptable pour ActiveDirectory contrôler un ordinateur de bureau utilisateur standard, mais il est une escalade des risques de privilège pour le bureau utilisateur standard contrôler ActiveDirectory.

![Diagramme montrant comment un système peut être dépendant d’un système de confiance plus élevé, mais pas sur un système de confiance inférieur avec des normes de sécurité inférieures](../media/securing-privileged-access-reference-material/PAW_RM_Fig09.JPG)

La relation de contrôle peut être introduite par plusieurs moyens, notamment les listes de contrôle d’accès (ACL) de sécurité sur les objets tels que les systèmes de fichiers, l’appartenance au groupe Administrateurs local sur un ordinateur, ou les agents installés sur un ordinateur en cours d’exécution en tant que système (avec la possibilité d’exécuter des scripts et du code arbitraire).

Un exemple souvent négligé est l’exposition par le biais d’ouverture de session, ce qui crée une relation de contrôle en exposant les informations d’identification administratives d’un système à un autre système. Il s’agit de la raison sous-jacente pourquoi le vol d’informations d’identification attaques telles que passer le hachage sont donc puissant. Lorsqu’un administrateur se connecte à un bureau d’utilisateur standard avec les informations d’identification de niveau 0, il les expose ces informations d’identification pour que le bureau, lui permet de contrôler ActiveDirectory et la création d’une voie d’escalade de privilèges vers ActiveDirectory. Pour plus d’informations sur ces attaques, voir [cette page](https://technet.microsoft.com/en-us/security/dn785092).

En raison du grand nombre de ressources qui dépendent de systèmes d’identité comme ActiveDirectory, vous devez réduire le nombre de systèmes que dépendent votre ActiveDirectory et les contrôleurs de domaine.

![Diagramme montrant que vous devez réduire le nombre de systèmes ActiveDirectory et dépendent des contrôleurs de domaine](../media/securing-privileged-access-reference-material/PAW_RM_Fig010.JPG)

Pour plus d’informations sur la réduction des principaux risques liés à ActiveDirectory, voir [cette page](http://aka.ms/hardenAD).

### <a name="OSBCS_BM"></a>Normes opérationnelles basées sur le principe de source propre
Cette section décrit les normes opérationnelles et les attentes pour le personnel administratif. Ces normes sont conçues pour sécuriser le contrôle administratif des systèmes informatiques d’une organisation contre les risques éventuellement créés par les pratiques et processus opérationnels.

![Diagramme montrant comment les normes sont conçues pour sécuriser le contrôle administratif des informations d’une organisation systèmes informatiques contre les risques éventuellement créés par les pratiques et processus opérationnels](../media/securing-privileged-access-reference-material/PAW_RM_Fig11.JPG)

#### <a name="integrating-the-standards"></a>Intégration des normes
Vous pouvez intégrer ces normes aux normes et les pratiques de globale de votre organisation. Vous pouvez les adapter aux besoins spécifiques, des outils disponibles et goût du risque de votre organisation, mais nous vous recommandons de minimes pour réduire les risques. Nous vous recommandons d’utiliser les paramètres par défaut dans ce guide en tant que le banc d’essai pour votre état final idéal et de gérer les deltas comme exceptions à traiter par ordre de priorité.

Les instructions sont organisées dans les sections suivantes:

-   Hypothèses

-   Comité consultatif

-   Pratiques opérationnelles

    -   Résumé

    -   Détails des normes

#### <a name="assumptions"></a>Hypothèses
Les normes de cette section supposent que l’organisation a les attributs suivants:

-   La plupart ou toutes les serveurs et stations de travail dans l’étendue sont joints à ActiveDirectory.

-   Tous les serveurs à gérer exécutent Windows Server2008R2 ou ultérieur et ont activé le mode restrictedadmin du protocole RDP.

-   Toutes les stations de travail à gérer exécutent Windows7 ou ultérieur et ont activé le mode restrictedadmin du protocole RDP.

    > [!NOTE]
    > Pour activer le mode restrictedadmin du protocole RDP, voir [cette page](http://aka.ms/RDPRA).

-   Les cartes à puce sont disponibles et délivrées aux comptes administratifs.

-   Le *Builtin\Administrator* pour chaque domaine a été désigné comme compte d’accès d’urgence

-   Une solution de gestion d’identité de l’entreprise est déployée.

-   [LAPS](http://aka.ms/laps) a été déployée sur les serveurs et stations de travail pour gérer le mot de passe du compte administrateur local

-   Il existe une solution de gestion de l’accès privilégié, comme MicrosoftIdentity Manager, en place, ou il existe un plan d’adopter.

-   Le personnel est affecté à surveiller les alertes de sécurité et y répondre.

-   La capacité technique à appliquer rapidement des mises à jour de sécurité Microsoft est disponible.

-   Les contrôleurs Baseboard management sur les serveurs ne seront pas utilisés, ou ils respectent des contrôles de sécurité stricts.

-   Les comptes d’administrateur et les groupes de serveurs (administrateurs de niveau 1) et les stations de travail (administrateurs de niveau 2) sont gérés par les administrateurs de domaine (niveau 0).

-   Il existe un comité consultatif (comité) ou une autre autorité désignée en place pour l’approbation des modifications ActiveDirectory.

#### <a name="change-advisory-board"></a>Comité consultatif
Un comité consultatif (comité) est l’autorité de discussion forum et l’approbation des modifications peut avoir un impact sur le profil de sécurité de l’organisation. Toutes les exceptions à ces normes doivent être soumises au comité avec une justification et une évaluation des risques.

Chaque norme dans ce document est réparti par sa criticité la norme pour un niveau donné.

![Diagramme montrant la norme des niveaux donnés](../media/securing-privileged-access-reference-material/PAW_RM_Fig12.JPG)

Toutes les exceptions relatives aux éléments obligatoires (signalés par un octogone rouge ou un triangle orange dans ce document) sont considérées comme temporaires et ils doivent être approuvées par le comité. Les instructions sont les suivantes:

-   La demande initiale exige l’acceptation du risque signée par le superviseur direct du personnel justification, et elle expire au bout de six mois.

-   Renouvellements exigent une justification et une acceptation du risque signées par un responsable de division, et ils expirent au bout de six mois.

Toutes les exceptions aux éléments recommandés (signalés par un cercle jaune dans ce document) sont considérées comme temporaires et doivent être approuvées par le comité. Les instructions sont les suivantes:

-   La demande initiale exige l’acceptation du risque signée par le superviseur direct du personnel justification, et elle expire au bout de 12mois.

-   Renouvellements exigent une justification et une acceptation du risque signées par un responsable de division, et ils expirent au bout de 12mois.

#### <a name="operational-practices-standards-summary"></a>Résumé des normes pratiques opérationnelles
Les colonnes niveau dans ce tableau font référence au niveau du compte d’administration, dont le contrôle affecte généralement toutes les ressources de ce niveau.

![Tableau montrant les niveaux du compte d’administration](../media/securing-privileged-access-reference-material/PAW_RM_Fig13.JPG)

Décisions opérationnelles prises régulièrement sont essentielles pour maintenir la sécurité de l’environnement. Ces normes pour les processus et les pratiques garantissent qu’une erreur opérationnelle ne mène pas à une vulnérabilité opérationnelle exploitable dans l’environnement.

##### <a name="administrator-enablement-and-accountability"></a>Responsabilité et habilitation
Les administrateurs doivent être informés, habilités, formés et tenus responsables afin de faire fonctionner l’environnement de manière aussi sécurisée que possible.

###### <a name="administrative-personnel-standards"></a>Normes relatives au personnel d’administration
Le personnel administratif doit être contrôlé pour garantir qu’ils sont dignes de confiance et ont besoin de privilèges d’administration:

-   Les antécédents du personnel avant de lui attribuer des privilèges d’administration.

-   Passez en revue les privilèges d’administration chaque trimestre pour déterminer quels membres du personnel ont toujours un légitime besoin pour un accès d’administration.

###### <a name="administrative-security-briefing-and-accountability"></a>Responsabilité et briefing sur la sécurité d’administration
Les administrateurs doivent être informés et responsables des risques pour l’organisation et leur rôle dans la gestion des risques. Les administrateurs doivent être formés annuel sur:

-   Environnement de menace général

    -   Adversaires identifiés

    -   Attaques techniques notamment pass-the-hash et vol des informations d’identification

-   Incidents et menaces spécifiques de l’organisation

-   Rôles d’administrateur dans la protection contre les attaques

    -   La gestion d’exposition des informations d’identification avec le modèle de niveau

    -   Utilisation de stations de travail administratives

    -   Utilisation du mode RestrictedAdmin de protocole Bureau à distance

-   Pratiques administratives spécifiques de l’organisation

    -   Passez en revue toutes les recommandations opérationnelles de cette norme

    -   Implémenter les principales règles suivantes:

        -   N’utilisez pas de comptes d’administration sur uniquement les stations de travail administratives

        -   Ne pas désactiver ni entraver les contrôles de sécurité sur votre compte ou les stations de travail (par exemple, les restrictions d’ouverture de session ou les attributs requis pour les cartes à puce)

        -   Signaler des problèmes ou une activité inhabituelle

Pour fournir la responsabilité, tous les employés avec les comptes administratifs doivent signer un document de d’administration privilège Code de conduite indiquant qu’ils acceptent de suivre les pratiques de la stratégie d’administration spécifiques de l’organisation.

###### <a name="provisioning-and-deprovisioning-processes-for-administrative-accounts"></a>Octroi et pour les comptes administratifs
Les normes suivantes doivent être remplies pour répondre aux exigences de cycle de vie.

-   Tous les comptes administratifs doivent être approuvés par l’autorité approbatrice indiquée dans le tableau suivant.

    -   L’approbation doit uniquement être octroyée si le personnel a légitimement besoin commercial de privilèges administratifs.

    -   Approbation de privilèges administratifs ne doit pas dépasser six mois.

-   Accès à des privilèges d’administration doit être immédiatement dans les situations:

    -   Employé change de poste.

    -   L’employé quitte l’organisation.

-   Comptes doivent être immédiatement désactivés l’employé a quitté l’organisation.

-   Comptes désactivés doivent être supprimés dans les six mois et l’enregistrement de leur suppression doit être entré dans les enregistrements du Conseil approbation des modifications.

-   Passez en revue tous les appartenances des comptes privilégiés chaque mois pour vous assurer que l’utilisateur n’ont reçu aucune autorisation inadéquate. Cela peut être remplacé par un outil automatisé qui avertit des modifications.

|Niveau de privilège du compte|Autorité approbatrice|Vérification des octrois|
|--------------|------------|----------------|
|Administrateur de niveau 0|Comité consultatif sur|Mensuelle ou automatisée|
|Administrateur de niveau 1|Administrateurs de niveau 0 ou sécurité|Mensuelle ou automatisée|
|Administrateur de niveau 2|Administrateurs de niveau 0 ou sécurité|Mensuelle ou automatisée|

##### <a name="operationalize-least-privilege"></a>Privilèges minimum
Ces normes permettent d’atteindre les privilèges minimum en réduisant le nombre d’administrateurs dans le rôle et la quantité de temps qu’ils disposent des privilèges.

> [!NOTE]
> Instaurer les privilèges minimum dans votre organisation nécessite de comprendre les rôles organisationnels, leurs exigences et leurs mécanismes de conception pour vous assurer qu’ils peuvent accomplir leur travail à l’aide de privilèges minimum. Instaurer des privilèges minimum dans un modèle d’administration fréquemment nécessite l’utilisation de plusieurs approches:
>
> -   Limite le nombre d’administrateurs ou les membres des groupes privilégiés
> -   Déléguer moins de privilèges aux comptes
> -   Fournir des privilèges temporaires sur demande
> -   Offrent la possibilité pour les autres membres du personnel d’effectuer des tâches (une approche «Concierge»)
> -   Fournir des processus pour l’accès d’urgence et de scénarios d’utilisation rare

###### <a name="limit-count-of-administrators"></a>Limiter le nombre d’administrateurs
Un minimum de deux personnes qualifiées doit être attribué à chaque rôle administratif pour garantir la continuité d’activité.

Si le nombre de personnes affectées à n’importe quel rôle dépasse deux, le comité doit approuver les raisons particulières de l’attribution de privilèges à chaque membre (y compris les deux d’origine). La justification de l’approbation doit inclure:

-   Les tâches techniques sont effectuées par les administrateurs qui requièrent des droits d’administration

-   La fréquence à laquelle les tâches sont exécutées

-   Pourquoi les tâches ne peut pas être effectuées par un autre administrateur en leur nom de raison spécifique

-   Tous les autres autres approches connues de l’octroi de privilèges et pourquoi elles ne sont pas acceptables de document

###### <a name="dynamically-assign-privileges"></a>Affecter dynamiquement des privilèges
Les administrateurs doivent obtenir des autorisations «juste-à-temps» pour les utiliser lorsqu’ils effectuent des tâches. Aucune autorisation n’est attribuée définitivement à des comptes administratifs.

> [!NOTE]
> Privilèges administratifs attribués définitivement créent naturellement une stratégie «privilèges maximum», car le personnel administratif nécessite un accès rapide aux autorisations pour maintenir la disponibilité opérationnelle s’il existe un problème. Autorisations juste-à-temps permettent de:
>
> -   Affecter des autorisations plus en détail, rapproche avec des privilèges minimum.
> -   Réduire le temps d’exposition des privilèges
> -   Suivi des autorisations permettent de détecter les attaques ou les abus.

##### <a name="manage-risk-of-credential-exposure"></a>Gérer les risques d’exposition des informations d’identification
Utilisez les pratiques suivantes pour gérer correctement le risque d’exposition des informations d’identification.

###### <a name="separate-administrative-accounts"></a>Comptes d’administration distincts
Tous les employés sont autorisés à disposent des privilèges d’administrateur doivent avoir des comptes distincts pour les fonctions d’administration qui sont des comptes d’utilisateur.

-   **Comptes d’utilisateur standard** -des privilèges d’utilisateur standard pour les tâches d’utilisateur standard, comme l’e-mail, la navigation web et à l’aide d’applications métier de. Ces comptes ne doivent pas recevoir des privilèges d’administration.

-   **Comptes d’administration** -comptes distincts créés pour les personnes qui disposent des privilèges d’administration appropriés. Un administrateur qui est requis pour gérer les ressources de chaque niveau doit avoir un compte distinct pour chaque niveau. Ces comptes doivent pas avoir accès au courrier électronique ou l’Internet public.

###### <a name="administrator-logon-practices"></a>Pratiques d’ouverture de session administrateur
Avant d’un administrateur peut ouvrir une session un ordinateur hôte interactive (localement par protocole RDP standard, à l’aide d’identification, ou à l’aide de la console de virtualisation), cet hôte doit respecter ou dépasser la norme du niveau du compte administrateur (ou un niveau supérieur).

Les administrateurs peuvent uniquement se connecter à des stations de travail admin avec leurs comptes administratifs. Les administrateurs seulement une session à des ressources gérées à l’aide de la technologie de support approuvée décrite dans la section suivante.

> [!NOTE]
> Cela est nécessaire, car la connexion à un hôte de manière interactive accorde le contrôle les informations d’identification à cet hôte.
>
> Consultez le [outils d’administration et Types d’ouverture de session](http://aka.ms/admintoolsecurity) pour plus d’informations sur les types d’ouverture de session, les outils de gestion courantes et exposition des informations d’identification.

###### <a name="use-of-approved-support-technology-and-methods"></a>Utilisation de méthodes et de la technologie de support approuvées
Les administrateurs qui prennent en charge des systèmes et utilisateurs distants doivent suivre ces recommandations pour empêcher tout adversaire ayant le contrôle de l’ordinateur distant de voler leurs informations d’identification d’administration.

-   Les options de support technique doivent être utilisées si elles sont disponibles.

-   Les options de support secondaire doivent être utilisées uniquement si l’option principale n’est pas disponible.

-   Méthodes de support interdites ne peuvent jamais être utilisées.

-   Aucun accès à internet de navigation ou par courrier électronique ne peuvent être effectuées par n’importe quel compte d’administration à tout moment.

###### <a name="tier-0-forest-domain-and-dc-administration"></a>Forêt de niveau 0, le domaine et administration du contrôleur de domaine
Vérifiez que les pratiques suivantes sont appliquées pour ce scénario:

-   **Prise en charge de serveur distant** -lors de l’accès à distance à un serveur, les administrateurs de niveau 0 doivent suivre ces recommandations:

    -   **Principale (outil)** -cette utilisation réseau des ouvertures de session (type 3) des outils à distance. Pour plus d’informations, voir [outils d’administration et Types d’ouverture de session](http://aka.ms/admintoolsecurity).

    -   **Principale (interactive)** -restrictedadmin du protocole RDP utilisation ou une Session RDP Standard à partir d’une station de travail administrateur avec un compte de domaine

    > [!NOTE]
    > Si vous disposez d’une solution de gestion de niveau 0 de privilège, ajoutez «qu’utilise des autorisations obtenues juste-à-temps à partir d’une solution de gestion de l’accès privilégié.»

-   **Prise en charge du serveur physique** - lorsque se trouvent physiquement dans une console de serveur ou à une console de machine virtuelle (outils Hyper-V ou VMWare), ces comptes n’ont aucune restriction de l’utilisation des outils d’administration spécifiques, uniquement les restrictions générales liées aux tâches d’utilisateur standard comme l’e-mail et la navigation sur l’internet public.

    > [!NOTE]
    > Administration de niveau 0 diffère de l’administration des autres niveaux, car toutes les ressources de niveau 0 disposent déjà d’un contrôle direct ou indirect de toutes les ressources. Par exemple, une personne malveillante de contrôler un contrôleur de domaine n’a aucun besoin de voler des informations d’identification auprès des administrateurs connectés puisqu’elle a déjà accès à toutes les informations d’identification de domaine dans la base de données.

###### <a name="tier-1-server-and-enterprise-application-support"></a>Serveur de niveau 1 et entreprise des applications
Vérifiez que les pratiques suivantes sont appliquées pour ce scénario:

-   **Prise en charge de serveur distant** -lors de l’accès à distance à un serveur, les administrateurs de niveau 1 doivent suivre ces recommandations:

    -   **Principale (outil)** -cette utilisation réseau des ouvertures de session (type 3) des outils à distance. Pour plus d’informations, voir [Mitigating Pass-the-Hash et autres vols d’informations d’identification](https://www.microsoft.com/pth) v1 (pages 42 à 47).

    -   **Principale (interactive)** -utilisation restrictedadmin du protocole RDP à partir d’une station de travail administrateur avec un compte de domaine qui utilise des autorisations obtenues juste-à-temps à partir d’une solution de gestion de l’accès privilégié.

    -   **Secondaire** -ouvrez une session sur le serveur à l’aide d’un mot de passe de compte local défini par LAPS à partir d’une station de travail administrateur.

    -   **Option interdite** -protocole RDP Standard ne peuvent pas être utilisé avec un compte de domaine.

    -   **Option interdite** -en utilisant le compte de domaine des informations d’identification en cours de session (par exemple, à l’aide de *RunAs* ou authentification auprès d’un partage). Cette pratique expose les informations d’identification d’ouverture de session au risque de vol.

-   **Prise en charge du serveur physique** - lorsque se trouvent physiquement dans une console de serveur ou à une console de machine virtuelle (outils Hyper-V ou VMWare), les administrateurs de niveau 1 doivent récupérer le mot de passe du compte local LAPS avant l’accès au serveur.

    -   **Principal** -récupérer le mot de passe du compte local défini par LAPS à partir d’une station de travail administrateur avant l’ouverture de session sur le serveur.

    -   **Option interdite** -ouverture de session avec un compte de domaine n’est pas autorisée dans ce scénario.

    -   **Option interdite** -en utilisant le compte de domaine des informations d’identification en cours de session (par exemple, d’identification ou authentification auprès d’un partage). Cette pratique expose les informations d’identification d’ouverture de session au risque de vol.

###### <a name="tier-2-help-desk-and-user-support"></a>Support utilisateur et support technique de niveau 2
Support technique et utilisateur organisations effectuent la prise en charge pour les utilisateurs finaux (qui ne requièrent des privilèges d’administration) et les stations de travail utilisateur (qui nécessite des privilèges d’administrateur).

**Prise en charge de l’utilisateur** -tâches incluent l’assistance des utilisateurs pour effectuer des tâches qui ne nécessitent aucune modification de la station de travail, souvent en leur montrant comment utiliser une fonctionnalité d’application ou une fonctionnalité du système d’exploitation.

-   **Prise en charge de l’utilisateur du support côté** -personnel de prise en charge le niveau 2 est physiquement à l’espace de travail de l’utilisateur.

    -   **Principal** -«procuration de privilège» peut être prise en charge sans aucun outil.

    -   **Option interdite** -ouverture de session avec les informations d’identification d’administration du compte de domaine n’est pas autorisée dans ce scénario. Basculer vers la prise en charge de la station de travail côté bureau si des privilèges d’administration sont requis.

-   **Prise en charge de l’utilisateur distant** -le niveau 2, le support technique est physiquement se connecter à l’utilisateur.

    -   **Principal** -l’Assistance à distance, Skype pour entreprises ou partage d’écran utilisateur similaire peut être utilisée. Pour plus d’informations, voir [quel est l’Assistance à distance Windows?](https://windows.microsoft.com/en-us/windows/what-is-windows-remote-assistance)

    -   **Option interdite** -ouverture de session avec les informations d’identification d’administration du compte de domaine n’est pas autorisée dans ce scénario. Basculer vers la prise en charge de la station de travail si des privilèges d’administration sont requis.

-   **Prise en charge de la station de travail** - tâches incluent une maintenance de la station de travail ou de résolution des problèmes qui nécessite l’accès à un système pour consulter des journaux, installer des logiciels, mise à jour de pilotes et ainsi de suite.

    -   **Prise en charge de la station de travail côté bureau** -personnel de prise en charge le niveau 2 est physiquement à une station de travail de l’utilisateur.

        -   **Principal** -récupérer le mot de passe du compte local défini par LAPS à partir d’une station de travail d’administration avant la connexion à la station de travail utilisateur.

        -   **Option interdite** -ouverture de session avec les informations d’identification d’administration du compte de domaine n’est pas autorisée dans ce scénario.

    -   **Prise en charge de la station de travail à distance** -le niveau 2, le support technique est physiquement se connecter à la station de travail.

        -   **Principal** -utilisation restrictedadmin du protocole RDP à partir d’une station de travail administrateur avec un compte de domaine qui utilise des autorisations obtenues juste-à-temps à partir d’une solution de gestion de l’accès privilégié.

        -   **Secondaire** -récupérer un mot de passe du compte local défini par LAPS à partir d’une station de travail d’administration avant la connexion à la station de travail utilisateur.

        -   **Option interdite** -utiliser le protocole RDP standard avec un compte de domaine.

###### <a name="no-browsing-the-public-internet-with-admin-accounts-or-from-admin-workstations"></a>Absence de navigation sur l’Internet public avec des comptes d’administrateur ou à partir de stations de travail admin
Le personnel administratif ne peut pas naviguer sur Internet ouvert en étant connecté avec un compte administratif ou ouvert une session sur une station de travail d’administration. Les seules exceptions autorisées sont l’utilisation d’un navigateur web pour administrer un service cloud, tels que MicrosoftAzure, Amazon Web Services, MicrosoftOffice 365 ou Gmail d’entreprise.

###### <a name="no-accessing-email-with-admin-accounts-or-from-admin-workstations"></a>Aucun accès à des e-mails avec des comptes d’administrateur ou à partir de stations de travail admin
Le personnel administratif ne peut pas accéder aux e-mails en étant connecté avec un compte administratif ou ouvert une session sur une station de travail d’administration.

###### <a name="store-service-and-application-account-passwords-in-a-secure-location"></a>Service de banque et l’application compte de mots de passe dans un emplacement sécurisé
Les instructions suivantes doivent être utilisées pour la sécurité physique traite qui contrôlent l’accès au mot de passe:

-   Verrouiller les mots de passe du compte de service dans un coffre physique.

-   Assurez-vous que seul le personnel approuvé à ou au-dessus de la classification de niveau du compte ont accès au mot de passe de compte.

-   Limiter le nombre de personnes qui accèdent aux mots de passe pour un nombre minimal de responsabilité.

-   Assurez-vous que tous les accès au mot de passe sont connecté, suivis et surveillés par un tiers désintéressé, par exemple, un responsable qui n’est pas formé pour effectuer l’administration informatique.

##### <a name="strong-authentication"></a>Authentification forte
Utilisez les pratiques suivantes pour configurer correctement une authentification forte.

###### <a name="enforce-smartcard-multi-factor-authentication-mfa-for-all-admin-accounts"></a>Mettre en œuvre de la carte à puce l’authentification multifacteur (MFA) pour tous les comptes d’administrateur
Aucun compte d’administrateur n’est autorisé à utiliser un mot de passe pour l’authentification. Les seules exceptions autorisées sont les comptes d’accès d’urgence qui sont protégés par les processus appropriés.

Liez tous les comptes administratifs à une carte à puce et activez l’attribut «**carte à puce requise pour l’ouverture de session Interactive**.»

Un script doit être implémenté pour réinitialiser automatiquement et régulièrement la valeur de hachage de mot de passe aléatoire par désactivation et réactivation immédiate de l’attribut «**carte à puce requise pour l’ouverture de session Interactive**.»

N’autorisez aucune exception pour les comptes utilisés par le personnel humain au-delà de comptes d’accès d’urgence.

###### <a name="enforce-multi-factor-authentication-for-all-cloud-admin-accounts"></a>Appliquer l’authentification multifacteur pour tous les comptes d’administrateur Cloud
Tous les comptes avec des privilèges d’administration dans un service cloud, tels que MicrosoftAzure et Office 365, doivent utiliser l’authentification multifacteur.

##### <a name="rare-use-emergency-procedures"></a>Utiliser des procédures d’urgence rares
Les pratiques opérationnelles doivent prendre en charge les normes suivantes:

-   Vérifiez les interruptions peuvent être résolues rapidement.

-   Vérifiez que les tâches à privilèges élevés rares puissent être effectuées si nécessaire.

-   Assurez-vous de procédures sécurisées soient utilisées pour protéger les informations d’identification et les privilèges.

-   Ce processus de suivi et d’approbation appropriés soient appliquées.

###### <a name="correctly-follow-appropriate-processes-for-all-emergency-access-accounts"></a>Suivre correctement les processus appropriés à tous les comptes d’accès d’urgence
Assurez-vous que chaque compte d’accès d’urgence dispose d’une feuille de suivi dans le coffre.

La procédure décrite dans la feuille de suivi du mot de passe doit être respectée pour chaque compte, notamment modifier le mot de passe après chaque utilisation et fermer la session sur toutes les stations de travail ou les serveurs utilisés après l’achèvement.

Toute utilisation des comptes doivent être approuvés par le comité avancées ou après les faits, en tant qu’utilisation d’urgence approuvée accès d’urgence.

###### <a name="restrict-and-monitor-usage-of-emergency-access-accounts"></a>Limiter et surveiller l’utilisation des comptes d’accès d’urgence
Pour toute utilisation des comptes d’accès d’urgence:

-   Admins du domaine autorisés uniquement permettre accéder aux comptes d’accès d’urgence avec des privilèges administratifs de domaine.

-   Les comptes d’accès d’urgence peuvent être utilisés uniquement sur les contrôleurs de domaine et d’autres hôtes de niveau 0.

-   Ce compte doit être utilisé uniquement pour:

    -   Effectuer la résolution des problèmes et correction des problèmes techniques qui empêchent l’utilisation des comptes d’administration corrects.

    -   Effectuer des tâches rares, comme:

        -   Administration du schéma

        -   Tâches de forêt qui nécessitent des privilèges d’administrateur d’entreprise Notez que le gestion de la topologie, notamment la gestion de site et sous-réseau ActiveDirectory est déléguée pour limiter l’utilisation de ces privilèges.

-   Toute l’utilisation de l’un de ces comptes doit avoir l’autorisation écrite par le chef de groupe de sécurité

-   La procédure de la feuille de suivi pour chaque compte d’accès d’urgence requiert le mot de passe d’être modifiés pour chaque utilisation. Un membre de l’équipe de sécurité doit valider le déroulement de cette opération correctement.

###### <a name="temporarily-assign-enterprise-admin-and-schema-admin-membership"></a>Attribuer temporairement une appartenance admin à admin et schéma de l’entreprise
Privilèges doivent être ajoutés selon les besoins et supprimée après utilisation. Le compte d’urgence doit disposer de ces privilèges seulement pendant la durée de la tâche à effectuer et pour un maximum de 10heures. Toutes les activités et la durée de ces privilèges doivent être capturés dans l’enregistrement de carte de modifier l’approbation, une fois la tâche terminée.

## <a name="ESAE_BM"></a>Approche de conception de forêt d’administration ESAE
Cette section contient une approche destinée à une forêt administrative basée sur l’architecture de référence Enhanced Security Administrative Environment (ESAE) déployé par les équipes de services professionnels de cybersécurité de Microsoft pour protéger les clients contre les attaques.

Forêts administratives dédiées permettent aux organisations d’héberger des comptes d’administration, les stations de travail et les groupes dans un environnement qui dispose de contrôles de sécurité plus puissant que l’environnement de production.

Cette architecture permet plusieurs contrôles de sécurité qui ne sont pas possibles ou facilement configurés dans une architecture à forêt unique, même si elle est gérée avec stations travail à accès privilégié (Paw). Cette approche permet l’approvisionnement de comptes non privilégié les utilisateurs standard dans la forêt d’administration qui sont des privilèges très élevés dans l’environnement de production, l’activation de mise en œuvre technique supérieure de la gouvernance. Cette architecture permet également l’utilisation de la fonctionnalité d’authentification sélective d’une approbation comme un moyen de limiter les ouvertures de session (et d’exposition des informations d’identification) aux seuls les hôtes autorisés. Dans les situations dans lesquelles un niveau supérieur de garantie est souhaité pour la forêt de production sans impliquer le coût et la complexité d’une recréation complète, une forêt d’administration peut fournir un environnement qui augmente le niveau d’assurance de l’environnement de production.

Pendant que cette approche ajoute une forêt pour un environnement ActiveDirectory, le coût et la complexité sont limités par la conception fixe, un encombrement matériel/logiciel small et un petit nombre d’utilisateurs.

> [!NOTE]
> Cette approche fonctionne bien pour administrer ActiveDirectory, mais de nombreuses applications ne sont pas compatibles avec gérée par les comptes à partir d’une forêt externe à l’aide d’une relation d’approbation standard.

Cette figure illustre une forêt ESAE utilisée pour l’administration des ressources de niveau 0 et une forêt PRIV configurée pour une utilisation avec la fonctionnalité de gestion de l’accès privilégié de MicrosoftIdentity Manager. Pour plus d’informations sur le déploiement d’une instance MIM PAM, voir [Privileged Identity Management pour les Services de domaine ActiveDirectory (ADDS)](https://technet.microsoft.com/en-us/library/mt150258.aspx) article.

![Figure montrant une forêt ESAE utilisée pour l’administration des ressources de niveau 0 et une forêt PRIV configurée pour une utilisation avec la fonctionnalité de gestion de l’accès privilégié de MicrosoftIdentity Manager](../media/securing-privileged-access-reference-material/PAW_RM_Fig14.JPG)

Une forêt d’administration dédiée est un domaine unique standard forêt ActiveDirectory dédié à la fonction de gestion ActiveDirectory. Domaines et forêts administratives peuvent être renforcés de façon plus stricte que les cas d’utilisation des forêts de production en raison du nombre limité.

Conception d’une forêt d’administration doit inclure les considérations suivantes:

-   **Étendue limitée** -la valeur principale d’une forêt d’administration est le niveau élevé de garantie de sécurité et de la surface d’attaque réduite aboutissant à un risque résiduel inférieur. La forêt peut être utilisée pour héberger des applications et autres fonctions de gestion, mais chaque augmentation de l’étendue augmente la surface d’attaque de la forêt et ses ressources. L’objectif est de limiter les fonctions des forêt et des utilisateurs administrateurs à l’intérieur de conserver la surface d’attaque minimale, de sorte que chaque augmentation de l’étendue doit être envisagée avec précaution.

-   **Configurations d’approbation** -configurer l’approbation à partir de forestss gérés ou les domaines de la forêt d’administration

    -   Une approbation à sens unique est requise à partir de l’environnement de production pour la forêt d’administration. Cela peut être une approbation de domaine ou une approbation de forêt. Le domaine ou forêt admin doit-elle pas approuver les domaines/forêts gérés pour gérer ActiveDirectory, même si d’autres applications peuvent nécessiter une relation d’approbation bidirectionnelle, la validation de la sécurité et de test.

    -   Authentification sélective doit être utilisée pour limiter les comptes dans la forêt d’administration à la seule connexion aux hôtes de production. Pour la gestion des contrôleurs de domaine et la délégation de droits dans ActiveDirectory, ceci nécessite généralement d’accorder «Autorisé à ouvrir une session» pour les contrôleurs de domaine pour les comptes d’administrateur 0désignés de niveau dans la forêt d’administration. Pour plus d’informations, voir Configuration des paramètres d’authentification sélective.

-   **Privilèges et renforcement de domaines** -la forêt d’administration doit être configurée avec des privilèges minimum en fonction de la configuration requise pour l’administration d’ActiveDirectory.

    -   Octroi des droits pour administrer des contrôleurs de domaine et de déléguer des autorisations nécessite l’ajout de comptes de forêt d’administration au groupe local BUILTIN\Administrators domaine. Il s’agit, car le groupe global Administrateurs du domaine ne peut pas avoir de membres d’un domaine externe.

    -   Le seul inconvénient à l’aide de ce groupe pour accorder des droits est qu’il n’a accès administratif aux nouveaux objets de stratégie de groupe par défaut. Cela peut être modifié en suivant la procédure de [cet article de la base de connaissances](https://support.microsoft.com/kb/321476) pour modifier les autorisations par défaut de schéma.

    -   Dans la forêt d’administration des comptes qui sont utilisés pour administrer l’environnement de production ne doivent pas recevoir des privilèges d’administration pour la forêt d’administration, domaines ou des stations de travail qu’il contient.

    -   Privilèges d’administration sur la forêt d’administration doivent être étroitement contrôlés par un processus hors connexion afin de réduire la possibilité d’effacer les journaux d’audit pour un attaquant ou malveillants. Cela vous permet également de s’assurer que le personnel avec des comptes d’administrateur de production ne peut pas assouplisse les restrictions sur leurs comptes et augmente le risque de l’organisation.

    -   La forêt d’administration doit respecter les configurations de ligne de base de la conformité de sécurité Microsoft (SCB) pour le domaine, notamment les configurations renforcées des protocoles d’authentification.

    -   Tous les hôtes de forêt d’administration doivent être automatiquement mis à jour avec les mises à jour de sécurité. Cela peut créer le risque d’interruption des opérations de maintenance de contrôleur de domaine, mais il assure une importante atténuation des risques de sécurité des vulnérabilités non corrigées.

        > [!NOTE]
        > Une instance de WindowsServerUpdateServices dédiée peut être configurée pour approuver automatiquement les mises à jour. Pour plus d’informations, voir la section «Approuver automatiquement les mises à jour pour Installation» dans approbation des mises à jour.

-   **Renforcement de la station de travail** -Build les stations de travail d’administration à l’aide de la [stations de travail de l’accès privilégié](../securing-privileged-access/privileged-access-workstations.md) (par le biais de la Phase 3), mais changer l’appartenance au domaine à la forêt d’administration au lieu de l’environnement de production.

-   **Serveur et renforcement de contrôleur de domaine** - tous les contrôleurs de domaine et serveurs de la forêt d’administration:

    -   Vérifiez tous les supports sont validés en utilisant les instructions de [Source propre pour support d’installation](http://aka.ms/cleansource)

    -   Vérifiez que les serveurs de la forêt d’administration doivent avoir les systèmes d’exploitation plus récents installés, même si cela n’est pas faisable en production.

    -   Hôtes de forêt d’administration doivent être automatiquement mis à jour avec les mises à jour de sécurité.

        > [!NOTE]
        > WindowsServerUpdateServices peut être configurés pour approuver automatiquement les mises à jour. Pour plus d’informations, voir la section «Approuver automatiquement les mises à jour pour Installation» dans approbation des mises à jour.

    -   Lignes de base de sécurité doivent être utilisés comme configurations de démarrage.

        > [!NOTE]
        > Les clients peuvent utiliser le Toolkit de conformité de sécurité (SCT) Microsoft pour configurer les lignes de base sur les ordinateurs hôtes d’administration.

    -   Démarrage sécurisé pour atténuer des personnes malveillantes ou contre les programmes malveillants tentant de charger du code non signé dans le processus de démarrage.

        > [!NOTE]
        > Cette fonctionnalité a été introduite dans Windows8 pour tirer parti de la Extensible Firmware Interface UEFI (Unified).

    -   Chiffrement de volume complet pour limiter la perte physique d’ordinateurs, tels que les ordinateurs portables d’administration utilisés à distance.

        > [!NOTE]
        > Voir [BitLocker](https://technet.microsoft.com/en-us/library/dn641993.aspx) pour plus d’informations.

    -   Restrictions USB pour vous protéger contre les vecteurs d’infection physique.

        > [!NOTE]
        > Voir [contrôle accès en lecture ou écriture aux dispositifs ou médias amovibles](https://technet.microsoft.com/en-us/library/cc730808(v=ws.10).aspx) pour plus d’informations.

    -   Isolement réseau pour vous protéger contre les attaques réseau et les actions d’administration par inadvertance. Les pare-feu des hôtes doivent bloquer toutes les connexions entrantes, sauf celles qui sont explicitement requises et bloquer tout accès Internet sortant.

    -   Logiciel anti-programme malveillant pour vous protéger contre les programmes malveillants et menaces connues.

    -   Analyse pour empêcher l’introduction de nouveaux vecteurs d’attaque pour Windows lors de l’installation de nouveaux logiciels de la surface d’attaque.

        > [!NOTE]
        > Utilisation d’outils tels que le [attaque Surface Analyzer (ASA)](https://www.microsoft.com/en-us/download/details.aspx?id=24487) aideront à évaluer les paramètres de configuration sur un ordinateur hôte et identifier les vecteurs d’attaque introduits par des modifications logicielles ou de configuration.

-   Renforcement des comptes

    -   L’authentification multifacteur doit être configurée pour tous les comptes dans la forêt d’administration, sauf. Au moins un compte d’administration doit être le mot de passe pour garantir un accès fonctionnera en cas de traiter les sauts de l’authentification multifacteur. Ce compte doit être protégé par un processus de contrôle physique draconien.

    -   Comptes configurés pour l’authentification multifacteur doivent être configurés pour définir un nouveau NTLM hachage sur comptes régulièrement. Ceci est possible en désactivant et l’activation de l’attribut de compte carte à puce est requise pour l’ouverture de session interactive.

        > [!NOTE]
        > Vous pouvez ainsi interrompre des opérations en cours qui utilisent ce compte, afin de ce processus doit uniquement être lancé quand les administrateurs ne pas utiliser le compte, comme la nuit ou le week-end.

-   Contrôles de détection

    -   Contrôles de détection pour la forêt d’administration doivent être conçus pour alerter en cas d’anomalies dans la forêt d’administration. Le nombre limité de scénarios autorisés et d’activités peut aider à spécifier ces contrôles de façon plus précise que l’environnement de production.

Pour plus d’informations sur les services Microsoft pour la conception et déploiement d’un ESAE pour votre environnement, consultez [cette page](https://www.microsoft.com/services).

## <a name="T0E_BM"></a>Équivalence de niveau 0
La plupart des organisations contrôle l’appartenance aux puissants groupes ActiveDirectory de niveau 0 comme administrateurs, Admins du domaine et administrateurs de l’entreprise.  De nombreuses organisations négligent le risque d’autres groupes qui sont pourtant équivalents privilège dans un environnement ActiveDirectory standard. Ces groupes offrent une voie d’escalade relativement facile pour un intrus des mêmes privilèges de niveau 0explicites à l’aide de diverses méthodes d’attaque.

Par exemple, un opérateur de serveur peut accéder à un média de sauvegarde d’un contrôleur de domaine et extraire les informations d’identification des fichiers dans ce média et les utiliser pour élever les privilèges.

Les organisations doivent contrôler et surveiller l’appartenance dans tous les groupes de niveau 0 (y compris l’appartenance imbriquée), y compris:

-   Administrateurs de l’entreprise

-   Admins du domaine

-   Administrateurs du schéma

-   BUILTIN\Administrators

-   Opérateurs de compte

-   Opérateurs de sauvegarde

-   Opérateurs d’impression

-   Opérateurs de serveur

-   Contrôleurs de domaine

-   Contrôleurs de domaine en lecture seule

-   Groupe Propriétaires créateurs de stratégie

-   Opérateurs de chiffrement

-   Utilisateurs du modèle COM distribué

-   Autres groupes délégués

    > [!NOTE]
    > «Autres groupes délégués» fait référence à des groupes qui peuvent être créés par votre organisation pour gérer les opérations d’annuaire pouvant aussi disposer accès de niveau 0 effectif.

## <a name="ATLT_BM"></a>Outils d’administration et Types d’ouverture de session
Il s’agit des informations de référence pour aider à identifier le risque d’exposition des informations d’identification associée à l’aide de différents outils d’administration pour l’administration à distance.

Dans un scénario d’administration à distance, les informations d’identification sont toujours exposées sur l’ordinateur source est toujours recommandé d’une station de travail digne de confiance de l’accès privilégié (PAW) pour les comptes sensibles ou à impact élevé. Indique si les informations d’identification sont exposées à un vol potentiel sur l’ordinateur (distant) cible dépendent principalement du type d’ouverture de session windows utilisé par la méthode de connexion.

Ce tableau comprend des recommandations pour les outils d’administration courants et les méthodes de connexion:

|Connexion<br />méthode|Type d’ouverture de session|Informations d’identification réutilisables sur la destination|Commentaires|
|-----------|-------|--------------------|------|
|Ouvrez une session sur la console|Interactive|v|Inclut l’accès à distance du matériel / des cartes d’extinction et des KVM réseau.|
|RUNAS|Interactive|v||
|RUNAS//NETWORK|Informations d’identification|v|Clone la session LSA pour un accès local, mais utilise les nouvelles informations d’identification lors de la connexion aux ressources réseau.|
|Bureau à distance (réussite)|Interactif distant|v|Si le client Bureau à distance est configuré pour partager des ressources et les appareils locaux, ceux peuvent également être compromis.|
|Bureau à distance (échec - type d’ouverture de session a été refusé)|Interactif distant|-|Par défaut, si les informations d’identification RDP d’ouverture de session échoue sont uniquement stockées très brièvement. Cela ne peut pas être le cas si l’ordinateur est compromis.|
|Commande net use * \\\SERVER|Réseau|-||
|Commande net use * \\\SERVER /u: utilisateur|Réseau|-||
|Composants logiciels enfichables MMC pour un ordinateur distant|Réseau|-|Exemple: Ordinateur de gestion, l’Observateur d’événements, le Gestionnaire de périphériques, Services|
|PowerShell WinRM|Réseau|-|Exemple: Enter-PSSession serveur|
|PowerShell WinRM avec CredSSP|Texte en clair réseau|v|Serveur New-PSSession<br />-Authentication Credssp<br />-Credential cred|
|PsExec sans informations d’identification explicites|Réseau|-|Exemple: PsExec \\\server cmd|
|PsExec avec informations d’identification explicites|Réseau + interactif|v|PsExec \\\server -u utilisateur -p pwd cmd<br />Crée plusieurs sessions d’ouverture de session.|
|Registre à distance|Réseau|-||
|Passerelle des services Bureau à distance|Réseau|-|Authentification sur la passerelle des services Bureau à distance.|
|Tâche planifiée|Traitement par lots|v|Mot de passe sera également enregistré en tant que secret LSA sur le disque.|
|Exécuter les outils en tant que service|Service|v|Mot de passe sera également enregistré en tant que secret LSA sur le disque.|
|Analyseurs de vulnérabilité|Réseau|-|La plupart des analyseurs utilisent par défaut des ouvertures de session réseau, bien que certains fournisseurs peuvent implémenter des ouvertures de session non-réseau et introduire plus de risque de vol d’informations d’identification.|

Pour l’authentification web, utilisez la référence indiquée dans le tableau ci-dessous:

|Connexion<br />méthode|Type d’ouverture de session|Informations d’identification réutilisables sur la destination|Commentaires|
|-----------|-------|--------------------|------|
|IIS «Authentification de base»|Texte en clair réseau<br />(IIS) 6.0 +<br /><br />Interactive<br />(avant IIS 6.0)|v||
|IIS «l’authentification intégrée Windows»|Réseau|-|Fournisseurs NTLM et Kerberos.|

Définitions des colonnes:

-   **Type d’ouverture de session** identifie le type d’ouverture de session lancé par la connexion.

-   **Les informations d’identification réutilisables sur la destination** indique que les types suivants d’informations d’identification sont stockées dans la mémoire de processus LSASS sur l’ordinateur de destination où le compte spécifié est connecté en local:

    -   Hachages LM et NT

    -   Tickets TGT Kerberos

    -   Mot de passe de texte en clair (le cas échéant).

    -

Les symboles dans ce tableau définie comme suit:

-   (-) indique que les informations d’identification ne sont pas exposées.

-   (v) Indique que les informations d’identification sont exposées.

Pour les applications de gestion qui ne sont pas dans ce tableau, vous pouvez déterminer le type d’ouverture de session à partir du champ de type d’ouverture de session dans l’auditer les événements d’ouverture de session. Pour plus d’informations, voir [auditer les événements d’ouverture de session](https://technet.microsoft.com/en-us/library/cc787567(v=ws.10).aspx).

Sur les ordinateurs basés sur Windows, toutes les authentifications sont traitées comme un des nombreux types d’ouverture de session, quel que soit l’authentification protocole ou l’authentificateur utilisé. Le tableau ci-dessous comprend les types d’ouverture de session les plus courants et leurs attributs relatifs au vol d’informations d’identification:

|Type d’ouverture de session|#|Authentificateurs acceptés|Informations d’identification réutilisables dans une session LSA|Exemples|
|-------|---|--------------|--------------------|------|
|Interactif (également appelé ouverture de session locale)|2|Mot de passe, carte à puce,<br />autres|Oui|Ouverture de session console<br />RUNAS;<br />Solutions de contrôle à distance de matériel (par exemple, KVM réseau ou d’accès à distance / carte d’extinction dans server)<br />Authentification de base IIS (avant IIS 6.0)|
|Réseau|3|Mot de passe,<br />Hachage NT,<br />Ticket Kerberos|Non (sauf si la délégation est activée, puis les tickets Kerberos sont présents)|NET USE,<br />Appels RPC;<br />Registre à distance;<br />Authentification Windows intégrée de IIS<br />Authentification Windows SQL|
|Traitement par lots|4|Mot de passe (généralement stocké en tant que secret LSA)|Oui|Tâches planifiées|
|Service|5|Mot de passe (généralement stocké en tant que secret LSA)|Oui|Services Windows|
|Texte en clair réseau|8|Mot de passe|Oui|Authentification de base IIS (IIS 6.0 et versions ultérieures);<br />Windows PowerShell avec CredSSP|
|Informations d’identification|9|Mot de passe|Oui|RUNAS//NETWORK|
|Interactif distant|10|Mot de passe, carte à puce,<br />autres|Oui|Bureau à distance (anciennement appelé «Services Terminal Server»)|

Définitions des colonnes:

-   **Type d’ouverture de session** est le type d’ouverture de session demandé.

-   **#**est l’identificateur numérique pour le type d’ouverture de session qui est indiqué dans les événements d’audit dans le journal de sécurité.

-   **Authentificateurs acceptés** indique les types d’authentificateurs sont en mesure de lancer une ouverture de session de ce type.

-   **Informations d’identification réutilisables** dans LSA session indique si le type d’ouverture de session entraîne la session LSA contenant les informations d’identification, telles que des mots de passe en texte en clair, les hachages NT ou les tickets Kerberos qui peuvent être utilisés pour s’authentifier auprès d’autres ressources réseau.

-   **Exemples** répertorie des scénarios courants dans lesquels le type d’ouverture de session est utilisé.

> [!NOTE]
> Pour plus d’informations sur les Types d’ouverture de session, consultez [SECURITY_LOGON_TYPE (énumération)](https://technet.microsoft.com/en-us/library/aa380129(VS.85).aspx).
