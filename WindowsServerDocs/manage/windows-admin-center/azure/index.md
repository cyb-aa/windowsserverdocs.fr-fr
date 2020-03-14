---
title: Connexion de Windows Server aux services hybrides Azure
description: Vous pouvez utiliser les services hybrides Azure pour étendre des déploiements locaux de Windows Server vers le cloud.
ms.technology: manage
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.localizationpriority: medium
ms.prod: windows-server
ms.date: 05/31/2019
ms.openlocfilehash: b82d2eaa9283d99993102f1656262e2eda86cfff
ms.sourcegitcommit: 0a0a45bec6583162ba5e4b17979f0b5a0c179ab2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79323321"
---
# <a name="connecting-windows-server-to-azure-hybrid-services"></a>Connexion de Windows Server aux services hybrides Azure

Vous pouvez utiliser les services hybrides Azure pour étendre des déploiements locaux de Windows Server vers le cloud. Ces services cloud fournissent un ensemble de fonctions utiles, à la fois pour l’extension des services locaux vers Azure et pour la gestion centralisée à partir d’Azure.

![Diagramme montrant une flèche entre les services locaux et le cloud pour une extension des services locaux vers Azure, et une flèche du cloud vers les services locaux pour une gestion centralisée avec Azure](../media/azure-services/hybrid-framing.png)

Avec les services hybrides Azure dans Windows Admin Center, vous pouvez :

