---
title: Questions fréquentes (FAQ) sur Windows Admin Center
description: Obtenez des réponses concernant Windows Admin Center (projet Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.date: 06/07/2019
ms.prod: windows-server-threshold
ms.openlocfilehash: d2a2acc9261ac1a25e140cc298cc58e4b0fb6e52
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70869046"
---
# <a name="windows-admin-center-frequently-asked-questions"></a>Questions fréquentes (FAQ) sur Windows Admin Center

> S’applique à : Windows Admin Center, Windows Admin Center Preview

Vous trouverez ici les réponses aux questions les plus fréquemment posées sur Windows Admin Center.

## <a name="what-is-windows-admin-center"></a>Qu’est-ce que Windows Admin Center ?

Windows Admin Center est une plateforme d’interface graphique utilisateur légère basée sur un navigateur et un ensemble d’outils destinés aux administrateurs informatiques pour gérer Windows Server et Windows 10. Il s’agit de l’évolution d’outils d’administration intégrés familiers, comme le Gestionnaire de serveur et MMC (Microsoft Management Console), qui se présente sous la forme d’une expérience actualisée, simplifiée, intégrée et sécurisée.

## <a name="can-i-use-windows-admin-center-in-production-environments"></a>Puis-je utiliser Windows Admin Center dans les environnements de production ?

Oui. Windows Admin Center est généralement disponible et prêt pour des déploiements d’utilisation et de production à grande échelle. Les fonctionnalités et principaux outils actuels de la plateforme répondent aux critères de version standard de Microsoft et à notre niveau de qualité en termes de facilité d’utilisation, de fiabilité, de performances, d’accessibilité, de sécurité et d’adoption.

[!INCLUDE [support-policy](../includes/support-policy.md)]

## <a name="how-much-does-it-cost-to-use-windows-admin-center"></a>Combien coûte l’utilisation de Windows Admin Center ?

Windows Admin Center n’engendre aucuns frais supplémentaires autres que Windows. Vous pouvez utiliser Windows Admin Center (disponible en téléchargement distinct) avec des licences valides de Windows Server ou Windows 10 sans coût supplémentaire : il est fourni sous licence dans un avenant au contrat CLUF de Windows.

## <a name="what-versions-of-windows-server-can-i-manage-with-windows-admin-center"></a>Quelles versions de Windows Server puis-je gérer avec Windows Admin Center ?

Windows Admin Center est optimisé pour Windows Server 2019 afin de prendre en charge des thèmes clés dans la version de Windows Server 2019 : en particulier, les scénarios de cloud hybride et la gestion d’infrastructure hyperconvergée. Bien que Windows Admin Center fonctionne mieux avec Windows Server 2019, il prend en charge la gestion de plusieurs versions que les clients utilisent déjà : Windows Server 2012 et les versions plus récentes sont entièrement pris en charge. Il existe également des fonctionnalités limitées pour la gestion de Windows Server 2008 R2.

## <a name="is-windows-admin-center-a-complete-replacement-for-all-traditional-in-box-and-rsat-tools"></a>Windows Admin Center remplace-t-il complètement l’ensemble des outils traditionnels intégrés et des outils d’administration de serveur distant ?

Non. Bien que Windows Admin Center puisse gérer de nombreux scénarios courants, il ne remplace pas complètement tous les outils traditionnels de la console MMC (Microsoft Management Console). Pour découvrir en détail quels sont les outils inclus dans Windows Admin Center, consultez les informations sur la [gestion des serveurs](../use/manage-servers.md) dans notre documentation. Windows Admin Center comprend les fonctionnalités clés suivantes dans sa solution Gestionnaire de serveur :

* Affichage des ressources et utilisation des ressources
* Gestion des certificats
* Gestion des appareils
* Observateur d’événements
* Explorateur de fichiers
* Gestion de pare-feu
* Gestion des applications installées
* Configuration des utilisateurs et groupes locaux
* Paramètres réseau
* Affichage/arrêt de processus et création de vidages de processus
* Modification du Registre
* Gestion des tâches planifiées
* Gestion des services Windows
* Activation/désactivation des rôles et des fonctionnalités
* Gestion des machines virtuelles Hyper-V et des commutateurs virtuels
* Gestion du stockage
* Gestion du réplica de stockage
* Gestion des mises à jour Windows
* Console PowerShell
* Connexion Bureau à distance

Windows Admin Center fournit également les solutions suivantes :

* Gestion de l’ordinateur : fournit une partie des fonctionnalités du Gestionnaire de serveur pour la gestion des ordinateurs clients Windows 10
* Gestionnaire du cluster de basculement : fournit une prise en charge de la gestion continue de clusters de basculement et de ressources de cluster
* Gestionnaire de cluster hyperconvergé : offre une toute nouvelle expérience personnalisée pour les espaces de stockage directs et Hyper-V. Il comprend le Tableau de bord et met en évidence les graphiques et les alertes de supervision.

Windows Admin Center est complémentaire des outils d’administration de serveur distant et ne les remplace pas, car des rôles tels qu’Active Directory, DHCP, DNS et IIS n’ont pas encore de fonctionnalités de gestion correspondantes exposées dans Windows Admin Center.

## <a name="can-windows-admin-center-be-used-to-manage-the-free-microsoft-hyper-v-server"></a>Windows Admin Center peut-il servir à gérer le serveur gratuit Microsoft Hyper-V Server ?

Oui. Windows Admin Center peut être utilisé pour gérer Microsoft Hyper-V Server 2016 et Microsoft Hyper-V Server 2012 R2.

## <a name="can-i-deploy-windows-admin-center-on-a-windows-10-computer"></a>Puis-je déployer Windows Admin Center sur un ordinateur Windows 10 ?

Oui, Windows Admin Center peut être installé sur Windows 10 (version 1709 ou version ultérieure) exécuté en mode bureau.  Windows Admin Center peut également être installé sur un serveur avec Windows Server 2016 ou une version ultérieure en mode passerelle, puis être accessible par le biais d’un navigateur web à partir d’un ordinateur Windows 10. [En savoir plus sur les options d’installation](../plan/installation-options.md).

## <a name="ive-heard-that-windows-admin-center-uses-powershell-under-the-hood-can-i-see-the-actual-scripts-that-it-uses"></a>Il paraît que Windows Admin Center utilise PowerShell en arrière-plan, puis-je voir les scripts qu’il utilise ?

Oui. La [fonctionnalité Showscript](../use/get-started.md#view-powershell-scripts-used-in-windows-admin-center) a été ajoutée dans Windows Admin Center Preview 1806 et est maintenant incluse dans le canal de disponibilité générale.

## <a name="are-there-any-plans-for-windows-admin-center-to-manage-windows-server-2008-r2-or-earlier"></a>Existe-t-il des plans pour que Windows Admin Center gère Windows Server 2008 R2 ou une version antérieure ?

Windows Admin Center prend maintenant en charge des fonctionnalités **limitées** pour la gestion de Windows Server 2008 R2. Windows Admin Center s’appuie sur les fonctionnalités de PowerShell et des technologies de plateforme qui n’existent pas dans Windows Server 2008 R2 et les versions antérieures, ce qui rend la prise en charge complète impossible. Windows Server 2008/2008 R2 approchent de la fin du support en janvier 2020. Microsoft recommande donc aux clients de [migrer vers Azure ou d’effectuer une mise à niveau vers la dernière version de Windows Server 2016](https://www.microsoft.com/en-us/cloud-platform/windows-server-2008).

## <a name="are-there-any-plans-for-windows-admin-center-to-manage-linux-connections"></a>Existe-t-il des plans pour que Windows Admin Center gère les connexions Linux ?

Nous étudions ce point à la demande des clients, mais aucun plan verrouillé n’est actuellement fourni et la prise en charge peut se composer uniquement d’une connexion à une console via SSH.

## <a name="which-web-browsers-are-supported-by-windows-admin-center"></a>Quels sont les navigateurs web pris en charge par Windows Admin Center ?

Les dernières versions des navigateurs Microsoft Edge (Windows 10, version 1709 ou version ultérieure) et Google Chrome sont testées et prises en charge sur Windows 10. [Affichez les problèmes connus spécifiques au navigateur](../support/known-issues.md#browser-specific-issues). D’autres navigateurs web modernes ou d’autres plateformes ne font pas actuellement partie de notre matrice de test et ne sont donc pas *officiellement* pris en charge.

## <a name="how-does-windows-admin-center-handle-security"></a>Comment la sécurité est-elle gérée par Windows Admin Center ?

Le trafic du navigateur vers la passerelle Windows Admin Center utilise le protocole HTTPS. Le trafic de la passerelle vers les serveurs gérés utilise PowerShell et WMI standard sur WinRM. Nous prenons en charge la Solution de mot de passe d’administrateur local (LAPS), la délégation contrainte basée sur les ressources, le contrôle d’accès à la passerelle à l’aide d’Active Directory ou d’Azure AD et le contrôle d’accès en fonction du rôle pour gérer les serveurs cibles.

## <a name="does-windows-admin-center-use-credssp"></a>Windows Admin Center utilise-t-il CredSSP ?

Oui, dans certains cas, Windows Admin Center exige CredSSP. Il est nécessaire pour transmettre vos informations d’identification pour l’authentification sur des machines au-delà du serveur spécifique que vous ciblez pour la gestion. Par exemple, si vous gérez des machines virtuelles sur le **serveur B**, mais que vous voulez stocker les fichiers VHDX pour ces machines virtuelles sur un partage de fichiers hébergé par le **serveur C**, Windows Admin Center doit utiliser CredSSP pour s’authentifier auprès du **serveur C** pour accéder au partage de fichiers.

Windows Admin Center gère automatiquement la configuration de CredSSP après vous avoir demandé votre consentement. Avant de configurer CredSSP, Windows Admin Center vérifie que le système dispose des [mises à jour](https://support.microsoft.com/help/4093492/credssp-updates-for-cve-2018-0886-march-13-2018) récentes de CredSSP. Quand CredSSP est activé, il y a un badge sur la Vue d’ensemble du serveur et une option pour le désactiver.

![CredSSP sur la vue d’ensemble du serveur](../media/CredSSP-overview.png)

CredSSP est actuellement utilisé dans les domaines suivants :

- Utilisation du stockage SMB désagrégé dans l’outil des machines virtuelles (l’exemple ci-dessus.)
- Utilisation de l’outil de mises à jour dans les solutions de gestion des clusters de basculement ou hyperconvergés, qui effectue la [mise à jour adaptée aux clusters](https://docs.microsoft.com/windows-server/failover-clustering/cluster-aware-updating) 

## <a name="are-there-any-cloud-dependencies"></a>Existe-t-il des dépendances au cloud ?

Windows Admin Center n’a pas besoin d’un accès à Internet, ni de Microsoft Azure. Windows Admin Center gère les instances Windows Server et Windows n’importe où : sur des systèmes physiques ou sur des machines virtuelles sur n’importe quel hyperviseur ou en cours d’exécution dans un cloud. Bien que l’intégration avec différents services Azure sera ajoutée au fil du temps, il s’agira de fonctionnalités à valeur ajoutée facultatives et non nécessaires pour utiliser Windows Admin Center.

## <a name="are-there-any-other-dependencies-or-prerequisites"></a>Existe-t-il d’autres dépendances ou prérequis ?

Windows Admin Center peut être installé sur la Mise à jour anniversaire Windows 10 Fall (1709) ou une version plus récente, ou Windows Server 2016 ou une version plus récente. Pour gérer Windows Server 2008 R2, 2012 ou 2012 R2, l’installation de Windows Management Framework 5.1 est requise sur ces serveurs. Il n’y a pas d’autres dépendances. IIS, les agents et SQL Server ne sont pas obligatoires.

## <a name="what-about-extensibility-and-3rd-party-support"></a>Qu’advient-il de l’extensibilité et du support tiers ?

Windows Admin Center dispose d’un SDK afin que tout le monde puisse écrire sa propre extension. En tant que plateforme, développer notre écosystème et favoriser l’extensibilité des partenaires est une priorité essentielle depuis le début. [Découvrez plus en détail le SDK Windows Admin Center.](../extend/extensibility-overview.md)

## <a name="can-i-manage-hyper-converged-infrastructure-with-windows-admin-center"></a>Puis-je gérer une infrastructure hyperconvergée avec Windows Admin Center ?

Oui. Windows Admin Center prend en charge la gestion de clusters hyperconvergés exécutant Windows Server 2016 ou Windows Server 2019. La solution de gestionnaire de cluster hyperconvergé dans Windows Admin Center était précédemment en préversion, mais elle est maintenant en **disponibilité générale**, avec certaines nouvelles fonctionnalités en préversion. Pour plus d’informations, [lisez des informations sur la gestion d’infrastructure hyperconvergée](../use/manage-hyper-converged.md).

## <a name="does-windows-admin-center-require-system-center"></a>Windows Admin Center nécessite-t-il System Center ?

Non. Windows Admin Center est complémentaire de System Center, mais System Center n’est pas nécessaire. [Découvrez plus en détail Windows Admin Center et System Center](related-management.md#system-center).

## <a name="can-windows-admin-center-replace-system-center-virtual-machine-manager-scvmm"></a>Windows Admin Center peut-il remplacer System Center Virtual Machine Manager (SCVMM) ?

Windows Admin Center et SCVMM sont complémentaires ; Windows Admin Center est destiné à remplacer les composants logiciels enfichables MMC (Microsoft Management Console) classiques et l’expérience d’administration de serveur.  Windows Admin Center n’est pas destiné à remplacer les aspects de supervision de SCVMM. [Découvrez plus en détail Windows Admin Center et System Center](related-management.md#system-center).

## <a name="what-is-windows-admin-center-preview-which-version-is-right-for-me"></a>Qu’est-ce que Windows Admin Center Preview et quelle est la version qui me convient le mieux ?

Deux versions de Windows Admin Center sont disponibles en téléchargement :

### <a name="windows-admin-center"></a>Windows Admin Center

* Cette version convient aux administrateurs informatiques qui ne sont pas en mesure de faire des mises à jour fréquemment ou qui souhaitent davantage de temps de validation pour les versions qu’ils utilisent en production. Notre mise en production en disponibilité générale actuelle est Windows Admin Center 1904.
* [!INCLUDE [support-policy](../includes/support-policy.md)]
* Pour obtenir la dernière version, [téléchargez-la ici](https://aka.ms/WACDownload).

### <a name="windows-admin-center-preview"></a>Windows Admin Center Preview

* Cette version convient aux administrateurs informatiques qui souhaitent les fonctionnalités les plus récentes et les plus inédites à une cadence régulière. Notre objectif est de fournir des versions de mise à jour ultérieures chaque mois environ. La plateforme principale continue à être prête pour la production et la licence fournit des droits d’utilisation en production. Toutefois, notez que vous verrez l’introduction de nouveaux outils et fonctionnalités clairement marqués comme étant en PRÉVERSION (PREVIEW) et qui conviennent à l’évaluation et au test.
* Pour obtenir la dernière version d’Insider Preview, les Insiders inscrits peuvent télécharger Windows Admin Center Preview directement à partir de la [page de téléchargement Windows Server Insider Preview](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewserver), sous la liste déroulante Téléchargements supplémentaires. Si vous n’êtes pas encore inscrit comme Insider, consultez [Bien démarrer avec Windows Server](https://insider.windows.com/en-us/for-business-getting-started-server/) sur le portail Windows Insiders pour Entreprises.

## <a name="why-was-windows-admin-center-chosen-as-the-final-name-for-project-honolulu"></a>Pourquoi « Windows Admin Center » a-t-il été choisi comme nom définitif du « projet Honolulu » ?

Windows Admin Center est le nom de produit officiel du « projet Honolulu » et renforce notre vision d’une expérience intégrée pour les administrateurs informatiques dans un large éventail de scénarios de gestion et d’administration principaux. Il met également en avant le fait que notre orientation client sur les besoins des administrateurs informatiques est la priorité centrale de nos investissements et de nos publications.

## <a name="where-can-i-learn-more-about-windows-admin-center-or-get-more-details-on-the-topics-above"></a>Où puis-je en savoir plus sur Windows Admin Center, ou obtenir plus d’informations sur les rubriques ci-dessus ?

Notre [page de lancement](https://aka.ms/WindowsAdminCenter) est le meilleur point de départ et propose des liens vers notre documentation qui vient d’être organisée, l’emplacement de téléchargement, la possibilité de fournir des commentaires, des informations de référence et d’autres ressources.

## <a name="what-is-the-version-history-of-windows-admin-center"></a>Qu’est-ce que l’historique des versions de Windows Admin Center ?

[Consultez l’historique des versions ici.](../overview.md#release-history)

## <a name="im-having-an-issue-with-windows-admin-center-where-can-i-get-help"></a>Je rencontre un problème avec Windows Admin Center. Où puis-je obtenir de l’aide ?

Consultez notre [guide de résolution des problèmes](../use/troubleshooting.md) et notre liste de [problèmes connus](../use/known-issues.md).
