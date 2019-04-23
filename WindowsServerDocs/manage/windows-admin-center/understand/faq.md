---
title: Forum aux Questions sur Windows Admin Center
description: Obtenez des réponses concernant Windows Admin Center (projet Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 39f862485cf938981aae37e352f3448998b7c9c0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829570"
---
# <a name="windows-admin-center-frequently-asked-questions"></a>Forum aux Questions sur Windows Admin Center

>S'applique à : Windows Admin Center, version préliminaire de Windows Admin Center

Vous trouverez ici les réponses aux questions les plus fréquemment posées sur Windows Admin Center.

## <a name="what-is-windows-admin-center"></a>Qu'est-ce que Windows Admin Center ?

Windows Admin Center est une plateforme d'interface utilisateur graphique légère, basée sur un navigateur et un ensemble d'outils qui sont destinés aux administrateurs informatiques pour gérer Windows Server et Windows 10. Il s’agit de l’évolution d'outils d’administration intégrés familiers, tels que le Gestionnaire de serveur et la Console MMC (Microsoft Management Console) sous forme d'expérience modernisée, simplifiée, intégrée et sécurisée.

## <a name="can-i-use-windows-admin-center-in-production-environments"></a>Puis-je utiliser Windows Admin Center dans les environnements de production ?

Oui. Windows Admin Center est généralement disponible et prêt pour de grands déploiements d’utilisation et de production. Comme la plateforme continue d’évoluer et de se développer, ses capacités actuelles et ses outils principaux (hors de la version Preview) répondent aux critères de version standard de Microsoft et de niveau de qualité en termes de facilité d’utilisation, de fiabilité, de performances, d'accessibilité, de sécurité et d'adoption.

## <a name="how-much-does-it-cost-to-use-windows-admin-center"></a>Combien coûte l'utilisation de Windows Admin Center ?

Windows Admin Center ne donne lieu à aucun frais supplémentaire par rapport à Windows. Vous pouvez utiliser Windows Admin Center (disponible sous forme de téléchargement distinct) avec des licences valides Windows Server ou Windows 10 sans coût supplémentaire : il est fourni sous licence par un accord CLUF Windows supplémentaire.

## <a name="what-versions-of-windows-server-can-i-manage-with-windows-admin-center"></a>Quelles versions de Windows Server puis-je gérer avec Windows Admin Center ?

Windows Admin Center est optimisé pour Windows Server 2019 activer les thèmes de clé dans la version de Windows Server 2019 : hybride cloud scénarios et gestion de l’infrastructure Hyper-convergée, en particulier. Même si Windows Admin Center fonctionnent mieux avec Windows Server 2019, il prend en charge la gestion de plusieurs versions que les clients utilisent déjà de : Windows Server 2012 et versions ultérieures sont entièrement pris en charge. Il existe également des fonctionnalités limitées pour la gestion de Windows Server 2008 R2.

## <a name="is-windows-admin-center-a-complete-replacement-for-all-traditional-in-box-and-rsat-tools"></a>Windows Admin Center remplace-t-il complètement l'ensemble des outils traditionnels intégrés et des outils d’administration de serveur distant ?

Non. Bien que Windows Admin Center puisse gérer de nombreux scénarios courants, il ne remplace pas complètement les outils traditionnels de la console MMC (Microsoft Management Console). Pour un examen détaillé de quels outils sont inclus avec Windows Admin Center, en savoir plus sur [la gestion des serveurs](..\use\manage-servers.md) dans notre documentation. Windows Admin Center comprend les fonctionnalités clés suivantes dans sa solution Gestionnaire de serveur :

* Affichage des ressources et utilisation des ressources
* Gestion des certificats
* Gestion des appareils
* Observateur d’événements
* Explorateur de fichiers
* Gestion de pare-feu
* La gestion des applications installées
* Configuration des utilisateurs et groupes locaux
* Paramètres réseau
* Affichage/arrêt de processus et création de vidages de processus
* Modification du Registre
* La gestion des tâches planifiées
* Gestion des Services Windows
* Activation/désactivation des rôles et fonctionnalités
* Gestion des machines virtuelles Hyper-V et des commutateurs virtuels
* Gestion du stockage
* La gestion de réplica de stockage
* Gestion des mises à jour Windows
* Console PowerShell
* Connexion Bureau à distance

Windows Admin Center fournit également ces solutions :

* Gestion de l’ordinateur : fournit un sous-ensemble de fonctionnalités du Gestionnaire de serveur pour la gestion des ordinateurs clients Windows 10
* Gestionnaire du cluster de basculement : prise en charge de la gestion de clusters de basculement et de ressources de cluster
* Gestionnaire de cluster hyperconvergé : offre une toute nouvelle expérience personnalisée pour les espaces de stockage direct et Hyper-V. Il comprend le Tableau de bord et met en évidence les graphiques et les alertes de surveillance.

Windows Admin Center est complémentaire et ne remplace pas les outils d’administration de serveur distant, car des rôles tels qu’Active Directory, DHCP, DNS, IIS n’ont pas encore de fonctions de gestion correspondantes qui apparaissent dans Windows Admin Center.

## <a name="can-windows-admin-center-be-used-to-manage-the-free-microsoft-hyper-v-server"></a>Windows Admin Center peut-il servir à gérer le serveur gratuit Microsoft Hyper-V Server ?

Oui. Windows Admin Center peut être utilisé pour gérer Microsoft Hyper-V Server 2016 et Microsoft Hyper-V Server 2012 R2.

## <a name="can-i-deploy-windows-admin-center-on-a-windows-10-computer"></a>Puis-je déployer Windows Admin Center sur un ordinateur Windows 10 ?

Oui, Windows Admin Center peut être installé sur Windows 10 (version 1709 ou version ultérieure) exécuté en mode bureau.  Windows Admin Center peut également être installé sur un serveur avec Windows Server 2016 ou plus prononcée dans le mode passerelle et ensuite accessible via un navigateur web à partir d’un ordinateur Windows 10. [En savoir plus sur les options d’installation](..\plan\installation-options.md).

## <a name="ive-heard-that-windows-admin-center-uses-powershell-under-the-hood-can-i-see-the-actual-scripts-that-it-uses"></a>J’ai entendu dire que Windows Admin Center utilise PowerShell sous le capot, puis-je voir les scripts qu’il utilise ?

Oui ! le [Showscript fonctionnalité](..\use\get-started.md#view-powershell-scripts-used-in-windows-admin-center) a été ajouté dans Windows Admin Center aperçu 1806 et est désormais inclus dans le canal de la disponibilité générale.

## <a name="are-there-any-plans-for-windows-admin-center-to-manage-windows-server-2008-r2-or-earlier"></a>Existe-t-il des plans pour que Windows Admin Center gère Windows Server 2008 R2 ou une version antérieure ?

Windows Admin Center prend désormais en charge **limitée** fonctionnalités pour gérer Windows Server 2008 R2. Windows Admin Center s'appuie sur les fonctionnalités de PowerShell et des technologies de plateforme qui n’existent pas dans Windows Server 2008 R2 et les versions antérieures, ce qui rend la prise en charge complète irréalisable. Windows Server 2008/2008 R2 s’approchent de fin de support technique de janvier 2020 donc Microsoft recommande aux clients [déplacement vers Azure ou de mise à niveau vers la dernière version de Windows Server](https://www.microsoft.com/en-us/cloud-platform/windows-server-2008).

## <a name="are-there-any-plans-for-windows-admin-center-to-manage-linux-connections"></a>Existe-t-il des plans pour que Windows Admin Center gère les connexions Linux ?

Nous étudions en raison de la demande du client, mais il n’existe actuellement aucun plan verrouillé pour fournir et prise en charge peut se composer uniquement d’une connexion de la console via SSH.

## <a name="which-web-browsers-are-supported-by-windows-admin-center"></a>Quels sont les navigateurs web pris en charge par Windows Admin Center ?

Les dernières versions des navigateurs Microsoft Edge (Windows 10, version 1709 ou version ultérieure) et Google Chrome sont testées et prises en charge sur Windows 10. [Afficher le navigateur spécifique des problèmes connus](..\use\known-issues.md#browser-specific-issues). D’autres navigateurs web modernes ou autres plateformes ne sont pas actuellement partie de notre matrice de test et sont donc pas *officiellement* pris en charge.

## <a name="how-does-windows-admin-center-handle-security"></a>Comment la sécurité est-elle gérée par Windows Admin Center ?

Le trafic entre le navigateur et la passerelle Windows Admin Center utilise le protocole HTTPS. Le trafic depuis la passerelle vers les serveurs gérés utilise PowerShell et WMI standard sur WinRM. Nous prenons en charge la Solution de mot de passe d'administrateur local (LAPS), la délégation contrainte basée sur les ressources, le contrôle d’accès de passerelle à l’aide d’Active Directory ou d'Azure AD et le contrôle d’accès basé sur des rôles pour gérer les serveurs cibles.

## <a name="are-there-any-cloud-dependencies"></a>Y a-t-il des dépendances cloud ?

Windows Admin Center ne nécessite pas d’accès à Internet, ni Microsoft Azure. Windows Admin Center gère les instances Windows Server et Windows n’importe où : sur les systèmes physiques ou sur des machines virtuelles sur n’importe quel hyperviseur ou en cours d’exécution dans un cloud. Bien que l’intégration avec les différents services Azure sera ajoutée au fil du temps, il s’agira de fonctionnalités à valeur ajoutée facultatives et on non obligatoires pour utiliser Windows Admin Center.

## <a name="are-there-any-other-dependencies-or-prerequisites"></a>Existe-t-il d’autres dépendances ou conditions préalables ?

Windows Admin Center peut être installé sur Windows 10 Fall Anniversary Update (1709) ou une version ultérieure, ou Windows Server 2016 ou une version plus récente. Pour gérer Windows Server 2008 R2, 2012 ou 2012 R2, l’installation de Windows Management Framework 5.1 est requise sur ces serveurs. Il n’y a pas d'autres dépendances. IIS n’est pas requis, les agents ne sont pas nécessaires, SQL Server n’est pas requis.

## <a name="what-about-extensibility-and-3rd-party-support"></a>Qu’advient-il de l’extensibilité et du support tiers ?

Windows Admin Center a un SDK disponible afin que tout le monde peut écrire leur propre extension. En tant que plate-forme, développer notre écosystème et favoriser l’extension des partenaires est une priorité essentielle depuis le début. [En savoir plus sur le SDK Windows Admin Center](..\extend\extensibility-overview.md).

## <a name="can-i-manage-hyper-converged-infrastructure-with-windows-admin-center"></a>Puis-je gérer une infrastructure hyperconvergée avec Windows Admin Center ?

Oui. Windows Admin Center prend en charge la gestion de clusters hyperconvergés exécutant Windows Server 2016 ou Windows Server 2019. La solution de gestionnaire de cluster hyperconvergé dans Windows Admin Center précédemment disponible en version préliminaire, mais maintenant **à la disposition générale**, avec certaines nouvelles fonctionnalités en version préliminaire. Pour plus d’informations, voir [en savoir plus sur la gestion d'infrastructure hyperconvergée](..\use\manage-hyper-converged.md).

## <a name="does-windows-admin-center-require-system-center"></a>Windows Admin Center nécessite-t-il System Center ?

Non. Windows Admin Center est complémentaire de System Center, mais System Center n’est pas obligatoire. [En savoir plus sur les Windows Admin Center et System Center](related-management.md#system-center).

## <a name="can-windows-admin-center-replace-system-center-virtual-machine-manager-scvmm"></a>Windows Admin Center peut-il remplacer System Center Virtual Machine Manager (SCVMM) ?

Windows Admin Center et SCVMM sont complémentaires ; Windows Admin Center est destiné à remplacer les composants logiciels enfichables de la console MMC (Microsoft Management Console) classique et l’expérience d’administration de serveur.  Windows Admin Center n’est pas destiné à remplacer les aspects de surveillance de SCVMM. [En savoir plus sur les Windows Admin Center et System Center](related-management.md#system-center).

## <a name="what-is-windows-admin-center-preview-which-version-is-right-for-me"></a>Qu’est-ce que Windows Admin Center Preview et quelle est la version qui me convient le mieux ?

Il existe deux versions de Windows Admin Center disponibles en téléchargement :

### <a name="windows-admin-center"></a>Windows Admin Center

* Cette version convient aux administrateurs informatiques qui ne sont pas en mesure de faire des mises à jour fréquemment ou qui souhaitent davantage de temps de validation pour les versions qu'ils utilisent en production. Notre mise en production à la disposition générale (GA) actuelle est Windows Admin Center 1809.5.
* Pour obtenir la dernière version GA, [téléchargez ici](https://aka.ms/WACDownload).

* La disponibilité générale de Windows Admin Center versions est prises en charge en continu, en fonction de Microsoft [politique de cycle de vie moderne](https://support.microsoft.com/help/30881/modern-lifecycle-policy). Cela signifie que seule la dernière version publiée de la disponibilité générale de Windows Admin Center est pris en charge et pris en charge, et les utilisateurs doivent « restez » par la mise à niveau vers la dernière version de la disponibilité générale de Windows Admin Center dans les 30 jours de disponibilité reste pris en charge. Cette stratégie s’applique à la plate-forme Windows Admin Center, ainsi que toutes les extensions de Microsoft qui sont la disponibilité générale et publiées dans l’extension Windows Admin Center de flux. Notez que certaines extensions peuvent être mises à jour plus fréquemment que d’autres, entre les versions de Windows Admin Center GA.

### <a name="windows-admin-center-preview"></a>Windows Admin Center Preview

* Cette version convient aux administrateurs informatiques qui souhaitent les toutes dernières fonctionnalités à une cadence régulière. Notre objectif est de fournir la prochaine mise à jour libère tous les mois environ. La plate-forme de base continue à être prête pour la production et la licence fournit des droits d’utilisation de production. Toutefois, notez que vous verrez l’introduction de nouveaux outils et fonctionnalités clairement marqués comme étant PREVIEW et conviennent à l'évaluation et au test.
* Pour obtenir la dernière version d’évaluation, les Insiders inscrits peuvent télécharger Windows Admin Center Preview directement à partir de la [page de téléchargement Windows Server Insider Preview](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewserver), sous la liste déroulante Additional Downloads. Si vous n’êtes pas encore inscrit comme Insider, voir [Prise en main avec Windows Server](https://insider.windows.com/en-us/for-business-getting-started-server/) sur le portail Windows Insiders pour Entreprises.

## <a name="why-was-windows-admin-center-chosen-as-the-final-name-for-project-honolulu"></a>Pourquoi « Windows Admin Center » a-t-il été choisi comme nom définitif du « projet Honolulu » ?

Windows Admin Center est le nom de produit officiel du « projet Honolulu » et renforce notre vision d’une expérience intégrée pour les administrateurs informatiques dans un large éventail de scénarios de gestion et d’administration principaux. Il met également en avant le fait que notre orientation client sur les besoins des administrateurs informatiques est la priorité centrale de nos investissements et de nos publications.

## <a name="where-can-i-learn-more-about-windows-admin-center-or-get-more-details-on-the-topics-above"></a>Où puis-je en savoir plus sur Windows Admin Center, ou obtenir plus d’informations sur les rubriques ci-dessus ?

Notre [page de lancement](https://aka.ms/WindowsAdminCenter) est le meilleur point de départ et propose des liens vers notre contenu de documentation nouvellement classé, l’emplacement de téléchargement, comment fournir des commentaires, des informations de référence et d'autres ressources.

## <a name="what-is-the-version-history-of-windows-admin-center"></a>Qu’est l’historique des versions de Windows Admin Center ?

[Afficher l’historique de version ici.](..\overview.md#release-history)

## <a name="im-having-an-issue-with-windows-admin-center-where-can-i-get-help"></a>Je rencontre un problème avec Windows Admin Center, où puis-je obtenir de l’aide ?

Consultez notre [guide de résolution des problèmes](..\use\troubleshooting.md) et notre liste de [problèmes connus](..\use\known-issues.md).
