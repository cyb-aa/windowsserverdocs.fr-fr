---
title: Serveur Windows qui se connecte aux services Azure hybride
description: Vous pouvez étendre les déploiements locaux de Windows Server sur le cloud à l’aide des services Azure hybride.
ms.technology: manage
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 04/12/2019
ms.openlocfilehash: 625bd4b79d277dfaa81767cd781c2ba1316d637e
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296934"
---
# Serveur Windows qui se connecte aux services Azure hybride

>S’applique à: Windows Server 2019, Windows Server 2016

Vous pouvez étendre les déploiements locaux de Windows Server sur le cloud à l’aide des services Azure hybride. Ces services cloud fournissent un tableau de fonctions intégrées utiles, y compris les éléments suivants:

- Protéger les machines virtuelles et utiliser cloud de sauvegarde et reprise haute disponibilité/avec Azure Site Recovery. 
- Suivre ce qui se passe dans vos applications, le réseau et l’infrastructure à l’aide d’analytique avancées et d’apprentissage automatique dans le moniteur Azure. 
- Simplifier la connectivité réseau vers Azure avec l’adaptateur réseau Azure.
- Machines virtuelles maintenir à jour avec la gestion des mises à jour Azure.

Services Azure hybride fonctionnent avec les serveurs Windows dans les opérations suivantes:

- Autonomes serveurs physiques et les machines virtuelles (VM)
- Clusters, y compris les clusters hyperconvergés certifiés par les programmes de [Windows Server (WSSD)](https://www.microsoft.com/en-us/cloud-platform/software-defined-datacenter) et [Azure Stack HCI](../../../azure-stack-hci/index.md)

Alors que vous pouvez configurer les services hybrides plus Azure à l’aide du portail Azure et téléchargement d’une ou deux, un grand nombre sont intégrés directement dans Windows Admin Center pour fournir une expérience d’installation simplifiée et une vue centrée sur le serveur des services.

## Outil de services Azure hybride

L’outil de services Azure hybrid dans [Windows Admin Center](../understand/windows-admin-center.md) consolide tous les services Azure intégrées dans un hub centralisé où vous pouvez découvrir facilement tous les services Azure disponibles valeur ajoutée à votre sur site ou hybride environnement. 

![Capture d’écran du centre d’administration Windows affichant l’outil Azure hybride Services](../media/azure-services/ahs-discover.png)

Si vous vous connectez à un serveur avec les services Azure déjà activés, l’outil de services Azure hybrid joue un volet unique pour voir tous les services activés sur ce serveur. Vous pouvez facilement accéder à l’outil approprié dans Windows Admin Center, lancer au portail Azure pour la gestion plus approfondie de ces services Azure ou en lecture plus avec la documentation à portée de main. 

![Capture d’écran du centre d’administration Windows affichant des services Azure qui sont déjà installés sur le serveur](../media/azure-services/ahs-dayN.png)

À partir de l’outil de services Azure hybride, vous pouvez:
- Sauvegarde Windows Server à partir de Windows Admin Center avec [sauvegarde Azure](azure-backup.md)
- Protéger vos Machines virtuelles Hyper-V à partir de Windows Admin Center avec [Azure Site Recovery](azure-site-recovery.md)
- Synchroniser votre serveur de fichiers avec le cloud, à l’aide de la [Synchronisation de fichier Azure](azure-file-sync.md)
- Gérer les mises à jour de système d’exploitation pour tous vos serveurs Windows, à la fois sur site ou dans le cloud, avec [La gestion des mises à jour Azure](azure-update-management.md)
- Surveiller les serveurs, à la fois sur site ou dans le cloud et configurer des alertes avec le [Moniteur Azure](azure-monitor.md)
- Connecter vos serveurs locaux à un réseau virtuel Azure avec [L’adaptateur réseau Azure](https://aka.ms/WACNetworkAdapter)

## Services pour les machines virtuelles et les serveurs autonomes

Il s’agit de la liste complète des services Azure qui fournissent des fonctionnalités aux serveurs autonomes et les machines virtuelles:

- **(Nouveau) Synchroniser votre serveur de fichiers avec le service cloud à l’aide de [La synchronisation de fichier Azure](https://aka.ms/afs)**  
Synchroniser les fichiers sur ce serveur avec les partages de fichiers Azure. Conserver tous vos fichiers en local ou utilisez cloud hiérarchisation et mettre en cache uniquement les plus fréquemment utilisées des fichiers sur le serveur, la hiérarchisation des données à froid sur le cloud. Les données dans le cloud peuvent être sauvegardées, ce qui élimine la nécessité à vous soucier de sauvegarde du serveur local. En outre, multi-site-synchronisation permettre synchroniser un ensemble de fichiers sur plusieurs serveurs.

- **Ajouter une couche de sécurité à Windows Admin Center en ajoutant l’authentification [Azure Active Directory (Azure AD)](https://azure.microsoft.com/services/active-directory/)**  
Vous pouvez ajouter une couche de sécurité supplémentaire pour Windows Admin Center en demander aux utilisateurs de s’authentifier à l’aide des identités Azure Active Directory (Azure AD) pour accéder à la passerelle. L’authentification Azure AD vous permet également de tirer parti des fonctionnalités de sécurité de Azure AD tels que l’accès conditionnel et l’authentification multifacteur.  
Pour plus d’informations, voir [configurer Azure AD l’authentification pour Windows Admin Center.](../configure/user-access-control.md#azure-active-directory)  

- **Protéger vos machines virtuelles de Hyper-V avec [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview)**  
Vous pouvez répliquer des charges de travail en cours d’exécution sur les ordinateurs virtuels afin que votre infrastructure critique soit protégée en cas d’incident. Windows Admin Center simplifie le programme d’installation et le processus de réplication de vos machines virtuelles sur vos serveurs Hyper-V ou les clusters, ce qui facilite la renforcer la résilience de votre environnement avec le service de récupération d’urgence d’Azure Site Recovery.  
Pour plus d’informations, consultez se [protéger vos machines virtuelles avec Azure Site Recovery et Windows Admin Center](azure-site-recovery.md).

- **Sauvegarder vos serveurs Windows, [Sauvegarde Azure](https://docs.microsoft.com/azure/backup/backup-overview)**  
Vous pouvez sauvegarder vos serveurs Windows vers Azure, pour vous protéger contre les ransomware, corruption et suppressions accidentelles ou malveillantes.  
Pour plus d’informations, voir [sauvegarder vos serveurs avec sauvegarde Azure](azure-backup.md).

- **Surveiller et obtenir les alertes par courrier électronique pour tous les serveurs dans votre environnement avec le [Moniteur Azure pour les Machines virtuelles](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-overview)**  
Vous pouvez utiliser Azure moniteur, également connue sous les informations de Machines virtuelles, pour surveiller l’intégrité du serveur et les événements, créer des alertes par courrier électronique, obtenir une vue consolidée des performances du serveur au sein de votre environnement et visualiser les applications, les systèmes, et les services connectés à un donnée serveur.  
Pour plus d’informations, voir [se connecter à vos serveurs à surveiller Azure et configurer des notifications par courrier électronique](azure-monitor.md).

- **Gérer les mises à jour du système d’exploitation de manière centralisée pour tous vos serveurs Windows avec [La gestion des mises à jour Azure](https://docs.microsoft.com/azure/automation/automation-update-management)**  
Vous pouvez gérer les mises à jour et des correctifs pour plusieurs serveurs et les machines virtuelles à partir d’un emplacement unique, plutôt que sur chaque serveur. Avec la gestion des mises à jour Azure, vous pouvez rapidement évaluer l’état des mises à jour disponibles, planifier l’installation des mises à jour requises et passer en revue les résultats de déploiement pour vérifier les mises à jour qui s’appliquent avec succès. Cela est possible si vos serveurs sont des machines virtuelles Azure, hébergée par d’autres fournisseurs de cloud, ou local.  
Pour plus d’informations, consultez [configurer les serveurs pour la gestion des mises à jour Azure](azure-update-management.md).

- **Connecter vos serveurs locaux à un réseau virtuel Azure avec une [Carte réseau de Azure](https://aka.ms/WACNetworkAdapter)**  
Vous pouvez ajouter une carte de réseau Azure à vos serveurs locaux pour vous aider à connecter en toute sécurité le serveur à un réseau virtuel Azure.  
Pour plus d’informations, consultez [configurer un point-à-site la connexion VPN entre un serveur de Windows en local et un réseau virtuel Azure](https://aka.ms/WACNetworkAdapter).

- **Gérer des machines virtuelles de Azure IaaS avec [Windows Admin Center](manage-azure-vms.md)**  
Vous pouvez utiliser Windows Admin Center pour gérer vos machines virtuelles Azure, ainsi que les ordinateurs locaux. En configurant votre passerelle Windows Admin Center pour se connecter à votre basculée Azure, vous pouvez gérer les machines virtuelles dans Azure à l’aide des outils cohérentes, simplifiées qui fournit Windows Admin Center. Pour plus d’informations, consultez [Configurer Windows Admin Center pour gérer les machines virtuelles dans Azure](manage-azure-vms.md).

## Services pour les clusters

Voici les services Azure qui fournissent des fonctionnalités pour les clusters dans son ensemble (elles ne sont pas toutes intégrées dans Windows Admin Center encore):

- [Analyser un cluster hyperconvergé avec le moniteur Azure](../../../storage/storage-spaces/configure-azure-monitor.md)
- [Protéger vos machines virtuelles avec Azure Site Recovery](azure-site-recovery.md)
- [Déployer un témoin de cloud de cluster](../../../failover-clustering/deploy-cloud-witness.md)

## Articles associés

- [Se connecter Windows Admin Center à Azure](azure-integration.md)
- [Déployer Windows Admin Center dans Azure](deploy-wac-in-azure.md)