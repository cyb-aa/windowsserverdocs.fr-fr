---
title: Vue d’ensemble de Windows Admin Center
description: Découvrez comment gérer Windows Server à l’aide de Windows Admin Center (projet Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 01/07/2020
ms.localizationpriority: high
ms.prod: windows-server
ms.openlocfilehash: 7b3a75258086a73fbd618c2e8221454d7e616556
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75949980"
---
# <a name="windows-admin-center"></a>Windows Admin Center

> S'applique à : Windows Admin Center, Windows Admin Center Preview

Windows Admin Center est une application basée sur navigateur et déployée localement qui permet de gérer les serveurs Windows, les clusters, l’infrastructure hyperconvergée ainsi que les PC Windows 10. Il est fourni sans coût supplémentaire en plus de Windows et est prêt à être utilisé en production.

Pour découvrir les nouveautés, consultez [Historique des versions](support/release-history.md).

## <a name="download-now"></a>Télécharger maintenant

**Téléchargez [Windows Admin Center](https://www.microsoft.com/evalcenter/evaluate-windows-admin-center)** à partir du centre d’évaluation Microsoft. Même si vous pouvez voir « Commencer l’évaluation », il s’agit de la version proposée en disponibilité générale pour une utilisation en production qui est incluse dans le cadre de votre licence Windows ou Windows Server.

Pour obtenir de l’aide sur l’installation, consultez [Installer](deploy/install.md). Pour obtenir des conseils concernant l’utilisation de Windows Admin Center, consultez [Bien démarrer](use/get-started.md).

Pour mettre à jour les versions de Windows Admin Center qui ne sont pas des préversions, utilisez Microsoft Update ou téléchargez et installez manuellement Windows Admin Center. Chaque version non-Preview de Windows Admin Center est prise en charge jusqu’à 30 jours après la publication de la prochaine version non-Preview. Pour plus d’informations, consultez notre [politique de support](support/index.md).

## <a name="windows-admin-center-scenarios"></a>Scénarios d’utilisation de Windows Admin Center

Voici quelques-unes des tâches que vous pouvez accomplir avec Windows Admin Center :

|     |     |
| --- | --- |
| ![](media/simple-icon.png)| **Simplifier la gestion des serveurs** <br/> Gérez vos serveurs et clusters avec des versions modernisées d’outils familiers tels que le Gestionnaire de serveur. Effectuez une installation en moins de cinq minutes et gérez les serveurs de votre environnement immédiatement, sans configuration cible requise. Pour plus d’informations, consultez [Qu’est-ce que Windows Admin Center ?](understand/what-is.md). |
| ![](media/future-icon.png)| **Utiliser des solutions hybrides** <br/> L’intégration à Azure vous permet de connecter éventuellement vos serveurs locaux à des services cloud appropriés. Pour plus d’informations, consultez [Services hybrides Azure](azure/index.md). |
| ![](media/secure-icon.png)| **Simplifier la gestion hyperconvergée** <br/> Simplifiez la gestion des clusters hyperconvergés Azure Stack HCI ou Windows Server. Utilisez des charges de travail simplifiées pour créer et gérer des machines virtuelles, des volumes Espaces de stockage direct, des solutions SDN (Software-Defined Networking), etc. Pour plus d’informations, consultez [Gérer une infrastructure hyperconvergée avec Windows Admin Center](use/manage-hyper-converged.md).|

Voici une vidéo qui vous donne une vue d’ensemble, suivie d’une affiche présentant plus de détails :
>[!VIDEO https://www.youtube.com/embed/WCWxAp27ERk]

[![Affiche Windows Admin Center](media/WAC1910Poster_thumb_small.PNG)](media/WAC1910Poster_thumb.png)

[Télécharger le PDF](https://github.com/MicrosoftDocs/windowsserverdocs/raw/master/WindowsServerDocs/manage/windows-admin-center/media/WindowsAdminCenter1910Poster.pdf)


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
            <h3>Configurer</h3>
            <ul>
            <li><a href="configure/settings.md">Paramètres de Windows Admin Center</a>
            <li><a href="configure/user-access-control.md">Autorisations et contrôle d’accès utilisateur</a>
            <li><a href="configure/shared-connections.md">Connexions partagées</a>
            <li><a href="configure/using-extensions.md">Extensions</a>
            <li><a href="configure/use-powershell.md">Automatiser avec PowerShell</a>
            </ul>
        </td>
    </tr>
    <tr>
        <td style="vertical-align: top;">
            <h3>Utiliser</h3>
            <ul>
            <li><a href="use/get-started.md">Démarrer et ajouter des connexions</a>
            <li><a href="use/manage-servers.md">Gérer les serveurs</a>
            <li><a href="use/deploy-hyperconverged-infrastructure.md">Déployer une infrastructure hyperconvergée</a>
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
            <h3>Assistance</h3>
            <ul>
            <li><a href="support/release-history.md">Historique des versions</a>
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

## <a name="video-based-learning"></a>Apprentissage vidéo

Voici quelques vidéos extraites de sessions Microsoft Ignite 2019 :

- [Windows Admin Center : libérer les fonctionnalités hybrides d’Azure](https://aka.ms/WAC-BRK3165)
- [Windows Admin Center : nouveautés et avenir](https://aka.ms/WAC-BRK2048)
- [Superviser, sécuriser et mettre à jour automatiquement vos serveurs locaux dans Azure à l’aide de Windows Admin Center](https://aka.ms/WAC-THR2146)
- [Travailler de manière plus productive avec les extensions tierces de Windows Admin Center](https://aka.ms/WAC-THR2140)
- [Devenir un expert de Windows Admin Center : bonnes pratiques pour le déploiement, la configuration et la sécurité](https://aka.ms/WAC-THR2135)
- [Windows Admin Center : combiner System Center et Microsoft Azure pour plus de valeur ajoutée](https://aka.ms/WAC-THR2176)
- [Comment utiliser les services hybrides Microsoft Azure avec Windows Admin Center et Windows Server](https://aka.ms/WAC-THR2073)
- [Questions et réponses en direct : gérer votre environnement serveur hybride avec Windows Admin Center](https://aka.ms/WAC-MLS1055)
- [Parcours d’apprentissage : technologies de gestion hybrides](https://aka.ms/WAC-HybridMgmtTech)
- [Atelier pratique : Windows Admin Center et services hybrides](https://aka.ms/WAC-HOL2019)

Voici quelques vidéos extraites de sessions Windows Server Summit 2019 :

- [Passer à l’hybride avec Windows Admin Center](https://aka.ms/WAC-WSS2019-GoHybridWAC)
- [Nouveautés de Windows Admin Center v1904](https://aka.ms/WAC-WSS2019-WhatsNewv1904)

Et voici quelques ressources supplémentaires :

- [Gestion des serveurs réinventée grâce à Windows Admin Center](https://aka.ms/WAC-ServerMgmtReimagined)
- [Gérer des serveurs et des machines virtuelles en tout lieu avec Windows Admin Center](https://aka.ms/WAC-Webinar2019)
- [Bien démarrer avec Windows Admin Center](https://www.youtube.com/embed/PcQj6ZklmK0)

## <a name="see-how-customers-are-benefitting-from-windows-admin-center"></a>Découvrez les avantages que Windows Admin Center offre aux clients

|     |
| --- |
| « Avec [Windows Admin Center], nous avons réduit de plus de 75 % le temps et les efforts nécessaires pour gérer le système de gestion. »<br> *- Rand Morimoto, directeur de Convergent Computing* |
| « Grâce à [Windows Admin Center], nous pouvons gérer nos clients à distance sans problème à partir d’un portail HTML5 et avec une intégration complète à Azure Active Directory, et nous pouvons améliorer la sécurité avec l’authentification multifacteur. »<br/> *- Silvio Di Benedetto, fondateur et consultant senior d'Inside Technologies* |
| « Nous avons pu déployer plus efficacement les références SKU [Server Core], ce qui a permis d’améliorer l’efficacité, la sécurité et l’automatisation des ressources, d’atteindre un bon niveau de productivité et de réduire les erreurs pouvant se produire lors de l’utilisation de scripts seuls. » <br/> *- Guglielmo Mengora, fondateur et PDG de VaiSulWeb* |
| « Avec [Windows Admin Center], les clients, en particulier du marché des PME, disposent d’un outil facile à utiliser pour gérer leur infrastructure interne. Il permet de réduire les tâches administratives et de gagner beaucoup de temps. En plus, [Windows Admin Center] n’engendre aucuns frais de licence supplémentaires ! » <br/> *- Helmut Otto, directeur général de SecureGUARD* |

[Découvrez d’autres entreprises qui utilisent Windows Admin Center dans leur environnement de production.](understand/case-studies.md)

## <a name="related-products"></a>Produits connexes

Windows Admin Center est conçu pour gérer un seul serveur ou cluster. Il complète, mais ne remplace pas les solutions de supervision et de gestion de Microsoft comme les Outils d’administration de serveur distant (RSAT), System Center, Intune ou Azure Stack.

[Découvrez comment Windows Admin Center complète les autres solutions de gestion de Microsoft.](understand/related-management.md)

## <a name="stay-updated"></a>Restez informé

![](//img-prod-cms-rt-microsoft-com.akamaized.net/cms/api/am/imageFileData/REOolR)[Suivez-nous sur Twitter](https://twitter.com/servermgmt)

![](//img-prod-cms-rt-microsoft-com.akamaized.net/cms/api/am/imageFileData/REOtyw)[Consultez nos blogs](https://blogs.technet.microsoft.com/servermanagement/)
