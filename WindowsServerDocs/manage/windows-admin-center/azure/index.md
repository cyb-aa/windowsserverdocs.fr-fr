---
title: Connexion de Windows Server à des services Azure hybrides
description: Vous pouvez étendre les déploiements locaux de Windows Server dans le cloud à l’aide des services Azure hybrides.
ms.technology: manage
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 05/31/2019
ms.openlocfilehash: 460399a57bc229b44d37a9fdd1e4938bf9e7d6ac
ms.sourcegitcommit: fe621b72d45d0259bac1d5b9031deed3dcbed29d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2019
ms.locfileid: "66455364"
---
# <a name="connecting-windows-server-to-azure-hybrid-services"></a>Connexion de Windows Server à des services Azure hybrides

>S’applique à : Windows Server 2019, Windows Server 2016, Windows Server (canal semi-annuel)

Vous pouvez étendre les déploiements locaux de Windows Server dans le cloud à l’aide des services Azure hybrides. Ces services de cloud computing fournissent un tableau de fonctions utiles, notamment les suivantes :

- Protéger les machines virtuelles et utiliser le nuage sauvegarde et récupération d’urgence (HA/DR) avec Azure Site Recovery. 
- Suivre le déroulement des vos applications, le réseau et l’infrastructure à l’aide de l’analytique avancée et d’apprentissage dans Azure Monitor. 
- Simplifier la connectivité réseau vers Azure avec carte de réseau Azure.
- Machines virtuelles mise à jour avec la gestion de mise à jour d’Azure.

Services hybrides Azure fonctionnent avec les serveurs Windows dans les configurations suivantes :

