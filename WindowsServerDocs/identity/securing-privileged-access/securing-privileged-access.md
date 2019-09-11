---
title: Sécurisation de l’accès privilégié
description: Approche progressive de la sécurisation de l’accès privilégié
ms.prod: windows-server-threshold
ms.topic: conceptual
ms.assetid: f5dec0c2-06fe-4c91-9bdc-67cc6a3ede60
ms.date: 02/25/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: mas
ms.openlocfilehash: 9080f7209660b225d795219127a71ece479855d1
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70869234"
---
# <a name="securing-privileged-access"></a>Sécurisation de l’accès privilégié

>S'applique à : Windows Server

La sécurisation de l’accès privilégié est une première étape essentielle pour établir des garanties de sécurité pour les ressources d’entreprise dans une organisation moderne. La sécurité de la plupart ou de la totalité des actifs de l’entreprise dans une organisation informatique dépend de l’intégrité des comptes privilégiés utilisés pour administrer, gérer et développer. Les cyber-attaquants ciblent souvent ces comptes et d’autres éléments d’accès privilégié pour accéder aux données et aux systèmes à l’aide d’attaques de vol d’informations d’identification telles que [Pass-The-hash et Pass-the-ticket](https://www.microsoft.com/pth).

La protection de l’accès privilégié contre les adversaires déterminés vous oblige à adopter une approche complète et réfléchie pour isoler ces systèmes des risques.

## <a name="what-are-privileged-accounts"></a>Qu’est-ce qu’un compte privilégié ?

Avant de parler de la manière de les sécuriser, vous pouvez définir des comptes privilégiés.

Les comptes privilégiés, tels que les administrateurs de Active Directory Domain Services disposent d’un accès direct ou indirect à la plupart ou à la totalité des ressources d’une organisation informatique, ce qui rend la compromission de ces comptes un risque important pour l’entreprise.

## <a name="why-securing-privileged-access-is-important"></a>Pourquoi la sécurisation de l’accès privilégié est-elle importante ?

Les cybercriminels se concentrent sur l’accès privilégié aux systèmes comme Active Directory (AD) pour accéder rapidement à toutes les données ciblées des organisations. Les approches de sécurité traditionnelles se sont concentrées sur le réseau et les pare-feu comme périmètre de sécurité principal, mais l’efficacité de la sécurité réseau a été considérablement réduite par deux tendances :

* Les organisations hébergent des données et des ressources en dehors de la limite réseau traditionnelle sur les ordinateurs portables, les appareils tels que les téléphones mobiles et les tablettes, les services Cloud et les appareils mobiles (BYOD)
* Les adversaires ont fait montre d’une capacité constante et suivie à obtenir des accès sur des stations de travail situées dans les limites du réseau par le biais du hameçonnage et d’autres attaques web et par e-mail.

Ces facteurs nécessitent la création d’un périmètre de sécurité moderne à partir des contrôles d’identité et d’autorisation, en plus de la stratégie de périmètre de réseau classique. Ici, un périmètre de sécurité est défini comme un ensemble de contrôles cohérents entre les ressources et leurs menaces. Les comptes privilégiés sont effectivement en mesure de contrôler ce nouveau périmètre de sécurité. il est donc essentiel de protéger l’accès privilégié.

![Diagramme montrant la couche d’identité d’une organisation](../media/securing-privileged-access/PAW_LP_Fig2.JPG)

Une personne malveillante qui gagne le contrôle d’un compte administratif peut utiliser ces privilèges pour augmenter son impact dans l’organisation cible, comme illustré ci-dessous :

![Diagramme montrant comment un adversaire qui prend le contrôle d’un compte administratif peut utiliser ces privilèges pour en tirer profit au détriment de l’organisation cible](../media/securing-privileged-access/PAW_LP_Fig3.JPG)

L’illustration ci-dessous représente deux chemins d’accès :

* Un chemin « bleu » dans lequel un compte d’utilisateur standard est utilisé pour l’accès non privilégié aux ressources, telles que la messagerie électronique et la navigation Web, ainsi que le travail quotidien.

   > [!NOTE]
   > Les éléments de chemin d’accès bleus décrits plus loin sur indiquent des protections environnementales étendues qui s’étendent au-delà des comptes d’administration.

* Chemin « rouge » dans lequel l’accès privilégié se produit sur un appareil renforcé pour réduire le risque de hameçonnage et d’autres attaques par le Web et par courrier électronique.

![Diagramme montrant le « chemin d’accès » séparé pour l’administration que la feuille de route établit pour isoler les tâches d’accès privilégié des tâches d’utilisateur standard à risque élevé telles que la navigation Web et l’accès à la messagerie](../media/securing-privileged-access/PAW_LP_Fig4.JPG)

## <a name="securing-privileged-access-roadmap"></a>Guide de sécurisation de l’accès privilégié

La feuille de route est conçue pour optimiser l’utilisation des technologies Microsoft que vous avez déjà déployées, tirer parti des technologies du Cloud pour améliorer la sécurité et intégrer les outils de sécurité tiers que vous avez peut-être déjà déployés.

La feuille de route des recommandations de Microsoft est divisée en trois phases :

* [Phase 1 : 30 premiers jours]()
   * Gagne rapidement avec un impact positif significatif.
* [Phase 2 : 90 jours]()
   * Améliorations incrémentielles significatives.
* [Phase 3 : Normale]()
   * Amélioration de la sécurité et entretien.

La feuille de route est hiérarchisée de sorte à planifier les implémentations les plus efficaces et les plus rapides en premier, conformément à notre expérience de ces attaques et de l’implémentation des solutions. 

Microsoft vous recommande de suivre cette feuille de route pour sécuriser l’accès privilégié contre ses adversaires déterminés. Vous pouvez ajuster cette feuille de route pour l’adapter à vos capacités existantes et aux besoins spécifiques de votre organisation.

> [!NOTE]
> La sécurisation de l’accès privilégié exige un large éventail d’éléments, notamment des composants techniques (défenses d’hôte, protections de comptes, gestion des identités, etc.), ainsi que des modifications à traiter et des pratiques et connaissances administratives. La chronologie de la feuille de route est approximative et repose sur notre expérience des implémentations de nos clients. La durée varie dans votre organisation en fonction de la complexité de votre environnement et de vos processus de gestion des modifications.

## <a name="phase-1-quick-wins-with-minimal-operational-complexity"></a>Phase 1 : Victoires rapides avec une complexité opérationnelle minimale

La phase 1 de la feuille de route est axée sur l’atténuation rapide des techniques d’attaque les plus fréquemment utilisées pour le vol et l’abus des informations d’identification. La phase 1 est conçue pour être implémentée en 30 jours environ et est décrite dans ce diagramme :

![Schéma de la phase 1 : 1. Compte d’administrateur et d’utilisateur distinct, 2. Mots de passe d’administrateur local juste-à-temps, 3. Station de travail d’administration, étape 1, 4. Détection des attaques d’identité](../media/securing-privileged-access/PAW_LP_Fig6.JPG)

### <a name="1-separate-accounts"></a>1. Comptes distincts

Pour aider à séparer les risques Internet (attaques par hameçonnage, navigation Web) des comptes d’accès privilégiés, créez un compte dédié pour tout le personnel disposant d’un accès privilégié. Les administrateurs ne doivent pas naviguer sur le Web, vérifier leur courrier électronique et effectuer des tâches de productivité de jour à jour avec des comptes à privilèges élevés. Pour plus d’informations, consultez la section [comptes d’administration distincts](securing-privileged-access-reference-material.md#separate-administrative-accounts) du document de référence.

Suivez les instructions de l’article [gérer les comptes d’accès d’urgence dans Azure ad](/azure/active-directory/users-groups-roles/directory-emergency-access) pour créer au moins deux comptes d’accès d’urgence, avec des droits d’administrateur attribués de manière permanente, dans vos environnements AD et Azure ad locaux. Ces comptes sont uniquement destinés à une utilisation lorsque les comptes d’administrateur traditionnels ne peuvent pas effectuer une tâche requise, par exemple dans le cas d’un incident.

### <a name="2-just-in-time-local-admin-passwords"></a>2. Mots de passe d’administrateur local juste-à-temps

Pour réduire le risque qu’un adversaire ne vole un hachage de mot de passe de compte d’administrateur local à partir de la base de données SAM locale et qu’il abuse d’autres ordinateurs, les organisations doivent s’assurer que chaque ordinateur dispose d’un mot de passe d’administrateur local unique. L’outil de solution de mot de passe administrateur local peut configurer des mots de passe aléatoires uniques sur chaque station de travail et chaque serveur les stocke dans Active Directory (AD) protégé par une liste de contrôle d’accès. Seuls les utilisateurs autorisés éligibles peuvent lire ou demander la réinitialisation de ces mots de passe de compte d’administrateur local. Vous pouvez obtenir le LAPS de travail à utiliser sur les stations de travail et les serveurs à partir [du centre de téléchargement Microsoft](http://Aka.ms/LAPS).

Vous trouverez des conseils supplémentaires pour l’utilisation d’un environnement avec des pattes et des pattes dans la section [normes opérationnelles basées sur le principe de source propre](securing-privileged-access-reference-material.md#operational-standards-based-on-clean-source-principle).

### <a name="3-administrative-workstations"></a>3. Stations de travail d’administration

En guise de mesure de sécurité initiale pour les utilisateurs disposant de privilèges d’administration Active Directory locaux Azure Active Directory et traditionnels, assurez-vous qu’ils utilisent des appareils Windows 10 configurés avec les [normes pour un appareil Windows 10 hautement sécurisé. ](/windows-hardware/design/device-experiences/oem-highly-secure). Les comptes administrateur privilégiés ne doivent pas être membres du groupe Administrateurs local des stations de travail d’administration.  L’élévation des privilèges via l’Access Control utilisateur (UAC) peut être utilisée lorsque des modifications de configuration apportées aux stations de travail sont requises.  En outre, la ligne de base de sécurité Windows 10 doit être appliquée aux stations de travail pour renforcer la sécurité de l’appareil.

### <a name="4-identity-attack-detection"></a>4. Détection des attaques d’identité

[Azure-protection avancée contre les menaces (ATP)](/azure-advanced-threat-protection/what-is-atp) est une solution de sécurité basée sur le Cloud qui identifie, détecte et vous aide à étudier les menaces avancées, les identités compromises et les actions malveillantes qui sont dirigées vers votre Active Directory local environnement.

## <a name="phase-2-significant-incremental-improvements"></a>Phase 2: Améliorations incrémentielles significatives

La phase 2 s’appuie sur le travail effectué au cours de la phase 1 et est conçue pour être effectuée dans environ 90 jours. Les étapes de cette phase sont illustrées dans ce diagramme :

![Schéma de la phase 2 : 1. Windows Hello entreprise/MFA, 2. Déploiement de la patte, 3. Privilèges juste-à-temps, 4. Credential Guard, 5. Informations d’identification divulguées, 6. Détection de vulnérabilité de mouvement latéral](../media/securing-privileged-access/PAW_LP_Fig7.JPG)

### <a name="1-require-windows-hello-for-business-and-mfa"></a>1. Exiger Windows Hello entreprise et MFA

Les administrateurs peuvent tirer parti de la facilité d’utilisation associée à Windows Hello entreprise. Les administrateurs peuvent remplacer leurs mots de passe complexes par une authentification forte à deux facteurs sur leurs PC. Une personne malveillante doit avoir à la fois l’appareil et les informations biométriques ou le code confidentiel, il est bien plus difficile d’obtenir un accès sans la connaissance de l’employé. Pour plus d’informations sur Windows Hello entreprise et le chemin d’accès à déployer, consultez l’article [vue d’ensemble de Windows Hello entreprise](/windows/security/identity-protection/hello-for-business/hello-overview) .

Activez Multi-Factor Authentication (MFA) pour vos comptes d’administrateur dans Azure AD à l’aide d’Azure MFA. Activez au minimum la [stratégie d’accès conditionnel de base de référence](/azure/active-directory/conditional-access/baseline-protection#require-mfa-for-admins) pour plus d’informations sur Azure Multi-Factor Authentication, consultez l’article [déployer des multi-Factor Authentication Azure basés](/azure/active-directory/authentication/howto-mfa-getstarted) sur le Cloud

### <a name="2-deploy-paw-to-all-privileged-identity-access-account-holders"></a>2. Déployer la patte sur tous les titulaires de comptes privilégiés d’accès aux identités

En poursuivant le processus de séparation des comptes privilégiés des menaces détectées dans les courriers électroniques, la navigation Web et d’autres tâches non administratives, vous devez implémenter des stations de travail à accès privilégié dédiées pour tous les membres du personnel disposant d’un accès privilégié à votre systèmes informatiques de l’organisation. Vous trouverez des conseils supplémentaires sur le déploiement de la patte dans l’article [stations de travail à accès privilégié](privileged-access-workstations.md#paw-phased-implementation).

### <a name="3-just-in-time-privileges"></a>3. Privilèges juste-à-temps

Pour réduire le temps d’exposition des privilèges et augmenter la visibilité de leur utilisation, fournissez des privilèges juste-à-temps (JIT) à l’aide d’une solution appropriée, telle que celles ci-dessous ou d’autres solutions tierces :

* Pour les services de domaine Active Directory (AD DS), utilisez la fonctionnalité [Privileged Access Manager (PAM)](/microsoft-identity-manager/pam/privileged-identity-management-for-active-directory-domain-services) de Microsoft Identity Manager (MIM).
* Pour Azure Active Directory, utilisez la fonctionnalité [Azure AD Privileged Identity Management (PIM)](/azure/active-directory/privileged-identity-management/pim-deployment-plan).

### <a name="4-enable-windows-defender-credential-guard"></a>4. Activer Windows Defender Credential Guard

L’activation d’Credential Guard permet de protéger les hachages de mot de passe NTLM, les tickets d’accord de ticket Kerberos et les informations d’identification stockées par les applications en tant qu’informations d’identification de domaine. Cette fonctionnalité permet d’éviter les attaques par vol d’informations d’identification, telles que Pass-The-hash ou Pass-the-ticket, en renforçant la difficulté de pivoter dans l’environnement à l’aide d’informations d’identification volées. Pour plus d’informations sur le fonctionnement de Credential Guard et son déploiement, consultez l’article [protéger les informations d’identification de domaine dérivé avec Windows Defender Credential Guard](/windows/security/identity-protection/credential-guard/credential-guard).

### <a name="5-leaked-credentials-reporting"></a>5. Rapport des informations d’identification divulguées

« Tous les jours, Microsoft analyse plus de 6 500 000 000 000 signaux afin d’identifier les menaces émergentes et de protéger les clients »- [Microsoft par les chiffres](https://news.microsoft.com/bythenumbers/cyber-attacks)

Activez Microsoft Azure AD Identity Protection pour signaler aux utilisateurs des informations d’identification divulguées afin que vous puissiez les corriger. [Azure ad Identity Protection](/azure/active-directory/identity-protection/index) peut être exploité pour aider votre organisation à protéger les environnements Cloud et hybride des menaces.

### <a name="6-azure-atp-lateral-movement-paths"></a>6. Azure ATP les chemins de mouvement latéral

Assurez-vous que les détenteurs de compte d’accès privilégié utilisent leur patte pour l’administration uniquement afin qu’un compte compromis non privilégié ne puisse pas accéder à un compte privilégié via des attaques par vol d’informations d’identification, telles que Pass-The-hash ou Pass-the-ticket. [Azure ATP chemins de mouvement latéral (LMPs)](/azure-advanced-threat-protection/use-case-lateral-movement-path) fournit des rapports faciles à comprendre pour identifier où les comptes privilégiés peuvent être ouverts pour être compromis.

## <a name="phase-3-security-improvement-and-sustainment"></a>Phase 3: Amélioration de la sécurité et entretien

La phase 3 de la feuille de route s’appuie sur les étapes prises dans les phases 1 et 2 pour renforcer votre position de sécurité. La phase 3 est représentée visuellement dans ce diagramme :

![Phase 3: 1. Passez en revue RBAC, 2. Réduire les surfaces d’attaque, 3. Intégrez les journaux avec SEIM, 4. Automatisation des informations d’identification divulguées](../media/securing-privileged-access/PAW_LP_Fig8.JPG)

Ces fonctionnalités s’appuient sur les étapes des phases précédentes et déplacent vos défenses dans une position plus proactive. Cette phase n’a pas de chronologie spécifique et peut mettre plus de temps à s’implémenter en fonction de votre organisation.

### <a name="1-review-role-based-access-control"></a>1. Vérifier le contrôle d’accès en fonction du rôle

À l’aide du modèle à trois niveaux décrit dans l’article [Active Directory modèle de niveau administratif](securing-privileged-access-reference-material.md), examinez et assurez-vous que les administrateurs de niveau inférieur ne disposent pas d’un accès administratif aux ressources de niveau supérieur (appartenances aux groupes, ACL sur les comptes d’utilisateur, etc.).

### <a name="2-reduce-attack-surfaces"></a>2. Réduire les surfaces d’attaque

Renforcer vos charges de travail d’identité, y compris les domaines, les contrôleurs de domaine, ADFS et les Azure AD Connect comme compromettre l’un de ces systèmes pourrait compromettre les autres systèmes de votre organisation. Les articles qui [réduisent la surface d’attaque Active Directory](../ad-ds/plan/security-best-practices/reducing-the-active-directory-attack-surface.md) et [cinq étapes de sécurisation de votre infrastructure d’identité](/azure/security/azure-ad-secure-steps) fournissent des conseils pour sécuriser vos environnements d’identités locaux et hybrides.

### <a name="3-integrate-logs-with-siem"></a>3. Intégrer les journaux avec SIEM

L’intégration de la journalisation dans un outil SIEM centralisé peut aider votre organisation à analyser, détecter et répondre aux événements de sécurité. Les articles [sur la surveillance Active Directory pour les signes de compromission](../ad-ds/plan/security-best-practices/monitoring-active-directory-for-signs-of-compromise.md) et [l’annexe L : Les événements à](../ad-ds/plan/appendix-l--events-to-monitor.md) surveiller fournissent des conseils sur les événements qui doivent être surveillés dans votre environnement.

Il s’agit d’une partie du plan le plus approprié, car l’agrégation, la création et le paramétrage des alertes dans une gestion des informations et des événements de sécurité (SIEM) requièrent des analystes qualifiés (contrairement à Azure ATP dans le plan de 30 jours, qui comprend des alertes prêtes à l’emploi)

### <a name="4-leaked-credentials---force-password-reset"></a>4. Informations d’identification divulguées-forcer la réinitialisation du mot de passe

Continuez à améliorer votre situation de sécurité en permettant à Azure AD Identity Protection de forcer automatiquement les réinitialisations de mot de passe lorsque les mots de passe sont suspectés d’être compromis. Les instructions de l’article [utiliser des événements à risque pour déclencher des multi-Factor Authentication et des modifications de mot de passe](/azure/active-directory/authentication/tutorial-risk-based-sspr-mfa) expliquent comment les activer à l’aide d’une stratégie d’accès conditionnel.

## <a name="am-i-done"></a>Ai-je terminé ?

La réponse brève est non.

Les personnes malintentionnées ne se bloquent jamais. vous ne pouvez donc pas le faire. Cette feuille de route peut aider votre organisation à se protéger contre les menaces actuellement connues, car les attaquants évoluent et changent constamment. Nous vous recommandons de consulter la sécurité dans le cadre d’un processus continu visant à augmenter le coût et à réduire le taux de réussite des adversaires ciblant votre environnement.

Bien qu’il ne s’agisse pas de la seule partie du programme de sécurité de votre organisation, la sécurisation de l’accès privilégié est un composant essentiel de votre stratégie de sécurité.
