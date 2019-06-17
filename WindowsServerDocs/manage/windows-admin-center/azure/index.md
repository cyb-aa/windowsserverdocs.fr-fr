---
title: Connexion de Windows Server aux services hybrides Azure
description: Vous pouvez utiliser les services hybrides Azure pour étendre des déploiements locaux de Windows Server vers le cloud.
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
# <a name="connecting-windows-server-to-azure-hybrid-services"></a>Connexion de Windows Server aux services hybrides Azure

>S’applique à : Windows Server 2019, Windows Server 2016, Windows Server (Canal semi-annuel)

Vous pouvez utiliser les services hybrides Azure pour étendre des déploiements locaux de Windows Server vers le cloud. Ces services cloud offrent un éventail de fonctions particulièrement utiles :

- Protégez les machines virtuelles, et utilisez la sauvegarde et la récupération d'urgence basées sur le cloud avec Azure Site Recovery. 
- Assurez le suivi des événements liés à vos applications, à votre réseau et à votre infrastructure à l'aide des fonctionnalités d'analytique avancée et de Machine Learning d'Azure Monitor. 
- Simplifiez la connectivité réseau à Azure avec la carte réseau Azure Network Adapter.
- Maintenez les machines virtuelles à jour avec Azure Update Management.

Les services hybrides Azure fonctionnent avec les serveurs Windows dans les configurations suivantes :

