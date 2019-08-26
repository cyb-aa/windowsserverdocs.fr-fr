---
title: Vue d’ensemble de Windows Admin Center
description: Découvrez comment gérer Windows Server à l’aide de Windows Admin Center (projet Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 08/22/2019
ms.localizationpriority: high
ms.prod: windows-server-threshold
ms.openlocfilehash: 7af852ba934de2dd29a76972d7a7ec73606e9b4c
ms.sourcegitcommit: 4fa147d552481d8279a5390f458a9f7788061977
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/23/2019
ms.locfileid: "70009120"
---
# <a name="windows-admin-center"></a>Windows Admin Center

> S’applique à : Windows Admin Center, Windows Admin Center Preview

**Windows Admin Center** (ancien nom de code : **Projet Honolulu**) est une évolution des outils de gestion intégrés de Windows Server. Il s’agit d’un seul écran qui regroupe tous les aspects de la gestion des serveurs locaux et distants. Comme il s’agit d’une expérience de gestion basée sur navigateur et déployée localement, ni Azure ni une connexion Internet ne sont nécessaires. Windows Admin Center vous offre un contrôle total sur tous les aspects de votre déploiement, notamment les réseaux privés qui ne sont pas connectés à Internet.

## <a name="introduction"></a>Introduction

>[!VIDEO https://www.youtube.com/embed/PcQj6ZklmK0]

![Image de Windows Admin Center](media/WAC1809Poster_thumb.PNG)

[Télécharger le PDF](https://github.com/MicrosoftDocs/windowsserverdocs/raw/master/WindowsServerDocs/manage/windows-admin-center/media/WindowsAdminCenter1809Poster.pdf)

## <a name="quick-start"></a>Démarrage rapide

Vous pouvez installer et exécuter Windows Admin Center dans votre environnement en quelques minutes :

1. [Télécharger](https://aka.ms/windowsadmincenter)
2. [Installer](deploy/install.md)
3. [Prise en main](use/get-started.md)

## <a name="contents-at-a-glance"></a>Aperçu rapide du contenu

<table>
    <tr></tr>
    <tr>
        <td style="vertical-align: top;">
            <h3>Comprendre</h3>
            <ul>
            <li><a href="understand/what-is.md">Qu’est-ce que Windows Admin Center ?</a>
            <li><a href="understand/faq.md">Questions fréquentes (FAQ)</a>
            <li><a href="understand/case-studies.md">Études de cas</a>
            <li><a href="understand/related-management.md">Produits de gestion associés</a>
            <li><a href="understand/videos.md">Vidéos</a>
            </ul>
        </td>
        <td style="vertical-align: top;">
            <h3>Planifier</h3>
            <ul>
            <li><a href="plan/installation-options.md">Quel type d’installation vous convient ?</a>
            <li><a href="plan/user-access-options.md">Options d’accès utilisateur</a>
            <br>
            </ul>
        </td>
    </tr>
    <tr>
        <td style="vertical-align: top;">
            <h3>Déployer</h3>
            <ul>
            <li><a href="deploy/prepare-environment.md">Préparer votre environnement</a>
            <li><a href="deploy/install.md">Installer Windows Admin Center</a>
            <li><a href="deploy/high-availability.md">Activer la haute disponibilité</a>
         </ul>
        </td>
        <td style="vertical-align: top;">
            <h3>Configuration</h3>
            <ul>
            <li><a href="configure/settings.md">Paramètres de Windows Admin Center</a>
            <li><a href="configure/user-access-control.md">Autorisations et contrôle d’accès utilisateur</a>
            <li><a href="configure/shared-connections.md">Connexions partagées</a>
            <li><a href="configure/using-extensions.md">Extensions</a>
            </ul>
        </td>
    </tr>
    <tr>
        <td style="vertical-align: top;">
            <h3>Utilisez</h3>
            <ul>
            <li><a href="use/get-started.md">Démarrer et ajouter des connexions</a>
            <li><a href="use/manage-servers.md">Gérer les serveurs</a>
            <li><a href="use/manage-hyper-converged.md">Gérer l’infrastructure hyperconvergée</a>
            <li><a href="use/manage-failover-clusters.md">Gérer les clusters de basculement</a>
            <li><a href="use/manage-virtual-machines.md">Gérer les machines virtuelles</a>
            <li><a href="use/logging.md">Journalisation</a>
            </ul>
        </td>
        <td style="vertical-align: top;">
            <h3>Se connecter à Azure</h3>
            <ul>
            <li><a href="azure/index.md">Services hybrides Azure</a></li>
            <li><a href="azure/azure-integration.md">Connecter Windows Admin Center à Azure</a></li>
            <li><a href="azure/deploy-wac-in-azure.md">Déployer Windows Admin Center dans Azure</a></li>
            <li><a href="azure/manage-azure-vms.md">Gérer les machines virtuelles Azure avec Windows Admin Center</a></li>
            </ul>
        </td>
    </tr>
    <tr>
            <td style="vertical-align: top;">
            <h3>Support</h3>
            <ul>
            <li><a href="support/index.md">Politique de support</a>
            <li><a href="support/troubleshooting.md">Étapes courantes de dépannage</a>
            <li><a href="support/known-issues.md">Problèmes connus</a>
            </ul>
        </td>
            <td style="vertical-align: top;">
            <h3>Étendre</h3>
            <ul>
            <li><a href="extend/extensibility-overview.md">Vue d’ensemble des extensions</a>
            <li><a href="extend/understand-extensions.md">Présentation des extensions</a>
            <li><a href="extend/developing-extensions.md">Développer une extension</a>
            <li><a href="extend/publish-extensions.md">Guides</a>
            <li><a href="extend/publish-extensions.md">Publication d’extensions</a>
            </ul>
        </td>
    </tr>

</table>

## <a name="release-history"></a>Historique des versions

Découvrez nos toutes dernières fonctionnalités publiées :

- Version [1908](https://aka.ms/wac1908) : comprend les mises à jour visuelles, Packetmon, FlowLog, l’intégration des clusters dans Azure Monitor et la prise en charge de WinRM sur HTTPS (port 5986).
- Version [1907](https://aka.ms/wac1907) - ajout de liens d’estimation des coûts Azure et améliorations apportées à l’importation/exportation et à l’étiquetage des machines virtuelles.
- Version [1906](https://aka.ms/wac1906) - Importer/exporter des machines virtuelles, changer de compte Azure, ajouter des connexions depuis Azure, expérience des paramètres de connectivité, améliorations des performances et outil de profilage des performances.
- La version 1904.1 est la toute dernière version GA - mise à jour de maintenance pour améliorer la stabilité des plug-ins de la passerelle.
- La version [1904](https://aka.ms/wac1904) est la version GA qui a introduit l’outil Azure Hybrid Services et mis les fonctionnalités jusqu’ici en préversion dans le canal de la disponibilité générale.
- La version [1903](https://aka.ms/wac1903) a ajouté les e-mails de notification depuis Azure Monitor, la possibilité d’ajouter des connexions au serveur ou PC depuis Active Directory et de nouveaux outils pour gérer Active Directory, DHCP et DNS.
- La version [1902](https://aka.ms/wac1902) a ajouté une liste de connexions partagées et des améliorations de la gestion du réseau SDN (software defined network), notamment de nouveaux outils SDN pour gérer les listes ACL, les connexions de passerelle et les réseaux logiques.
- La version [1812](https://aka.ms/wac1812) ajoute un thème foncé (en préversion), les paramètres de configuration de l’alimentation, des informations BMC et la prise en charge de PowerShell pour gérer les [extensions](./configure/using-extensions.md#manage-extensions-with-powershell) et les [connexions](./use/get-started.md#use-powershell-to-import-or-export-your-connections-with-tags).
- La version [1809.5](https://aka.ms/wac1809.5) est une mise à jour GA cumulative qui présente diverses améliorations de la qualité et du fonctionnement, ainsi que des résolutions de bogues dans toute la plateforme et quelques nouvelles fonctionnalités dans la solution de gestion d’infrastructure hyperconvergée.
- La version [1809](https://cloudblogs.microsoft.com/windowsserver/2018/09/20/windows-admin-center-1809-and-sdk-now-generally-available/) est une version GA qui a mis les fonctionnalités jusqu’ici en préversion dans le canal GA.
- La version [1808](https://aka.ms/WACPreview1808-InsiderBlog) a ajouté l’outil Applications installées, un grand nombre d’améliorations et d’importantes mises à jour de la préversion du SDK.
- La version [1807](https://aka.ms/WACPreview1807-InsiderBlog) a ajouté une expérience de connexion à Azure simplifiée, des améliorations à la page d’inventaire des machines virtuelles, la fonctionnalité de partage de fichiers, l'intégration de la gestion des mises à jour Azure et plus encore. 
- La version [1806](https://aka.ms/WACPreview1806-InsiderBlog) a ajouté l’affichage du script PowerShell, la gestion SDN, les connexions à 2008 R2, le réseau SDN, les tâches planifiées et beaucoup d’autres améliorations.
- Version 1804.25 - mise à jour de maintenance pour prendre en charge les utilisateurs qui installent Windows Admin Center dans des environnements entièrement hors connexion.
- Version [1804](https://cloudblogs.microsoft.com/windowsserver/2018/04/12/announcing-windows-admin-center-our-reimagined-management-experience/) - Le projet Honolulu devient Windows Admin Center et ajoute des fonctionnalités de sécurité et le contrôle d’accès en fonction du rôle. Notre première version GA.
- La version [1803](https://blogs.windows.com/windowsexperience/2018/03/13/announcing-project-honolulu-technical-preview-1803-and-rsat-insider-preview-for-windows-10) a ajouté la prise en charge du contrôle d’accès Azure AD, la journalisation détaillée, le contenu redimensionnable et une série d’améliorations d’outil.
- La version [1802](https://blogs.windows.com/windowsexperience/2018/02/13/announcing-windows-server-insider-preview-build-17093-project-honolulu-technical-preview-1802) a ajouté la prise en charge de l’accessibilité, de la localisation, des déploiements à haute disponibilité, du balisage, des paramètres de l’hôte Hyper-V et de l’authentification de passerelle.
- La version [1712](https://blogs.windows.com/windowsexperience/2017/12/19/announcing-project-honolulu-technical-preview-1712-build-05002) a ajouté d’autres fonctionnalités de machine virtuelle et des améliorations des performances dans tous les outils.
- La version [1711](https://cloudblogs.microsoft.com/windowsserver/2017/12/01/1711-update-to-project-honolulu-technical-preview-is-now-available/) a ajouté des outils très attendus (Bureau à distance et PowerShell), ainsi que d’autres améliorations.
- La version [1709](https://cloudblogs.microsoft.com/windowsserver/2017/09/22/project-honolulu-technical-preview-is-now-available-for-download/) est la première préversion publique que nous avons publiée.

## <a name="stay-updated"></a>Restez informé

![ ](//img-prod-cms-rt-microsoft-com.akamaized.net/cms/api/am/imageFileData/REOolR)[Suivez-nous sur Twitter](https://twitter.com/servermgmt)

![](//img-prod-cms-rt-microsoft-com.akamaized.net/cms/api/am/imageFileData/REOtyw)[Consultez nos blogs](https://blogs.technet.microsoft.com/servermanagement/)
