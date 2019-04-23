---
title: Microsoft Hyper-V Server 2016
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: f815ada0-4c63-4e73-9c24-dc5eb21526c7
author: KBDAzure
ms.author: kathydav
ms.date: 07/26/2017
ms.openlocfilehash: 9a1b6ea7b9abc94f63a1390b6fa18e4c8d4a1822
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839370"
---
# <a name="microsoft-hyper-v-server-2016"></a>Microsoft Hyper-V Server 2016

Microsoft Hyper-V Server 2016 est un support\-produit autonome qui contient uniquement l’hyperviseur Windows, un modèle de pilote de Windows Server et les composants de virtualisation. Il fournit une solution de virtualisation fiable et simple pour vous aider à améliorer votre utilisation du serveur et réduire les coûts.

La technologie d’hyperviseur de Windows dans Microsoft Hyper-V Server 2016 est identique à ce qui est dans l’Hyper\-rôle V sur Windows Server 2016. Par conséquent, la majorité du contenu disponible pour Hyper\-rôle V sur Windows Server 2016 s’applique également à Microsoft Hyper-V Server 2016.

## <a name="hyper-v-server-resources-for-it-pros"></a>Hyper\-ressources serveur V pour les professionnels de l’informatique

|Tâches|Ressources|
|-|-|
|![Répond aux exigences symbole](media/All_Symbols_MeetsRequirements.png)|**Évaluer Hyper-V**<br /><br />-   [Vue d’ensemble de la technologie Hyper-V](hyper-v-technology-overview.md)<br />- [Quelles sont les nouveautés dans Hyper-V sur Windows Server 2016](what-s-new-in-hyper-v-on-windows.md)<br />-   [Configuration système requise pour Hyper-V sur Windows Server 2016](system-requirements-for-hyper-v-on-windows.md)<br />-   [Systèmes d’exploitation d’invités Windows pris en charge pour Hyper-V](supported-windows-guest-operating-systems-for-hyper-v-on-windows.md)<br />-   [Prise en charge Linux et les Machines virtuelles FreeBSD](supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows.md)<br />-   [Compatibilité des fonctionnalités par génération et invité](hyper-v-feature-compatibility-by-generation-and-guest.md)<br /><br />**Plan pour Hyper-V**<br /><br />-Décidez quels [génération d’ordinateur virtuel](plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v.md) répond à vos besoins. <br/>-Si vous déplacez ou importation d’ordinateurs virtuels, décider quand [mise à niveau de la version](deploy/upgrade-virtual-machine-version-in-hyper-v-on-windows-or-windows-server.md). <br />- [Évolutivité](plan/plan-hyper-v-scalability-in-windows-server.md) <br />- [Mise en réseau](plan/plan-hyper-v-networking-in-windows-server.md) <br />- [Sécurité](plan/plan-hyper-v-security-in-windows-server.md)|
|![Obtenir la prise en main de symbole](media/All_Symbols_GetStarted.png)|**Bien démarrer avec le serveur Hyper-V**<br /><br />[Téléchargez et installez Microsoft Hyper\-V Server 2016](https://www.microsoft.com/evalcenter/evaluate-hyper-v-server-2016). Cette opération installe l’hyperviseur Windows, un modèle de pilote de Windows Server et les composants de virtualisation. Il est similaire à l’exécution de l’option d’installation Server Core de Windows Server 2016 et le Hyper\-rôle V.|
|![Gérer des symboles](media/All_Symbols_Administrator.png)|**Configurer et gérer le serveur Hyper-V**<br /><br />Hyper\-V Server n’a pas une interface utilisateur graphique \(GUI\). Vous pouvez utiliser les outils suivants pour configurer et gérer Hyper\-V serveur.<br /><br />-   [Configurer une installation Server Core de Windows Server 2016 avec Sconfig.cmd](../../get-started/sconfig-on-ws2016.md) pour mettre à jour les paramètres de domaine ou groupe de travail, modifier la mise à jour de Windows Paramètres, activer la gestion à distance et bien plus encore.<br />-Utilisez une boucle [invite de commandes](../../administration/windows-commands/windows-commands.md) pour les commandes non disponibles dans Sconfig.<br />-Utilisez [Hyper\-Gestionnaire V](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/remote_host_management) ou [de Virtual Machine Manager](https://docs.microsoft.com/system-center/vmm) gérer à distance Hyper\-V serveur. À utiliser Hyper\-V Manager, [installer Hyper\-rôle V sur Windows 10](https://docs.microsoft.com/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v) ou [Windows Server 2016](get-started/install-the-hyper-v-role-on-windows-server.md).<br />-Consultez [installer Server Core](../../get-started/getting-started-with-server-core.md) pour les options de gestion supplémentaires pour les fonctions de serveur de base qui ne sont pas spécifiques à Hyper\-V. La plupart des méthodes de gestion documentées il fonctionnent également avec Hyper\-V serveur.<br /><br />**Configurer et gérer Hyper\-machines virtuelles V**<br /><br />-   [Créer un commutateur virtuel pour les ordinateurs virtuels Hyper-V](get-started/create-a-virtual-switch-for-hyper-v-virtual-machines.md)<br />-   [Créer une machine virtuelle dans Hyper-V](get-started/create-a-virtual-machine-in-hyper-v.md)<br />-   [Choisissez entre les points de contrôle standard ou de production](manage/choose-between-standard-or-production-checkpoints-in-hyper-v.md)<br />-   [Activer ou désactiver des points de contrôle](manage/enable-or-disable-checkpoints-in-hyper-v.md)<br />-   [Gérer les machines virtuelles Windows avec PowerShell Direct](manage/manage-windows-virtual-machines-with-powershell-direct.md) <br /><br />**Déployer**<br /><br />-   [Configurer des hôtes pour la migration dynamique sans Clustering de basculement](deploy/set-up-hosts-for-live-migration-without-failover-clustering.md)<br />- [Mettre à niveau les nœuds de cluster Windows Server](../../failover-clustering/cluster-operating-system-rolling-upgrade.md)<br />- [Mise à niveau de version de la machine virtuelle](deploy/upgrade-virtual-machine-version-in-hyper-v-on-windows-or-windows-server.md)<br />|
