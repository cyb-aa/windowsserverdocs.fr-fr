---
title: "Stations de travail de l’accès privilégié"
description: "Sécurité de Windows Server"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 93589778-3907-4410-8ed5-e7b6db406513
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 06/09/2016
ms.openlocfilehash: ce64974d771a11ef11257bceef1c3fde1797a7da
ms.sourcegitcommit: 3bf47cf4e25896725137d983d63b8041a53cb9a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/25/2018
---
# <a name="privileged-access-workstations"></a>Stations de travail de l’accès privilégié

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Stations travail à accès privilégié (Paw) fournissent un système d’exploitation dédié pour les tâches sensibles soient protégées contre les attaques Internet et les vecteurs de menace. Séparer ces tâches et comptes sensibles de quotidiennement utiliser des stations de travail et périphériques fournit très forte protection contre les attaques par hameçonnage, applications et des vulnérabilités du système d’exploitation, diverses attaques d’emprunt d’identité et les attaques de vol d’informations d’identification telles que l’enregistrement de frappe, [Pass-the-Hash](https://www.microsoft.com/en-us/download/details.aspx?id=36036), et [Pass-The-Ticket](https://download.microsoft.com/download/7/7/A/77ABC5BD-8320-41AF-863C-6ECFB10CB4B9/Mitigating%20Pass-the-Hash%20(PtH)%20Attacks%20and%20Other%20Credential%20Theft%20Techniques_English.pdf).

## <a name="architecture-overview"></a>Vue d’ensemble de l’architecture
Le diagramme ci-dessous illustre un «canal» distinct pour l’administration (une tâche très sensible) qui est créé en conservant des comptes d’administration dédiés distincts et les stations de travail.

![Diagramme montrant un «canal» distinct d’administration (une tâche très sensible) qui est créée en conservant des comptes d’administration dédiés distincts et les stations de travail](../media/privileged-access-workstations/PAWFig1.JPG)

Cette approche architecturale s’appuie sur les protections trouvées dans Windows10 [Credential Guard](https://technet.microsoft.com/en-us/library/mt483740%28v=vs.85%29.aspx) et [Device Guard](https://technet.microsoft.com/en-us/library/dn986865(v=vs.85).aspx) et va au-delà de ces protections pour les tâches et comptes sensibles.

Cette méthodologie est appropriée pour les comptes ayant accès aux ressources à valeur élevée:

-   **Des privilèges d’administration** les Paw fournissent une sécurité accrue pour un fort impact rôles d’administration et de tâches. Cette architecture peut être appliquée à l’administration de nombreux types de systèmes, y compris les domaines ActiveDirectory et des forêts, les clients de MicrosoftAzure ActiveDirectory, aux clients Office 365, réseaux de contrôle de processus (PCN), données (SCADA) et contrôle des systèmes, des distributeurs (DAB) et appareils de Point de vente (PDV).

-   **Haute sensibilité travailleurs** l’approche utilisée dans un PAW peut également fournir une protection pour les tâches de traitement des informations hautement confidentielles et personnel, telles que celles impliquant une activité fusion et d’Acquisition d’annonce avant, versions préliminaire des rapports financiers, présence de médias sociaux d’organisation, communications executive, les secrets commerciaux non brevetées, recherche sensible ou autres données propriétaires ou sensibles. Ce guide ne pas discuter de la configuration de ces scénarios de traitement des informations en profondeur ou inclure ce scénario dans les instructions techniques.

    > [!NOTE]
    > MicrosoftIT utilise les Paw (en interne dénommé «stations de travail admin sécurisées», ou SAW) pour gérer l’accès sécurisé aux systèmes à haute valeur internes au sein de Microsoft. Ce guide propose ci-dessous des détails supplémentaires sur l’utilisation de PAW chez Microsoft dans la section «Comment Microsoft utilise les stations de travail admin». Pour plus d’informations sur cette approche d’environnement de ressources haute valeur, reportez-vous à l’article [protection des ressources à haute valeur avec les stations de travail admin sécurisées](https://msdn.microsoft.com/en-us/library/mt186538.aspx).

Ce document décrit pourquoi cette pratique est recommandée pour la protection des comptes privilégiés à impact élevé, ce que ces solutions PAW ressemblent pour la protection des privilèges d’administrateur et comment déployer rapidement une solution PAW pour l’administration des services de domaine et le cloud.

Ce document fournit des instructions détaillées pour l’implémentation de plusieurs configurations PAW et inclut des instructions d’implémentation détaillées pour vous aider à démarrer sur la protection des comptes à impact élevé courantes:

-   **Phase 1 - déploiement immédiat pour les administrateurs ActiveDirectory** cela fournit rapidement un PAW qui peut protéger locaux les rôles d’administration de domaine et de forêt

-   **Phase 2 - étendre les PAW à tous les administrateurs** Active la protection pour les administrateurs de services cloud comme Office 365 et Azure, serveurs d’entreprise, les applications d’entreprise et stations de travail

-   **Phase 3 - sécurité avancée avec PAW** traite les protections supplémentaires et les considérations relatives à la sécurité des PAW

### <a name="why-a-dedicated-workstation"></a>Pourquoi une station de travail dédiée?
L’environnement de menace actuel pour les organisations est répandue d’hameçonnages sophistiqués et d’autres attaques internet qui continue du risque de compromettre la sécurité pour les comptes exposés à internet et les stations de travail.

Cet environnement de menace nécessite une organisations à adopter une posture de sécurité «supposent violation» lors de la conception des protections pour les ressources à valeur élevée, comme les comptes d’administration et des ressources d’entreprise sensibles. Ces ressources à valeur élevée doivent être protégées contre les direct menaces internet ainsi que les attaques montées à partir d’autres stations de travail, les serveurs et les périphériques de l’environnement.

![Figure montrant le risque pour les ressources gérées Si une personne malveillante prend le contrôle d’une station de travail utilisateur où les informations d’identification sensibles sont utilisées](../media/privileged-access-workstations/PAWFig2.JPG)

Cette figure illustre les risques pour les ressources gérées Si une personne malveillante prend le contrôle d’une station de travail utilisateur où les informations d’identification sensibles sont utilisées.

Une personne malveillante de contrôle d’un système d’exploitation a de nombreuses façons d’accéder à toutes les activités sur la station de travail de manière illégale et emprunter l’identité du compte légitime. Diverses techniques d’attaque connues et inconnues permet d’obtenir ce niveau d’accès. Les volumes croissants et la sophistication des cyberattaques ont rendu nécessaire d’étendre ce concept de séparation pour séparer complètement les systèmes d’exploitation de client pour les comptes sensibles. Pour plus d’informations sur ces types d’attaques, visitez le [site Web Pass The Hash](https://www.microsoft.com/pth) pour consulter des livres blancs, des vidéos et plus encore.

L’approche PAW est une extension de la pratique recommandée établie d’utiliser des comptes d’utilisateur et d’administration séparés pour le personnel administratif. Cette pratique utilise un compte d’administration affecté de manière individuelle qui est complètement distinct de compte d’utilisateur standard de l’utilisateur. PAW s’appuie sur cette pratique de séparation du compte en fournissant une station de travail digne de confiance pour les comptes sensibles.

> [!NOTE]
> MicrosoftIT utilise les Paw (en interne dénommé «stations de travail admin sécurisées», ou SAW) pour gérer l’accès sécurisé aux systèmes à haute valeur internes au sein de Microsoft.  Ce guide propose des détails supplémentaires sur l’utilisation de PAW chez Microsoft dans la section «Comment Microsoft utilise les stations de travail admin»
>
> Pour plus d’informations sur cette approche d’environnement de ressources haute valeur, reportez-vous à l’article [protection des ressources à haute valeur avec les stations de travail admin sécurisées](https://msdn.microsoft.com/en-us/library/mt186538.aspx).

Ce guide PAW est conçu pour vous aider à implémenter cette fonctionnalité pour protéger les comptes à valeur élevée telles que les administrateurs informatiques à privilèges élevés et les comptes d’entreprise haute sensibilité. Le guide vous aide à vous:

-   Limiter l’exposition des informations d’identification aux seuls hôtes approuvés

-   Fournir une station de travail haute sécurité aux administrateurs afin de pouvoir facilement effectuer des tâches d’administration.

Restreindre les comptes sensibles à l’utilisation exclusive de Paw renforcés est une protection simple pour ces comptes qui est utilisable pour les administrateurs et très difficile à défaire pour.

#### <a name="alternate-approaches---limitations-considerations-and-integration"></a>Autres approches - restrictions, considérations et intégration
Cette section contient des informations sur la façon dont la sécurité des approches compare à PAW et comment intégrer correctement ces approches dans une architecture PAW. Toutes ces approches comportent des risques importants lors de l’implémentation de manière isolée, mais peuvent ajouter de valeur à une implémentation PAW dans certains scénarios.

**Credential Guard et MicrosoftPassport**

Introduite dans Windows10, [Credential Guard](https://technet.microsoft.com/en-us/library/mt483740%28v=vs.85%29.aspx) utilise le matériel et la sécurité basée sur la virtualisation pour atténuer les attaques de vol d’informations d’identification courantes, telles que Pass-the-Hash, en protégeant les informations d’identification dérivées. La clé privée des informations d’identification utilisées par [MicrosoftPassport](http://aka.ms/passport) peut être également être protégé par le matériel du Module de plateforme sécurisée (TPM).

Il s’agit d’atténuations puissantes, mais les stations de travail peuvent encore être vulnérables à certaines attaques même si les informations d’identification sont protégées par Credential Guard ou Passport. Les attaques peuvent inclure l’utilisation abusive des privilèges et des informations d’identification directement à partir d’un appareil, la réutilisation des informations d’identification précédemment volées avant d’activer Credential Guard et l’abus des configurations d’applications gestion outils et faible sur la station de travail.

Le Guide PAW de cette section inclut l’utilisation d’un grand nombre de ces technologies pour les tâches et les comptes à sensibilité élevée.

**Machines virtuelles administratives**

Un ordinateur virtuel d’administration (VM) est un système d’exploitation dédié pour les tâches administratives hébergé sur un ordinateur de bureau utilisateur standard. Cette approche est similaire à PAW, en fournissant un système d’exploitation dédié pour les tâches d’administration, il a une erreur irrécupérable dans la mesure où dépend de la machine virtuelle administrative sur le bureau utilisateur standard pour sa sécurité.

Le diagramme ci-dessous illustre la capacité des pirates à suivre la chaîne de contrôle à l’objet cible de vos centres d’intérêt avec une VM Admin sur une station de travail utilisateur et qu’il est difficile de créer un chemin d’accès sur la configuration inverse.

L’architecture PAW ne permet pas pour l’hébergement d’une machine virtuelle administrative sur une station de travail utilisateur, mais une machine virtuelle d’utilisateur avec une image d’entreprise standard peut être hébergé sur un ordinateur hôte PAW pour fournir au personnel un PC unique pour toutes les responsabilités.

![Diagramme de l’architecture PAW](../media/privileged-access-workstations/PAWFig9.JPG)

**Serveur de renvoi**

Administration architectures «Serveur de renvoi» configurer un petit nombre des serveurs console d’administration et limiter personnel leur utilisation pour les tâches administratives. Cela est généralement lié à des services Bureau à distance, une solution de virtualisation de présentation 3 rd-party ou une technologie Virtual Desktop Infrastructure (VDI).

Cette approche est souvent proposée pour atténuer les risques pour l’administration et qu’il fournit des garanties de sécurité, mais l’approche de serveur renvoi elle-même est vulnérable à certaines attaques car il ne respecte pas la [principe de «source» propre](http://aka.ms/cleansource). Le principe de source propre nécessite toutes les dépendances de sécurité de confiance en tant que l’objet sécurisé.

![Figure montrant une relation de contrôle simple](../media/privileged-access-workstations/PAWFig3.JPG)

Cette figure illustre une relation de contrôle simple. N’importe quel sujet ayant le contrôle d’un objet est une dépendance de sécurité de cet objet. Si un pirate peut contrôler une dépendance de sécurité d’un objet cible (sujet), il peut contrôler cet objet.

La session d’administration sur le serveur de renvoi s’appuie sur l’intégrité de l’ordinateur local y accéder. Si cet ordinateur est une station de travail utilisateur soumis à des attaques par hameçonnage et d’autres vecteurs d’attaque sur internet, la session d’administration est également soumis à ces risques.

![Figure montrant comment les pirates peuvent suivre une chaîne de contrôle établie à l’objet cible de vos centres d’intérêt](../media/privileged-access-workstations/PAWFig4.JPG)

L’illustration ci-dessus montre comment les pirates peuvent suivre une chaîne de contrôle établie à l’objet cible de vos centres d’intérêt.

Alors que certains contrôles de sécurité avancés, comme l’authentification multifacteur permettre augmenter la difficulté à une personne malveillante prenant en charge cette session d’administration à partir de la station de travail utilisateur, aucune fonctionnalité de sécurité peuvent entièrement protéger contre les attaques techniques lorsqu’une personne malveillante a accès administratif de l’ordinateur source (par exemple, injection de commandes illicites dans une session légitime, piratage des processus légitimes et ainsi de suite.)

La configuration par défaut dans ce guide de PAW installe les outils d’administration sur le PAW, mais une architecture de serveur de renvoi peut également être ajoutée si nécessaire.

![Figure montrant comment inverser la relation de contrôle et d’accéder aux applications de l’utilisateur à partir d’une station de travail administrateur ne laissent au pirate aucun chemin d’accès à l’objet ciblé](../media/privileged-access-workstations/PAWFig5.JPG)

Cette illustration montre comment inverser la relation de contrôle et d’accéder aux applications de l’utilisateur à partir d’une station de travail administrateur ne laissent au pirate aucun chemin d’accès à l’objet ciblé. Le serveur de renvoi utilisateur est toujours exposé à des risques donc appropriés, les contrôles de détection et les processus de réponse doivent toujours être appliqués pour cet ordinateur accessible sur internet.

Cette configuration nécessite de suivre les pratiques opérationnelles en étroite collaboration pour vous assurer qu’ils saisie accidentelle des informations d’identification d’administrateur dans la session utilisateur sur son bureau, les administrateurs.

![Figure montrant comment accéder à un serveur de renvoi administratif à partir d’un PAW n’ajoute aucun chemin d’accès pour la personne malveillante dans les ressources d’administration](../media/privileged-access-workstations/PAWFig6.JPG)

Cette illustration montre comment accéder à un serveur de renvoi administratif à partir d’un PAW n’ajoute aucun chemin d’accès pour la personne malveillante dans les ressources d’administration. Un serveur de renvoi avec un PAW dans ce cas vous permet de consolider le nombre d’emplacements pour surveiller l’activité d’administration et de distribuer des outils et applications d’administration. Cela ajoute une certaine complexité de la conception, mais peut simplifier les mises à jour de logiciels et la surveillance sécurité si un grand nombre de comptes et stations de travail est utilisé dans votre implémentation PAW. Le serveur de renvoi doivent être créé et configuré pour les normes de sécurité similaire en tant que le PAW.

**Solutions de gestion des privilèges**

Solutions de gestion des privilèges sont des applications qui fournissent un accès temporaire à privilèges discrets ou des comptes privilégiés à la demande. Solutions de gestion des privilèges sont un composant primordial d’une stratégie complète pour sécuriser l’accès privilégié et fournir une visibilité extrêmement importante et responsabilisation des activités administratives.

Ces solutions utilisent généralement un flux de travail flexible pour accorder l’accès et beaucoup ont des fonctionnalités de sécurité supplémentaires et des fonctionnalités telles que la gestion de mots de passe de compte de service et l’intégration avec les serveurs de renvoi administratif. Il existe de nombreuses solutions sur le marché qui fournissent des fonctionnalités de gestion, y compris la gestion des accès privilégiés MicrosoftIdentity Manager (MIM) (PAM) de privilège.

Microsoft recommande d’utiliser un PAW pour les solutions de gestion des accès privilège. Accès à ces solutions doit être accordé uniquement aux Paw. Microsoft ne recommande pas l’utilisation de ces solutions en remplacement d’un PAW car l’accès à des privilèges à l’aide de ces solutions à partir d’un ordinateur de bureau utilisateur potentiellement compromis ne respecte pas la [source propre](http://aka.ms/cleansource) principe comme illustré dans le diagramme ci-dessous:

![Diagramme montrant comment Microsoft ne recommande pas à l’aide de ces solutions en remplacement d’un PAW, car l’accès à des privilèges à l’aide de ces solutions à partir d’un ordinateur de bureau utilisateur potentiellement compromis viole le principe de source propre](../media/privileged-access-workstations/PAWFig7.JPG)

Fournir un PAW pour accéder à ces solutions vous permet de bénéficier des avantages de sécurité de PAW et la solution de gestion de privilèges, comme illustré dans ce diagramme:

![Diagramme montrant comment fournir un PAW pour accéder à ces solutions vous permet de bénéficier des avantages de sécurité de PAW et la solution de gestion de privilèges](../media/privileged-access-workstations/PAWFig8.JPG)

> [!NOTE]
> Ces systèmes doivent être classés au niveau plus élevé du privilège gérer et de protéger à ou au-dessus de ce niveau de sécurité. Ceux-ci sont généralement configurés pour gérer les ressources de niveau 0 et les solutions de niveau 0 et doivent être classés au niveau 0.
> Pour plus d’informations sur le modèle de niveau, voir [http://aka.ms/tiermodel](http://aka.ms/tiermodel) pour plus d’informations sur les groupes de niveau 0, consultez l’équivalence de niveau 0dans [sécurisation de l’accès référence privilégié](../securing-privileged-access/securing-privileged-access-reference-material.md).

Pour plus d’informations sur le déploiement de gestion de l’accès privilégié MicrosoftIdentity Manager (MIM) (PAM), voir [http://aka.ms/mimpamdeploy](http://aka.ms/mimpamdeploy)

#### <a name="how-microsoft-is-using-admin-workstations"></a>Comment Microsoft utilise les stations de travail admin
Microsoft utilise l’approche d’architecturale PAW en interne sur nos systèmes ainsi qu’avec nos clients. Microsoft utilise les stations de travail administratives en interne dans un certain nombre de capacités, notamment l’administration de l’infrastructure de MicrosoftIT, développement de l’infrastructure Microsoft cloud fabric operations et des autres ressources à valeur élevée.

Ce guide est directement basé sur l’architecture de référence de station de travail à accès privilégié (PAW) déployé par nos équipes de services professionnels de cybersécurité pour protéger les clients contre les attaques. Les stations de travail d’administration sont également un élément clé de la protection la plus robuste pour les tâches d’administration, l’architecture de référence de forêt d’administration améliorée sécurité Administrative Environment (ESAE).

Pour plus d’informations sur la forêt d’administration ESAE, consultez [approche de conception de forêt d’administration ESAE](http://aka.ms/ESAE) section [sécurisation de l’accès référence privilégié](../securing-privileged-access/securing-privileged-access-reference-material.md).

Pour plus d’informations sur le recours aux services Microsoft pour déployer un PAW ou ESAE pour votre environnement, contactez votre représentant Microsoft ou visitez [cette page](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx).

### <a name="what-is-a-privileged-access-workstation-paw"></a>Qu’est une station de travail à accès privilégié (PAW)?
En termes plus simples, un PAW est une station de travail renforcée et verrouillée conçue pour fournir des garanties de haute sécurité pour les tâches et comptes sensibles.  Les Paw sont recommandés pour l’administration des systèmes d’identité, la services cloud et la structure de cloud privé ainsi que les fonctions sensibles de l’entreprise.

> [!NOTE]
> L’architecture PAW ne nécessite pas un mappage 1:1des comptes pour les stations de travail, s’il s’agit d’une configuration commune. Un PAW crée un environnement de station de travail de confiance qui peut être utilisé par un ou plusieurs comptes.

Afin d’offrir la plus grande sécurité, Paw doivent toujours exécuter le plus à jour et sécurisé système d’exploitation: Microsoft recommande fortement Windows10 entreprise, qui inclut un certain nombre de fonctionnalités de sécurité supplémentaires non disponibles dans les autres éditions (en particulier, [Credential Guard](https://technet.microsoft.com/en-us/library/mt483740%28v=vs.85%29.aspx) et [Device Guard](https://technet.microsoft.com/en-us/library/dn986865(v=vs.85).aspx)).

> [!NOTE]
> Les organisations sans accès à Windows10 entreprise peuvent utiliser Windows10 Pro, qui comprend de nombreuses technologies fondamentales critiques pour les Paw, y compris le démarrage sécurisé, BitLocker et Bureau à distance.  Clients de l’éducation peuvent utiliser Windows10 éducation.  Windows10 famille ne doit pas être utilisé pour un PAW.
>
> Pour un tableau de comparaison des différentes éditions de Windows10, lisez [cet article](https://www.microsoft.com/en-us/WindowsForBusiness/Compare).

Les contrôles de sécurité dans le PAW visent à atténuer l’impact le plus élevé et les risques les plus probables de compromission. Il s’agit notamment d’atténuation des attaques sur l’environnement et d’atténuation des risques que les contrôles du PAW se dégradent au fil du temps:

-   **Attaques Internet** -la plupart des attaques provenant de sources internet directement ou indirectement et utilisent internet pour l’exfiltration, les commandes et contrôle (C2). Isoler le PAW de l’internet public est un élément clé pour garantir que le PAW n’est pas compromis.

-   **Risque de facilité d’utilisation** -si un PAW est trop difficile à utiliser pour les tâches quotidiennes, les administrateurs seront encouragés créer des solutions de contournement pour faciliter leurs tâches. Souvent, ces solutions de contournement Ouvrez la station de travail d’administration et les comptes à des risques de sécurité, il est donc essentiel d’impliquer et permettre aux utilisateurs PAW pour atténuer ces problèmes de facilité d’utilisation en toute sécurité. Cette opération est effectuée souvent en écoutant leurs commentaires, l’installation des outils et scripts requis pour effectuer leur travail et en garantissant le personnel administratif connaissent pourquoi ils doivent utiliser un PAW, quels un PAW est et comment l’utiliser correctement et avec succès.

-   **Risques de l’environnement** -étant donné que de nombreux autres ordinateurs et les comptes de l’environnement sont exposés au répertoire de risques internet ou indirectement, un PAW doit être protégé contre les attaques de ressources compromises dans l’environnement de production. Cela nécessite de limiter les outils de gestion et les comptes qui ont accès au Paw au strict minimum requis pour sécuriser et surveiller ces stations de travail spécialisées.

-   **Fournir de falsification de la chaîne** - s’il est impossible de supprimer tous les risques possibles de falsification de la chaîne logistique pour le matériel et logiciel, en prenant quelques mesures essentielles peuvent atténuer les vecteurs d’attaque critiques qui sont accessibles aux pirates. Cela inclut la validation de l’intégrité de tous les supports d’installation ([principe de Source](http://aka.ms/cleansource)) et à l’aide d’un fournisseur approuvé et digne de confiance pour le matériel et logiciel.

-   **Les attaques physiques** -les Paw pouvant être physiquement mobiles et utilisés en dehors des emplacements physiquement sécurisés, ils doivent être protégés contre les attaques qui exploitent l’accès physique à l’ordinateur.

> [!NOTE]
> Un PAW ne protègera pas un environnement d’un pirate qui a déjà acquis un accès administratif sur une forêt ActiveDirectory.
> Étant donné que de nombreuses implémentations existantes des Services de domaine ActiveDirectory ont été utilisées pendant des années, au risque de vol d’informations d’identification, organisations doivent supposer violation et envisagez la possibilité que peut avoir une compromission des informations d’identification d’administrateur entreprise ou de domaine non détectée. Une organisation qui soupçonne la compromission de domaine doit envisager l’utilisation des services de réponse aux incidents professionnels.
>
> Pour plus d’informations sur les instructions de réponse et de récupération, consultez «Répondre à une activité suspecte» et «Récupérer d’une violation» de sections de [Mitigating Pass-the-Hash et autres vols d’informations d’identification](https://www.microsoft.com/pth), version2.
>
> Visitez [réponse aux incidents de Microsoft et les services de récupération](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx) page pour plus d’informations.

### <a name="paw-hardware-profiles"></a>Profils matériels PAW
Le personnel administratif est également des utilisateurs standard trop: ils doivent avoir non seulement un PAW, mais aussi une station de travail utilisateur standard pour vérifier votre messagerie électronique, naviguer sur le web et accéder à une ligne d’entreprise des applications d’entreprise.  S’assurer que les administrateurs peuvent rester productifs et sécurisés est essentielle pour la réussite du déploiement d’un PAW.  Une solution sécurisée qui limite considérablement la productivité est abandonnée par les utilisateurs en faveur de ce qui améliore la productivité (même si elle s’effectue de façon non sécurisée).

Afin d’équilibrer les besoins de sécurité et de productivité, Microsoft recommande d’utiliser un de ces profils matériels PAW:

-   **Matériel dédié** -séparer les appareils dédiés pour les tâches d’utilisateur et les tâches d’administration

-   **Utilisation simultanée** -un appareil unique qui peut s’exécuter des tâches d’administration et les tâches utilisateur simultanément en tirant parti de la virtualisation du système d’exploitation ou d’une présentation.

Les organisations peuvent utiliser qu’un seul profil ou les deux. Il n’existe aucun problème d’interopérabilité entre les profils matériels, et les organisations ont la possibilité de faire correspondre le profil matériel pour les besoins spécifiques et la situation d’un administrateur donné.

> [!NOTE]
> Il est essentiel que, dans tous ces scénarios, personnel administratif reçoive un compte d’utilisateur standard qui est distinct du comptes administratifs. Les comptes d’administration doivent uniquement être utilisées sur le système d’exploitation du PAW.

Ce tableau récapitule les avantages et inconvénients relatifs de chaque profil matériel du point de vue de facilité d’utilisation opérationnelle et la productivité et de sécurité.  Les deux approches matérielles fournissent une sécurité renforcée pour les comptes administrateur contre le vol d’informations d’identification et de réutilisation.

|**Scénario**|**Avantages**|**Inconvénients**|
|--------|---------|-----------|
|Matériel dédié|-Signal fort pour la sensibilité des tâches<br />-Meilleure séparation de sécurité|-Espace de bureau supplémentaires<br />-Poids supplémentaires (pour le travail à distance)<br />-Coût du matériel|
|Utilisation simultanée|-Coût matériel<br />-Expérience sur appareil unique|-Partage de clavier/souris crée risque d’erreurs/risques accidentelles|

Ce guide contient des instructions détaillées pour la configuration de PAW pour l’approche de matériel dédié. Si vous avez des exigences pour les profils matériels à utilisation simultanée, vous pouvez adapter les instructions de fonction de ce guide vous-même ou passer par une entreprise de services professionnels comme Microsoft pour vous aider.

**Matériel dédié**

Dans ce scénario, un PAW est utilisé pour l’administration est complètement distincte de l’ordinateur qui est utilisé pour les activités quotidiennes telles que le courrier électronique, modification de documents et le travail de développement. Tous les outils d’administration et les applications sont installées sur le PAW et toutes les applications de productivité sont installées sur la station de travail utilisateur standard. Les instructions étape par étape de ce guide sont basées sur ce profil matériel.

**Utilisation simultanée - Ajout d’une machine virtuelle d’utilisateur local**

Dans ce scénario d’utilisation simultanée, un PC unique est utilisé pour les tâches d’administration et les activités quotidiennes telles que le courrier électronique, modification de documents et le travail de développement. Dans cette configuration, le système d’exploitation utilisateur est disponible hors connexion (pour modifier des documents et travailler sur la mise en cache localement par courrier électronique), mais nécessite le processus de support et de matériel capable de gérer cet état déconnecté.

![Diagramme montrant un PC unique dans un scénario d’utilisation simultanée utilisé pour les tâches d’administration et les activités quotidiennes telles que courrier électronique, modification de documents et le travail de développement](../media/privileged-access-workstations/PAWFig10.JPG)

Le matériel physique exécute deux systèmes d’exploitation localement:

-   **Système d’exploitation Admin** -l’hôte physique exécute Windows10 sur l’hôte PAW pour les tâches administratives

-   **Utilisateur du système d’exploitation** -invité d’ordinateurs virtuels client Hyper-V A Windows10 exécute une image d’entreprise

Avec Windows10/Hyper-V, un ordinateur virtuel d’invité (également exécutant Windows10) peut avoir une expérience utilisateur riche, y compris son, vidéo et applications de communication Internet comme Skype entreprise.

Dans cette configuration, les tâches quotidiennes qui ne nécessitent pas de privilèges d’administrateur sont effectuent dans l’ordinateur virtuel de système d’exploitation utilisateur qui possède une image Windows10 d’entreprise standard et ne sont pas soumises aux restrictions appliquées à l’hôte PAW. Toutes les tâches administratives sont effectuée sur le système d’exploitation de l’administrateur.

Pour ce faire, suivez les instructions de ce guide pour l’hôte PAW, ajouter des fonctionnalités Client Hyper-V, créer une machine virtuelle utilisateur, puis installez une image d’entreprise Windows10 sur la machine virtuelle utilisateur.

Lecture [Client Hyper-V](https://technet.microsoft.com/en-us/library/hh857623.aspx) article pour plus d’informations sur cette fonctionnalité. Veuillez noter que le système d’exploitation dans les machines virtuelles invitées devez disposer d’une licence par [licence produit Microsoft](https://www.microsoft.com/en-us/Licensing/product-licensing/products.aspx), décrit également [ici](https://www.microsoft.com/en-us/Licensing/learn-more/brief-windows-virtual-machine.aspx).

**Utilisation simultanée - Ajout de RemoteApp, de RDP ou une infrastructure VDI**

Dans ce scénario d’utilisation simultanée, un PC unique est utilisé pour les tâches d’administration et utiliser des activités quotidiennes telles que la messagerie, de modification de documents et de développement. Dans cette configuration, les systèmes d’exploitation utilisateur sont déployés et gérés de manière centralisée (sur le cloud ou dans votre centre de données), mais ne sont pas disponibles hors connexion.

![Figure montrant un PC unique dans un scénario d’utilisation simultanée utilisé pour les tâches d’administration et le travail des activités quotidiennes telles que la messagerie, de modification de documents et de développement](../media/privileged-access-workstations/PAWFig11.JPG)

Le matériel physique exécute un seul système d’exploitation PAW localement pour les tâches administratives et contacte Microsoft ou 3eservice Bureau à distance tiers pour les applications utilisateur telles que courrier électronique, modification de documents et des applications métier.

Dans cette configuration, les tâches quotidiennes qui ne nécessitent pas de privilèges d’administrateur sont effectuent dans le OS(es) à distance et les applications qui ne sont pas soumises aux restrictions appliquées à l’hôte PAW. Toutes les tâches administratives sont effectuée sur le système d’exploitation de l’administrateur.

Pour ce faire, suivez les instructions de ce guide pour l’hôte PAW, autorisez la connectivité réseau aux services Bureau à distance et puis ajouter des raccourcis au bureau de l’utilisateur PAW pour accéder aux applications. Les services Bureau à distance peuvent être hébergés de nombreuses façons, dont:

-   Un service Bureau à distance ou VDI existant (local ou dans le cloud)

-   Un nouveau service que vous installez en local ou dans le cloud

-   Azure RemoteApp à l’aide des modèles Office 365préconfigurés ou vos propres images d’installation

Pour plus d’informations sur Azure RemoteApp, visitez [cette page](https://www.remoteapp.windowsazure.com).

### <a name="paw-scenarios"></a>Scénarios PAW
Cette section contient des conseils sur les scénarios auxquels ce guide de PAW doit être appliqué. Dans tous les scénarios, les administrateurs doivent être formés à utiliser uniquement les Paw pour effectuer le support des systèmes distants. Pour encourager l’utilisation réussie et sécurisée, tous les utilisateurs PAW doit être également être encouragés à fournir des commentaires pour améliorer l’expérience PAW et ces commentaires doivent être examinés attentivement pour l’intégration avec votre programme PAW.

Dans tous les scénarios, les renforcement supplémentaires lors des phases ultérieures et différents profils matériels dans ce guide peuvent être utilisés pour répondre aux exigences de sécurité ou de facilité d’utilisation des rôles.

> [!NOTE]
> Notez que ce guide différencie explicitement l’accès à des services spécifiques sur internet (par exemple, des portails d’administration Azure et Office 365) et «Ouvrir Internet» de tous les ordinateurs hôtes et services.

Consultez le [page modèle de couche](http://aka.ms/tiermodel) pour plus d’informations sur les désignations.

|**Scénarios**|**Utiliser PAW?**|**Considérations de sécurité et d’étendue**|
|---------|--------|---------------------|
|Administrateurs d’ActiveDirectory - niveau 0|Oui|Un PAW créé avec le Guide de Phase 1 est suffisant pour ce rôle.<br /><br />-Une forêt d’administration peut être ajoutée pour fournir le niveau de protection pour ce scénario. Pour plus d’informations sur la forêt d’administration ESAE, consultez [approche de conception de forêt d’administration ESAE](http://aka.ms/esae)<br />-Un PAW peut servir à gérer plusieurs domaines ou plusieurs forêts.<br />-Si les contrôleurs de domaine sont hébergés sur une Infrastructure en tant que Service (IaaS) ou solution de virtualisation locale, vous devez définir des priorités l’implémentation des Paw pour les administrateurs de ces solutions|
|Services Admin d’Azure IaaS et PaaS - niveau 0 ou 1 (voir Considérations de conception et d’étendue)|Oui|Un PAW créé à l’aide de l’aide fournie dans la Phase 2 est suffisant pour ce rôle.<br /><br />-Les Paw doivent être utilisés au moins l’administrateur Global et l’administrateur de facturation des abonnements. Vous devez également utiliser les Paw pour les administrateurs délégués des serveurs critiques ou sensibles.<br />-Les Paw doivent être utilisés pour gérer le système d’exploitation et applications qui fournissent la synchronisation d’annuaires et de la fédération des identités pour les services cloud comme [Azure AD Connect](https://azure.microsoft.com/en-us/documentation/articles/active-directory-aadconnect/) et ActiveDirectory Federation Services (ADFS).<br />-Les restrictions du réseau sortant doivent autoriser la connectivité uniquement aux services cloud autorisés à l’aide du guide dans la Phase 2. Aucun accès internet ouvert ne doivent être autorisées à partir des Paw.<br />-EMET doit être configuré pour tous les navigateurs utilisés sur la station de travail **Remarque:** un abonnement est considéré comme niveau 0 pour une forêt si les contrôleurs de domaine ou d’autres hôtes de niveau 0 sont présents dans l’abonnement. Un abonnement est de niveau 1 si aucun serveur de niveau 0 n’est hébergés dans Azure|
|Locataire administrateur d’Office 365 <br />-Niveau 1|Oui|Un PAW créé à l’aide de l’aide fournie dans la Phase 2 est suffisant pour ce rôle.<br /><br />-Des Paw doivent être utilisés au moins pour l’administrateur de facturation des abonnements, administrateur général, administrateur Exchange, administrateur SharePoint et rôles d’administrateur utilisateur gestion. Vous devez également fortement envisager l’utilisation de Paw pour les administrateurs délégués de données très sensibles ou critiques.<br />-EMET doit être configuré pour tous les navigateurs utilisés sur la station de travail<br />-Les restrictions du réseau sortant doivent autoriser la connectivité uniquement aux services Microsoft à l’aide du guide dans la Phase 2. Aucun accès internet ouvert ne doivent être autorisées à partir des Paw.|
|Autres IaaS ou PaaS de cloud administrateur de service<br />-Niveau 0 ou 1 (voir Considérations de conception et d’étendue)||Un PAW créé à l’aide de l’aide fournie dans la Phase 2 est suffisant pour ce rôle.<br /><br />-Les Paw doivent être utilisés pour n’importe quel rôle qui possède des droits administratifs sur les machines virtuelles hébergé dans le cloud, y compris la capacité à installer des agents, l’exportation des fichiers de disque dur ou accéder au stockage où sont stockées les disques durs dotés de systèmes d’exploitation, des données sensibles ou des données d’entreprise critiques.<br />-Les restrictions du réseau sortant doivent autoriser la connectivité uniquement aux services Microsoft à l’aide du guide dans la Phase 2. Aucun accès internet ouvert ne doivent être autorisées à partir des Paw.<br />-EMET doit être configuré pour tous les navigateurs utilisés sur la station de travail. **Remarque:** un abonnement est de niveau 0 pour une forêt si les contrôleurs de domaine ou d’autres hôtes de niveau 0 sont présents dans l’abonnement. Un abonnement est de niveau 1 si aucun serveur de niveau 0 n’est hébergés dans Azure.|
|Administrateurs de la virtualisation<br />-Niveau 0 ou 1 (voir Considérations de conception et d’étendue)|Oui|Un PAW créé à l’aide de l’aide fournie dans la Phase 2 est suffisant pour ce rôle.<br /><br />-Les Paw doivent être utilisés pour n’importe quel rôle qui possède des droits administratifs sur les machines virtuelles, y compris la capacité à installer des agents, l’exportation des fichiers de disque dur virtuel ou accéder au stockage où sont stockés des disques durs avec les informations du système d’exploitation invité, des données sensibles ou des données d’entreprise critiques. **Remarque:** un système de virtualisation (et ses administrateurs) sont considérés de niveau 0 pour une forêt si les contrôleurs de domaine ou d’autres hôtes de niveau 0 sont présents dans l’abonnement. Un abonnement est de niveau 1 si aucun serveur de niveau 0 n’est hébergés dans le système de virtualisation.|
|Administrateurs de Maintenance de serveur<br />-Niveau 1|Oui|Un PAW créé à l’aide de l’aide fournie dans la Phase 2 est suffisant pour ce rôle.<br /><br />-Un PAW doit être utilisé pour les administrateurs qui mettre à jour, des correctifs et dépanner les serveurs d’entreprise et les applications qui s’exécutent Windows server, Linux et autres systèmes d’exploitation.<br />-Outils de gestion dédiés peuvent avoir à être ajoutés pour les Paw gérer la plus grande échelle de ces administrateurs.|
|Administrateurs de station de travail utilisateur <br />-Niveau 2|Oui|Un PAW créé à l’aide des instructions fournies dans la Phase 2 est suffisant pour les rôles qui ont des droits d’administration sur les périphériques de l’utilisateur final (telles que le support technique rôles).<br /><br />-D’autres applications peuvent doivent être installées sur les Paw pour permettre la gestion des tickets et autres fonctions de prise en charge.<br />-EMET doit être configuré pour tous les navigateurs utilisés sur la station de travail.<br />    Outils de gestion dédiés peuvent avoir à être ajoutés pour les Paw gérer la plus grande échelle de ces administrateurs.|
|SQL, SharePoint, ou cœur de métier (LOB) Admin<br />-Niveau 1||Un PAW créé avec le Guide de Phase 2 est suffisant pour ce rôle.<br /><br />-Outils de gestion supplémentaires peuvent avoir à être installé sur les Paw pour permettre aux administrateurs de gérer les applications sans avoir à se connecter aux serveurs à l’aide des services Bureau à distance.|
|Utilisateurs gérant la présence de médias sociaux|Partiellement|Un PAW créé à l’aide fournie dans la Phase 2 peut être utilisé comme point de départ pour assurer la sécurité de ces rôles.<br /><br />-Protégez et gérez les comptes de médias sociaux à l’aide d’Azure ActiveDirectory (AAD) pour le partage, la protection et le suivi des accès aux comptes de médias sociaux.<br />    Pour plus d’informations sur cette fonctionnalité lire [ce billet de blog](http://blogs.technet.com/b/ad/archive/2015/02/20/azure-ad-automated-password-roll-over-for-facebook-twitter-and-linkedin-now-in-preview.aspx).<br />-Les restrictions du réseau sortant doivent autoriser la connexion à ces services. Cela est possible en autorisant les connexions internet ouvert (beaucoup plus important risque de sécurité qui annule de nombreuses assurances des PAW) ou en autorisant uniquement des adresses DNS requises pour le service (peut être difficile à obtenir).|
|Utilisateurs standard|N°|Pendant la procédure de renforcement peut être utilisée pour les utilisateurs standard, PAW est conçu pour les comptes à partir de l’accès internet ouverts que la plupart des utilisateurs ont besoin pour accomplir leur travail.|
|Borne/VDI invitée|N°|Pendant la procédure de renforcement peut être utilisée pour un système de borne pour les invités, l’architecture PAW est conçu pour fournir une sécurité accrue pour les comptes à sensibilité élevée, pas davantage de sécurité pour les comptes à sensibilité moindre.|
|Utilisateur VIP (exécutif, chercheur, etc.).|Partiellement|Un PAW créé à l’aide des instructions fournies dans la Phase 2 peut être utilisé comme point de départ pour assurer la sécurité pour ces rôles<br /><br />-Ce scénario est similaire à un bureau d’utilisateur standard, mais a généralement un profil d’application plus petit, plus simple et bien connu. Ce scénario nécessite généralement la découverte et la protection des données sensibles, services et applications (ce qui peuvent ou ne peuvent pas être installées sur les postes de travail).<br />-Ces rôles requièrent généralement un degré élevé de sécurité et de très haut degré de facilité d’utilisation, qui nécessitent des modifications de conception pour répondre aux préférences de l’utilisateur.|
|Systèmes de contrôle industriel (SCADA, PCN et les contrôleurs de domaine)|Partiellement|Un PAW créé à l’aide des instructions fournies dans la Phase 2 peut être utilisé comme point de départ pour assurer la sécurité pour ces rôles ICS car la plupart des consoles (y compris des normes communes comme SCADA et PCN) ne nécessitent pas de navigation sur l’Internet public et la vérification de la messagerie.<br /><br />-Les applications utilisées pour contrôler les machines physiques devra être intégrées et testées pour assurer la compatibilité et protégées de manière appropriée|
|Système d’exploitation incorporé|N°|Pendant la procédure de renforcement de PAW peut être utilisé pour les systèmes d’exploitation intégrés, une solution personnalisée doit être développé pour la sécurisation renforcée dans ce scénario.|

> [!NOTE]
> **Scénarios de combinaison** certains personnel peut-être avoir des responsabilités administratives qui couvrent plusieurs scénarios.
> Dans ces cas, les règles de clés à prendre en considération sont que les règles de modèle de niveau doivent être suivies à tout moment. Consultez la page modèle de niveau pour plus d’informations.

> [!NOTE]
> **Mise à l’échelle du programme PAW** comme votre programme évolue pour englober plusieurs administrateurs et les rôles, vous devez continuer à garantir la conformité aux normes de sécurité et facilité d’utilisation. Cela peut nécessiter de mettre à jour votre service informatique prennent en charge les structures ou créer de nouveaux pour résoudre les défis spécifiques de PAW tels que des processus d’intégration PAW, gestion des incidents, gestion de la configuration et les défis de collecte de commentaires à la facilité d’utilisation de l’adresse.  Un exemple peut être que votre organisation décide d’activer des scénarios de travail à domicile pour les administrateurs, ce qui nécessite une transition à partir du bureau Paw sur ordinateur portable, un changement qui peut impliquer des considérations de sécurité supplémentaires.  Un autre exemple courant consiste à créer ou mettre à jour des formations pour les nouveaux administrateurs, formations qui doivent désormais inclure le contenu sur l’utilisation appropriée des PAW (y compris leur importance et qu’un PAW et n’est pas).  Pour plus de considérations à garder à l’esprit lors de l’évolution de votre programme PAW, consultez la Phase 2des instructions.

Ce guide contient des instructions détaillées pour la configuration de PAW pour les scénarios, comme indiqué plus haut. Si vous avez des exigences pour d’autres scénarios, vous pouvez adapter les instructions de fonction de ce guide vous-même ou passer par une entreprise de services professionnels comme Microsoft pour vous aider.

Pour plus d’informations sur le recours aux services Microsoft pour concevoir un PAW adapté à votre environnement, contactez votre représentant Microsoft ou visitez [cette page](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx).

### <a name="paw-installation-instructions"></a>Instructions d’Installation de PAW
Étant donné que le PAW doit fournir une source sûre et fiable pour l’administration, il est essentiel que le processus de création soit sécurisé et fiable.  Cette section fournit des instructions détaillées qui vous permet de créer votre propre PAW à l’aide de principes et concepts très similaires à celles utilisées par MicrosoftIT et Microsoft et d’ingénierie organisations de gestion du service.

Les instructions sont divisées en trois phases axées sur la mise des atténuations les plus critiques rapidement en place progressivement augmentation et extension de l’utilisation de PAW pour l’entreprise.

-   Phase 1 - déploiement immédiat pour les administrateurs d’ActiveDirectory

-   Phase 2 - étendre les PAW à tous les administrateurs

-   Phase 3 - sécurité avancée avec PAW

Il est important de noter que les phases doivent toujours être effectuées dans l’ordre même si elles sont planifiées et implémentées dans le cadre du même projet global.

#### <a name="phase-1---immediate-deployment-for-active-directory-administrators"></a>Phase 1 - déploiement immédiat pour les administrateurs d’ActiveDirectory
Objectif: Fournit rapidement un PAW qui peut protéger les rôles d’administration domaine et de la forêt locale.

Portée: Les administrateurs de niveau 0, y compris les administrateurs de l’entreprise, Admins du domaine (pour tous les domaines) et les administrateurs d’autres systèmes d’identité faisant autorité.

Phase 1 se concentre sur les administrateurs qui gèrent votre domaine ActiveDirectory local, les rôles critiques fréquemment ciblés par des personnes malveillantes. Ces systèmes d’identité fonctionneront efficacement pour protéger ces administrateurs, si vos contrôleurs de domaine ActiveDirectory (contrôleurs de domaine) sont hébergés dans des centres de données locale, sur l’Infrastructure Azure en tant que Service (IaaS) ou un autre fournisseur IaaS.

Pendant cette phase, vous allez créer la structure d’unité d’organisation (UO) ActiveDirectory administrative sécurisée pour héberger votre station de travail de l’accès privilégié (PAW), ainsi que de déployer les Paw eux-mêmes.  Cette structure comprend également les stratégies de groupe et les groupes requis pour prendre en charge le PAW.  Vous allez créer la majeure partie de la structure à l’aide de scripts PowerShell qui sont disponibles sur [Galerie TechNet](http://aka.ms/pawmedia).

Les scripts créeront les unités d’organisation et les groupes de sécurité suivants:

-   Unités d’organisation (UO)

    -   Six nouvelles UO de niveau supérieur: administrateur; groupes; serveurs de niveau 1; stations de travail. Comptes d’utilisateur; et mise en quarantaine de l’ordinateur.  Chaque unité d’organisation de niveau supérieur contient un nombre d’unités d’organisation enfants.

-   Groupes

    -   Six nouveaux sécurisée des groupes globaux: Maintenance de réplication de niveau 0; maintenance du serveur de niveau 1; opérateurs de Service Desk; maintenance de la station de travail; utilisateurs PAW. Maintenance PAW.

Vous allez également créer un nombre d’objets de stratégie de groupe: Configuration PAW - ordinateur; configuration PAW - utilisateur; restrictedAdmin requis - ordinateur; restrictions sortantes de PAW; limiter la station de travail d’ouverture de session; limiter l’ouverture de session de serveur.

Phase 1 inclut les étapes suivantes:

1.  Remplir les conditions préalables

    1.  **Assurez-vous que tous les administrateurs utilisent des comptes distincts et individuels pour les activités d’administration et l’utilisateur final** (y compris le courrier électronique, navigation sur Internet, les applications métier d’et autres activités non administratives).  Affectation d’un compte d’administration pour chaque distinct personnel autorisé à partir de leur compte d’utilisateur standard est essentielle pour le modèle PAW, car seuls certains comptes seront autorisés à se connecter au PAW lui-même.

        > [!NOTE]
        > Chaque administrateur doit utiliser son propre compte pour l’administration.  Ne partagez pas un compte d’administration.

    2.  **Réduire le nombre d’administrateurs privilégié de niveau 0**.  Étant donné que chaque administrateur doit utiliser un PAW, ce qui réduit le nombre d’administrateurs réduit le nombre de Paw requis ainsi que les coûts associés. Le nombre moindre d’administrateurs entraîne également une exposition réduite à ces privilèges et les risques associés. S’il est possible pour les administrateurs dans un emplacement de partager un PAW, les administrateurs dans des emplacements physiques séparés besoin de Paw distincts.

    3.  **Acquérez du matériel à partir d’un fournisseur de confiance qui répond à toutes les exigences techniques**. Microsoft recommande l’acquisition du matériel qui répond aux exigences techniques de l’article [protéger les informations d’identification de domaine avec Credential Guard](https://technet.microsoft.com/en-us/library/mt483740%28v=vs.85%29.aspx).

        > [!NOTE]
        > Les PAW installés sur le matériel sans ces fonctionnalités peuvent fournir une protection significative, mais les fonctionnalités de sécurité avancées telles que Credential Guard et Device Guard ne sera pas disponibles.  Credential Guard et Device Guard ne sont pas requis pour le déploiement de la Phase 1, mais sont vivement recommandées dans le cadre de la Phase 3 (renforcement de la sécurité avancée).

        Assurez-vous que le matériel utilisé pour le PAW provient d’un fabricant et un fournisseur dont pratiques de sécurité sont approuvées par l’organisation. Il s’agit d’une application du principe de source propre pour fournir la sécurité de la chaîne.

        > [!NOTE]
        > Pour plus d’informations sur l’importance de la sécurité de la chaîne logistique, visitez [ce site](https://www.microsoft.com/security/cybersecurity/).

    4.  Acquérez et validez les logiciels d’application requis Windows10 Édition entreprise. Obtenir le logiciel requis pour le PAW et validez-les utilisant les instructions de [Source propre pour support d’installation](http://aka.ms/cleansource).

        -   Windows10 Édition entreprise

        -   [Outils d’Administration de serveur distant](https://www.microsoft.com/en-us/download/details.aspx?id=45520.) pour Windows10

        -   Windows10 [lignes de base de sécurité](http://aka.ms/win10baselines)

        -   [Atténuation améliorée expérience Toolkit (EMET)](https://www.microsoft.com/en-us/download/details.aspx?id=49166)

            > [!NOTE]
            > Microsoft publie les hachages MD5 pour tous les systèmes d’exploitation et applications sur MSDN, mais pas tous les éditeurs de logiciels fournissent une documentation similaire.  Dans ce cas, les autres stratégies seront nécessaires.  Pour plus d’informations sur la validation du logiciel, reportez-vous à [Source propre](http://aka.ms/cleansource) pour support d’installation.

    5.  **Assurez-vous que vous avez un serveur WSUS est disponible sur l’intranet**. Vous devez un serveur WSUS sur l’intranet pour télécharger et installer les mises à jour pour PAW. Ce serveur WSUS doit être configuré pour approuver automatiquement toutes les mises à jour de sécurité pour Windows10 ou un personnel administratif doit avoir de la responsabilité d’approuver rapidement les mises à jour logicielles.

        > [!NOTE]
        > Pour plus d’informations, voir la section «Approuver automatiquement les mises à jour pour Installation» dans le [des conseils d’approbation des mises à jour](https://technet.microsoft.com/en-us/library/cc708458(v=ws.10).aspx).

2.  Déployer l’infrastructure d’unité d’organisation Admin pour héberger les Paw

    1.  Télécharger la bibliothèque de scripts PAW de [Galerie TechNet](http://aka.ms/PAWmedia)

        > [!NOTE]
        > Télécharger tous les fichiers et les enregistrer dans le même répertoire et les exécuter dans l’ordre indiqué ci-dessous.  Create-PAWGroups dépend de la structure d’unité d’organisation créée par Create-PAWOUs et Set-PAWOUDelegation repose sur les groupes créés par Create-PAWGroups.
        > Ne modifiez pas les scripts ou le fichier de valeurs séparées par des virgules (CSV).

    2.  **Exécutez le script .ps1 Create-PAWOUs**.  Ce script crée la nouvelle structure d’unité d’organisation (UO) dans ActiveDirectory et bloquer l’héritage de stratégie de groupe sur les nouvelles unités d’organisation appropriées.

    3.  **Exécutez le script .ps1 Create-PAWGroups**.  Ce script créera de nouveaux groupes de sécurité globaux dans les unités d’organisation appropriées.

        > [!NOTE]
        > Alors que ce script crée les groupes de sécurité, il sera les remplit pas.

    4.  **Exécutez le script .ps1 Set-PAWOUDelegation**.  Ce script affecte les autorisations pour les nouvelles unités d’organisation aux groupes appropriés.

3.  **Déplacez les comptes de niveau 0vers l’UO Admin\Tier 0\Accounts**. Déplacez chaque compte qui est membre de l’administrateur de domaine, administrateurs de l’entreprise, ou de niveau 0groupes équivalents (y compris l’appartenance imbriquée) à cette unité d’organisation. Si votre organisation possède vos propres groupes ajoutés à ces groupes, vous devez les déplacer vers l’UO Admin\Tier 0\Groups.

    > [!NOTE]
    > Pour plus d’informations sur les groupes niveau 0, consultez «Équivalence de niveau 0» dans [sécurisation de l’accès référence privilégié](../securing-privileged-access/securing-privileged-access-reference-material.md).

4.  Ajoutez les membres appropriés aux groupes pertinents

    1.  **Les utilisateurs PAW** - ajoutez les administrateurs de niveau 0 avec le domaine ou d’administrateur d’entreprise identifié à l’étape1 de la Phase 1.

    2.  **Maintenance PAW** - ajouter au moins un compte qui sera utilisé pour la maintenance PAW et dépannage des tâches. Les comptes de Maintenance de PAW seront utilisés rarement.

        > [!NOTE]
        > N’ajoutez pas le même compte d’utilisateur ou le groupe utilisateurs de PAW et Maintenance de PAW.  Le modèle de sécurité PAW repose en partie sur l’hypothèse que le compte d’utilisateur PAW a des droits privilégiés sur les systèmes gérés ou sur le PAW lui-même, mais pas les deux.
        >
        > -   Cela est important pour la création des bonnes pratiques administratives et habitudes en Phase 1.
        > -   Cela est extrêmement important pour la Phase 2 et au-delà, afin d’éviter l’élévation des privilèges via le PAW, car les Paw servent à couvrir les niveaux.
        >
        > Dans l’idéal, aucun membre du personnel n’est affecté à plusieurs niveaux pour appliquer le principe de répartition des responsabilités, mais Microsoft reconnaît que de nombreuses organisations est limitée personnel (ou autres exigences d’organisation) qui ne permet pas d’atteindre cette séparation complète. Dans ces cas, le même personnel peut être affecté à ces deux rôles, mais n’utilisez pas le même compte pour ces fonctions.

5.  **Créer l’objet stratégie de groupe (GPO) «Configuration de PAW – ordinateur» et le lien vers l’unité d’organisation appareils de niveau 0** («appareils» sous Tier 0\Admin).  Dans cette section, vous allez créer un nouveau «PAW Configuration – ordinateur» GPO qui fournit des protections spécifiques pour ces Paw.

    > [!NOTE]
    > **N’ajoutez pas ces paramètres à la stratégie de domaine par défaut**.  Cela affecterait potentiellement les opérations sur l’ensemble de votre environnement ActiveDirectory.  Uniquement configurer ces paramètres dans la stratégie de groupe nouvellement créé décrit ici et les appliquer uniquement à l’unité d’organisation du PAW.

    1.  **Accès de Maintenance PAW** - ce paramètre va définir l’appartenance de groupes privilégiés spécifiques des Paw sur un ensemble spécifique d’utilisateurs. Accédez à *Configuration\Preferences\Control du Panneau de configuration utilisateurs sécurité\Stratégies* et les groupes et suivez les étapes ci-dessous:

        1.  Cliquez sur **New** et cliquez sur **groupe Local**

        2.  Sélectionnez le **mise à jour** action et sélectionnez «administrateurs (intégré)» (n’utilisez pas le bouton Parcourir pour sélectionner le groupe de domaine Administrateurs).

        3.  Sélectionnez le **supprimer tous les utilisateurs membres** et **supprimer tous les groupes de membres** cases à cocher

        4.  Ajoutez Maintenance de PAW (pawmaint) et administrateur (là encore, n’utilisez pas le bouton Parcourir pour sélectionner administrateur).

            > [!NOTE]
            > N’ajoutez pas le groupe d’utilisateurs PAW à la liste des membres du groupe Administrateurs local.  Pour vous assurer que les utilisateurs PAW ne peuvent pas accidentellement ou intentionnellement modifier les paramètres de sécurité du PAW lui-même, ils ne doivent pas être membres du groupe Administrateurs local.

            Pour plus d’informations sur l’utilisation de préférences de stratégie de groupe pour modifier l’appartenance au groupe, reportez-vous à l’article TechNet [configurer un élément groupe Local](https://technet.microsoft.com/en-us/library/cc732525.aspx).

    2.  **Limiter l’appartenance au groupe Local** -ce paramètre garantit que l’appartenance aux groupes d’administrateurs locaux sur la station de travail est toujours vide

        1.  Accédez à l’ordinateur Configuration\Preferences\Control du Panneau de configuration sécurité\Stratégies utilisateurs et groupes et suivez les étapes ci-dessous:

            1.  Cliquez sur **New** et cliquez sur **groupe Local**

            2.  Sélectionnez le **mise à jour** action et sélectionnez «opérateurs de sauvegarde (intégré)» (n’utilisez pas le bouton Parcourir pour sélectionner le groupe de domaine opérateurs de sauvegarde).

            3.  Sélectionnez le **supprimer tous les utilisateurs membres** et **supprimer tous les groupes de membres** cases à cocher.

            4.  N’ajoutez pas de membres au groupe.  Cliquez simplement sur **OK**.  En affectant une liste vide, stratégie de groupe sera automatiquement supprimer tous les membres et garantir une liste des membres vide que chaque stratégie de groupe est actualisée.

        2.  Suivez les étapes ci-dessus pour les groupes supplémentaires suivants:

            -   Opérateurs de chiffrement

            -   Administrateurs Hyper-V

            -   Opérateurs de Configuration réseau

            -   Utilisateurs avec pouvoir

            -   Utilisateurs du Bureau à distance

            -   Duplicateurs

        3.  Restrictions de connexion PAW - ce paramètre limite les comptes qui peuvent se connecter au PAW. Suivez les étapes ci-dessous pour configurer ce paramètre:

            1.  Accédez à ordinateur Configuration ordinateur\Stratégies\Paramètres Windows\Paramètres sécurité\Stratégies locales\affectation des droits utilisateur\autoriser une session localement.

            2.  Sélectionnez définir ces paramètres de stratégie, puis ajoutez «Utilisateurs PAW» et les administrateurs (là encore, n’utilisez pas le bouton Parcourir pour sélectionner les administrateurs).

        4.  **Bloquer le trafic réseau entrant** -ce paramètre garantit qu’aucun trafic réseau entrant non sollicité n’est autorisé au PAW. Suivez les étapes ci-dessous pour configurer ce paramètre:

            1.  Accédez à Configuration ordinateur\Stratégies\Paramètres Windows\Paramètres de Sécurité\pare-feu Windows avec Advanced sécurité\Pare-feu Windows avec fonctions avancées de sécurité et suivez les étapes ci-dessous:

                1.  Cliquez avec le bouton droit sur le pare-feu Windows avec fonctions avancées de sécurité et sélectionnez **importer la stratégie**.

                2.  Cliquez sur **Oui** pour accepter que cette opération remplace les stratégies de pare-feu existantes.

                3.  Accédez à PAWFirewall.wfw et sélectionnez **ouvrir**.

                4.  Cliquez sur **OK**.

                    > [!NOTE]
                    > Vous pouvez ajouter des adresses ou sous-réseaux qui doivent atteindre le PAW avec le trafic non sollicité à ce stade (par exemple, logiciel gestion ou d’analyse de sécurité.
                    > Les paramètres dans le fichier WFW seront activer le pare-feu en mode «Bloc – par défaut» pour tous les profils de pare-feu, désactiver la fonctionnalité de fusion de règle et activer la journalisation des paquets ignorés et réussies. Ces paramètres bloquent le trafic remplaceraient tout en autorisant la communication bidirectionnelle sur les connexions établies à partir du PAW, empêcher les utilisateurs disposant d’un accès à partir de la création de règles de pare-feu locales qui remplacent les paramètres de stratégie de groupe et vous assurer que le trafic entrant et sortant dans le PAW est enregistré.
                    > **Ouvrir ce pare-feu pour développer la surface d’attaque pour le PAW et augmente le risque de sécurité. Avant d’ajouter des adresses, consultez la section Gestion et exploitation de PAW dans ce guide**.

        5.  **Configurer Windows Update pour WSUS** -suivez les étapes ci-dessous pour modifier les paramètres pour configurer Windows Update pour les Paw:

            1.  Accédez à Configuration ordinateur\Stratégies\Modèles d’administration\Composants Windows\Windows mises à jour et suivez les étapes ci-dessous:

                1.  Activer la **stratégie configurer les mises à jour automatiques**.

                2.  Sélectionnez l’option **4 - téléchargement automatique et planification des installations**.

                3.  Modifiez l’option **jour de l’installation planifiée** à **0 - tous les jours** et l’option **heure d’installation planifiée** à votre préférence de l’organisation.

                4.  Activer l’option **spécifier l’emplacement intranet du service de mise à jour Microsoft** stratégie, et spécifiez dans les deux options l’URL du serveur WSUS d’ESAE.

        6.  Lier le «PAW Configuration – ordinateur» objet stratégie de groupe comme suit:

            |Stratégie|Emplacement du lien|
            |-----|---------|
            |Configuration PAW - ordinateur |Admin\Tier 0\Devices|

6.  **Créer l’objet stratégie de groupe (GPO) «Configuration de PAW – utilisateur» et le lien vers l’unité d’organisation de comptes de niveau 0 («comptes» sous Tier 0\Admin)**.  Dans cette section, vous allez créer un nouveau «PAW Configuration – utilisateur» GPO qui fournit des protections spécifiques pour ces Paw.

    > [!NOTE]
    > N’ajoutez pas ces paramètres à la stratégie de domaine par défaut

    1.  **Bloquer la navigation internet** - pour empêcher la navigation sur internet, cette option définit une adresse de proxy d’une adresse de bouclage (127.0.0.1).

        1.  Allez dans configuration\preferences\paramètres windows\registre. Avec le bouton droit de Registre, sélectionnez **New** \> **élément de Registre** et configurez les paramètres suivants:

            1.  Action: remplacer

            2.  La ruche: HKEY_CURRENT_USER

            3.  Chemin de la clé: Software\Microsoft\Windows\CurrentVersion\Internet Settings

            4.  Nom de la valeur: ProxyEnable

                > [!NOTE]
                > Ne sélectionnez pas la zone à gauche du nom de la valeur par défaut.

            5.  Type de valeur: REG_DWORD

            6.  Données de la valeur: 1

                1.  un.  Cliquez sur l’onglet commun et sélectionnez **supprimer cet élément lorsqu’il n’est plus appliqué**.

                2.  Dans l’onglet commun, sélectionnez **ciblage au niveau élément** et cliquez sur **ciblage**.

                3.  Cliquez sur **nouvel élément** et sélectionnez **groupe de sécurité**.

                4.  Sélectionnez le bouton «...» et recherchez le groupe d’utilisateurs PAW.

                5.  Cliquez sur **nouvel élément** et sélectionnez **groupe de sécurité**.

                6.  Sélectionnez le bouton «...» et recherchez le **administrateurs des Services Cloud** groupe.

                7.  Cliquez sur le **administrateurs des Services Cloud** de l’élément, puis cliquez sur **Options de l’élément**.

                8.  Sélectionnez **n’est pas**.

                9. Cliquez sur **OK** sur la fenêtre de ciblage.

            7.  Cliquez sur **OK** pour terminer la stratégie de groupe ProxyServer

        2.  Allez dans configuration\preferences\paramètres windows\registre. Avec le bouton droit de Registre, sélectionnez **New** \> **élément de Registre** et configurez les paramètres suivants:

            -   Action: remplacer

            -   La ruche: HKEY_CURRENT_USER

            -   Chemin de la clé: Software\Microsoft\Windows\CurrentVersion\Internet Settings

            -   Nom de la valeur: proxyserveur

                > [!NOTE]
                > Ne sélectionnez pas la zone à gauche du nom de la valeur par défaut.

            -   Type de valeur: REG_SZ

            -   Données de la valeur: 127.0.0.1:80

                1.  Cliquez sur le **commune** onglet et sélectionnez **supprimer cet élément lorsqu’il n’est plus appliqué**.

                2.  Sur le **commune** onglet sélectionnez **ciblage au niveau élément** et cliquez sur **ciblage**.

                3.  Cliquez sur **nouvel élément** et le groupe de sécurité sélectionnez.

                4.  Sélectionnez le bouton «...» et ajoutez le groupe d’utilisateurs de PAW.

                5.  Cliquez sur **nouvel élément** et le groupe de sécurité sélectionnez.

                6.  Sélectionnez le bouton «...» et recherchez le **administrateurs des Services Cloud** groupe.

                7.  Cliquez sur le **administrateurs des Services Cloud** de l’élément, puis cliquez sur **Options de l’élément**.

                8.  Sélectionnez **n’est pas**.

                9. Cliquez sur **OK** sur la fenêtre de ciblage.

        3.  Cliquez sur **OK** pour terminer le paramètre de stratégie de groupe ProxyServer

    2.  Accédez à l’utilisateur Configuration ordinateur\Stratégies\Modèles d’administration\Composants Windows\Internet Explorer, puis activez les options ci-dessous. Ces paramètres empêchent les administrateurs de remplacer manuellement les paramètres de proxy.

        1.  Activer la **désactiver la modification de la Configuration automatique** paramètres.

        2.  Activer la **empêcher la modification des paramètres de proxy**.

7.  **Empêcher les administrateurs de la connexion à des hôtes de niveau inférieur**.  Dans cette section, nous allons configurer des stratégies de groupe pour empêcher les comptes administratifs privilégiés de journalisation sur les hôtes de niveau inférieur.

    1.  Créez le nouvel **restriction de connexion à une station de travail** GPO - ce paramètre empêchera de niveau 0 et comptes d’administrateur de niveau 1 se connecter à des stations de travail standard.  Cet objet de stratégie de groupe doit être lié à l’unité d’organisation de niveau supérieur de «Stations de travail» et ont les paramètres suivants:

        - (i) Dans l’ordinateur Configuration ordinateur\Stratégies\Paramètres Windows\Paramètres sécurité\Stratégies locales\affectation des droits utilisateur\refuser connexion en tant que traitement par lots, sélectionnez **définir ces paramètres de stratégie** et ajoutez les groupes de niveau 1 et le niveau 0:

            **Groupes à ajouter aux paramètres de stratégie:**

            Administrateurs de l’entreprise

            Admins du domaine

            Administrateurs du schéma

            DOMAINE\Administrateurs

            Opérateurs de compte

            Opérateurs de sauvegarde

            Opérateurs d’impression

            Opérateurs de serveur

            Contrôleurs de domaine

            Read-Only Contrôleurs de domaine

            Groupe Propriétaires créateurs de stratégie

            Opérateurs de chiffrement

            > [!NOTE]
            > Remarque: Groupes de niveau 0intégrés, consultez l’équivalence de niveau 0 pour plus d’informations.

            Autres groupes délégués

            > [!NOTE]
            > Remarque: N’importe quel groupe personnalisé créé avec l’accès de niveau 0 effectif, consultez l’équivalence de niveau 0 pour plus d’informations.

            Administrateurs de niveau 1

            > [!NOTE]
            > Remarque: Ce groupe a été créé précédemment dans la Phase 1

        - (ii) Dans l’ordinateur Configuration ordinateur\Stratégies\Paramètres Windows\Paramètres sécurité\Stratégies locales\Attribution des droits utilisateur\refuser connexion en tant que service, sélectionnez **définir ces paramètres de stratégie** et ajoutez les groupes de niveau 1 et le niveau 0:

            **Groupes à ajouter aux paramètres de stratégie:**

            Administrateurs de l’entreprise

            Admins du domaine

            Administrateurs du schéma

            DOMAINE\Administrateurs

            Opérateurs de compte

            Opérateurs de sauvegarde

            Opérateurs d’impression

            Opérateurs de serveur

            Contrôleurs de domaine

            Read-Only Contrôleurs de domaine

            Groupe Propriétaires créateurs de stratégie

            Opérateurs de chiffrement

            > [!NOTE]
            > Remarque: Groupes de niveau 0intégrés, consultez l’équivalence de niveau 0 pour plus d’informations.

            Autres groupes délégués

            > [!NOTE]
            > Remarque: N’importe quel groupe personnalisé créé avec l’accès de niveau 0 effectif, consultez l’équivalence de niveau 0 pour plus d’informations.

            Administrateurs Teir 1

            > [!NOTE]
            > Remarque: Ce groupe a été créé précédemment dans la Phase 1

    2.  Créez le nouvel **restriction de connexion serveur** GPO - ce paramètre empêchera les comptes d’administrateur de niveau 0 de se connecter aux serveurs de niveau 1.  Cet objet de stratégie de groupe doit être lié à l’unité d’organisation de niveau supérieur de «Serveurs de niveau 1» et ont les paramètres suivants:

        -   (i) Dans l’ordinateur Configuration ordinateur\Stratégies\Paramètres Windows\Paramètres sécurité\Stratégies locales\affectation des droits utilisateur\refuser connexion en tant que traitement par lots, sélectionnez **définir ces paramètres de stratégie** et ajoutez les groupes de niveau 0:

            **Groupes à ajouter aux paramètres de stratégie:**

            Administrateurs de l’entreprise

            Admins du domaine

            Administrateurs du schéma

            DOMAINE\Administrateurs

            Opérateurs de compte

            Opérateurs de sauvegarde

            Opérateurs d’impression

            Opérateurs de serveur

            Contrôleurs de domaine

            Read-Only Contrôleurs de domaine

            Groupe Propriétaires créateurs de stratégie

            Opérateurs de chiffrement

            > [!NOTE]
            > Remarque: Groupes de niveau 0intégrés, consultez l’équivalence de niveau 0 pour plus d’informations.

            Autres groupes délégués

            > [!NOTE]
            > Remarque: N’importe quel groupe personnalisé créé avec l’accès de niveau 0 effectif, consultez l’équivalence de niveau 0 pour plus d’informations.

        -   (ii) Dans l’ordinateur Configuration ordinateur\Stratégies\Paramètres Windows\Paramètres sécurité\Stratégies locales\Attribution des droits utilisateur\refuser connexion en tant que service, sélectionnez **définir ces paramètres de stratégie** et ajoutez les groupes de niveau 0:

            **Groupes à ajouter aux paramètres de stratégie:**

            Administrateurs de l’entreprise

            Admins du domaine

            Administrateurs du schéma

            DOMAINE\Administrateurs

            Opérateurs de compte

            Opérateurs de sauvegarde

            Opérateurs d’impression

            Opérateurs de serveur

            Contrôleurs de domaine

            Read-Only Contrôleurs de domaine

            Groupe Propriétaires créateurs de stratégie

            Opérateurs de chiffrement

            > [!NOTE]
            > Remarque: Groupes de niveau 0intégrés, consultez l’équivalence de niveau 0 pour plus d’informations.

            Autres groupes délégués

            > [!NOTE]
            > Remarque: N’importe quel groupe personnalisé créé avec l’accès de niveau 0 effectif, consultez l’équivalence de niveau 0 pour plus d’informations.

        -   (iii) Dans l’ordinateur Configuration ordinateur\Stratégies\Paramètres Windows\Paramètres sécurité\Stratégies locales\Attribution des droits utilisateur\refuser localement, sélectionnez **définir ces paramètres de stratégie** et ajoutez les groupes de niveau 0:

            **Groupes à ajouter aux paramètres de stratégie:**

            Administrateurs de l’entreprise

            Admins du domaine

            Administrateurs du schéma

            Opérateurs de compte

            Opérateurs de sauvegarde

            Opérateurs d’impression

            Opérateurs de serveur

            Contrôleurs de domaine

            Read-Only Contrôleurs de domaine

            Groupe Propriétaires créateurs de stratégie

            Opérateurs de chiffrement

            > [!NOTE]
            > Remarque: Groupes de niveau 0intégrés, consultez l’équivalence de niveau 0 pour plus d’informations.

            Autres groupes délégués

            > [!NOTE]
            > Remarque: N’importe quel groupe personnalisé créé avec l’accès de niveau 0 effectif, consultez l’équivalence de niveau 0 pour plus d’informations.

8.  Déployer votre Paw

    > [!NOTE]
    > Assurez-vous que le PAW est déconnecté du réseau pendant le processus de génération du système d’exploitation.

    1.  Installer Windows10 à l’aide du support d’installation de source propre que vous avez obtenu précédemment.

        > [!NOTE]
        > Vous pouvez utiliser MicrosoftDeployment Toolkit (MDT) ou un autre système de déploiement automatisé d’images pour automatiser le déploiement de PAW, mais vous devez vous assurer que le processus de génération est au moins aussi fiable que le PAW. Cherchent spécifiquement des images d’entreprise et les systèmes de déploiement (y compris les fichiers ISO, les packages de déploiement, etc.) comme mécanisme de persistance donc préexistants images ou systèmes de déploiement ne doivent pas être utilisés.
        >
        > Si vous automatisez le déploiement du PAW, vous devez:
        >
        > -   Créer le système à l’aide du support d’installation validé en utilisant les recommandations de [Source propre pour support d’installation](http://aka.ms/cleansource).
        > -   Assurez-vous que le système de déploiement automatisé est déconnecté du réseau pendant le processus de génération du système d’exploitation.

    2.  Définir un mot de passe complexe pour le compte d’administrateur local.  N’utilisez pas un mot de passe qui a été utilisé pour un autre compte dans l’environnement.

        > [!NOTE]
        > Microsoft recommande d’utiliser [Solution de mot de passe d’administrateur Local (LAPS)](https://www.microsoft.com/en-us/download/details.aspx?id=46899) pour gérer le mot de passe administrateur local pour toutes les stations de travail, y compris les Paw.  Si vous utilisez LAPS, assurez-vous que vous uniquement accorder groupe Maintenance de PAW le droit de lire les mots de passe gérés par LAPS pour les Paw.

    3.  Installez Remote Server Administration Tools pour Windows10 à l’aide du support d’installation de source propre.

    4.  Installez Enhanced Mitigation Experience Toolkit (EMET) à l’aide du support d’installation de source propre.

        > [!NOTE]
        > Veuillez noter que, au moment de la dernière mise à jour de ce guide, EMET 5.5 était toujours en version bêta.  Dans la mesure où il s’agit de la première version prise en charge sur Windows10, il est inclus ici en dépit de son état de la version bêta.  Si l’installation de la version bêta sur le PAW dépasse votre tolérance au risque, vous pouvez ignorer cette étape pour l’instant.

    5.  Connectez le PAW au réseau.  Assurez-vous que le PAW peut se connecter au contrôleur de domaine au moins un (DC).

    6.  À l’aide d’un compte qui est membre du groupe Maintenance de PAW, exécutez la commande PowerShell suivante à partir du PAW nouvellement créé pour le joindre au domaine dans l’unité d’organisation appropriée:

        `Add-Computer -DomainName Fabrikam -OUPath "OU=Devices,OU=Tier 0,OU=Admin,DC=fabrikam,DC=com"`

        Remplacez les références à *Fabrikam* avec votre nom de domaine, le cas échéant.  Si votre nom de domaine s’étend à plusieurs niveaux (par exemple, child.fabrikam.com), ajoutez les noms supplémentaires avec les «DC = «identificateur dans l’ordre dans lequel elles apparaissent dans le nom de domaine complet du domaine.

        > [!NOTE]
        > Si vous avez déployé un [forêt d’administration ESAE](http://aka.ms/esae) (pour les administrateurs de niveau 0dans la Phase 1) ou un [MicrosoftIdentity Manager (MIM) privileged access management (PAM)](http://aka.ms/mimpamdeploy) (de niveau 1 et 2administrateurs dans la Phase 2), vous joignez le PAW au domaine dans cet environnement au lieu du domaine de production.

    7.  S’appliquent à toutes les mises à jour de Windows critiques et importantes avant d’installer tout autre logiciel (y compris les outils d’administration, les agents, etc..).

    8.  Forcer l’application de stratégie de groupe.

        1.  Ouvrez une invite de commandes avec élévation de privilèges et entrez la commande suivante:

            `Gpupdate /force /sync`

        2.  Redémarrez l’ordinateur

    9. **(Facultatif) **Installer d’autres outils requis pour les administrateurs ActiveDirectory. Installez les autres outils ou scripts requis pour effectuer des tâches. Veillez à évaluer les risques d’exposition des informations d’identification sur les ordinateurs cibles avec tout outil avant de l’ajouter à un PAW. Accès [cette page](http://aka.ms/logontypes) pour obtenir plus d’informations sur l’évaluation des outils d’administration et les méthodes de connexion pour le risque d’exposition des informations d’identification. Veillez à obtenir tous les supports d’installation utilisant les instructions de [Source propre pour support d’installation](http://aka.ms/cleansource).

        > [!NOTE]
        > À l’aide d’un serveur de renvoi d’un emplacement central pour ces outils permettre réduire la complexité, même si elle ne constitue pas une limite de sécurité.

    10. **(Facultatif) **Télécharger et installer le logiciel d’accès à distance requis. Si les administrateurs utiliseront le PAW à distance pour l’administration, installez le logiciel d’accès à distance à l’aide du Guide de sécurité de votre fournisseur de solutions d’accès à distance. Veillez à obtenir tous les supports d’installation utilisant les instructions de Source propre pour support d’installation.

        > [!NOTE]
        > Étudiez attentivement tous les risques impliqués dans l’accès à distance via un PAW.  Si un PAW mobile permet plusieurs scénarios importants, y compris le travail à domicile, logiciel d’accès à distance peut être vulnérable aux attaques et compromettre un PAW.

    11. Validez l’intégrité du système PAW en examinant et en confirmant que tous les paramètres appropriés sont en place en suivant les étapes ci-dessous:

        1.  Confirmer qu’uniquement les stratégies de groupe spécifiques à PAW sont appliquées pour le PAW

            1.  Ouvrez une invite de commandes avec élévation de privilèges et entrez la commande suivante:

                `Gpresult /scope computer /r`

            2.  Passez en revue la liste résultante et vérifiez que le groupe uniquement les stratégies qui s’affichent sont celles que vous avez créé précédemment.

        2.  Vérifiez que les comptes d’utilisateurs supplémentaires sont membres des groupes privilégiés sur le PAW à l’aide de la procédure ci-dessous:

            1.  Ouvrez **modifier les utilisateurs et groupes locaux** (lusrmgr.msc), sélectionnez **groupes**et vérifiez que seuls les membres du groupe Administrateurs local sont le compte administrateur local et le groupe de sécurité global Maintenance de PAW.

                > [!NOTE]
                > Le groupe d’utilisateurs PAW ne doit pas être un membre du groupe Administrateurs local.  Seuls les membres doivent être le compte administrateur local et le groupe de sécurité global Maintenance de PAW (et les utilisateurs PAW ne doit pas être soit un membre de ce groupe global).

            2.  Également à l’aide de **modifier les utilisateurs et groupes locaux**, assurez-vous que les groupes suivants n’ont aucun membre:

                -   Opérateurs de sauvegarde

                -   Opérateurs de chiffrement

                -   Administrateurs Hyper-V

                -   Opérateurs de Configuration réseau

                -   Utilisateurs avec pouvoir

                -   Utilisateurs du Bureau à distance

                -   Duplicateurs

    12. **(Facultatif) **Si votre organisation utilise des informations de sécurité et de la solution de gestion (SIEM) des événements, vérifiez que le PAW est [configuré pour transférer les événements sur le système à l’aide de transfert des événements de Windows (WEF)](http://blogs.technet.com/b/jepayne/archive/2015/11/24/monitoring-what-matters-windows-event-forwarding-for-everyone-even-if-you-already-have-a-siem.aspx) ou enregistré avec la solution afin que la solution SIEM reçoive activement les événements et des informations à partir du PAW.  Les détails de cette opération varient en fonction de votre solution SIEM.

        > [!NOTE]
        > Si votre solution SIEM nécessite un agent qui s’exécute en tant que système ou un compte d’administration local sur les Paw, assurez-vous que les solutions SIEM sont gérées avec le même niveau de confiance que vos contrôleurs de domaine et les systèmes d’identité.

    13. **(Facultatif) **Si vous avez choisi de déployer LAPS pour gérer le mot de passe du compte administrateur local sur votre PAW, vérifiez que le mot de passe est enregistré correctement.

        -   À l’aide d’un compte disposant des autorisations pour lire les mots de passe gérés par LAPS, ouvrez **ActiveDirectory Users and Computers** (dsa.msc).  Vérifiez que l’option fonctionnalités avancées est activée et puis cliquez sur l’objet ordinateur approprié.  Sélectionnez l’onglet Éditeur d’attribut et confirmez que la valeur de msSVSadmPwd est remplie avec un mot de passe valide.

#### <a name="phase-2---extend-paw-to-all-administrators"></a>Phase 2 - étendre les PAW à tous les administrateurs
Étendue: Tous les utilisateurs disposant de droits administratifs sur les applications critiques et des dépendances.  Cela doit inclure au moins les administrateurs de serveurs d’applications, d’intégrité opérationnelle et de surveillance des solutions, les solutions de virtualisation, les systèmes de stockage et les périphériques réseau de la sécurité.

> [!NOTE]
> Les instructions de cette phase supposent que la Phase 1 a été effectuée dans son intégralité.  Ne commencez pas la Phase 2 jusqu'à ce que vous avez effectué toutes les étapes de la Phase 1.

Après avoir confirmé que toutes les étapes ont été réalisées, effectuez les étapes ci-dessous pour effectuer la Phase 2:

1.  **(Recommandé) **Activer **RestrictedAdmin** mode - activer cette fonctionnalité sur vos serveurs existants et les stations de travail, puis appliquez l’utilisation de cette fonctionnalité. Cette fonctionnalité nécessite que les serveurs cibles pour exécuter Windows Server2008R2 ou version ultérieure et cibler des stations de travail pour exécuter Windows7 ou version ultérieure.

    1.  Activer **RestrictedAdmin** mode sur vos serveurs et les stations de travail en suivant les instructions disponibles sur cette [page](http://aka.ms/rdpra).

        > [!NOTE]
        > Avant d’activer cette fonctionnalité pour les serveurs exposés à internet, vous devez envisager le risque que des pirates parviennent à authentifier ces serveurs avec un hachage de mot de passe volé précédemment.

    2.  Créer un objet stratégie de groupe (GPO) «RestrictedAdmin requis – ordinateur». Cette section crée un objet de stratégie de groupe qui applique l’utilisation de la /RestrictedAdmin commutateur pour les connexions Bureau à distance sortantes, protéger les comptes contre le vol d’informations d’identification sur les systèmes cibles

        -   Accédez à ordinateur configuration informations identification\restreindre la délégation des informations d’identification à des serveurs distants et la valeur **activé**.

    3.  Lien le **RestrictedAdmin** requis - ordinateur aux approprié niveau 1 et/ou niveau 2périphériques en utilisant les options de stratégie ci-dessous:

        Configuration PAW - ordinateur

        -> emplacement du lien: Admin\Tier 0\Devices (existant)

        Configuration PAW - utilisateur

        -> emplacement du lien: Admin\Tier 0\Accounts

        RestrictedAdmin requis - ordinateur

        -> admin\Tier1\Devices ou -> admin\Tier2\Devices (les deux sont facultatifs)

        > [!NOTE]
        > Cela n’est pas nécessaire pour les systèmes de niveau 0, ces systèmes ayant déjà dans un contrôle total de toutes les ressources dans l’environnement.

2.  Déplacer des objets de niveau 1vers les unités d’organisation appropriées.

    1.  Déplacer les groupes de niveau 1vers le Admin\Tier 1\Groups UO. Recherchez tous les groupes qui accorder les droits d’administration suivants et déplacement dans cette unité d’organisation.

        -   Administrateur local sur plusieurs serveurs

        -   Accès administratif aux services cloud

        -   Accès administratif aux applications d’entreprise

    2.  Déplacez les comptes de niveau 1vers l’UO Admin\Tier 1\ACCOUNTS. Déplacez chaque compte qui est membre de ces groupes de niveau 1 (y compris l’appartenance imbriquée) à cette unité d’organisation.

3.  Ajoutez les membres appropriés aux groupes pertinents

    -   **Administrateurs de niveau 1** -ce groupe contient les administrateurs de niveau 1 qui seront limités à partir de la journalisation sur les ordinateurs hôtes de niveau 2. Ajoutez toutes vos groupes d’administration de niveau 1 qui ont des privilèges d’administration sur les serveurs ou des services internet.

        > [!NOTE]
        > Si le personnel administratif a la responsabilité de gérer des ressources sur plusieurs niveaux, vous devez créer un compte administratif distinct par niveau.

4.  Activer Credential Guard afin de réduire le risque de vol d’informations d’identification et de réutilisation.  Credential Guard est une nouvelle fonctionnalité de Windows10 qui restreint l’accès de l’application des informations d’identification, la prévention des attaques de vol d’informations d’identification (y compris Pass-the-Hash).  Credential Guard est complètement transparent pour l’utilisateur final et nécessite des efforts et l’heure d’installation minimale.  Pour plus d’informations sur Credential Guard, y compris les étapes de déploiement et la configuration matérielle requise, reportez-vous à l’article [protéger les informations d’identification de domaine avec Credential Guard](https://technet.microsoft.com/en-us/library/mt483740%28v=vs.85%29.aspx).

    > [!NOTE]
    > Device Guard doit être activée pour configurer et utiliser la protection des informations d’identification.  Toutefois, vous n’êtes pas obligé de configurer les autres protections protection d’appareils afin d’utiliser la protection des informations d’identification.

5.  **(Facultatif) **Activer la connectivité aux Services Cloud. Cette étape permet la gestion des services cloud, comme Azure et Office 365 avec des garanties de sécurité approprié. Cette étape est également requise pour MicrosoftIntune pour gérer les Paw.

    > [!NOTE]
    > Ignorez cette étape si aucune connectivité cloud n’est requise pour l’administration des services cloud ou de gestion par Intune.

    Ces étapes seront restreindre la communication sur internet aux seuls services cloud autorisés (mais pas sur l’internet public) et ajoutent des protections pour les navigateurs et autres applications qui traitent le contenu à partir d’internet. Ces Paw pour l’administration ne doit jamais être utilisé pour les tâches d’utilisateur standard comme les communications sur internet et la productivité.

    Pour activer la connectivité à PAW services, procédez comme suit:

    1.  Configurez PAW pour autoriser uniquement les destinations Internet autorisées.  Lorsque vous étendez votre déploiement PAW pour activer l’administration du cloud, vous devez autoriser l’accès aux services autorisés en filtrant les accès à partir de l’internet public où les attaques peuvent être plus facilement lancées contre vos administrateurs.

        1.  Créer **administrateurs des Services Cloud** de groupe et ajouter tous les comptes qui nécessitent un accès aux services cloud sur internet.

        2.  Télécharger le PAW *proxy.pac* à partir de fichiers [Galerie TechNet](http://aka.ms/pawmedia) et publiez-le sur un site Web interne.

            > [!NOTE]
            > Vous devez mettre à jour le *proxy.pac* fichier après téléchargement pour vous assurer qu’il est à jour et complet.  
            > Microsoft publie toutes les Office 365 et Azure URL actuelle du bureau [centre de Support](https://support.office.com/en-us/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2?ui=en-US&rs=en-US&ad=US).
            >
            > Vous devrez peut-être ajouter d’autres destinations Internet valides pour l’ajouter à cette liste pour les autres fournisseurs IaaS, mais ne pas ajouter de productivité, divertissements, actualités ou rechercher des sites à cette liste.
            >
            > Vous devrez également régler le fichier PAC pour prendre en charge une adresse proxy valide à utiliser pour ces adresses.

            > [!NOTE]
            > Vous pouvez également restreindre l’accès à partir du PAW à l’aide d’un proxy web ainsi défense en profondeur. Nous déconseillons l’utilisation cela par lui-même sans fichier PAC comme il sera uniquement restreindre l’accès pour les Paw alors connectés au réseau d’entreprise.

            Ces instructions supposent que vous utilisez Internet Explorer (ou MicrosoftEdge) pour l’administration d’Office 365, Azure et autres services cloud. Microsoft vous recommande de configurer des restrictions similaires pour les navigateurs tiers 3edont vous avez besoin pour l’administration. Navigateurs Web sur les Paw doivent uniquement être utilisés pour l’administration des services cloud et jamais pour la navigation web générale.

        3.  Une fois que vous avez configuré le *proxy.pac* de fichier, la mise à jour Configuration PAW - GPO de l’utilisateur.

            1.  Allez dans configuration\preferences\paramètres windows\registre. Avec le bouton droit de Registre, sélectionnez **New** \> **élément de Registre** et configurez les paramètres suivants:

                1.  Action: remplacer

                2.  Ruche: HKEY_ CURRENT_USER

                3.  Chemin de la clé: Software\Microsoft\Windows\CurrentVersion\Internet Settings

                4.  Nom de la valeur: AutoConfigUrl

                    > [!NOTE]
                    > Ne sélectionnez pas le **par défaut** située à gauche du nom de valeur.

                5.  Type de valeur: REG_SZ

                6.  Données de la valeur: Saisissez l’URL complète pour le *proxy.pac* fichier, y compris http:// et le nom de fichier - http://proxy.fabrikam.com/proxy.pac par exemple.  L’URL peut également être une partie - http://proxy/proxy.pac par exemple,

                    > [!NOTE]
                    > Le fichier PAC peut également être hébergé sur un partage de fichiers, avec la syntaxe de file://server.fabrikan.com/share/proxy.pac, mais cela nécessite une autorisation du protocole file://. Consultez la section «Remarque: File://-based Proxy Scripts déconseillées» de ce [Configuration du Proxy Web présentation](http://blogs.msdn.com/b/ieinternals/archive/2013/10/11/web-proxy-configuration-and-ie11-changes.aspx) blog des détails supplémentaires sur la configuration de la valeur de Registre requise.

                7.  Cliquez sur le **commune** onglet et sélectionnez **supprimer cet élément lorsqu’il n’est plus appliqué**.

                8.  Sur le **commune** onglet sélectionnez **ciblage au niveau élément** et cliquez sur **ciblage**.

                9. Cliquez sur **nouvel élément** et sélectionnez **groupe de sécurité**.

                10. Sélectionnez le bouton «...» et recherchez le **administrateurs des Services Cloud** groupe.

                11. Cliquez sur **nouvel élément** et sélectionnez **groupe de sécurité**.

                12. Sélectionnez le bouton «...» et recherchez le **utilisateurs PAW** groupe.

                13. Cliquez sur le **utilisateurs PAW** de l’élément, puis cliquez sur **Options de l’élément**.

                14. Sélectionnez **n’est pas**.

                15. Cliquez sur **OK** sur la fenêtre de ciblage.

                16. Cliquez sur **OK** pour terminer la **AutoConfigUrl** paramètre de stratégie de groupe.

    2.  Appliquer des lignes de base de sécurité de Windows10 et lien d’accès Service Cloud les lignes de base de sécurité pour Windows et pour le service cloud accéder (si nécessaire) pour les unités d’organisation appropriées en suivant les étapes ci-dessous:

        1.  Extrayez le contenu du fichier ZIP de lignes de base sécurité de Windows10.

        2.  Créez ces objets de stratégie de groupe, [importer la stratégie](https://technet.microsoft.com/en-us/library/cc753786.aspx) paramètres, et [lien](https://technet.microsoft.com/en-us/library/cc732979.aspx) conformément à cette table. Liez chaque stratégie à chaque emplacement et assurez-vous que l’ordre suit la table (entrées inférieures dans la table doivent être appliquée ultérieurement et versions ultérieures de priorité):

            **Stratégies:**

            |||
            |-|-|
            |CM Windows10 - sécurité de domaine|N/a - ne pas lier maintenant|
            |SCM Windows10 TH2 - ordinateur|Admin\Tier 0\Devices|
            ||Admin\Tier 1\Devices|
            ||Admin\Tier 2\Devices|
            |SCM Windows10 TH2-BitLocker|Admin\Tier 0\Devices|
            ||Admin\Tier 1\Devices|
            ||Admin\Tier 2\Devices|
            |SCM Windows10 - Credential Guard|Admin\Tier 0\Devices|
            ||Admin\Tier 1\Devices|
            ||Admin\Tier 2\Devices|
            |SCM Internet Explorer - ordinateur|Admin\Tier 0\Devices|
            ||Admin\Tier 1\Devices|
            ||Admin\Tier 2\Devices|
            |Configuration PAW - ordinateur|Admin\Tier 0\Devices (existant)|
            ||Admin\Tier 1\Devices (nouveau lien)|
            ||Admin\Tier 2\Devices (nouveau lien)|
            |RestrictedAdmin requis - ordinateur|Admin\Tier 0\Devices|
            ||Admin\Tier 1\Devices|
            ||Admin\Tier 2\Devices|
            |SCM Windows10 - utilisateur|Admin\Tier 0\Devices|
            ||Admin\Tier 1\Devices|
            ||Admin\Tier 2\Devices|
            |Internet Explorer SCM - utilisateur|Admin\Tier 0\Devices|
            ||Admin\Tier 1\Devices|
            ||Admin\Tier 2\Devices|
            |Configuration PAW - utilisateur|Admin\Tier 0\Devices (existant)|
            ||Admin\Tier 1\Devices (nouveau lien)|
            ||Admin\Tier 2\Devices (nouveau lien)|

            > [!NOTE]
            > Le «SCM Windows10 – sécurité de domaine «objet stratégie de groupe peut être liée au domaine indépendamment du PAW, mais affecte l’ensemble du domaine.

6.  **(Facultatif) **Installer d’autres outils requis pour les administrateurs de niveau 1. Installez les autres outils ou scripts requis pour effectuer des tâches. Veillez à évaluer les risques d’exposition des informations d’identification sur les ordinateurs cibles avec tout outil avant de l’ajouter à un PAW. Pour plus d’informations sur l’évaluation des outils d’administration et aux méthodes de risque d’exposition des informations d’identification visitez [cette page](http://aka.ms/logontypes). Veillez à obtenir tous les supports d’installation utilisant les instructions de Source propre pour support d’installation

7.  Identifiez et obtenez en toute sécurité les logiciels et les applications requises pour l’administration.  Cela est similaire au travail effectué en Phase 1, mais avec une portée plus large en raison de l’augmentation du nombre d’applications, des services et des systèmes sécurisés.

    > [!NOTE]
    > Veillez à protéger ces nouvelles applications (y compris les navigateurs web) en activant les protections fournies par EMET.

    Exemples d’applications et logiciels supplémentaires:

    -   MicrosoftAzure PowerShell

    -   PowerShell pour Office 365 (également appelé Module MicrosoftOnline Services)

    -   Logiciels de gestion de service basée sur la Console de gestion Microsoft ou d’application

    -   Logiciels de gestion de service ou les propriétaire application (non-basée sur MMC)

        > [!NOTE]
        > De nombreuses applications sont désormais exclusivement gérées via les navigateurs web, y compris de nombreux services cloud.  Même si cela réduit le nombre d’applications qui doivent être installées sur un PAW, elle présente également des risques de problèmes d’interopérabilité de navigateur.  Vous devrez peut-être déployer un navigateur web non Microsoft sur des instances spécifiques de PAW pour activer l’administration des services spécifiques.  Si vous déployez un navigateur web supplémentaire, assurez-vous que vous suivez toutes les principes de source propre et sécuriser le navigateur en fonction des conseils sur la sécurité du fournisseur.

8.  **(Facultatif) **Télécharger et installer des agents de gestion requis.

    > [!NOTE]
    > Si vous choisissez d’installer des agents de gestion supplémentaires (surveillance, sécurité, gestion de la configuration, etc.), il est essentiel de vérifier que les systèmes de gestion sont approuvés au même niveau que les contrôleurs de domaine et les systèmes d’identité. Pour plus d’informations, consultez Gestion et mise à jour des Paw.

9. Évaluez votre infrastructure pour identifier les systèmes qui nécessitent les protections de sécurité supplémentaires fournies par un PAW.  Assurez-vous de savoir exactement quels systèmes doivent être protégées.  Poser des questions essentielles relatives aux ressources, telles que:

    -   Où sont les systèmes cibles qui doivent être gérés?  Ils collectées dans un seul emplacement physique, ou connectés à un seul sous-réseau bien défini?

    -   Le nombre de systèmes sont-ils présents?

    -   Ces systèmes dépendent-ils sur d’autres systèmes (virtualisation, stockage, etc.), et si tel est le cas, comment ces systèmes gérés?  Comment les systèmes critiques sont-ils exposés à ces dépendances, et quels sont les risques associés avec ces dépendances?

    -   Comment critiques sont les services gérés sont-ils, et quelle est la perte attendue si ces services étaient compromis?

        > [!NOTE]
        > Inclure vos services cloud dans cette évaluation: les pirates ciblent plus en plus des déploiements de cloud non sécurisés, et il est essentiel que vous administriez ces services de façon aussi sécurisée que vous le feriez pour vos applications critiques locales.

        Utilisez cette évaluation pour identifier les systèmes qui nécessitent une protection supplémentaire, puis étendez votre programme PAW aux administrateurs de ces systèmes.  Des exemples courants de systèmes qui profitent grandement de l’administration basée sur les PAW comprennent SQLServer (sur site et SQL Azure), les applications de ressources humaines et les logiciels financiers.

        > [!NOTE]
        > Si une ressource est gérée à partir d’un système Windows, il peut être géré avec un PAW, même si l’application elle-même s’exécute sur un système d’exploitation autres que Windows ou sur une plateforme cloud non Microsoft.  Par exemple, le propriétaire d’un abonnement Amazon Web Services doit uniquement utiliser un PAW pour administrer ce compte.

10. Développez une méthode de demande et de distribution pour le déploiement de Paw à l’échelle de votre organisation.  En fonction du nombre de Paw que vous choisissez de déployer en Phase 2, vous devrez peut-être automatiser le processus.

    -   Envisagez de développer une demande formelle et le processus d’approbation pour les administrateurs à utiliser pour obtenir un PAW.  Ce processus permet de normaliser le processus de déploiement, garantir la responsabilité pour les appareils PAW et vous aider à identifier les lacunes dans le déploiement de PAW.

    -   Comme indiqué précédemment, cette solution de déploiement doit être distincte de méthodes d’automatisation existantes (qui a peut-être déjà été compromis) et doit suivre les principes énoncés dans la Phase 1.

        > [!NOTE]
        > Tout système qui gère les ressources doit lui-même être géré au niveau de confiance identique ou supérieur.

11. Passez en revue et déployer des profils matériels PAW supplémentaires si nécessaire.  Le profil matériel que vous avez choisi pour le déploiement de la Phase 1 peut ne pas convenir pour tous les administrateurs.  Passez en revue les profils matériels et sélectionnez éventuellement les profils matériels PAW supplémentaires pour répondre aux besoins des administrateurs.  Par exemple, le profil matériel dédié (distinct PAW et utilisation quotidienne stations de travail) peut être inapproprié pour un administrateur qui se déplace souvent: dans ce cas, vous pouvez choisir de déployer le profil utilisation simultanée (PAW avec machine virtuelle d’utilisateur) pour cet administrateur.

12. Tenez compte des communications, culturelles et opérationnelles et les besoins de formation qui accompagnent un déploiement PAW étendu.   Un changement aussi significatif à un modèle d’administration nécessite naturellement à la gestion des modifications, et il est essentiel d’intégrer le projet de déploiement lui-même.  Au minimum les points suivants:

    -   Comment vous communiquent les modifications apportées à la direction pour assurer la prise en charge?  Un projet sans communiquerez-vous sauvegarde est susceptible d’échouer, ou à la moins très difficile à financer et faire accepter.

    -   Comment documenterez-vous le nouveau processus pour les administrateurs?  Ces modifications doivent être documentées et communiquées non seulement aux administrateurs existants (qui doivent changer leurs habitudes et gérer les ressources d’une façon différente), mais également pour les nouveaux administrateurs (ceux promus dans ou recrutés depuis l’extérieur de l’organisation).  Il est essentiel que la documentation soit claire et explique complètement l’importance des menaces, rôle des PAW pour protéger les administrateurs et comment utiliser les PAW correctement.

        > [!NOTE]
        > Cela est particulièrement important pour les rôles à rotation élevée, y compris, mais pas limité au personnel du support technique.

    -   Comment garantirez-vous la conformité avec le nouveau processus?  Tandis que le modèle PAW inclut un nombre de contrôles techniques pour éviter l’exposition des informations d’identification privilégiées, il est impossible à éviter totalement toute exposition possible uniquement à l’aide de contrôles techniques.  Par exemple, bien qu’il soit possible d’empêcher un administrateur de se connecter avec succès sur un ordinateur de bureau utilisateur avec les informations d’identification privilégiées, le simple fait de tenter l’ouverture de session peut exposer les informations d’identification aux logiciels malveillants installés sur ce bureau de l’utilisateur.  Il est donc essentiel de détailler non seulement les avantages du modèle PAW, mais les risques de non-conformité.  Cela doit être complété par des audits et des alertes afin que l’exposition des informations d’identification peut être détectée et traitée rapidement.

#### <a name="phase-3-extend-and-enhance-protection"></a>Phase 3: Étendre et améliorer la Protection
Étendue: Ces protections améliorent les systèmes créés en Phase 1, en renforçant la protection de base avec des fonctionnalités avancées, y compris l’authentification multifacteur et les règles d’accès réseau.

> [!NOTE]
> Cette phase peut être effectuée à tout moment après que la Phase 1 a été effectuée.  Il n’est pas dépendante à la fin de la Phase 2 et peut donc être exécutée avant, simultanées, ou après la Phase 2.

Suivez les étapes ci-dessous pour configurer cette phase:

1.  Activer l’authentification multifacteur pour les comptes privilégiés.  L’authentification multifacteur renforce la sécurité du compte en demandant à l’utilisateur à fournir un jeton physique en plus des informations d’identification.  L’authentification multifacteur complète des stratégies d’authentification extrêmement bien, mais elle ne dépend pas de stratégies d’authentification pour le déploiement (et, de même, les stratégies d’authentification ne nécessitent pas l’authentification multifacteur).  Microsoft recommande d’utiliser une de ces formes d’authentification multifacteur:

    -   **Carte à puce**: une carte à puce est un appareil physique résistant aux falsifications et portable qui fournit une seconde vérification pendant le processus d’ouverture de session Windows.  En demandant à un individu de posséder une carte pour la connexion, vous pouvez réduire le risque d’informations d’identification volées à distance.  Pour plus d’informations sur la carte à puce d’ouverture de session dans Windows, reportez-vous à l’article [vue d’ensemble de la carte à puce](https://technet.microsoft.com/en-us/library/hh831433.aspx).

    -   **Carte à puce virtuelle**: une carte à puce virtuelle présente les mêmes avantages de sécurité des cartes à puce comme physiques, avec l’avantage d’être liées à un matériel spécifique.  Pour plus d’informations sur le déploiement et la configuration matérielle requise, reportez-vous aux articles [vue d’ensemble de carte à puce virtuelles](https://technet.microsoft.com/en-us/library/dn593708.aspx) et [commencer avec les cartes à puce virtuelles: Guide pas à pas](https://technet.microsoft.com/en-us/library/dn579260.aspx).

    -   **MicrosoftPassport**: MicrosoftPassport permet aux utilisateurs de s’authentifier auprès d’un compte Microsoft, d’un compte ActiveDirectory, un compte MicrosoftAzure ActiveDirectory (Azure AD) ou un service non-Microsoft qui prend en charge l’authentification Fast ID Online (FIDO). Après une vérification initiale en deux étapes lors de l’inscription de MicrosoftPassport, MicrosoftPassport est configuré sur l’appareil de l’utilisateur et l’utilisateur définit un mouvement, qui peut être Windows Hello ou un code confidentiel. Informations d’identification MicrosoftPassport sont une paire de clés asymétrique, ce qui peut être générée dans des environnements isolés de Modules de plateforme sécurisée (TPM).
        Pour plus d’informations sur MicrosoftPassport, lisez [vue d’ensemble de MicrosoftPassport](https://technet.microsoft.com/en-us/library/dn985839%28v=vs.85%29.aspx) article.

    -   **L’authentification multifacteur Azure**: l’authentification multifacteur Azure (MFA) fournit la sécurité d’un second facteur de vérification, ainsi que la protection renforcée par l’analyse basée sur une formation-ordinateur et d’analyse.  Azure MFA peut sécuriser non seulement les administrateurs Azure, mais nombreuses autres solutions, y compris les applications web, Azure ActiveDirectory et des solutions locales telles que l’accès à distance et Bureau à distance.  Pour plus d’informations sur l’authentification multifacteur Azure, reportez-vous à l’article [multi-Factor Authentication](https://azure.microsoft.com/en-us/services/multi-factor-authentication).

2.  **Liste d’autorisation sécurisée des applications à l’aide de la protection des appareils et/ou AppLocker**.  En limitant la capacité du code non approuvé ou non signée à s’exécuter sur un PAW, vous réduisez encore la probabilité d’activités malveillantes de compromissions.  Windows comprend deux options principales pour le contrôle d’application:

    -   **AppLocker**: AppLocker permet aux administrateurs de contrôler les applications pouvant être exécutées sur un système donné.  AppLocker peut être centralisée contrôlé via la stratégie de groupe et appliqué à des utilisateurs ou groupes (pour une application ciblée aux utilisateurs de Paw).  Pour plus d’informations sur AppLocker, reportez-vous à l’article TechNet [vue d’ensemble d’AppLocker](https://technet.microsoft.com/library/hh831440.aspx).

    -   **Device Guard**: la nouvelle fonctionnalité de Device Guard fournit un contrôle étendu application en fonction du matériel qui, contrairement à AppLocker, ne peut pas être remplacé sur l’appareil concerné.  Comme AppLocker, Device Guard peut être contrôlée via une stratégie de groupe et destiné à des utilisateurs spécifiques.  Pour plus d’informations sur la limitation de l’utilisation des applications avec Device Guard, reportez-vous à l’article TechNet [Guide de déploiement de Device Guard](https://technet.microsoft.com/en-us/library/mt463091(v=vs.85).aspx).

3.  **Utilisez les utilisateurs protégés, les stratégies d’authentification et Silos d’authentification pour protéger davantage les comptes privilégiés**.  Les membres des utilisateurs protégés sont soumis à des stratégies de sécurité supplémentaire qui protègent les informations d’identification stockées dans l’agent de sécurité locale (LSA) et réduisent le risque de vol d’informations d’identification et de réutilisation.  Stratégies d’authentification et silos contrôlent comment privilégiés utilisateurs peuvent accéder aux ressources dans le domaine.  Collectivement, ces protections renforcent de façon spectaculaire la sécurité du compte de ces utilisateurs privilégiés.  Pour plus d’informations sur ces fonctionnalités, reportez-vous à l’article web [comment configurer des comptes protégés](https://technet.microsoft.com/en-us/library/dn518179.aspx).

    > [!NOTE]
    > Ces protections sont destinées à compléter, sans les remplacer, les mesures de sécurité à la Phase 1.  Les administrateurs doivent toujours utiliser des comptes distincts pour l’administration et une utilisation générale.

### <a name="managing-and-updating-paws"></a>La gestion et la mise à jour des Paw
Les Paw doivent avoir des fonctionnalités anti-programme malveillant et mises à jour logicielles doivent être appliquées rapidement pour conserver l’intégrité de ces stations de travail.

Gestion de la configuration supplémentaire, la surveillance opérationnelle et la gestion de la sécurité peuvent également être utilisées avec Paw, mais leur intégration doit être envisagée avec précaution, car chaque fonctionnalité de gestion présente également des risques de compromission de PAW à travers cet outil. S’il est judicieux d’introduire des fonctions de gestion avancées dépend d’un nombre de facteurs, notamment:

-   L’état de sécurité et les pratiques des capacités de gestion (y compris les pratiques pour la mise à jour de logiciels pour l’outil, les rôles administratifs et les comptes de ces rôles et les systèmes d’exploitation l’outil est hébergé ou géré à partir de toutes les autres dépendances matérielles ou logicielles de cet outil)

-   La fréquence et la quantité des déploiements de logiciels et mises à jour sur vos Paw

-   Configuration requise pour des informations détaillées sur la configuration et d’inventaire

-   Surveillance des exigences de la sécurité

-   Les normes organisationnelles et autres facteurs spécifiques à l’organisation

Par le principe de source propre, tous les outils utilisés pour gérer ou surveiller les Paw doivent être approuvés au niveau ou au-dessus du niveau de Paw. Ceci nécessite généralement ces outils pour être gérés à partir d’un PAW afin de n’assurer aucune dépendance de sécurité à partir de stations de travail plus faible privilège.

Le tableau suivant décrit les différentes approches qui peuvent être utilisés pour gérer et surveiller les Paw:

|Approche|Considérations relatives à|
|------|---------|
|Par défaut dans le PAW<br /><br />-WindowsServerUpdateServices<br />-Windows Defender|-Aucun coût supplémentaire<br />-Exécute les fonctions de sécurité de base requise<br />-Les instructions fournies dans ce guide|
|Gérer avec [Intune](https://technet.microsoft.com/en-us/library/jj676587.aspx)|<ul><li>Fournit un contrôle et visibilité en cloud<br /><br /><ul><li>Déploiement de logiciels</li><li>O gérer les mises à jour logicielles</li><li>Gestion des stratégies de pare-feu Windows</li><li>Protection anti-programme malveillant</li><li>Assistance à distance</li><li>Gestion des licences logicielles.</li></ul></li><li>Aucune infrastructure serveur requise</li><li>Nécessite «Activer la connectivité pour Cloud Services» comme suit dans la Phase 2</li><li>Si l’ordinateur PAW n’est pas joint à un domaine, vous devez appliquer les lignes de base SCM aux images locales avec les outils fournis dans le téléchargement de ligne de base de sécurité.</li></ul>|
|Nouvelles instances de SystemCenter pour gérer les Paw|-Assure la visibilité et le contrôle de configuration, le déploiement de logiciels et mises à jour de sécurité<br />-Nécessite une infrastructure serveur distincte, avec le niveau de Paw et compétent pour ce personnel dotés de privilèges élevés|
|Gestion des Paw avec les outils de gestion existants|-Crée un risque significatif de compromission des Paw, sauf si l’infrastructure de gestion existante est élevée au niveau de sécurité de Paw **Remarque:** Microsoft décourage généralement cette approche, sauf si votre organisation a une raison spécifique de l’utiliser. Dans notre expérience, il est généralement très coûteux de donner à tous ces outils (et leurs dépendances de sécurité) sur le niveau de sécurité des Paw.<br />-La plupart de ces outils assurent la visibilité et contrôle de configuration, le déploiement de logiciels et mises à jour de sécurité|
|Analyse de sécurité ou outils de surveillance nécessitant l’accès administrateur|Comprend un outil qui installe un agent ou requiert un compte disposant d’un accès.<br /><br />-Nécessite de mettre la garantie de sécurité outil jusqu’au niveau de Paw.<br />-Peut nécessiter ce qui réduit la posture de sécurité de Paw pour prendre en charge la fonctionnalité de l’outil (ouvrir des ports, installer Java ou d’autres intergiciels, etc.), la création d’une décision compromis de sécurité,|
|Informations et événements gestion de la sécurité (SIEM)|<ul><li>Si SIEM est sans agent<br /><br /><ul><li>Peut accéder aux événements sur les Paw sans accès administratif à l’aide d’un compte dans le **lecteurs des journaux d’événements** groupe</li><li>Nécessite l’ouverture des ports de réseau pour autoriser le trafic entrant à partir des serveurs SIEM</li></ul></li><li>Si SIEM requiert un agent, voir les autres lignes **analyse de sécurité ou outils de surveillance nécessitant l’accès administrateur**.</li></ul>|
|Transfert d’événements Windows|-Fournit une méthode de transfert d’événements de sécurité de Paw à un collecteur externe ou un SIEM sans agent<br />-Il est possible d’accéder aux événements sur les Paw sans accès administratif<br />-Ne nécessite pas l’ouverture des ports de réseau pour autoriser le trafic entrant à partir des serveurs SIEM|

#### <a name="operating-paws"></a>Exploitation des Paw
La solution PAW doit être exploitée en respectant les normes des [normes opérationnelles](http://aka.ms/securitystandards) basé sur le principe de Source.

## <a name="related-topics"></a>Rubriques connexes
[Services de cybersécurité de Microsoft](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx)

[Aperçu de Premier: comment atténuer Pass-the-Hash et autres formes de vol d’informations d’identification](https://channel9.msdn.com/Blogs/Taste-of-Premier/Taste-of-Premier-How-to-Mitigate-Pass-the-Hash-and-Other-Forms-of-Credential-Theft)

[MicrosoftAdvanced Analytique contre les menaces](http://aka.ms/ata)

[Protéger les informations d’identification de domaine dérivées avec Credential Guard](https://technet.microsoft.com/en-us/library/mt483740%28v=vs.85%29.aspx)

[Vue d’ensemble de Device Guard](https://technet.microsoft.com/en-us/library/dn986865(v=vs.85).aspx)

[Protection des ressources à haute valeur avec les stations de travail admin sécurisées](https://msdn.microsoft.com/en-us/library/mt186538.aspx)

[Mode utilisateur isolé dans Windows10 avec Dave Probert (Channel 9)](https://channel9.msdn.com/Blogs/Seth-Juarez/Isolated-User-Mode-in-Windows-10-with-Dave-Probert)

[Isolé des processus en Mode utilisateur et des fonctionnalités dans Windows10 avec Logan Gabriel (Channel 9)](http://channel9.msdn.com/Blogs/Seth-Juarez/Isolated-User-Mode-Processes-and-Features-in-Windows-10-with-Logan-Gabriel)

[Plus sur les processus et fonctionnalités en Mode utilisateur isolé de Windows10 avec Dave Probert (Channel 9)](https://channel9.msdn.com/Blogs/Seth-Juarez/More-on-Processes-and-Features-in-Windows-10-Isolated-User-Mode-with-Dave-Probert)

[Atténuation de vol d’informations d’identification à l’aide de la Windows10 Mode utilisateur isolé (Channel 9)](https://channel9.msdn.com/Blogs/Seth-Juarez/Mitigating-Credential-Theft-using-the-Windows-10-Isolated-User-Mode)

[L’activation de la Validation KDC stricte dans Windows Kerberos](https://www.microsoft.com/en-us/download/details.aspx?id=6382)

[Nouveautés de l’authentification Kerberos pour Windows Server2012](https://technet.microsoft.com/library/hh831747.aspx)

[L’Assurance du mécanisme d’authentification pour ADDS dans Windows Server2008R2 Step-by-Step Guide](https://technet.microsoft.com/library/dd378897(v=ws.10).aspx)

[Module de plateforme sécurisée](C:\sd\docs\p_ent_keep_secure\p_ent_keep_secure\trusted_platform_module_technology_overview.xml)
