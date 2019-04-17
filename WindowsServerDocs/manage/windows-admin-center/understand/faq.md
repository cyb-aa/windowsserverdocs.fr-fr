---
title: Forum aux Questions sur Windows Admin Center
description: Obtenez des réponses concernant Windows Admin Center (projet Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.date: 04/12/2019
ms.prod: windows-server-threshold
ms.openlocfilehash: 2f1591a32147e3c11ba635f1a11d36b7f38a3470
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296741"
---
# Forum aux Questions sur Windows Admin Center

>S’applique à: Windows Admin Center, Windows Admin Center Preview

Vous trouverez ici les réponses aux questions les plus fréquemment posées sur Windows Admin Center.

## Qu'est-ce que Windows Admin Center?

Windows Admin Center est une plateforme d'interface utilisateur graphique légère, basée sur un navigateur et un ensemble d'outils qui sont destinés aux administrateurs informatiques pour gérer Windows Server et Windows10. Il s’agit de l’évolution d'outils d’administration intégrés familiers, tels que le Gestionnaire de serveur et la Console MMC (Microsoft Management Console) sous forme d'expérience modernisée, simplifiée, intégrée et sécurisée.

## Puis-je utiliser Windows Admin Center dans les environnements de production?

Oui. Windows Admin Center est généralement disponible et prêt pour de grands déploiements d’utilisation et de production. Les capacités actuelles et les outils de core répondent aux critères de version standard de Microsoft et nos de qualité pour facilité d’utilisation, la fiabilité, performances, accessibilité, sécurité et l’adoption.

[!INCLUDE [support-policy](../includes/support-policy.md)]

## Combien coûte l'utilisation de Windows Admin Center?

Windows Admin Center ne donne lieu à aucun frais supplémentaire par rapport à Windows. Vous pouvez utiliser Windows Admin Center (disponible sous forme de téléchargement distinct) avec des licences valides Windows Server ou Windows10 sans coût supplémentaire: il est fourni sous licence par un accord CLUF Windows supplémentaire.

## Quelles versions de Windows Server puis-je gérer avec Windows Admin Center?

Windows Admin Center est optimisé pour Windows Server 2019 activer des thèmes clés dans la version de Windows Server 2019: cloud hybride scénarios et la gestion de l’infrastructure hyperconvergée en particulier. Bien que Windows Admin Center fonctionne mieux avec Windows Server 2019, il prend en charge la gestion de plusieurs versions que les clients utilisent déjà: Windows Server 2012 plus récente sont entièrement prises en charge. Il existe également des fonctionnalités limitées pour la gestion de Windows Server 2008 R2.

## Windows Admin Center remplace-t-il complètement l'ensemble des outils traditionnels intégrés et des outils d’administration de serveur distant?

Non. Bien que Windows Admin Center puisse gérer de nombreux scénarios courants, il ne remplace pas complètement les outils traditionnels de la console MMC (Microsoft Management Console). Pour une présentation détaillée à quels outils sont inclus avec Windows Admin Center, consultez les informations sur [la gestion des serveurs](..\use\manage-servers.md) dans notre documentation. Windows Admin Center comprend les fonctionnalités clés suivantes dans sa solution Gestionnaire de serveur:

* Affichage des ressources et utilisation des ressources
* Gestion des certificats
* Gestion des appareils
* Observateur d’événements
* Explorateur de fichiers
* Gestion du pare-feu
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
* La gestion des réplicas de stockage
* Gestion des mises à jour Windows
* Console PowerShell
* Connexion Bureau à distance

Windows Admin Center fournit également ces solutions:

* Gestion de l’ordinateur: fournit un sous-ensemble de fonctionnalités du Gestionnaire de serveur pour la gestion des ordinateurs clients Windows10
* Gestionnaire du cluster de basculement: prise en charge de la gestion de clusters de basculement et de ressources de cluster
* Gestionnaire de cluster hyperconvergé: offre une toute nouvelle expérience personnalisée pour les espaces de stockage direct et Hyper-V. Il comprend le Tableau de bord et met en évidence les graphiques et les alertes de surveillance.

Windows Admin Center est complémentaire et ne remplace pas les outils d’administration de serveur distant, car des rôles tels qu’Active Directory, DHCP, DNS, IIS n’ont pas encore de fonctions de gestion correspondantes qui apparaissent dans Windows Admin Center.

## Windows Admin Center peut-il servir à gérer le serveur gratuit Microsoft Hyper-V Server?

Oui. Windows Admin Center peut être utilisé pour gérer Microsoft Hyper-V Server2016 et Microsoft Hyper-V Server2012R2.

## Puis-je déployer Windows Admin Center sur un ordinateur Windows 10?

Oui, Windows Admin Center peut être installé sur Windows10 (version1709 ou version ultérieure) exécuté en mode bureau.  Windows Admin Center peut également être installé sur un serveur Windows Server 2016 ou supérieure, en mode passerelle et puis être accessible via un navigateur web sur un ordinateur Windows 10. [En savoir plus sur les options d’installation](..\plan\installation-options.md).

## J’ai entendu dire que Windows Admin Center utilise PowerShell en coulisses, je peux voir les scripts réels qu’il utilise?

Oui! la [fonctionnalité Showscript](..\use\get-started.md#view-powershell-scripts-used-in-windows-admin-center) a été ajoutée dans Windows Admin Center Preview 1806 et est désormais inclus dans le canal GA.

## Existe-t-il des plans pour que Windows Admin Center gère Windows Server2008 R2 ou une version antérieure?

Windows Admin Center prend désormais en charge des fonctionnalités **limitées** pour gérer Windows Server 2008 R2. Windows Admin Center s'appuie sur les fonctionnalités de PowerShell et des technologies de plateforme qui n’existent pas dans Windows Server2008R2 et les versions antérieures, ce qui rend la prise en charge complète irréalisable. Windows Server 2008/2008 R2 approchent de la fin du support en janvier 2020, donc Microsoft recommande aux clients [migrer vers Azure ou mettre à niveau vers la dernière version de Windows Server](https://www.microsoft.com/en-us/cloud-platform/windows-server-2008).

## Existe-t-il des plans pour que Windows Admin Center gère les connexions Linux?

Nous étudions ce point en raison de la demande du client, mais il n’y a actuellement aucun plan verrouillé et prise en charge peut se composer uniquement d’une connexion de console sur SSH.

## Quels sont les navigateurs web pris en charge par Windows Admin Center?

Les dernières versions des navigateurs Microsoft Edge (Windows10, version1709 ou version ultérieure) et Google Chrome sont testées et prises en charge sur Windows10. [Problèmes connus de vue navigateur spécifiques](..\support\known-issues.md#browser-specific-issues). Autres navigateurs web modernes ou autres plateformes ne font pas actuellement partie de notre matrice de test et ne sont donc pas *officiellement* pris en charge.

## Comment la sécurité est-elle gérée par Windows Admin Center?

Le trafic entre le navigateur et la passerelle Windows Admin Center utilise le protocole HTTPS. Le trafic depuis la passerelle vers les serveurs gérés utilise PowerShell et WMI standard sur WinRM. Nous prenons en charge la Solution de mot de passe d'administrateur local (LAPS), la délégation contrainte basée sur les ressources, le contrôle d’accès de passerelle à l’aide d’Active Directory ou d'Azure AD et le contrôle d’accès basé sur des rôles pour gérer les serveurs cibles.

## Windows Admin Center utilise-t-il CredSSP?

Oui, dans de rares cas Windows Admin Center requiert CredSSP. Cela est nécessaire pour transmettre vos informations d’identification pour l’authentification sur les ordinateurs au-delà du serveur spécifique que vous ciblez pour la gestion. Par exemple, si vous gérez des machines virtuelles sur le **serveur B**, mais que vous souhaitez stocker les fichiers vhdx pour les machines virtuelles sur un partage de fichiers hébergé par **le serveur C**, Windows Admin Center doit utiliser CredSSP pour s’authentifier avec **le serveur C** pour accéder à la partage de fichiers.

Windows Admin Center gère la configuration de CredSSP automatiquement après l’invite de consentement auprès de vous. Avant de configurer CredSSP, Windows Admin Center vérifie pour vous assurer que le système dispose de la récente CredSSP [mises à jour](https://support.microsoft.com/help/4093492/credssp-updates-for-cve-2018-0886-march-13-2018). Tandis que CredSSP est activé, il y aura un badge sur la vue d’ensemble de serveur, ainsi qu’une option pour le désactiver:

![CredSSP sur la vue d’ensemble du serveur](../media/CredSSP-overview.png)

CredSSP n’est actuellement utilisée dans les zones suivantes:

- À l’aide de décompose stockage SMB dans l’outil de machines virtuelles (l’exemple de ci-dessus.)
- À l’aide des mises à jour de l’outil dans le basculement ou cluster Hyperconvergé solutions de gestion, qui effectue [La mise à jour adaptée aux clusters](https://docs.microsoft.com/windows-server/failover-clustering/cluster-aware-updating) 

## Y a-t-il des dépendances cloud?

Windows Admin Center ne nécessite pas d’accès à Internet, ni Microsoft Azure. Windows Admin Center gère les instances Windows Server et Windows n’importe où: sur les systèmes physiques ou sur des machines virtuelles sur n’importe quel hyperviseur ou en cours d’exécution dans un cloud. Bien que l’intégration avec les différents services Azure sera ajoutée au fil du temps, il s’agira de fonctionnalités à valeur ajoutée facultatives et on non obligatoires pour utiliser Windows Admin Center.

## Existe-t-il d’autres dépendances ou conditions préalables?

Windows Admin Center peut être installé sur Windows10 Fall Anniversary Update (1709) ou une version ultérieure, ou Windows Server2016 ou une version plus récente. Pour gérer Windows Server2008R2, 2012 ou 2012R2, l’installation de Windows Management Framework5.1 est requise sur ces serveurs. Il n’y a pas d'autres dépendances. IIS n’est pas requis, les agents ne sont pas nécessaires, SQL Server n’est pas requis.

## Qu’advient-il de l’extensibilité et du support tiers?

Windows Admin Center a un kit de développement disponible pour que tout le monde peut écrire leur propres extension. En tant que plate-forme, développer notre écosystème et favoriser l’extension des partenaires est une priorité essentielle depuis le début. [En savoir plus sur le SDK Windows Admin Center](..\extend\extensibility-overview.md).

## Puis-je gérer une infrastructure hyperconvergée avec Windows Admin Center?

Oui. Windows Admin Center prend en charge la gestion de clusters hyperconvergés exécutant Windows Server 2016 ou Windows Server 2019. La solution du Gestionnaire de cluster hyperconvergé dans Windows Admin Center a été précédemment dans l’aperçu, mais est désormais **disponible**avec de nouvelles fonctionnalités dans l’aperçu. Pour plus d’informations, voir [en savoir plus sur la gestion d'infrastructure hyperconvergée](..\use\manage-hyper-converged.md).

## Windows Admin Center nécessite-t-il System Center?

Non. Windows Admin Center est complémentaire de System Center, mais System Center n’est pas obligatoire. [En savoir plus sur Windows Admin Center et System Center](related-management.md#system-center).

## Windows Admin Center peut-il remplacer System Center Virtual Machine Manager (SCVMM)?

Windows Admin Center et SCVMM sont complémentaires; Windows Admin Center est destiné à remplacer les composants logiciels enfichables de la console MMC (Microsoft Management Console) classique et l’expérience d’administration de serveur.  Windows Admin Center n’est pas destiné à remplacer les aspects de surveillance de SCVMM. [En savoir plus sur Windows Admin Center et System Center](related-management.md#system-center).

## Qu’est-ce que Windows Admin Center Preview et quelle est la version qui me convient le mieux?

Il existe deux versions de Windows Admin Center disponibles en téléchargement:

### Windows Admin Center

* Cette version convient aux administrateurs informatiques qui ne sont pas en mesure de faire des mises à jour fréquemment ou qui souhaitent davantage de temps de validation pour les versions qu'ils utilisent en production. Notre version (GA) disponible actuelle est Windows Admin Center 1904.
* [!INCLUDE [support-policy](../includes/support-policy.md)]
* Pour obtenir la dernière version, [Téléchargez ici](https://aka.ms/WACDownload).

### Windows Admin Center Preview

>[!NOTE]
>La version GA actuelle (Windows Admin Center 1904) contient toutes les fonctionnalités d’aperçu précédente.
>La version d’évaluation Insider retourne dans les prochains mois.

* Cette version convient aux administrateurs informatiques qui souhaitent les toutes dernières fonctionnalités à une cadence régulière. Notre objectif est de fournir des versions de mise à jour ultérieure chaque mois environ. La plate-forme de base continue à être prête pour la production et la licence fournit des droits d’utilisation de production. Toutefois, notez que vous verrez l’introduction de nouveaux outils et fonctionnalités clairement marqués comme étant PREVIEW et conviennent à l'évaluation et au test.
* Pour obtenir la dernière version d’évaluation Insider Preview, les Insiders inscrits peuvent télécharger Windows Admin Center Preview directement à partir de [Page de téléchargement de Windows Server Insider Preview](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewserver), sous la Additional downloads. Si vous n’êtes pas encore inscrit comme Insider, voir [Prise en main avec Windows Server](https://insider.windows.com/en-us/for-business-getting-started-server/) sur le portail Windows Insiders pour Entreprises.

## Pourquoi «Windows Admin Center» a-t-il été choisi comme nom définitif du «projet Honolulu»?

Windows Admin Center est le nom de produit officiel du «projet Honolulu» et renforce notre vision d’une expérience intégrée pour les administrateurs informatiques dans un large éventail de scénarios de gestion et d’administration principaux. Il met également en avant le fait que notre orientation client sur les besoins des administrateurs informatiques est la priorité centrale de nos investissements et de nos publications.

## Où puis-je en savoir plus sur Windows Admin Center, ou obtenir plus d’informations sur les rubriques ci-dessus?

Notre [page de lancement](https://aka.ms/WindowsAdminCenter) est le meilleur point de départ et propose des liens vers notre contenu de documentation nouvellement classé, l’emplacement de téléchargement, comment fournir des commentaires, des informations de référence et d'autres ressources.

## Quel est l’historique des versions de Windows Admin Center?

[Afficher l’historique de version ici.](..\overview.md#release-history)

## Je rencontre un problème avec Windows Admin Center, où puis-je obtenir de l’aide?

Consultez notre [guide de résolution des problèmes](..\use\troubleshooting.md) et notre liste de [problèmes connus](..\use\known-issues.md).
