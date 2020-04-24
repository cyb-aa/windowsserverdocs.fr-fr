---
title: Sécurisation de l’accès privilégié
description: Approche par étape pour la sécurisation de l’accès privilégié
ms.prod: windows-server
ms.topic: conceptual
ms.assetid: f5dec0c2-06fe-4c91-9bdc-67cc6a3ede60
ms.date: 02/25/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: mas
ms.openlocfilehash: 5fefdb83667ba7893218de21df1f6c36cae40e12
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2020
ms.locfileid: "80855122"
---
# <a name="securing-privileged-access"></a>Sécurisation de l’accès privilégié

>S'applique à : Windows Server

La sécurisation de l’accès privilégié est une première étape essentielle pour établir des garanties de sécurité pour les ressources d’entreprise dans une organisation moderne. Dans une organisation, la sécurité de la plupart ou de la totalité des biens d’entreprise dépend de l’intégrité des comptes privilégiés utilisés pour l’administration, la gestion et le développement. Les pirates informatiques ciblent souvent ces comptes et d’autres éléments de l’accès privilégié pour accéder aux données et aux systèmes grâce à des attaques leur permettant de voler des informations d’identification comme [Pass-the-Hash et Pass-the-Ticket](https://www.microsoft.com/pth).

La protection de l’accès privilégié contre ses adversaires déterminés vous oblige à adopter une approche complète et réfléchie pour isoler ces systèmes des risques.

## <a name="what-are-privileged-accounts"></a>Qu’est-ce qu’un compte privilégié ?

Avant d’apprendre à le sécuriser, découvrons d’abord ce qu’est un compte privilégié.

Les comptes privilégiés, tels que les administrateurs des services de domaine Active Directory disposent d’un accès direct ou indirect à la plupart ou à la totalité des biens d’un service informatique. La compromission de ces comptes représente donc un risque important pour l’entreprise.

## <a name="why-securing-privileged-access-is-important"></a>Pourquoi est-il important de sécuriser l’accès privilégié ?

Les pirates informatiques se concentrent sur l’accès privilégié aux systèmes tels qu’Active Directory (AD) pour accéder rapidement à l’ensemble des données ciblées d’une organisation. Les approches de sécurité traditionnelles se concentrent sur le réseau et les pare-feu en tant que périmètre de sécurité principal, mais l’efficacité de la sécurité réseau a considérablement diminué suite à deux tendances :

* Les organisations hébergent des données et des ressources, situées en dehors des délimitations du réseau traditionnel, sur les PC mobiles de l’entreprise, des appareils tels que des téléphones portables et des tablettes, des services cloud et des appareils BYOD.
* Les adversaires ont fait montre d’une capacité constante et suivie à obtenir des accès sur des stations de travail situées dans les limites du réseau par le biais du hameçonnage et d’autres attaques web et par e-mail.

Ces facteurs nécessitent la mise en place d’un périmètre de sécurité moderne à partir des contrôles d’authentification et d’autorisation d’identité, en plus de la stratégie de périmètre de réseau classique. Un périmètre de sécurité se définit ici comme un ensemble cohérent de contrôles entre les biens et les menaces qui pèsent sur eux. Les comptes privilégiés contrôlent en effet ce nouveau périmètre de sécurité. Il est donc essentiel de protéger l’accès privilégié.

![Diagramme montrant la couche d’identité d’une organisation](../media/securing-privileged-access/PAW_LP_Fig2.JPG)

Un pirate informatique qui prend le contrôle d’un compte administratif peut utiliser ces privilèges pour renforcer leur impact sur l’organisation cible, comme illustré ci-dessous :

![Diagramme montrant comment un adversaire qui prend le contrôle d’un compte administratif peut utiliser ces privilèges pour en tirer profit au détriment de l’organisation cible](../media/securing-privileged-access/PAW_LP_Fig3.JPG)

L’illustration ci-dessous montre deux chemins d’accès :

* Un chemin d’accès « bleu » dans lequel un compte d’utilisateur standard est utilisé pour l’accès non privilégié aux ressources, telles que la messagerie électronique et la navigation Web et où les tâches quotidiennes s’effectuent.

   > [!NOTE]
   > Les éléments de chemin d’accès bleus décrits plus loin indiquent les vastes mesures de protection de l’environnement qui s’étendent au-delà des comptes administratifs.

* Un chemin d’accès « rouge » dans lequel l’accès privilégié se produit sur un appareil renforcé pour réduire le risque d’hameçonnage et d’autres attaques par le Web et par courrier électronique.

![Diagramme montrant le « chemin d’accès » distinct d’administration que la feuille de route établit pour isoler les tâches à accès privilégié des tâches d’utilisateur standard très risquées, comme la navigation sur Internet et l’accès aux e-mails.](../media/securing-privileged-access/PAW_LP_Fig4.JPG)

## <a name="securing-privileged-access-roadmap"></a>Sécurisation de la feuille de route de l’accès privilégié

La feuille de route est conçue pour maximiser l’utilisation de technologies Microsoft que vous avez déjà déployées, pour exploiter des technologies de cloud pour renforcer la sécurité et pour intégrer des outils de sécurité tiers que vous avez peut-être déjà déployés.

La feuille de route des recommandations de Microsoft comporte 3 phases :

* [Phase 1 : 30 premiers jours]()
   * Gains rapides avec impact positif significatif.
* [Phase 2 : 90 jours]()
   * Améliorations progressives significatives.
* [Phase 3 : En cours]()
   * Amélioration et maintien de la sécurité.

La feuille de route est hiérarchisée de sorte à planifier les implémentations les plus efficaces et les plus rapides en premier, conformément à notre expérience de ces attaques et de l’implémentation des solutions. 

Microsoft vous recommande de suivre cette feuille de route pour sécuriser l’accès privilégié contre ses adversaires déterminés. Vous pouvez ajuster cette feuille de route pour l’adapter à vos capacités existantes et aux besoins spécifiques de votre organisation.

> [!NOTE]
> La sécurisation de l’accès privilégié exige un large éventail d’éléments, notamment des composants techniques (défenses d’hôte, protections de comptes, gestion des identités, etc.), ainsi que des modifications à traiter et des pratiques et connaissances administratives. La chronologie de la feuille de route est approximative et repose sur notre expérience des implémentations de nos clients. La durée varie dans votre organisation en fonction de la complexité de votre environnement et de vos processus de gestion des modifications.

## <a name="phase-1-quick-wins-with-minimal-operational-complexity"></a>Phase 1 : Gains rapides avec complexité opérationnelle minimale

La phase 1 de la feuille de route vise à atténuer rapidement les techniques d’attaque les plus souvent utilisées en matière de vol et d’abus d’informations d’identification. La phase 1, représentée dans ce diagramme, est conçue pour être implémentée dans un délai d’environ 30 jours :

![Diagramme de la phase 1 : 1. Compte d’administrateur et d’utilisateur distinct, 2. Mots de passe d’administrateur local juste-à-temps, 3. Station de travail administrateur, étape 1 et 4 Détection des attaques d’identité](../media/securing-privileged-access/PAW_LP_Fig6.JPG)

### <a name="1-separate-accounts"></a>1. Comptes distincts

Pour mieux séparer les risques Internet (attaques par hameçonnage, navigation web) des comptes à accès privilégié, créez un compte dédié pour tous les employés ayant un accès privilégié. Les administrateurs ne doivent pas naviguer sur le Web, vérifier leurs e-mails ou effectuer des tâches de productivité quotidiennes avec des comptes dotés de privilèges élevés. Pour plus d’informations, consultez la section [Comptes d’administration distincts](securing-privileged-access-reference-material.md#separate-administrative-accounts) du document de référence.

Suivez les instructions de l’article [Gérer des comptes d’accès d’urgence dans Azure AD](/azure/active-directory/users-groups-roles/directory-emergency-access) pour créer au moins deux comptes d’accès d’urgence avec des droits d’administrateur affectés de manière permanente dans vos environnements AD et Azure AD locaux. Ces comptes doivent être utilisés uniquement lorsque les comptes d’administrateur traditionnels ne peuvent pas effectuer une tâche requise, par exemple dans le cas d’un incident.

### <a name="2-just-in-time-local-admin-passwords"></a>2. Mots de passe d’administrateur local juste-à-temps

Pour éviter qu’un adversaire ne parvienne à voler un hachage de mot de passe de compte d’administrateur local à partir de la base de données SAM locale et à l’utiliser pour attaquer d’autres ordinateurs, les organisations doivent s’assurer que chaque ordinateur dispose de son propre mot de passe d’administrateur local. L’outil de solution de mot de passe d’administrateur local (LAPS) permet de configurer des mots de passe aléatoires uniques sur chaque station de travail. Chaque serveur les stocke dans Active Directory (AD), qui est protégé par une liste de contrôle d’accès. Seuls les utilisateurs autorisés éligibles peuvent lire ou demander la réinitialisation de ces mots de passe de compte d’administrateur local. Vous pouvez obtenir les LAPS pour les utiliser sur les stations de travail et les serveurs à partir du [Centre de téléchargement Microsoft](https://aka.ms/LAPS).

Pour plus d’informations sur l’exploitation d’un environnement avec LAPS et PAW, consultez la section [Normes opérationnelles basées sur un principe de source propre](securing-privileged-access-reference-material.md#operational-standards-based-on-clean-source-principle).

### <a name="3-administrative-workstations"></a>3. Stations de travail administratives

En guise de mesure de sécurité initiale pour les utilisateurs disposant de privilèges d’administration pour Azure Active Directory et de privilèges d’administration traditionnels locaux pour Active Directory, assurez-vous qu’ils utilisent des appareils Windows 10 configurés avec les [normes relatives à un appareil Windows 10 hautement sécurisé](/windows-hardware/design/device-experiences/oem-highly-secure). Les comptes d’administrateur privilégiés ne doivent pas être membres du groupe d’administrateurs local des stations de travail d’administration.  L’élévation des privilèges via le contrôle d’accès utilisateur (UAC) peut être utilisée lorsque des modifications de configuration doivent être apportées aux stations de travail.  En outre, la base de référence de sécurité Windows 10 doit être appliquée aux stations de travail pour renforcer la sécurité de l’appareil.

### <a name="4-identity-attack-detection"></a>4. Détection des attaques d’identité

[Azure Advanced Threat Protection (ATP) (protection avancée contre les menaces)](/azure-advanced-threat-protection/what-is-atp) est une solution de sécurité cloud qui permet d’identifier, de détecter et d’examiner les menaces avancées, les identités compromises et les actions malveillantes initiées contre votre environnement Active Directory local.

## <a name="phase-2-significant-incremental-improvements"></a>Phase 2 : Améliorations progressives significatives

La phase 2 s’appuie sur le travail effectué au cours de la phase 1 et est conçue pour être effectuée en 90 jours environ. Les étapes de cette phase sont illustrées dans ce diagramme :

![Diagramme de la phase 2 : 1. Windows Hello Entreprise/MFA, 2. Déploiement de PAW; 3. Privilèges juste-à-temps, 4. Credential Guard, 5. Informations d’identification divulguées, 6. Détection de vulnérabilité de mouvement latéral](../media/securing-privileged-access/PAW_LP_Fig7.JPG)

### <a name="1-require-windows-hello-for-business-and-mfa"></a>1. Nécessite Windows Hello Entreprise et MFA

Les administrateurs peuvent tirer avantage de la facilité d’utilisation associée à Windows Hello Entreprise. Les administrateurs peuvent remplacer leurs mots de passe complexes par une authentification forte à 2 facteurs sur leur ordinateurs. Étant donné qu’une personne malveillante doit disposer à la fois de l’appareil et des informations biométriques ou du code confidentiel, il est beaucoup plus difficile de se connecter à l’insu de l’employé. Pour plus d’informations sur Windows Hello Entreprise et le chemin d’accès à déployer, consultez l’article [Vue d’ensemble de Windows Hello Entreprise](/windows/security/identity-protection/hello-for-business/hello-overview)

Activez l’authentification multifacteur (MFA) pour vos comptes d’administrateur dans Azure AD à l’aide d’Azure MFA. Activez au minimum la [stratégie d’accès conditionnel de protection de base de référence](/azure/active-directory/conditional-access/baseline-protection#require-mfa-for-admins). Pour plus d’informations sur l’authentification multifacteur Azure, consultez l’article [Déployer des authentifications multifacteur basées sur le Cloud](/azure/active-directory/authentication/howto-mfa-getstarted)

### <a name="2-deploy-paw-to-all-privileged-identity-access-account-holders"></a>2. Déployer le PAW sur tous les titulaires d’un compte d’accès d’identité privilégié

Pour poursuivre le processus de séparation des comptes privilégiés des menaces détectées dans les courriers électroniques, la navigation Web et d’autres tâches non administratives, vous devez implémenter des stations de travail à accès privilégié (PAW) dédiées pour tous les membres du personnel disposant d’un accès privilégié aux systèmes informatiques de votre entreprise. Pour plus d’informations sur le déploiement de PAW, consultez l’article [Stations de travail à accès privilégié](privileged-access-workstations.md#paw-phased-implementation).

### <a name="3-just-in-time-privileges"></a>3. Privilèges juste-à-temps

Pour réduire la durée d’exposition des privilèges et augmenter la visibilité de leur utilisation, octroyez des privilèges juste-à-temps (JIT) à l’aide d’une solution appropriée, comme celles ci-dessous ou d’autres solutions tierces :

* Pour les services de domaine Active Directory (AD DS), utilisez la fonctionnalité [Privileged Access Manager (PAM)](/microsoft-identity-manager/pam/privileged-identity-management-for-active-directory-domain-services) de Microsoft Identity Manager (MIM).
* Pour Azure Active Directory, utilisez la fonctionnalité [Azure AD Privileged Identity Management (PIM)](/azure/active-directory/privileged-identity-management/pim-deployment-plan).

### <a name="4-enable-windows-defender-credential-guard"></a>4. Activer Windows Defender Credential Guard

L’activation de Credential Guard permet de protéger les hachages de mot de passe NTLM, les tickets TGT (Ticket Granting Ticket) Kerberos. et les informations d’identification stockées par les applications en tant qu’informations d’identification de domaine. Cette fonctionnalité permet d’éviter les attaques par vol d’informations d’identification, telles que Pass-The-hash ou Pass-the-ticket, en rendant la navigation dans l’environnement à l’aide d’informations d’identification volées plus difficile. Pour plus d’informations sur le fonctionnement de Credential Guard et son déploiement, consultez l’article [Protéger les informations d’identification de domaine dérivé avec Windows Defender Credential Guard](/windows/security/identity-protection/credential-guard/credential-guard).

### <a name="5-leaked-credentials-reporting"></a>5. Rapports sur les informations d’identification divulguées

« Microsoft analyse chaque jour plus de 6,5 billions de signaux afin de détecter les menaces émergentes et de protéger les clients » –[Microsoft en chiffres](https://news.microsoft.com/bythenumbers/cyber-attacks)

Activez Microsoft Azure AD Identity Protection pour signaler les utilisateurs dont les informations d’identification ont été divulguées afin de pouvoir y remédier. [Azure AD Identity Protection](/azure/active-directory/identity-protection/index) peut être utilisé pour aider votre entreprise à protéger les environnements cloud et hybrides contre les menaces.

### <a name="6-azure-atp-lateral-movement-paths"></a>6. Chemins de mouvement latéral d’Azure ATP

Assurez-vous que les titulaires de compte d’accès privilégié utilisent leur PAW uniquement à des fins d’administration afin qu’un compte non privilégié compromis ne puisse pas accéder à un compte privilégié via des attaques de vol d’informations d’identification, telles que Pass-The-hash ou Pass-the-ticket. [Les chemins de mouvement latéral d’Azure ATP (LMP)](/azure-advanced-threat-protection/use-case-lateral-movement-path) fournissent des rapports faciles à comprendre pour détecter quels comptes privilégiés peuvent faire face à des risques de compromission.

## <a name="phase-3-security-improvement-and-sustainment"></a>Phase 3 : Amélioration et maintien de la sécurité

La phase 3 de la feuille de route s’appuie sur les étapes des phases 1 et 2 afin de renforcer votre posture de sécurité. Ce diagramme est une représentation visuelle de la phase 3 :

![Phase 3 : 1. Examiner RBAC, 2. Réduire les surfaces d’attaques, 3. Intégrer les journaux avec SEIM, 4. Automatisation d’informations d’identification divulguées](../media/securing-privileged-access/PAW_LP_Fig8.JPG)

Ces fonctionnalités s’appuient sur les étapes des phases précédentes et font évoluer vos défenses vers une attitude plus proactive. Cette phase n’a pas de chronologie spécifique et peut prendre plus de temps à mettre en œuvre en fonction de votre organisation.

### <a name="1-review-role-based-access-control"></a>1. Examiner le contrôle d’accès basé sur les rôles

À l’aide du modèle à trois niveaux décrit dans l’article [Modèle de niveau administratif Active Directory](securing-privileged-access-reference-material.md), examinez et vérifiez que les administrateurs de niveau inférieur n’ont pas d’accès administratif à des ressources de niveau supérieur (appartenances aux groupes, ACL sur les comptes d’utilisateur, etc.).

### <a name="2-reduce-attack-surfaces"></a>2. Réduire les surfaces d’attaques

Renforcer vos charges de travail d’identité, y compris les domaines, les contrôleurs de domaine, ADFS et les Azure AD Connect, car la compromission de l’un de ces systèmes peut entraîner la compromission d’autres systèmes de votre entreprise. Les articles [Réduire la surface d’attaque d’Active Directory](../ad-ds/plan/security-best-practices/reducing-the-active-directory-attack-surface.md) et [Sécuriser votre infrastructure d’identité en 5 étapes](/azure/security/azure-ad-secure-steps) vous apporte des conseils sur la sécurisation de vos environnements d’identités locaux et hybrides.

### <a name="3-integrate-logs-with-siem"></a>3. Intégrer des journaux avec SIEM

L’intégration de la journalisation dans un outil SIEM centralisé peut aider votre organisation à analyser, détecter et répondre aux événements de sécurité. Les articles [Surveillance des signes de compromission d’Active Directory](../ad-ds/plan/security-best-practices/monitoring-active-directory-for-signs-of-compromise.md) et [Annexe L : Événements à surveiller](../ad-ds/plan/appendix-l--events-to-monitor.md) vous apporte des conseils sur les événements qui devraient être surveillés dans votre environnement.

Cela fait partie du plan supplémentaire, car l’agrégation, la création et le paramétrage des alertes dans une gestion des informations et des événements de sécurité (SIEM) requièrent des analystes qualifiés (contrairement à Azure ATP pour le plan de 30 jours, qui comprend des alertes prêtes à l’emploi)

### <a name="4-leaked-credentials---force-password-reset"></a>4. Informations d’identification divulguées – Forcer la réinitialisation du mot de passe

Continuez à améliorer votre posture de sécurité en permettant à Azure AD Identity Protection de forcer automatiquement les réinitialisations de mot de passe lorsqu’ils sont susceptibles d’avoir été compromis. Les instructions de l’article [Utiliser des événements à risque pour déclencher une authentification multifacteur et des modifications de mot de passe](/azure/active-directory/authentication/tutorial-risk-based-sspr-mfa) vous explique comment l’activer à l’aide d’une stratégie d’accès conditionnel.

## <a name="am-i-done"></a>Ai-je terminé ?

Pour faire court, la réponse est non.

Les personnes malintentionnées ne s’arrêtent jamais et vous devez donc être constamment vigilant. Cette feuille de route peut aider votre entreprise à se protéger contre les menaces actuellement connues, car les pirates informatiques sont en constante évolution. Nous vous recommandons de considérer la sécurité comme un processus continu visant à augmenter le coût et à réduire le taux de réussite des adversaires qui ciblent votre environnement.

Bien qu’il ne s’agisse pas de la seule partie du programme de sécurité de votre entreprise, la sécurisation de l’accès privilégié est un élément essentiel de votre stratégie de sécurité.
