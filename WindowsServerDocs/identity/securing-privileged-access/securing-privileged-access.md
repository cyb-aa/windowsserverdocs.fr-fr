---
title: Sécurisation de l’accès privilégié
description: Approche progressive pour la sécurisation de l’accès privilégié
ms.prod: windows-server-threshold
ms.topic: conceptual
ms.assetid: f5dec0c2-06fe-4c91-9bdc-67cc6a3ede60
ms.date: 02/25/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: mas
ms.openlocfilehash: 0d54a94d51a4d1e0a1d28f78ec39bf16bc3d9100
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822010"
---
# <a name="securing-privileged-access"></a>Sécurisation de l’accès privilégié

>S'applique à : Windows Server

La sécurisation de l’accès privilégié est une première étape essentielle pour établir des garanties de sécurité pour les ressources d’entreprise dans une organisation moderne. La sécurité de la plupart ou toutes les ressources d’entreprise dans une organisation informatique dépend de l’intégrité des comptes privilégiés utilisé pour administrer, gérer et développer. Les pirates informatiques ciblent souvent ces comptes et autres éléments de l’accès privilégié pour accéder aux données et les systèmes à l’aide de vol de ces informations d’identification comme [Pass-the-Hash et Pass-the-Ticket](https://www.microsoft.com/pth).

Protection de l’accès privilégié contre ses adversaires déterminés vous oblige à adopter une approche complète et réfléchie pour isoler ces systèmes des risques.

## <a name="what-are-privileged-accounts"></a>Quelles sont les comptes privilégiés ?

Avant d’expliquer comment sécuriser leur permet de définir des comptes privilégiés.

Comptes privilégiés, tels que les administrateurs des Services de domaine Active Directory ont un accès direct ou indirect pour la plupart ou toutes les ressources dans une organisation informatique, effectue une compromission de ces comptes un risque significatif pour l’entreprise.

## <a name="why-securing-privileged-access-is-important"></a>Pourquoi la sécurisation de l’accès privilégié est-elle importante ?

Les cybercriminels se concentrent sur l’accès privilégié aux systèmes tels que Active Directory (AD) pour accéder rapidement à l’ensemble d’une organisation ciblée des données. Approches de sécurité traditionnelles se concentrent sur les pare-feu et le réseau en tant que périmètre de sécurité principal, mais l’efficacité de la sécurité réseau a considérablement diminué suite à deux tendances :

* Les organisations hébergent des données et ressources en dehors des limites du réseau traditionnel sur enterprise mobile PC, périphériques tels que des téléphones portables et tablettes, services cloud et apportent votre propre appareil (BYOD)
* Les adversaires ont fait montre d’une capacité constante et suivie à obtenir des accès sur des stations de travail situées dans les limites du réseau par le biais du hameçonnage et d’autres attaques web et par e-mail.

Ces facteurs nécessitent la création d’un périmètre de sécurité modernes en dehors de l’authentification et l’autorisation des contrôles d’identité en plus de la stratégie du périmètre réseau traditionnel. Ici un périmètre de sécurité est défini comme un ensemble cohérent de contrôles entre des ressources et les menaces pour leur. Comptes privilégiés sont effectivement dans le contrôle de ce nouveau périmètre de sécurité, il est donc essentiel de protéger l’accès privilégié.

![Diagramme montrant la couche d’identité d’une organisation](../media/securing-privileged-access/PAW_LP_Fig2.JPG)

Un attaquant qui prend le contrôle d’un compte administratif peut utiliser ces privilèges pour augmenter leur impact dans l’organisation cible, comme illustré ci-dessous :

![Diagramme montrant comment un adversaire qui prend le contrôle d’un compte administratif peut utiliser ces privilèges pour en tirer profit au détriment de l’organisation cible](../media/securing-privileged-access/PAW_LP_Fig3.JPG)

L’illustration ci-dessous montre deux chemins d’accès :

* Un chemin d’accès « bleu », où un compte d’utilisateur standard est utilisé pour l’accès non privilégié aux ressources telles que la messagerie et de navigation sur le web et de journée de travail sont terminées.

   > [!NOTE]
   > Les éléments de chemin de couleur bleue décrites par la suite indiquent large protections environnementales qui s’étendent au-delà les comptes d’administration.

* Un chemin d’accès « rouge » où un accès privilégié se produit sur un appareil renforcé pour réduire les risques du phishing et autres attaques web et par e-mail.

![Diagramme montrant le « chemin » distinct d’administration que la feuille de route établit pour isoler les tâches à accès privilégié à haut risque tâches d’utilisateur standard comme la navigation sur Internet et l’accès aux e-mails](../media/securing-privileged-access/PAW_LP_Fig4.JPG)

## <a name="securing-privileged-access-roadmap"></a>Sécurisation de feuille de route pour un accès privilégié

La feuille de route est conçue pour maximiser l’utilisation de technologies Microsoft que vous avez déjà déployé, tirez parti des technologies de cloud pour améliorer la sécurité et intégrer les outils de sécurité tiers 3e que déjà peut-être avez-vous déployé.

La feuille de route des recommandations de Microsoft est divisé en 3 phases :

* [Phase 1 : 30 premiers jours]()
   * Gains rapides avec un impact positif significatif.
* [Phase 2 : 90 jours]()
   * Améliorations incrémentielles significatives.
* [Phase 3 : En cours]()
   * Amélioration de la sécurité et sustainment.

La feuille de route est hiérarchisée de sorte à planifier les implémentations les plus efficaces et les plus rapides en premier, conformément à notre expérience de ces attaques et de l’implémentation des solutions. 

Microsoft vous recommande de suivre cette feuille de route pour sécuriser l’accès privilégié contre ses adversaires déterminés. Vous pouvez ajuster cette feuille de route pour l’adapter à vos capacités existantes et aux besoins spécifiques de votre organisation.

> [!NOTE]
> La sécurisation de l’accès privilégié exige un large éventail d’éléments, notamment des composants techniques (défenses d’hôte, protections de comptes, gestion des identités, etc.), ainsi que des modifications à traiter et des pratiques et connaissances administratives. La chronologie de la feuille de route est approximative et repose sur notre expérience des implémentations de nos clients. La durée varie dans votre organisation en fonction de la complexité de votre environnement et de vos processus de gestion des modifications.

## <a name="phase-1-quick-wins-with-minimal-operational-complexity"></a>Phase 1 : Gains rapides avec une complexité opérationnelle minimale

Phase 1 de la feuille de route vise à atténuer rapidement les techniques d’attaque les plus fréquemment utilisées de vol d’informations d’identification et les abus. Phase 1 est conçue pour être implémentée dans un délai de 30 jours et est représentée dans ce diagramme :

![Diagramme de la phase 1 : 1. Administrateur et utilisateur compte distinct, 2. Juste à temps administrateur local les mots de passe, 3. Administrateur station de travail étape 1, 4. Détection des attaques identité](../media/securing-privileged-access/PAW_LP_Fig6.JPG)

### <a name="1-separate-accounts"></a>1. Comptes distincts

Pour aider à séparer les risques internet (attaques par hameçonnage, navigation web) à partir de privilège accéder aux comptes, créez un compte dédié pour tous les employés avec un accès privilégié. Les administrateurs ne doivent pas être navigation sur le web, la vérification de leur courrier électronique et effectuer des tâches quotidiennes productivité avec des comptes à privilèges élevés. Vous trouverez plus d’informations dans la section [séparer les comptes d’administration](securing-privileged-access-reference-material.md#separate-administrative-accounts) du document de référence.

Suivez les instructions dans l’article [gérer les comptes d’accès d’urgence dans Azure AD](/azure/active-directory/users-groups-roles/directory-emergency-access) pour créer au moins deux accès d’urgence des comptes, attribués définitivement en tant qu’administrateur, dans votre sur site AD et Azure AD environnements . Ces comptes doivent uniquement être utilisés lorsque les comptes d’administrateur traditionnels ne parvenez pas à effectuer une tâche requise comme dans un le cas d’incident.

### <a name="2-just-in-time-local-admin-passwords"></a>2. Uniquement dans les mots de passe administrateur local uniques

Pour atténuer le risque d’un adversaire vol d’un hachage de mot de passe du compte administrateur local à partir de la base de données SAM locale et son utilisation pour attaquer d’autres ordinateurs, les organisations doivent vérifier que chaque machine dispose d’un mot de passe d’administrateur local uniques. L’outil de la Solution de mot de passe d’administrateur Local (LAPS) permettre configurer des mots de passe aléatoires uniques sur chaque station de travail et serveur de les stocker dans Active Directory (AD) protégés par une liste ACL. Seuls les utilisateurs autorisés éligibles capable de lire ou de demander la réinitialisation de ces mots de passe du compte administrateur local. Vous pouvez obtenir les LAPS pour une utilisation sur les stations de travail et des serveurs de [du Microsoft Download Center](http://Aka.ms/LAPS).

Vous trouverez des conseils supplémentaires pour l’exploitation d’un environnement avec LAPS et des stations dans la section [normes opérationnelles basées sur le principe de source propre](securing-privileged-access-reference-material.md#operational-standards-based-on-clean-source-principle).

### <a name="3-administrative-workstations"></a>3. Stations de travail administratives

Mesure de sécurité initiaux pour les utilisateurs avec Azure Active Directory et des privilèges d’administrateur local traditionnel à Active Directory, assurez-vous qu’ils utilisent des appareils Windows 10 configurés avec le [normes pour un Windows hautement sécurisée APPAREIL 10](/windows-hardware/design/device-experiences/oem-highly-secure). 

### <a name="4-identity-attack-detection"></a>4. Détection des attaques identité

[Azure Advanced Threat Protection (ATP)](/azure-advanced-threat-protection/what-is-atp) est une solution de sécurité basée sur le cloud qui identifie, détecte et vous permet d’examiner les menaces avancées, les identités compromises et les actions utilisateur interne malveillant dirigées vers votre actif en local Environnement de répertoire.

## <a name="phase-2-significant-incremental-improvements"></a>Phase 2 : Améliorations significatives incrémentielles

Phase 2 s’appuie sur le travail effectué dans la phase 1 et est conçue pour être terminés en 90 jours environ. Les étapes de cette phase sont illustrées dans ce diagramme :

![Diagramme de la phase 2 : 1. Windows Hello entreprise / MFA, 2. Déploiement PAW, 3. Juste à temps des privilèges, 4. Credential Guard, 5. Informations d’identification volées, 6. Détection des vulnérabilités de mouvement latéral](../media/securing-privileged-access/PAW_LP_Fig7.JPG)

### <a name="1-require-windows-hello-for-business-and-mfa"></a>1. Nécessitent Windows Hello pour l’entreprise et l’authentification Multifacteur

Les administrateurs peuvent bénéficier de la facilité d’utilisation associée à Windows Hello entreprise. Administrateurs peuvent remplacer leurs mots de passe complexes avec une authentification forte à deux facteurs sur leurs PC. Un attaquant doit disposer de l’appareil et les informations biométriques ou un code PIN, il est beaucoup plus difficile d’accéder sans avoir connaissance de l’employé. Vous trouverez plus d’informations sur Windows Hello entreprise et le chemin d’accès de déploiement dans l’article [Windows Hello pour une vue d’ensemble de l’entreprise](/windows/security/identity-protection/hello-for-business/hello-overview)

Activer l’authentification multifacteur (MFA) pour vos comptes administrateur dans Azure AD à l’aide d’Azure MFA. Au minimum activer le [stratégie d’accès conditionnel de protection de ligne de base](/azure/active-directory/conditional-access/baseline-protection#require-mfa-for-admins) vous trouverez plus d’informations sur l’authentification multifacteur Azure dans l’article [déployer basé sur le cloud Azure multi-Factor Authentication](/azure/active-directory/authentication/howto-mfa-getstarted)

### <a name="2-deploy-paw-to-all-privileged-identity-access-account-holders"></a>2. Déployer les PAW à tous les propriétaires de comptes d’accès PIM

Continuer le processus de séparation des comptes privilégiés contre les menaces trouvées dans le courrier électronique, navigation web et d’autres tâches non administratives, vous devez implémenter des stations de travail dédié d’accès privilégié (PAW) pour tous les employés avec un accès privilégié à votre systèmes d’information de l’organisation. Vous trouverez des conseils supplémentaires pour le déploiement de PAW dans l’article [stations de travail de l’accès privilégié](privileged-access-workstations.md#paw-phased-implementation).

### <a name="3-just-in-time-privileges"></a>3. Juste à des privilèges de temps

Pour réduire le temps d’exposition des privilèges et augmenter la visibilité sur leur utilisation, fournir des privilèges juste à temps (JIT) à l’aide d’une solution appropriée, telles que celles ci-dessous ou autres solutions de tiers :

* Pour les services de domaine Active Directory (AD DS), utilisez la fonctionnalité [Privileged Access Manager (PAM)](/microsoft-identity-manager/pam/privileged-identity-management-for-active-directory-domain-services) de Microsoft Identity Manager (MIM).
* Pour Azure Active Directory, utilisez la fonctionnalité [Azure AD Privileged Identity Management (PIM)](/azure/active-directory/privileged-identity-management/pim-deployment-plan).

### <a name="4-enable-windows-defender-credential-guard"></a>4. Activer Windows Defender Credential Guard

L’activation de Credential Guard permet de protéger les hachages de mot de passe NTLM, Ticket Granting Ticket Kerberos et les informations d’identification stockées par les applications en tant qu’informations d’identification de domaine. Cette fonctionnalité permet d’empêcher les attaques de vol d’informations d’identification, telles que Pass-the-Hash ou Pass-The-Ticket en augmentant la difficulté de glissement dans l’environnement à l’aide des informations d’identification volées. Vous trouverez des informations sur le fonctionnement de Credential Guard et son déploiement dans l’article [protéger dérivés des informations d’identification de domaine avec Windows Defender Credential Guard](/windows/security/identity-protection/credential-guard/credential-guard).

### <a name="5-leaked-credentials-reporting"></a>5. Informations d’identification volées reporting

« Tous les jours, Microsoft analyse les signaux plus 6.5 trillions afin d’identifier les menaces émergentes et protéger les clients » - [par les numéros de Microsoft](https://news.microsoft.com/bythenumbers/cyber-attacks)

Activer Microsoft Azure AD Identity Protection à créer des rapports sur les utilisateurs avec les informations d’identification volées afin que vous pouvez les résoudre. [Azure AD Identity Protection](/azure/active-directory/identity-protection/index) peut être exploitée pour aider votre organisation à protéger les environnements de cloud et hybrides contre les menaces.

### <a name="6-azure-atp-lateral-movement-paths"></a>6. Chemins de mouvement latéral d’ATP Azure

Vérifiez les privilèges d’accès compte titulaires sont à l’aide de leur PAW pour l’administration uniquement, afin que des comptes non privilégiés un compromis ne peut pas accéder à un compte privilégié via des attaques contre le vol d’informations d’identification, telles que Pass-the-Hash ou Pass-The-Ticket. [Azure ATP latéral chemins de mouvement (LMPs)](/azure-advanced-threat-protection/use-case-lateral-movement-path) fournit facile à comprendre la création de rapports pour identifier où les comptes disposant de privilèges peuvent être ouverts pour compromettre.

## <a name="phase-3-security-improvement-and-sustainment"></a>Phase 3 : Sustainment et amélioration de la sécurité

Phase 3 de la feuille de route s’appuie sur les mesures prises dans les étapes 1 et 2 pour renforcer votre posture de sécurité. Phase 3 est une représentation visuelle dans ce diagramme :

![Phase 3 : 1. Passez en revue les RBAC, 2. Réduire les surfaces d’attaque, 3. Intégrer des journaux avec SEIM, 4. Informations d’identification volées automation](../media/securing-privileged-access/PAW_LP_Fig8.JPG)

Ces fonctionnalités seront reposent sur les étapes à partir des phases précédentes et évoluer vos défenses vers une attitude plus proactive. Cette phase n’a aucune chronologie spécifique et peut prendre plus de temps à implémenter en fonction de votre organisation individuelle.

### <a name="1-review-role-based-access-control"></a>1. Passez en revue le contrôle d’accès en fonction du rôle

L’utilisation du modèle à plusieurs niveaux trois décrites dans l’article [modèle de niveau administratif Active Directory](securing-privileged-access-reference-material.md), passez en revue et vérifiez les administrateurs de niveau inférieur n’ont pas d’accès administratif aux ressources de niveau supérieur (appartenances aux groupes, les ACL sur comptes d’utilisateur, etc....).

### <a name="2-reduce-attack-surfaces"></a>2. Réduire les surfaces d’attaque

Renforcer vos charges de travail identité y compris les domaines, les contrôleurs de domaine, AD FS et Azure AD Connect en tant qu’un de ces systèmes compromettre peut entraîner la compromission d’autres systèmes de votre organisation. Les articles [en réduisant la Surface d’attaque Active Directory](../ad-ds/plan/security-best-practices/reducing-the-active-directory-attack-surface.md) et [cinq étapes pour la sécurisation de votre infrastructure d’identité](/azure/security/azure-ad-secure-steps) fournissent des conseils pour protéger vos locaux et les hybrides environnements d’identité.

### <a name="3-integrate-logs-with-siem"></a>3. Intégrer des journaux à SIEM

L’intégration de journalisation dans un outil SIEM centralisé peut aider votre organisation à analyser, détecter et répondre aux événements de sécurité. Les articles [surveillance d’Active Directory pour les signes de compromission](../ad-ds/plan/security-best-practices/monitoring-active-directory-for-signs-of-compromise.md) et [annexe L: Événements à surveiller](../ad-ds/plan/appendix-l--events-to-monitor.md) fournissent des conseils sur les événements qui doivent être surveillés dans votre environnement.

Cela fait partie de l’au-delà planifier qu’agrège, création et réglage des alertes dans une gestion des informations et événements sécurité (SIEM) nécessite des analystes compétents (contrairement à Azure ATP dans le plan de 30 jours qui comprend en dehors de la zone d’alertes)

### <a name="4-leaked-credentials---force-password-reset"></a>4. Informations d’identification volées - réinitialisation de mot de passe de Force

Continuer à améliorer votre posture de sécurité en activant Azure AD Identity Protection forcer automatiquement les réinitialisations de mot de passe lorsque les mots de passe sont suspectées de compromission. Les instructions figurant dans l’article [utiliser des événements à risque déclencheur multi-Factor Authentication et des modifications de mot de passe](/azure/active-directory/authentication/tutorial-risk-based-sspr-mfa) explique comment activer cette option à l’aide d’une stratégie d’accès conditionnel.

## <a name="am-i-done"></a>Ai-je terminé ?

La réponse est aucune.

Les pirates jamais arrêter, par conséquent, aucune des deux peuvent vous. Cette feuille de route peut aider votre organisation à protéger contre actuellement connues des menaces comme les pirates seront évoluent constamment and -shift. Nous vous recommandons de que considérer la sécurité comme un processus continu visant à augmenter le coût et en réduisant le taux de réussite des adversaires ciblant votre environnement.

Bien qu’il ne soit pas la seule partie du programme de sécurité de votre organisation sécurisation de l’accès privilégié est un composant essentiel de votre stratégie de sécurité.