- Serveurs physiques autonomes et machines virtuelles
- Clusters, y compris les clusters hyperconvergés certifiés par les programmes [Azure Stack HCI](https://docs.microsoft.com/azure-stack/operator/azure-stack-hci-overview) et [WSSD (Windows Server Software-Defined)](https://www.microsoft.com/en-us/cloud-platform/software-defined-datacenter)

Vous pouvez configurer la plupart des services Azure hybrides à l'aide du portail Azure et d'un ou deux téléchargements, mais beaucoup d'entre eux sont directement intégrés à Windows Admin Center pour simplifier le processus de configuration et offrir une vue des services centrée sur le serveur.

## <a name="azure-hybrid-services-tool"></a>Outil Azure Hybrid Services

L'outil Azure Hybrid Services disponible dans [Windows Admin Center](../understand/windows-admin-center.md) centralise tous les services Azure intégrés au sein d'un hub à partir duquel vous pouvez facilement accéder aux services Azure qui apportent de la valeur à votre environnement local ou hybride. 

![Capture d'écran de Windows Admin Center présentant l'outil Azure Hybrid Services](../media/azure-services/ahs-discover.png)

Si vous vous connectez à un serveur sur lequel des services Azure sont déjà activés, l'outil Azure Hybrid Services fait office de volet unique présentant tous les services activés sur ce serveur. Vous pouvez facilement accéder à l'outil pertinent depuis Windows Admin Center, lancer le portail Azure pour une gestion plus approfondie de ces services Azure, ou en savoir plus grâce à la documentation disponible. 

![Capture d'écran de Windows Admin Center présentant les services Azure déjà installés sur le serveur](../media/azure-services/ahs-dayN.png)

L'outil Azure Hybrid Services vous permet d'effectuer les opérations suivantes :
- Sauvegardez votre serveur Windows à partir de Windows Admin Center avec [Sauvegarde Azure](azure-backup.md).
- Protégez vos machines virtuelles Hyper-V à partir de Windows Admin Center avec [Azure Site Recovery](azure-site-recovery.md).
- Synchronisez votre serveur de fichiers avec le cloud en utilisant [Azure File Sync](azure-file-sync.md).
- Gérez les mises à jour du système d'exploitation de tous vos serveurs Windows, locaux ou cloud, avec [Azure Update Management](azure-update-management.md).
- Surveillez les serveurs, locaux ou cloud, et configurez des alertes avec [Azure Monitor](azure-monitor.md).
- Connectez vos serveurs locaux à un réseau virtuel Azure avec la carte réseau [Azure Network Adapter](https://aka.ms/WACNetworkAdapter).

## <a name="services-for-stand-alone-servers-and-vms"></a>Services pour serveurs autonomes et machines virtuelles

Voici la liste complète des services Azure qui fournissent des fonctionnalités aux serveurs autonomes et aux machines virtuelles :

- **(Nouveau) Synchronisez votre serveur de fichiers avec le cloud en utilisant [Azure File Sync](https://aka.ms/afs)**  
Synchronisez des fichiers sur ce serveur avec des partages de fichiers Azure. Conservez tous vos fichiers localement ou utilisez la hiérarchisation cloud, et mettez uniquement en cache les fichiers les plus fréquemment utilisés sur le serveur, en hiérarchisant les données brutes dans le cloud. Les données présentes dans le cloud peuvent être sauvegardées, ce qui évite d'avoir à se soucier de la sauvegarde des serveurs locaux. De plus, la synchronisation multisite permet de synchroniser un ensemble de fichiers sur plusieurs serveurs.

- **Ajoutez une couche de sécurité à Windows Admin Center avec l'authentification [Azure Active Directory (Azure AD)](https://azure.microsoft.com/services/active-directory/)**  
Vous pouvez ajouter une couche de sécurité supplémentaire à Windows Admin Center en demandant aux utilisateurs de s'authentifier à l'aide des identités Azure Active Directory (Azure AD) pour accéder à la passerelle. L'authentification Azure AD vous permet également de tirer parti des fonctionnalités de sécurité d'Azure AD, comme l'accès conditionnel et l'authentification multifacteur.  
Pour plus d'informations, consultez [Configurer l'authentification Azure AD pour Windows Admin Center.](../configure/user-access-control.md#azure-active-directory)  

- **Protégez vos machines virtuelles Hyper-V avec [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview)**  
Vous pouvez répliquer les charges de travail exécutées sur les machines virtuelles afin que votre infrastructure stratégique soit protégée en cas d'incident. Windows Admin Center simplifie la configuration et le processus de réplication de vos machines virtuelles sur vos serveurs ou clusters Hyper-V, facilitant ainsi la résilience de votre environnement avec le service de récupération d'urgence d'Azure Site Recovery.  
Pour plus d'informations, consultez [Protéger vos machines virtuelles avec Azure Site Recovery et Windows Admin Center](azure-site-recovery.md).

- **Sauvegardez vos serveurs Windows avec [Sauvegarde Azure](https://docs.microsoft.com/azure/backup/backup-overview)**  
Vous pouvez sauvegarder vos serveurs Windows sur Azure afin de vous protéger des suppressions accidentelles ou malveillantes, des altérations de données et des rançongiciels.  
Pour plus d'informations, consultez [Sauvegarder vos serveurs avec Sauvegarde Azure](azure-backup.md).

- **Surveillez tous les serveurs de votre environnement et recevez des alertes par e-mail pour ceux-ci avec [Azure Monitor pour machines virtuelles](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-overview)**  
Vous pouvez utiliser Azure Monitor, également appelé Virtual Machines Insights, pour surveiller l'intégrité des serveurs et les événements liés à ceux-ci, créer des alertes par e-mail, bénéficier d'une vue centralisée des performances des serveurs de votre environnement, et visualiser les applications, systèmes et services connectés à un serveur donné.  
Pour plus d'informations, consultez [Connecter vos serveurs à Azure Monitor et configurer des notifications par e-mail](azure-monitor.md).

- **Centralisez la gestion des mises à jour du système d'exploitation de tous vos serveurs Windows avec [Azure Update Management](https://docs.microsoft.com/azure/automation/automation-update-management)**  
Vous pouvez gérer les mises à jour et les correctifs de différents serveurs et machines virtuelles à partir d'un emplacement unique, plutôt qu'au cas par cas. Avec Azure Update Management, vous pouvez rapidement évaluer le statut des mises à jour disponibles, planifier l'installation des mises à jour requises et examiner les résultats des déploiements pour vérifier que les mises à jour ont bien été installées. Cela s'applique aussi bien aux machines virtuelles Azure qu'aux serveurs hébergés par d'autres fournisseurs de services cloud ou aux serveurs locaux.  
Pour plus d'informations, consultez [Configurer des serveurs pour Azure Update Management](azure-update-management.md).

- **Connectez vos serveurs locaux à un réseau virtuel Azure à l'aide d'une carte réseau [Azure Network Adapter](https://aka.ms/WACNetworkAdapter)**  
Vous pouvez ajouter une carte réseau Azure à vos serveurs locaux pour les connecter en toute sécurité à un réseau virtuel Azure.  
Pour plus d'informations, consultez [Configurer une connexion VPN point à site entre un serveur Windows local et un réseau virtuel Azure](https://aka.ms/WACNetworkAdapter).

- **Gérez des machines virtuelles Azure IaaS avec [Windows Admin Center](manage-azure-vms.md)**  
Vous pouvez utiliser Windows Admin Center pour gérer vos machines virtuelles Azure ainsi que vos ordinateurs locaux. En configurant votre passerelle Windows Admin Center pour qu'elle se connecte à votre réseau virtuel Azure, vous pouvez gérer les machines virtuelles dans Azure à l'aide des outils simplifiés et cohérents fournis par Windows Admin Center. Pour plus d'informations, consultez [Configurer Windows Admin Center pour gérer les machines virtuelles dans Azure](manage-azure-vms.md).

## <a name="services-for-clusters"></a>Services pour clusters

Il s'agit des services Azure qui fournissent des fonctionnalités aux clusters dans leur ensemble (ces services ne sont pas encore tous intégrés à Windows Admin Center) :

- [Surveiller un cluster hyperconvergé avec Azure Monitor](../../../storage/storage-spaces/configure-azure-monitor.md)
- [Protéger vos machines virtuelles avec Azure Site Recovery](azure-site-recovery.md)
- [Déployer un témoin de cloud dans un cluster](../../../failover-clustering/deploy-cloud-witness.md)

## <a name="see-also"></a>Voir également

- [Connecter Windows Admin Center à Azure](azure-integration.md)
- [Déployer Windows Admin Center dans Azure](deploy-wac-in-azure.md)