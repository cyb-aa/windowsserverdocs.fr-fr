---
title: Nouveautés de Windows Server, version 1803
description: Présentation des nouvelles fonctionnalités de calcul, d’identité, de gestion, d’automatisation, de mise en réseau, de sécurité et de stockage.
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
author: greg-lindsay
ms.author: greg-lindsay
ms.localizationpriority: high
ms.date: 05/07/2018
ms.openlocfilehash: a489d3f8958304d685116186f5db9e1c854114bf
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/17/2019
ms.locfileid: "65976539"
---
# <a name="whats-new-in-windows-server-version-1803"></a>Nouveautés de Windows Server version 1803

>S'applique à : Windows Server (canal semi-annuel)

<img src="../media/landing-icons/new.png" style='float:left; padding:.5em;' alt="Icon showing a newspaper">&nbsp;Pour découvrir les dernières fonctionnalités de Windows, consultez [Nouveautés de Windows Server](whats-new-in-windows-server.md). Le contenu de cette section décrit les nouveautés et les modifications de Windows Server, version 1803. Les nouvelles fonctionnalités et modifications répertoriées ici sont celles qui sont susceptibles d'avoir l'impact le plus important quand vous utilisez cette version. Consultez également [Mise à jour Windows Server (canal semi-annuel)](https://cloudblogs.microsoft.com/windowsserver/2018/03/29/windows-server-semi-annual-channel-update/).

## <a name="windows-admin-center"></a>Windows Admin Center

Le projet Honolulu s’appelle désormais **Windows Admin Center**.
<br>&nbsp;
> [!video https://www.youtube.com/embed/WCWxAp27ERk?autoplay=false]

[Windows Admin Center](https://docs.microsoft.com/windows-server/manage/windows-admin-center/overview) consolide tous les aspects de la gestion des serveurs locaux et distants. Windows Admin Center est une expérience de gestion basée sur un navigateur et déployée localement. Comme elle ne nécessite pas de connexion Internet, cela vous donne un contrôle total sur tous les aspects de votre déploiement de Windows Server.

## <a name="windows-server-release-strategy"></a>Stratégie de publication de Windows Server

Windows Server version 1709 a été publiée en septembre 2017 en tant que première version du canal semi-annuel. Le canal semi-annuel suit une cadence de publication plus rapide et répond aux commentaires de ceux qui souhaitent une innovation rapide tous les mois. Il complète le canal de maintenance à long terme, dans lequel de nouvelles versions sont publiées tous les 2 à 3 ans.

En fonction de la télémétrie et des commentaires, ces canaux ont démontré qu’ils respectent la stratégie générale suivante :
- Le canal semi-annuel est idéal pour les applications modernes et les scénarios d’innovation, tels que les conteneurs et les micro-services.
- Le canal de maintenance à long terme est la version privilégiée pour les scénarios d’infrastructure principaux, par exemple un centre de données défini par logiciel et une infrastructure hyperconvergée (HCI). 

Les scénarios spécifiques pour le canal semi-annuel et le canal de maintenance à long terme sont les suivants :

|   | Canal de maintenance à long terme |  Canal semi-annuel |
| ------------- | ------------- | ------------ |
| Scénarios recommandés     | Serveurs de fichiers usage général, charges de travail internes et tierces, applications traditionnelles, rôles d’infrastructure, centre de données défini par logiciel et infrastructure hyperconvergée  | Scénarios d’applications en conteneur, d’hôtes de conteneur et d’application bénéficiant d’une innovation plus rapide |
| Nouvelles versions  | Tous les 2 à 3 ans  | Tous les 6 mois |
| Support  | 5 ans de support standard + 5 ans de support étendu  | 18 mois |
| Éditions  | Toutes les éditions disponibles de Windows Server  | Éditions Standard et Datacenter |
| Qui peut l’utiliser  | Tous les clients par le biais de tous les canaux | Clients Software Assurance et cloud uniquement |
| Options d’installation  | Server Core et Serveur avec Expérience utilisateur  | Server Core pour hôte et image de conteneur et image du conteneur Nano Server |

## <a name="application-platform-and-containers"></a>Conteneurs et plateforme d’applications

- Optimisation
    - L’image de base du conteneur Server Core est réduite de 30 % à partir de Windows Server, version 1709. 
    - La compatibilité des applications est également améliorée pour vous aider dans la conteneurisation d’applications traditionnelles.
    - Les performances de démarrage de conteneur et d’exécution sont également améliorées grâce à divers correctifs et optimisations.
- Mise en réseau du conteneur : la prise en charge des proxy Localhost et http a été ajoutée et la scalabilité et le temps de démarrage du conteneur ont été améliorés.
- Outils : la prise en charge de Curl.exe, Tar.exe et SSH a été améliorée pour compléter PowerShell pour les scénarios de création et de débogage.

### <a name="server-core-container-image"></a>Image de conteneur Server Core

Un conteneur Server Core plus petit avec une meilleure compatibilité des applications est désormais disponible. Des informations détaillées sont disponibles [ici](https://blogs.technet.microsoft.com/virtualization/2018/01/22/a-smaller-windows-server-core-container-with-better-application-compatibility/).

- Des fonctionnalités et des rôles facultatifs inutilisés ont été supprimés. Pour plus d’informations, consultez [Fonctionnalités, rôles et services de rôle non présents dans les conteneurs Server Core](../administration/server-core/server-core-container-removed-roles.md).
    - Taille de téléchargement diminuée à 1,58 Go, soit 30 % de réduction par rapport à Windows Server, version 1709.
    - Taille sur disque réduite à 3,61 Go, soit 20 % de réduction par rapport à Windows Server, version 1709.
- L’image de conteneur Nano Server est inférieure à 100 Mo

### <a name="windows-subsystem-for-linux-wsl"></a>Sous-système Windows pour Linux (WSL)

WSL permet aux administrateurs de serveur d’utiliser les outils et les scripts existants de Linux sur Windows Server. De nombreuses améliorations présentées dans le [blog de la ligne de commande](https://blogs.msdn.microsoft.com/commandline/tag/wsl/) font désormais partie de Windows Server, y compris les tâches en arrière-plan, DriveFS, WSLPath et bien plus encore.

### <a name="kubernetes"></a>Kubernetes 

Kubernetes (généralement appelé K8s) est un système open source pour l’automatisation du déploiement, la mise à l’échelle et la gestion des applications en conteneur développées sous la direction de la [Cloud Native Computing Foundation](https://www.cncf.io). 

Dans Windows Server version 1709, les utilisateurs pouvaient bénéficier de Kubernetes sur les fonctionnalités de mise en réseau Windows, notamment les suivantes :
- Compartiments de pods partagés : les pods d’infrastructure et de travail partagent désormais un compartiment réseau (comparable à un espace de noms Linux)
- Optimisation du point de terminaison : grâce au partage des compartiments, les services de conteneurs doivent suivre au moins deux fois moins de points de terminaison qu’auparavant.
- Optimisation du chemin des données : les améliorations apportées à la plateforme de filtrage virtuel (VFP, Virtual Filtering Platform) et au service HNS (Host Networking Service) permettent un équilibrage de charge basé sur le noyau.

Avec la publication de Windows Server, version 1803, davantage de fonctionnalités seront disponibles dans les prochaines versions de Kubernetes : 
- [Plug-ins de stockage](https://github.com/Microsoft/K8s-Storage-Plugins) pour les conteneurs Windows orchestrés par Kubernetes.
- Mise en réseau à l’échelle du cloud à travers des initiatives telles que notre partenariat avec la prise en charge de [Tigera sur le Projet Calico](https://cloudblogs.microsoft.com/windowsserver/2017/12/07/securing-modernized-apps-and-simplified-networking-on-windows-with-calico/).
- Prise en charge de la plateforme Windows des pods isolés Hyper-V avec plusieurs conteneurs par pod.

### <a name="application-compatibility-and-feature-parity-issues-fixed"></a>Problèmes de compatibilité des applications et de parité des fonctionnalité corrigés

- Microsoft Message Queuing (MSMQ) s’installe désormais dans un conteneur Server Core.
- Un problème d’interruption des compteurs de performance ASP.net a été corrigé.
- Un problème dans lequel les services s’exécutant dans des conteneurs ne recevaient pas de notification d’arrêt a été corrigé.
    - Plus précisément, la notification est remplacée par CTRL_SHUTDOWN_EVENT pour les images basées sur un conteneur Server Core ou Nano Server. En outre, la notification dans les images de conteneur basées sur Server Core est étendue pour affecter tous les processus en cours d’exécution dans le conteneur, y compris l’envoi de notifications d’arrêt de service pour les services qui s’exécutent dans le conteneur.
- Une incompatibilité de la charge de docker pull et docker avec le paramètre de stratégie qui détermine si la protection BitLocker est requise pour que les lecteurs de données fixes soient accessibles en écriture (FDVDenyWriteAccess) a été corrigée. 

## <a name="storage"></a>Stockage

Avec cette version, il est possible d’empêcher le service Gestionnaire de ressources du serveur de fichiers de créer un journal des modifications (également appelé journal USN) sur tous les volumes au démarrage du service. Cela permet d’économiser de l’espace sur chaque volume, mais désactivera la classification des fichiers en temps réel. Pour plus d’informations, consultez [Vue d’ensemble du Gestionnaire de ressources du serveur de fichiers](https://docs.microsoft.com/windows-server/storage/fsrm/fsrm-overview).

## <a name="features-added-to-server-core"></a>Fonctionnalités ajoutées à Server Core

Le rôle Serveur de transport dans le rôle des Services de déploiement Windows (WDS) a été ajouté à Server Core.

Le Serveur de transport contient uniquement les composants de mise en réseau principaux de WDS. Vous pouvez utiliser le Serveur de transport pour créer des espaces de noms de multidiffusion qui transmettent les données (y compris les images du système d’exploitation) à partir d’un serveur autonome. Vous pouvez également l’utiliser si vous souhaitez disposer d’un serveur PXE qui permette aux clients d’utiliser un démarrage PXE et de télécharger votre propre application d’installation personnalisée. Vous devez utiliser cette option si vous souhaitez utiliser l’un de ces scénarios.

Vous pouvez utiliser la commande Windows PowerShell suivante pour activer le service de Serveur de transport sur Server Core :

```
Install-WindowsFeature -Name WDS
```

## <a name="see-also"></a>Voir également

[Informations de publication de Windows Server](https://docs.microsoft.com/windows-server/get-started/windows-server-release-info)<br>
[Nouveautés de Windows 10 version 1803 concernant les contenus destinés aux professionnels de l’informatique](https://docs.microsoft.com/windows/whats-new/whats-new-windows-10-version-1803)