- [Protéger les machines virtuelles et utiliser la sauvegarde et la reprise d’activité basées sur le cloud](#back-up-and-protect-your-on-premises-servers-and-vms).  
- [Étendre la capacité locale avec le stockage et le calcul dans Azure, et simplifier la connectivité réseau à Azure](#extend-on-premises-capacity-with-azure).
- [Centraliser la supervision, la gouvernance, la configuration et la sécurité parmi vos applications, votre réseau et votre infrastructure avec l’aide des services de gestion Azure intelligents](#centrally-manage-your-hybrid-environment-from-azure).  

Vous pouvez configurer la plupart des services Azure hybrides à l’aide du portail Azure en téléchargeant une application et en effectuant une configuration manuelle. Beaucoup d’entre eux sont directement intégrés à Windows Admin Center pour simplifier le processus de configuration et offrir une vue des services centrée sur le serveur. Windows Admin Center fournit également des liens hypertexte intelligents pratiques vers le portail Azure pour voir les ressources Azure connectées ainsi qu’une vue centralisée de votre environnement hybride.

## <a name="discover-integrated-services-in-the-azure-hybrid-services-tool"></a>Découvrir les services intégrés dans l’outil Azure Hybrid Services

L'outil Azure Hybrid Services disponible dans [Windows Admin Center](../overview.md) centralise tous les services Azure intégrés au sein d'un hub à partir duquel vous pouvez facilement accéder aux services Azure qui apportent de la valeur à votre environnement local ou hybride.  

![Capture d'écran de Windows Admin Center présentant l'outil Azure Hybrid Services](../media/azure-services/ahs-discover.png)

Si vous vous connectez à un serveur sur lequel des services Azure sont déjà activés, l'outil Azure Hybrid Services fait office de volet unique présentant tous les services activés sur ce serveur. Vous pouvez facilement accéder à l'outil pertinent depuis Windows Admin Center, lancer le portail Azure pour une gestion plus approfondie de ces services Azure, ou en savoir plus grâce à la documentation disponible.  

![Capture d'écran de Windows Admin Center présentant les services Azure déjà installés sur le serveur](../media/azure-services/ahs-dayN.png)

L'outil Azure Hybrid Services vous permet d'effectuer les opérations suivantes :

- Sauvegardez votre serveur Windows à partir de Windows Admin Center avec [Sauvegarde Azure](azure-backup.md).
- Protégez vos machines virtuelles Hyper-V à partir de Windows Admin Center avec [Azure Site Recovery](azure-site-recovery.md).
- Synchronisez votre serveur de fichiers avec le cloud en utilisant [Azure File Sync](azure-file-sync.md).
- Gérez les mises à jour du système d'exploitation de tous vos serveurs Windows, locaux ou cloud, avec [Azure Update Management](azure-update-management.md).
- Surveillez les serveurs, locaux ou cloud, et configurez des alertes avec [Azure Monitor](azure-monitor.md).
- Appliquez des stratégies de gouvernance à vos serveurs locaux par le biais d’Azure Policy à l’aide d’[Azure Arc pour serveurs](https://docs.microsoft.com/azure/azure-arc/servers/overview).
- Sécurisez vos serveurs et bénéficiez d’une protection avancée contre les menaces avec [Azure Security Center](https://docs.microsoft.com/azure/security-center/windows-admin-center-integration).
- Connectez vos serveurs locaux à un réseau virtuel Azure avec la carte réseau [Azure Network Adapter](https://aka.ms/WACNetworkAdapter).
- Faites en sorte que les machines virtuelles Azure ressemblent à votre réseau local avec le [réseau étendu Azure](https://go.microsoft.com/fwlink/?linkid=2109517&clcid=0x409).

## <a name="back-up-and-protect-your-on-premises-servers-and-vms"></a>Sauvegardez et protégez vos machines virtuelles et serveurs locaux.

- **Sauvegardez vos serveurs Windows avec [Sauvegarde Azure](https://docs.microsoft.com/azure/backup/backup-overview)**  
Vous pouvez sauvegarder vos serveurs Windows sur Azure afin de vous protéger des suppressions accidentelles ou malveillantes, des altérations de données et des rançongiciels.  
Pour plus d'informations, consultez [Sauvegarder vos serveurs avec Sauvegarde Azure](azure-backup.md).

- **Protégez vos machines virtuelles Hyper-V avec [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview)**  
Vous pouvez répliquer les charges de travail exécutées sur les machines virtuelles afin que votre infrastructure stratégique soit protégée en cas d'incident. Windows Admin Center simplifie la configuration et le processus de réplication de vos machines virtuelles sur vos serveurs ou clusters Hyper-V, facilitant ainsi la résilience de votre environnement avec le service de reprise d’activité après sinistre d'Azure Site Recovery.  
Pour plus d'informations, consultez [Protéger vos machines virtuelles avec Azure Site Recovery et Windows Admin Center](azure-site-recovery.md).

- **Utilisez la réplication synchrone ou asynchrone basée sur les blocs vers une machine virtuelle dans Azure à l’aide du [réplica de stockage](https://docs.microsoft.com/windows-server/storage/storage-replica/storage-replica-overview)** .  
Vous pouvez configurer la réplication basée sur les blocs ou sur les volumes à un niveau serveur-à-serveur à l’aide du réplica de stockage sur un serveur ou une machine virtuelle secondaire. Windows Admin Center vous permet de créer une machine virtuelle Azure spécifiquement pour votre cible de réplication, ce qui vous aide à dimensionner et à configurer correctement le stockage sur une nouvelle machine virtuelle Azure.  
Pour plus d’informations, consultez [Réplication de serveur à serveur avec le réplica de stockage](https://docs.microsoft.com/windows-server/storage/storage-replica/storage-replica-ui).  

## <a name="extend-on-premises-capacity-with-azure"></a>Étendre la capacité locale avec Azure

### <a name="extend-storage-capacity"></a>Étendre la capacité de stockage

- **Synchronisez votre serveur de fichiers avec le cloud à l’aide d’[Azure File Sync](https://aka.ms/afs)**  
Synchronisez des fichiers sur ce serveur avec des partages de fichiers Azure. Conservez tous vos fichiers localement ou utilisez la hiérarchisation cloud pour libérer de l’espace, et mettez uniquement en cache les fichiers les plus fréquemment utilisés sur le serveur, en hiérarchisant les données brutes dans le cloud. Les données présentes dans le cloud peuvent être sauvegardées, ce qui évite d'avoir à se soucier de la sauvegarde des serveurs locaux. De plus, la synchronisation multisite permet de synchroniser un ensemble de fichiers sur plusieurs serveurs.
Pour plus d’informations, consultez [Synchroniser votre serveur de fichiers avec le cloud en utilisant Azure File Sync](azure-file-sync.md).

- **Migrez le stockage vers une machine virtuelle dans Azure à l’aide du [service de migration de stockage](https://docs.microsoft.com/windows-server/storage/storage-migration-service/overview)**  
Utilisez l’outil pas à pas pour inventorier des données sur des serveurs Windows et Linux, puis transférer les données vers une nouvelle machine virtuelle Azure. Windows Admin Center peut créer pour ce travail une machine virtuelle Azure de taille appropriée et configurée correctement pour recevoir les données de votre serveur source.  
Pour plus d’informations, consultez [Utiliser le service de migration de stockage pour migrer un serveur](https://docs.microsoft.com/windows-server/storage/storage-migration-service/migrate-data).

### <a name="extend-compute-capacity"></a>Étendre la capacité de calcul

- **Créez une machine virtuelle Azure sans quitter Windows Admin Center**  
À partir de la page *Toutes les connexions* dans Windows Admin Center, accédez à **Ajouter**, puis sélectionnez **Créer** sous **Machine virtuelle Azure**. Vous pouvez même joindre votre machine virtuelle Azure à un domaine et configurer le stockage à partir de cet outil de création étape par étape.

- **Tirez parti d’Azure pour atteindre le quorum sur votre cluster de basculement avec [témoin de cloud](https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness)**  
Au lieu d’investir dans du matériel supplémentaire pour atteindre le quorum sur un cluster à deux nœuds, vous pouvez utiliser un compte Stockage Azure pour servir de témoin de cluster pour votre cluster Azure Stack HCI ou tout autre cluster de basculement.  
Pour plus d’informations, voir [Déployer un témoin de cloud pour un cluster de basculement](https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness).  

### <a name="simplify-network-connectivity-between-your-on-premises-and-azure-networks"></a>Simplifier la connectivité réseau entre vos réseaux locaux et Azure

- **Connectez vos serveurs locaux à un réseau virtuel Azure avec la [carte réseau Azure Network Adapter](https://aka.ms/WACNetworkAdapter)**  
Laissez Windows Admin Center simplifier la configuration d’un VPN de point à site à partir d’un serveur local sur un réseau virtuel Azure.  

- **Faites en sorte que les machines virtuelles Azure ressemblent à votre réseau local avec le [réseau étendu Azure](https://go.microsoft.com/fwlink/?linkid=2109517&clcid=0x409)**  
Windows Admin Center peut configurer un VPN de site à site et étendre vos adresses IP locales sur votre réseau virtuel Azure pour vous permettre de migrer plus facilement les charges de travail dans Azure sans rompre les dépendances envers les adresses IP.

## <a name="centrally-manage-your-hybrid-environment-from-azure"></a>Gérer de façon centralisée votre environnement hybride à partir d’Azure

- **Surveillez tous les serveurs de votre environnement et recevez des alertes par e-mail pour ceux-ci avec [Azure Monitor pour machines virtuelles](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-overview)**  
Vous pouvez utiliser Azure Monitor, également appelé Virtual Machines Insights, pour surveiller l'intégrité des serveurs et les événements liés à ceux-ci, créer des alertes par e-mail, bénéficier d'une vue centralisée des performances des serveurs de votre environnement, et visualiser les applications, systèmes et services connectés à un serveur donné. Windows Admin Center peut également configurer des alertes par e-mail par défaut pour les événements de performances et d’intégrité du serveur et du cluster.  
Pour plus d'informations, consultez [Connecter vos serveurs à Azure Monitor et configurer des notifications par e-mail](azure-monitor.md).

- **Centralisez la gestion des mises à jour du système d'exploitation de tous vos serveurs Windows avec [Azure Update Management](https://docs.microsoft.com/azure/automation/automation-update-management)**  
Vous pouvez gérer les mises à jour et les correctifs de différents serveurs et machines virtuelles à partir d'un emplacement unique, plutôt qu'au cas par cas. Avec Azure Update Management, vous pouvez rapidement évaluer le statut des mises à jour disponibles, planifier l'installation des mises à jour requises et examiner les résultats des déploiements pour vérifier que les mises à jour ont bien été installées. Cela s'applique aussi bien aux machines virtuelles Azure qu'aux serveurs hébergés par d'autres fournisseurs de services cloud ou aux serveurs locaux.  
Pour plus d'informations, consultez [Configurer des serveurs pour Azure Update Management](azure-update-management.md).

- **Améliorez votre posture de sécurité et bénéficiez d’une protection avancée contre les menaces avec [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro)**  
Azure Security Center est un système unifié de gestion de la sécurité de l’infrastructure qui renforce la posture de sécurité de vos centres de données et fournit une protection avancée contre les menaces pour vos charges de travail hybrides dans le cloud, qu’elles soient dans Azure ou non, ainsi que localement. Avec Windows Admin Center, vous pouvez facilement configurer et connecter vos serveurs à Azure Security Center.  
Pour plus d’informations, consultez [Intégrer Azure Security Center à Windows Admin Center (préversion)](https://docs.microsoft.com/azure/security-center/windows-admin-center-integration).  

- **Appliquez des stratégies et garantissez la conformité au sein de votre environnement hybride avec [Azure Arc pour serveurs](https://docs.microsoft.com/azure/azure-arc/servers/overview) et [Azure Policy](https://docs.microsoft.com/azure/governance/policy/overview)**  
Inventoriez, organisez et gérez des serveurs locaux à partir d’Azure. Vous pouvez régir les serveurs à l’aide d’Azure Policy, contrôler l’accès à l’aide de RBAC et activer des services de gestion supplémentaires à partir d’Azure.  

## <a name="clusters-versus-stand-alone-servers-and-vms"></a>Clusters versus machines virtuelles et serveurs autonomes

Les services hybrides Azure fonctionnent avec les serveurs Windows dans les configurations suivantes :

- Serveurs physiques autonomes et machines virtuelles
- Clusters, y compris les clusters hyperconvergés certifiés par les programmes [Azure Stack HCI](../../../azure-stack-hci/index.md) et [WSSD (Windows Server Software-Defined)](https://www.microsoft.com/cloud-platform/software-defined-datacenter)

### <a name="services-for-stand-alone-servers-and-vms"></a>Services pour serveurs autonomes et machines virtuelles

Voici la liste complète des services Azure qui fournissent des fonctionnalités aux serveurs autonomes et aux machines virtuelles :

- Sauvegardez votre serveur Windows à partir de Windows Admin Center avec [Sauvegarde Azure](azure-backup.md).
- Protégez vos machines virtuelles Hyper-V à partir de Windows Admin Center avec [Azure Site Recovery](azure-site-recovery.md).
- Synchronisez votre serveur de fichiers avec le cloud en utilisant [Azure File Sync](azure-file-sync.md).
- Gérez les mises à jour du système d'exploitation de tous vos serveurs Windows, locaux ou cloud, avec [Azure Update Management](azure-update-management.md).
- Surveillez les serveurs, locaux ou cloud, et configurez des alertes avec [Azure Monitor](azure-monitor.md).
- Appliquez des stratégies de gouvernance à vos serveurs locaux par le biais d’Azure Policy à l’aide d’[Azure Arc pour serveurs](https://docs.microsoft.com/azure/azure-arc/servers/overview).
- Sécurisez vos serveurs et bénéficiez d’une protection avancée contre les menaces avec [Azure Security Center](https://docs.microsoft.com/azure/security-center/windows-admin-center-integration).
- Connectez vos serveurs locaux à un réseau virtuel Azure avec la carte réseau [Azure Network Adapter](https://aka.ms/WACNetworkAdapter).
- Faites en sorte que les machines virtuelles Azure ressemblent à votre réseau local avec le [réseau étendu Azure](https://go.microsoft.com/fwlink/?linkid=2109517&clcid=0x409).

### <a name="services-for-clusters"></a>Services pour clusters

Il s’agit des services Azure qui fournissent des fonctionnalités aux clusters dans leur ensemble :

- [Surveiller un cluster hyperconvergé avec Azure Monitor](../../../storage/storage-spaces/configure-azure-monitor.md)
- [Protéger vos machines virtuelles avec Azure Site Recovery](azure-site-recovery.md)
- [Déployer un témoin de cloud dans un cluster](../../../failover-clustering/deploy-cloud-witness.md)  

## <a name="other-azure-integrated-abilities-of-windows-admin-center"></a>Autres fonctionnalités intégrées à Azure de Windows Admin Center

- **[Ajouter des connexions de machines virtuelles Azure](manage-azure-vms.md) dans Windows Admin Center**  
Vous pouvez utiliser Windows Admin Center pour gérer vos machines virtuelles Azure ainsi que vos ordinateurs locaux. En configurant votre passerelle Windows Admin Center pour qu'elle se connecte à votre réseau virtuel Azure, vous pouvez gérer les machines virtuelles dans Azure à l'aide des outils simplifiés et cohérents fournis par Windows Admin Center.  
Pour plus d'informations, consultez [Configurer Windows Admin Center pour gérer les machines virtuelles dans Azure](manage-azure-vms.md).

- **Ajoutez une couche de sécurité à Windows Admin Center avec l'authentification [Azure Active Directory (Azure AD)](https://azure.microsoft.com/services/active-directory/)**  
Vous pouvez ajouter une couche de sécurité supplémentaire à Windows Admin Center en demandant aux utilisateurs de s'authentifier à l'aide des identités Azure Active Directory (Azure AD) pour accéder à la passerelle. L'authentification Azure AD vous permet également de tirer parti des fonctionnalités de sécurité d'Azure AD, comme l'accès conditionnel et l'authentification multifacteur.  
Pour plus d’informations, consultez [Configurer l’authentification Azure AD pour Windows Admin Center](../configure/user-access-control.md#azure-active-directory).  

- **Gérer les ressources Azure directement par le biais d’[Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) incorporé dans Windows Admin Center**  
Tirez parti d’Azure Cloud Shell pour obtenir une expérience Bash ou PowerShell dans Windows Admin Center et vous permettre d’accéder facilement aux tâches de gestion Azure.  
Pour plus d’informations, consultez [Vue d’ensemble d’Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).


## <a name="see-also"></a>Voir aussi

- [Connecter Windows Admin Center à Azure](azure-integration.md)
- [Déployer Windows Admin Center dans Azure](deploy-wac-in-azure.md)