- Serveurs physiques autonomes et des machines virtuelles (VM)
- Clusters, y compris les clusters hyperconvergés certifiés par le [Azure Stack HCL](https://docs.microsoft.com/azure-stack/operator/azure-stack-hci-overview), et [Windows Server Software-Defined (WSSD)](https://www.microsoft.com/en-us/cloud-platform/software-defined-datacenter) programmes

Bien que vous pouvez configurer la plupart des hybride services Azure à l’aide du portail Azure et un téléchargement ou deux, nombreuses sont directement intégrés à Windows Admin Center pour fournir une expérience d’installation simplifiée et une vue centrée sur le serveur des services.

## <a name="azure-hybrid-services-tool"></a>Outil de services Azure hybride

Les services Azure hybrid outil [Windows Admin Center](../understand/windows-admin-center.md) consolide tous les services Azure intégrés dans un hub centralisé où vous pouvez facilement découvrir tous les services Azure disponibles qui apportent de valeur pour votre réseau local ou environnement hybride. 

![Capture d’écran de de Windows Admin Center montrant l’outil Azure Hybrid Services](../media/azure-services/ahs-discover.png)

Si vous vous connectez à un serveur avec les services Azure déjà activés, l’outil de services Azure hybrid sert un volet unique pour afficher tous les services activés sur ce serveur. Vous pouvez facilement accéder à l’outil approprié au sein de Windows Admin Center, lancez au portail Azure pour la gestion plus approfondie de ces services Azure, ou de lecture plus avec la documentation à portée de main. 

![Capture d’écran de de Windows Admin Center montrant des services Azure qui sont déjà installés sur le serveur](../media/azure-services/ahs-dayN.png)

À partir de l’outil services hybride Azure, vous pouvez :
- Sauvegarder votre serveur Windows à partir de Windows Admin Center avec [sauvegarde Azure](azure-backup.md)
- Protéger vos Machines virtuelles de Hyper-V à partir de Windows Admin Center avec [Azure Site Recovery](azure-site-recovery.md)
- Synchroniser votre serveur de fichiers avec le cloud, à l’aide de [Azure File Sync](azure-file-sync.md)
- Gérer les mises à jour de système d’exploitation pour tous vos serveurs Windows, localement ou dans le cloud, avec [Azure Update Management](azure-update-management.md)
- Surveiller les serveurs, en local ou dans le cloud et configurer les alertes avec [Azure Monitor](azure-monitor.md)
- Connecter vos serveurs locaux à un réseau virtuel Azure avec [carte de réseau Azure](https://aka.ms/WACNetworkAdapter)

## <a name="services-for-stand-alone-servers-and-vms"></a>Services pour les machines virtuelles et les serveurs autonomes

Il s’agit de la liste complète des services Azure qui fournissent des fonctionnalités pour les machines virtuelles et les serveurs autonomes :

- **(Nouveau) Synchroniser votre serveur de fichiers avec le cloud à l’aide [Azure File Sync](https://aka.ms/afs)**  
Synchroniser les fichiers sur ce serveur avec des partages de fichiers Azure. Conserver tous vos fichiers locaux ou utilisez la hiérarchisation et cloud cache uniquement les fichiers sur le serveur, la hiérarchisation des données à froid vers le cloud fréquemment utilisés. Données dans le cloud peuvent être sauvegardées, évitant ainsi à vous soucier de sauvegarde du serveur local. En outre, plusieurs à la synchronisation du site peut synchroniser un ensemble de fichiers sur plusieurs serveurs.

- **Ajouter une couche de sécurité pour Windows Admin Center en ajoutant [Azure Active Directory (Azure AD)](https://azure.microsoft.com/services/active-directory/) l’authentification**  
Vous pouvez ajouter une couche supplémentaire de sécurité pour Windows Admin Center en demandant aux utilisateurs de s’authentifier en utilisant des identités Azure Active Directory (Azure AD) pour accéder à la passerelle. L’authentification Azure AD vous permet également de tirer parti des fonctionnalités de sécurité d’Azure AD telles que l’accès conditionnel et l’authentification multifacteur.  
Pour plus d’informations, consultez [configurer Azure AD l’authentification pour Windows Admin Center.](../configure/user-access-control.md#azure-active-directory)  

- **Protéger vos machines virtuelles de Hyper-V avec [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview)**  
Vous pouvez répliquer des charges de travail exécutées sur des machines virtuelles afin que votre infrastructure critique pour l’entreprise est protégée en cas de sinistre. Windows Admin Center simplifie le programme d’installation et le processus de réplication de vos machines virtuelles sur vos serveurs Hyper-V ou des clusters, ce qui facilite Booster la résilience de votre environnement avec le service de récupération d’urgence d’Azure Site Recovery.  
Pour plus d’informations, consultez [protéger vos machines virtuelles avec Azure Site Recovery et Windows Admin Center](azure-site-recovery.md).

- **Sauvegardez vos serveurs Windows avec [sauvegarde Azure](https://docs.microsoft.com/azure/backup/backup-overview)**  
Vous pouvez sauvegarder vos serveurs Windows vers Azure, contribue à vous protéger contre les ransomware, la corruption et les suppressions accidentelles ou malveillantes.  
Pour plus d’informations, consultez [sauvegarder vos serveurs avec Azure Backup](azure-backup.md).

- **Surveillez et obtenez des alertes par courrier électronique pour tous les serveurs dans votre environnement avec [Azure Monitor pour les Machines virtuelles](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-overview)**  
Vous pouvez utiliser Azure Monitor, également appelé Insights de Machines virtuelles, pour surveiller l’intégrité du serveur et des événements, créer des alertes par courrier électronique, obtenir une vue consolidée des performances du serveur au sein de votre environnement et visualiser les applications, systèmes, et les services connectés à une donnée serveur.  
Pour plus d’informations, consultez [connecter vos serveurs à Azure Monitor et de configurer des notifications par courrier électronique](azure-monitor.md).

- **Gérer de manière centralisée des mises à jour du système d’exploitation pour tous vos serveurs Windows avec [Azure Update Management](https://docs.microsoft.com/azure/automation/automation-update-management)**  
Vous pouvez gérer les mises à jour et correctifs pour plusieurs serveurs et machines virtuelles à partir d’un emplacement unique, plutôt que sur un serveur par serveur. Avec la gestion de mise à jour d’Azure, vous pouvez rapidement évaluer l’état des mises à jour disponibles, planifier l’installation des mises à jour requises et passer en revue les résultats de déploiement pour vérifier les mises à jour sont appliquées correctement. Cela est possible si vos serveurs sont des machines virtuelles Azure, hébergé par d’autres fournisseurs cloud, ou en local.  
Pour plus d’informations, consultez [configurer des serveurs pour la gestion des mises à jour Azure](azure-update-management.md).

- **Connecter vos serveurs locaux à un réseau virtuel Azure avec un [carte de réseau Azure](https://aka.ms/WACNetworkAdapter)**  
Vous pouvez ajouter un adaptateur de réseau Azure à vos serveurs locaux pour vous aider à connecter en toute sécurité le serveur à un réseau virtuel Azure.  
Pour plus d’informations, consultez [configurer une connexion VPN de point à site entre un serveur de Windows en local et un réseau virtuel Azure](https://aka.ms/WACNetworkAdapter).

- **Gérer des machines virtuelles Azure IaaS avec [Windows Admin Center](manage-azure-vms.md)**  
Vous pouvez utiliser Windows Admin Center pour gérer vos machines virtuelles Azure, ainsi que les machines locales. En configurant votre passerelle Windows Admin Center pour vous connecter à votre réseau virtuel Azure, vous pouvez gérer des machines virtuelles dans Azure à l’aide des outils cohérents et simplifiées Windows Admin Center fournit. Pour plus d’informations, consultez [configurer Windows Admin Center pour gérer des machines virtuelles dans Azure](manage-azure-vms.md).

## <a name="services-for-clusters"></a>Services pour les clusters

Il s’agit des services Azure qui fournissent des fonctionnalités aux clusters dans sa globalité (ils ne sont pas toutes intégrées dans Windows Admin Center encore) :

- [Surveiller un cluster hyperconvergé avec Azure Monitor](../../../storage/storage-spaces/configure-azure-monitor.md)
- [Protégez vos machines virtuelles avec Azure Site Recovery](azure-site-recovery.md)
- [Déployer un témoin de cloud de cluster](../../../failover-clustering/deploy-cloud-witness.md)

## <a name="see-also"></a>Voir également

- [Connecter Windows Admin Center à Azure](azure-integration.md)
- [Déployer Windows Admin Center dans Azure](deploy-wac-in-azure.md)