---
title: "Sécurisation de l’accès privilégié"
description: "Sécurité de Windows Server"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f5dec0c2-06fe-4c91-9bdc-67cc6a3ede60
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: eb83903204b00ef6c1eb116554ec54bc2211a399
ms.sourcegitcommit: 7b01b54032ec56432116626e08fbd92508c3a7d5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/09/2018
---
# <a name="securing-privileged-access"></a>Sécurisation de l’accès privilégié

>S’applique à: Windows Server2016

Sécurisation des privilèges accès est une première étape essentielle pour établir des garanties de sécurité pour des ressources d’entreprise dans une organisation moderne. La sécurité de la plupart ou toutes les ressources d’entreprise dans une organisation dépend de l’intégrité des comptes privilégiés qui administrent et gèrent les systèmes informatiques. Pirates ciblent ces comptes et autres éléments de l’accès privilégié pour accéder rapidement à des données ciblées et des systèmes à l’aide d’attaques de vol d’informations d’identification comme [Pass-the-Hash et Pass-the-Ticket](https://www.microsoft.com/pth).

Protection d’accès administratif contre déterminé adversaires vous obligent à adopter une approche complète et réfléchie pour isoler ces systèmes des risques. Cette figure illustre les trois phases des recommandations de séparation et de protection de l’administration dans cette feuille de route:

![Diagramme des trois phases des recommandations de séparation et de protection de l’administration dans cette feuille de route](../media/securing-privileged-access/PAW_LP_Fig1.JPG)

Objectifs de la feuille de route:

-   **plan sur 2 à 4semaines**: réduire rapidement les techniques d’attaque plus fréquemment utilisées

-   **plan sur 1 à 3mois**: générer une visibilité et contrôle de l’activité d’administration

-   **plan + de 6mois**: poursuivre l’élaboration de défenses vers une attitude sécuritaire plus proactive

Microsoft vous recommande de que suivre cette feuille de route pour sécuriser l’accès privilégié contre ses adversaires déterminés. Vous pouvez ajuster cette feuille de route pour répondre aux exigences spécifiques de votre organisation et vos capacités existantes.

> [!NOTE]
> Sécurisation des privilèges accès nécessite un large éventail d’éléments, y compris les composants techniques (défenses d’hôte, protections de comptes, gestion des identités, etc.), ainsi que des modifications à traiter et pratiques et connaissances administratives.

## <a name="why-is-securing-privileged-access-important"></a>Pourquoi la sécurisation de l’accès privilégié est-elle importante?
Dans la plupart des organisations, la sécurité de la plupart ou toutes les ressources d’entreprise dépend de l’intégrité des comptes privilégiés qui administrent et gèrent les systèmes informatiques. Pirates sont concentrent sur l’accès privilégié aux systèmes tels que des données ciblées ActiveDirectory pour accéder rapidement à l’ensemble d’une organisation.

Approches traditionnelles de la sécurité se concentrent sur l’utilisation de points d’entrée et d’un réseau d’organisations en tant que périmètre de sécurité principal, mais l’efficacité de la sécurité réseau a considérablement diminué suite à deux tendances:

-   Les organisations hébergent des données et de ressources en dehors des limites du réseau traditionnel sur les PC mobiles de l’entreprise, appareils tels que les services cloud et des tablettes, téléphones mobiles et les appareils BYOD

-   Adversaires ont fait montre une capacité constante et suivie à obtenir l’accès sur les stations de travail à l’intérieur de la limite de réseau par le biais du hameçonnage et d’autres attaques web et par e-mail.

Le remplacement naturel du périmètre de sécurité réseau dans une entreprise moderne complexe est les contrôles d’authentification et d’autorisation de couche d’identité d’une organisation. Comptes administratifs privilégiés ont effectivement le contrôle de ce nouveau «périmètre de sécurité». il est essentiel de protéger l’accès privilégié:

![Diagramme montrant la couche d’identité d’une organisation](../media/securing-privileged-access/PAW_LP_Fig2.JPG)

Un adversaire qui prend le contrôle d’un compte administratif peut utiliser ces privilèges pour en tirer profit au détriment de l’organisation cible, comme illustré ci-dessous:

![Diagramme montrant comment un adversaire qui prend le contrôle d’un compte administratif peut utiliser ces privilèges pour en tirer profit au détriment de l’organisation cible](../media/securing-privileged-access/PAW_LP_Fig3.JPG)

Pour plus d’informations sur les types d’attaques qui débouchent dans le contrôle des comptes administratifs, visitez le [site Web Pass The Hash](https://www.microsoft.com/pth) pour consulter des livres blancs, des vidéos et plus encore.

Cette figure illustre le «canal» distinct pour l’administration de la feuille de route établit pour isoler les tâches à accès privilégié des tâches d’utilisateur standard un risque élevé comme la navigation web et accéder à la messagerie.

![Diagramme montrant le «canal» distinct d’administration que la feuille de route établit pour isoler les tâches à accès privilégié des tâches d’utilisateur standard un risque élevé comme la navigation web et accéder à la messagerie](../media/securing-privileged-access/PAW_LP_Fig4.JPG)

Étant donné que l’adversaire peut prendre le contrôle de l’accès privilégié à l’aide de différentes méthodes, atténuer ce risque exige une approche technique détaillée et holistique, comme indiqué dans cette feuille de route. La feuille de route est isoler et renforcer les éléments de votre environnement qui permettent l’accès privilégié par atténuations dans chaque zone de la colonne de défense dans la figure ci-après:

![Tableau montrant les colonnes d’attaque et de défense](../media/securing-privileged-access/PAW_LP_Fig5.JPG)

## <a name="security-privileged-access-roadmap"></a>Feuille de route de l’accès privilégiée de sécurité
La feuille de route est conçue pour maximiser l’utilisation de technologies qui vous est peut-être déjà déployée, tirer parti des technologies de sécurité actuelles et à venir des clés et intégrer des outils de sécurité 3ème partie peuvent déjà avoir déployé.

La feuille de route des recommandations de Microsoft comporte 3étapes:

-   Plan sur 2 à 4semaines: réduire rapidement les techniques d’attaque plus fréquemment utilisées

-   Plan sur 1 à 3mois: générer une visibilité et contrôle de l’activité d’administration

-   Plan de + de 6mois: poursuivre l’élaboration de défenses vers une attitude sécuritaire plus proactive

Chaque étape de la feuille de route est conçue pour augmenter le coût et la difficulté pour les adversaires d’attaquer l’accès privilégié pour votre site et de ressources de cloud. La feuille de route est hiérarchisée pour planifier les plus efficaces et les implémentations plus rapides tout d’abord basées sur nos expériences de ces attaques et l’implémentation de solution.

> [!NOTE]
> La chronologie de la feuille de route est approximative et est basée sur notre expérience avec des implémentations de client. La durée varie dans votre organisation en fonction de la complexité de votre environnement et votre processus de gestion des modifications.

### <a name="security-privileged-access-roadmap-stage-1"></a>Sécurisation de feuille de route accès privilégié: Phase 1
Phase 1 de la feuille de route vise à atténuer rapidement les techniques d’attaque plus fréquemment utilisées de vol d’informations d’identification et les abus. Phase 1 est conçue pour être implémentée dans un délai de 2 à 4semaines et est représentée dans ce diagramme:

![Figure montrant que la phase 1 est conçue pour être implémentée dans un délai de 2 à 4semaines](../media/securing-privileged-access/PAW_LP_Fig6.JPG)

Étape1 de la feuille de route de sécurité d’accès privilégié comprend les composants suivants:

**1. Séparer le compte d’administrateur pour les tâches d’administration**

Pour mieux séparer les risques internet (attaques par hameçonnage, navigation web) des privilèges administratifs, créez un compte dédié pour tous les employés dotés de privilèges administratifs. Des conseils supplémentaires sur cette sont inclus dans les instructions de PAW publiées [ici](http://Aka.ms/CyberPAW).

**2. Privilégié accès stations de travail à la Phase 1: Administrateurs d’ActiveDirectory**

Pour mieux séparer les risques internet (attaques par hameçonnage, navigation web) des privilèges administratifs de domaine, créer des stations de travail dédié pour l’accès privilégié (Paw) pour le personnel avec des privilèges d’administration ActiveDirectory. Il s’agit de la première étape d’un programme PAW et Phase 1des recommandations publiées [ici](http://Aka.ms/CyberPAW).

**3. Mots de passe administrateur Local uniques pour les stations de travail**

**4. Mots de passe d’administrateur Local uniques pour les serveurs**

Pour atténuer le risque d’un pirate voler un hachage de mot de passe du compte administrateur local à partir de la base de données SAM locale et son utilisation pour attaquer d’autres ordinateurs, vous devez utiliser l’outil LAPS pour configurer des mots de passe aléatoires uniques sur chaque serveur et la station de travail et enregistrer les mots de passe dans ActiveDirectory. Vous pouvez obtenir la Solution de mot de passe administrateur Local pour une utilisation sur les serveurs et stations de travail [ici](http://Aka.ms/LAPS).

Vous pouvez trouver des conseils supplémentaires pour un environnement avec LAPS et des stations [ici](http://aka.ms/securitystandards).

### <a name="security-privileged-access-roadmap-stage-2"></a>Sécurisation de feuille de route accès privilégié: Phase 2
Phase 2 s’appuie sur les atténuations à partir de la phase 1 et est conçu pour être implémentée dans environ 1 à 3mois. Les étapes de cette phase sont illustrées dans ce diagramme:

![Diagramme montrant les étapes de la phase 2](../media/securing-privileged-access/PAW_LP_Fig7.JPG)

**1. Phases PAW 2 et 3: tous les administrateurs et renforcement supplémentaire**

Pour séparer les risques internet de tous les comptes administratifs privilégiés, reprenez les stations, vous avez commencé à la phase 1 et implémenter des stations de travail dédiées pour tous les employés dotés de l’accès privilégié. Cette correspond à la Phase 2 et 3des recommandations publiées [ici](http://Aka.ms/CyberPAW).

**2. Temporaires disposer de privilèges (aucun administrateur permanent)**

Pour réduire le temps d’exposition des privilèges et augmenter la visibilité de leur utilisation, octroyez des privilèges juste à temps (JIT) à l’aide d’une solution appropriée, comme celles ci-dessous:

-   Pour les Services de domaine ActiveDirectory (ADDS), utilisez MicrosoftIdentity Manager (MIM) de [Privileged Access Manager (PAM)](https://technet.microsoft.com/en-us/library/mt150258.aspx) fonctionnalité.

-   Pour Azure ActiveDirectory, utilisez [Azure AD Privileged Identity Management (PIM)](http://aka.ms/AzurePIM) fonctionnalité.

**3. Multi-Factor d’élévation limitée dans le temps**

Pour augmenter le niveau d’assurance d’authentification d’administrateur, vous devez exiger une authentification multifacteur avant l’octroi de privilèges.
Cela peut se faire avec MIM PAM et Azure ADPIM à l’aide de l’authentification multifacteur Azure (MFA).

**4. Just Enough administration (JEA) pour la Maintenance du contrôleur de domaine**

Pour réduire la quantité de comptes avec des privilèges d’administration de domaine et le risque d’exposition associé, utiliser la fonctionnalité uniquement assez Administration (JEA) dans PowerShell pour effectuer les opérations de maintenance courantes sur les contrôleurs de domaine. La technologie JEA autorise certains utilisateurs à effectuer des tâches administratives spécifiques sur les serveurs (tels que les contrôleurs de domaine) sans leur octroyer de droits d’administrateur. Télécharger ce guide, de [TechNet](http://aka.ms/JEA).

**5. Inférieur surface d’attaque de domaine et les contrôleurs de domaine**

Pour diminuer les risques d’adversaires de prendre le contrôle d’une forêt, vous devez diminuer les voies que peut suivre un pirate d’acquérir le contrôle des contrôleurs de domaine ou des objets dans le contrôle du domaine. Suivez les instructions pour réduire ce risque publié [ici](http://aka.ms/HardenAD).

**6. Détection des attaques**

Pour obtenir la visibilité dans le vol d’informations d’identification active et identité attaques afin que vous pouvez répondre rapidement aux événements et dégâts, déployer et configurer [Analytique de menace avancée (ATA)](https://www.microsoft.com/ata).

Avant d’installer ATA, vous devez vous assurer que vous disposez d’un processus en place pour gérer un incident de sécurité majeur qu’ATA peut détecter.

-   Pour plus d’informations sur la configuration d’un processus de réponse aux incidents, consultez [réponse aux Incidents de sécurité informatique](https://aka.ms/irr) et «Répondre à une activité suspecte» et «Récupérer d’une violation» de sections de [Mitigating Pass-the-Hash et autres vols d’informations d’identification](https://www.microsoft.com/pth), version2.

-   Pour plus d’informations sur le recours aux services Microsoft pour vous aider à préparer votre processus de réplication initiale pour ATA généré des événements et le déploiement d’ATA, contactez votre représentant Microsoft en accédant à [cette page](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx).

-   Accès [cette page](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx) pour plus d’informations sur le recours aux services Microsoft pour vous aider à rechercher et de récupération à partir d’un incident

-   Pour implémenter ATA, suivez le guide de déploiement disponible [ici](http://aka.ms/ata).

### <a name="security-privileged-access-roadmap-stage-3"></a>Sécurisation de feuille de route accès privilégié: Phase 3
Étape3 de la feuille de route s’appuie sur les atténuations des phases 1 et 2 pour renforcer et ajouter d’autres atténuations dans tout le spectre. Phase 3 est une représentation visuelle dans ce diagramme:

![Diagramme montrant l’étape3](../media/securing-privileged-access/PAW_LP_Fig8.JPG)

Ces fonctionnalités seront appuient sur les atténuations des stades précédents et évoluer vos défenses vers une attitude plus proactive.

**1. Moderniser les rôles et modèle de délégation**

Pour réduire les risques de sécurité, vous devez revoir tous les aspects de votre modèle de délégation pour être compatible avec les règles du modèle de niveau, prendre en charge des rôles d’administration des services cloud et intégrer la facilité d’utilisation de l’administrateur comme un principe clé et les rôles. Ce modèle doit exploiter les fonctionnalités JIT et JEA déployées dans les étapes précédentes, ainsi que les technologies d’automatisation des tâches pour atteindre ces objectifs.

**2. La carte à puce ou authentification Passport pour tous les administrateurs**

Pour augmenter le niveau de sécurité et la facilité d’utilisation de l’authentification d’administrateur, vous devez exiger une authentification renforcée pour tous les comptes administratifs hébergés dans Azure ActiveDirectory et dans WindowsServerActiveDirectory (y compris les comptes fédérés à un service cloud).

**3. Forêt d’administration pour les administrateurs d’ActiveDirectory**

Pour fournir le niveau de protection pour les administrateurs d’ActiveDirectory, configurez un environnement sans dépendances de sécurité sur votre ActiveDirectory de production et qui est isolé des attaques provenant de tous, mais les systèmes plus fiables dans votre environnement de production. Pour plus d’informations sur l’architecture ESAE, visitez [cette page](http://aka.ms/esae).

**4. Stratégie d’intégrité de code pour les contrôleurs de domaine (Server2016)**

Pour limiter le risque de programmes non autorisés sur vos contrôleurs de domaine à partir d’opérations d’attaque adversaire et erreurs administratives par mégarde, configurez l’intégrité du Code de Windows Server2016 pour noyau (pilotes) et mode utilisateur (applications) afin d’autoriser uniquement les exécutables autorisés à s’exécuter sur l’ordinateur.

**5. Dotées des contrôleurs de domaine virtuels (Server2016 Hyper-V Fabric)**

Pour protéger les contrôleurs de domaine virtualisés contre les vecteurs d’attaque qui exploitent perte inhérente d’un ordinateur virtuel de la sécurité physique, utilisez cette nouvelle fonctionnalité Server2016 Hyper-V pour empêcher le vol de secrets ActiveDirectory à partir de contrôleurs de domaine virtuel. À l’aide de cette solution, vous pouvez chiffrer des machines virtuelles 2 de génération pour protéger les données contre l’inspection, le vol et la falsification par les administrateurs réseau et de stockage ainsi que pour renforcer l’accès à la machine virtuelle contre les attaques d’administrateurs hôte Hyper-V.

## <a name="am-i-done"></a>J’ai terminé?
Fin de cette feuille de route vous disposera protections fort accès privilégié pour les attaques qui sont actuellement adversaires connues et disponibles aujourd'hui. Malheureusement, les menaces de sécurité seront évoluent constamment et MAJ, nous vous recommandons de consulter la sécurité comme un processus continu sur augmenter le coût et en réduisant le taux de réussite des adversaires qui ciblent votre environnement.

Sécurisation des privilèges accès est une première étape essentielle pour établir des garanties de sécurité pour des ressources d’entreprise dans une organisation moderne, mais il n’est pas la seule partie d’un programme de sécurité complet qui comprend les éléments tels que la stratégie, opérations, la sécurité des informations, serveurs, applications, PC, périphériques, la structure de cloud et autres composants fournissent les garanties de sécurité que vous avez besoin.

Pour plus d’informations sur la création d’une feuille de route de sécurité complète, consultez la section «responsabilités du client et feuille de route» de la sécurité du Cloud de Microsoft document Enterprise Architects disponible [ici](http://aka.ms/securecustomer).

Pour plus d’informations sur le recours aux services Microsoft pour vous aider à ces rubriques, contactez votre représentant Microsoft ou visitez [cette page](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx).

### <a name="related-topics"></a>Rubriques connexes
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

[Module de plateforme sécurisée](https://docs.microsoft.com/en-us/windows/device-security/tpm/trusted-platform-module-overview)
