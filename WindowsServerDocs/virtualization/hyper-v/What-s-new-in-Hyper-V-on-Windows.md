---
title: Quelles sont les nouveautés dans Hyper-V sur Windows Server 2016
description: Fournit un résumé des nouvelles fonctionnalités dans Hyper-V
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1a65a98e-54b6-4c41-9732-1e3d32fe3a5f
author: KBDAzure
ms.author: kathydav
ms.date: 09/21/2017
ms.openlocfilehash: 8b7d9233b105f710d620b5142205fb2eadd0248a
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/14/2019
ms.locfileid: "67141371"
---
# <a name="whats-new-in-hyper-v-on-windows-server"></a>Quelles sont les nouveautés dans Hyper-V sur Windows Server

>S'applique à : Windows Server 2019, Microsoft Hyper-V Server 2016, Windows Server 2016
  
Cet article décrit les fonctionnalités nouvelles et modifiées de Hyper-V sur Windows Server 2019, Windows Server 2016 et Microsoft Hyper-V Server 2016. Pour utiliser les nouvelles fonctionnalités sur des machines virtuelles créées avec Windows Server 2012 R2 et déplacé ou importés sur un serveur qui exécute Hyper-V sur Windows Server 2019 ou Windows Server 2016, vous devez manuellement mettre à niveau la version de configuration de machine virtuelle. Pour obtenir des instructions, consultez [version mise à niveau virtual machine](deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md).  
  
Voici ce qui est inclus dans cet article et que la fonctionnalité est nouveau ou mis à jour.  

## <a name="windows-server-version-1903"></a>Windows Server, version 1903

### <a name="add-hyper-v-manager-to-server-core-installations-updated"></a>Ajouter le Gestionnaire Hyper-V pour les installations Server Core (mis à jour)

Comme vous le savez peut-être, nous recommandons d’utiliser l’option d’installation Server Core lors de l’utilisation de Windows Server, canal semi-annuel en production. Toutefois, Server Core par défaut omet un nombre d’outils de gestion utiles. Vous pouvez ajouter les plus nombreux outils couramment utilisés en installant la fonctionnalité de compatibilité des applications, mais il y a toujours eu de certains outils manquants.

Par conséquent, en fonction des commentaires des clients, nous avons ajouté un ou plusieurs outils à la fonctionnalité de compatibilité des applications dans cette version : Gestionnaire Hyper-V (virtmgmt.msc).

Pour plus d’informations, consultez [fonctionnalité de compatibilité d’application Server Core](../../get-started-19/install-fod-19.md).

## <a name="windows-server-2019"></a>Windows Server 2019

### <a name="security-shielded-virtual-machines-improvements-new"></a>Sécurité : Améliorations de Machines virtuelles protégées (nouveau)

- **Améliorations d’office de branche**

    Vous pouvez maintenant exécuter des machines virtuelles dotées d'une protection maximale sur des ordinateurs ayant une connectivité intermittente au Service Guardian hôte en tirant parti du nouveau [SGH de secours](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-manage-branch-office#fallback-configuration) et des fonctionnalités du [mode hors connexion](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-manage-branch-office#offline-mode). Le SGH de secours vous permet de configurer un deuxième ensemble d'URL qu'Hyper-V peut essayer s'il ne peut pas atteindre votre serveur SGH principal.

    Le mode hors connexion vous permet de continuer à démarrer vos ordinateurs virtuels protégés même si le SGH n'est pas accessible, tant que l'ordinateur virtuel a démarré avec succès une fois et que la configuration de sécurité de l'hôte n'a pas changé.

- **Améliorations du dépannage**

    Nous avons également simplifié le [dépannage de vos machines virtuelles](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-troubleshoot-shielded-vms) en autorisant la prise en charge du Mode de Session étendu VMConnect et de PowerShell Direct. Ces outils sont particulièrement utiles si vous avez perdu la connectivité réseau à votre ordinateur virtuel et que vous devez mettre à jour sa configuration pour restaurer l'accès.

    Ces fonctionnalités n'ont pas besoin d'être configurées et deviennent automatiquement disponibles lorsqu'un ordinateur virtuel protégé est placé sur un ordinateur hôte Hyper-V exécutant Windows Server version 1803 ou version ultérieure.

- **Prise en charge de Linux**

    Si vous exécutez des environnements à systèmes d'exploitation mixtes, Windows Server 2019 prend désormais en charge l'exécution d'Ubuntu, Red Hat Enterprise Linux et SUSE Linux Enterprise Server dans les machines virtuelles.

## <a name="windows-server-2016"></a>Windows Server 2016

### <a name="compatible-with-connected-standby-new"></a>Compatible avec la veille connectée \(nouveau\)

Lorsque le rôle Hyper-V est installé sur un ordinateur qui utilise le mode d’alimentation toujours AOAC connecté (AOAC), le **veille connectée** état d’alimentation est désormais disponible.  
  
### <a name="discrete-device-assignment-new"></a>Attribution discrète d’appareils \(nouveau\)

Cette fonctionnalité vous permet de donner un machine virtuelle un accès direct et exclusives pour certains périphériques PCIe. À l’aide d’un appareil de cette façon contourne la pile de virtualisation Hyper-V, ce qui entraîne un accès plus rapide. Pour plus d’informations sur le matériel pris en charge, consultez « Attribution discrète d’appareils » dans [configuration système requise pour Hyper-V sur Windows Server 2016](System-requirements-for-Hyper-V-on-Windows.md). Pour plus d’informations, notamment l’utilisation de cette fonctionnalité et les considérations, consultez le billet «[attribution discrète d’appareil, Description et arrière-plan](https://blogs.technet.microsoft.com/virtualization/2015/11/19/discrete-device-assignment-description-and-background/)» dans le blog de virtualisation.

### <a name="encryption-support-for-the-operating-system-disk-in-generation-1-virtual-machines-new"></a>Prise en charge du chiffrement pour le disque de système d’exploitation dans des machines virtuelles de génération 1 \(nouveau)

Vous pouvez désormais protéger le disque de système d’exploitation à l’aide du chiffrement de lecteur BitLocker sur des machines virtuelles de génération 1. Une nouvelle fonctionnalité, le stockage de clés, crée un lecteur de petite et dédié pour stocker la clé de BitLocker du lecteur système. Pour cela, au lieu d’utiliser un virtuel Trusted Platform Module (TPM), qui est disponible uniquement dans les machines virtuelles de génération 2. Pour déchiffrer le disque et démarrer l’ordinateur virtuel, l’hôte Hyper-V doit faire partie d’une infrastructure protégée autorisée ou a la clé privée à partir d’un des gardiens de la machine virtuelle. Stockage de clés nécessite une machine virtuelle de la version 8. Pour plus d’informations sur la version de la machine virtuelle, consultez [version de mise à niveau machine virtuelle dans Hyper-V sur Windows 10 ou Windows Server 2016](./deploy/upgrade-virtual-machine-version-in-hyper-v-on-windows-or-windows-server.md).  
  
### <a name="host-resource-protection-new"></a>Héberger de protection des ressources \(nouveau\)

Cette fonctionnalité empêche un ordinateur virtuel à l’aide de plus de sa part de ressources système en recherchant des niveaux excessive d’activité. Cela peut aider à empêcher une activité excessive de la machine virtuelle à partir de dégrader les performances de l’hôte ou d’autres machines virtuelles. Lorsque l’analyse détecte une machine virtuelle avec une activité excessive, l’ordinateur virtuel reçoit moins de ressources. Cette surveillance et la mise en œuvre est désactivée par défaut. Utiliser Windows PowerShell pour activer ou désactiver. Pour l’activer, exécutez cette commande :  
  
```
Set-VMProcessor TestVM -EnableHostResourceProtection $true
```

Pour plus d’informations sur cette applet de commande, consultez [Set-VMProcessor](https://docs.microsoft.com/powershell/module/hyper-v/set-vmprocessor).

### <a name="hot-add-and-remove-for-network-adapters-and-memory-new"></a>Chaud ajouter et supprimer des cartes réseau et mémoire \(nouveau\)

Vous pouvez maintenant ajouter ou supprimer une carte réseau, tandis que la machine virtuelle est en cours d’exécution, sans temps d’arrêt acquitter. Cela fonctionne pour les machines virtuelles de génération 2 qui exécutent des systèmes d’exploitation Windows ou Linux.  
  
Vous pouvez également ajuster la quantité de mémoire attribuée à une machine virtuelle pendant son exécution, même si vous n’avez pas activé la mémoire dynamique. Cela fonctionne pour la génération 1 et machines virtuelles de génération 2, exécutant Windows Server 2016 ou Windows 10.  

### <a name="hyper-v-manager-improvements-updated"></a>Améliorations du Gestionnaire Hyper-V \(mis à jour\) 
  
-   **Prise en charge des informations d’identification de remplacement** -vous pouvez désormais utiliser un ensemble différent d’informations d’identification dans le Gestionnaire Hyper-V lorsque vous vous connectez à un autre hôte distant Windows Server 2016 ou Windows 10. Vous pouvez également enregistrer ces informations d’identification pour le rendre plus facile de se reconnecter.  
  
-   **Gérer des versions antérieures** -avec le Gestionnaire des Hyper-V dans Windows Server 2019, Windows Server 2016 et Windows 10, vous pouvez gérer les ordinateurs exécutant Hyper-V sur Windows Server 2012, Windows 8, Windows Server 2012 R2 et Windows 8.1.  
  
-   **Protocole de gestion mis à jour** -Gestionnaire Hyper-V est désormais communique avec les hôtes Hyper-V distants à l’aide du protocole WS-MAN, celui-ci autorisant l’authentification CredSSP, Kerberos ou NTLM. Lorsque vous utilisez CredSSP pour vous connecter à un hôte Hyper-V distant, vous pouvez effectuer une migration dynamique sans activer la délégation dans Active Directory. L’infrastructure WS-MAN rend également plus facile à activer un ordinateur hôte pour la gestion à distance. WS-MAN se connecte par le biais du port 80 qui est ouvert par défaut.  
  
### <a name="integration-services-delivered-through-windows-update-updated"></a>Les services d’intégration fournis via Windows Update \(mis à jour\) 

Mises à jour des services d’intégration pour les invités Windows sont distribuées via Windows Update. Pour les fournisseurs de services et aux hébergeurs de nuage privé, cela place le contrôle de l’application de mises à jour dans les mains des clients qui possèdent les machines virtuelles. Clients peuvent désormais mettre à jour leurs ordinateurs virtuels de Windows avec toutes les mises à jour, y compris les services d’intégration, à l’aide d’une méthode unique. Pour plus d’informations sur les services d’intégration pour les invités Linux, consultez [Linux Machines virtuelles et FreeBSD sur Hyper-V](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md).  
  
> [!IMPORTANT]  
> Le fichier d’image vmguest.iso n’est plus nécessaire, afin qu’il n’est pas inclus avec Hyper-V sur Windows Server 2016.  
  
### <a name="linux-secure-boot-new"></a>Démarrage sécurisé de Linux \(nouveau\) 

Les systèmes d’exploitation Linux s’exécutant sur des ordinateurs virtuels de génération 2 peuvent désormais démarrer avec l’option de démarrage sécurisé est activée. Ubuntu 14.04 et versions ultérieur, SUSE Linux Enterprise Server 12 et ultérieur, Red Hat Enterprise Linux 7.0 et versions ultérieur et CentOS 7.0 et versions ultérieures sont activés pour le démarrage sécurisé sur les ordinateurs hôtes qui exécutent Windows Server 2016. Avant de démarrer la machine virtuelle pour la première fois, vous devez configurer la machine virtuelle pour utiliser l’autorité de certification Microsoft UEFI. Pour cela, dans le Gestionnaire Hyper-V, Virtual Machine Manager ou une session Windows Powershell avec élévation de privilèges. Pour Windows PowerShell, exécutez cette commande :  
  
```  
Set-VMFirmware TestVM -SecureBootTemplate MicrosoftUEFICertificateAuthority  
```  
  
Pour plus d’informations sur les machines virtuelles Linux sur Hyper-V, consultez [Linux Machines virtuelles et FreeBSD sur Hyper-V](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md). Pour plus d’informations sur l’applet de commande, consultez [Set-VMFirmware](https://docs.microsoft.com/powershell/module/hyper-v/set-vmfirmware).

### <a name="more-memory-and-processors-for-generation-2-virtual-machines-and-hyper-v-hosts-updated"></a>Plus de mémoire et de processeurs pour les ordinateurs virtuels de génération 2 et les hôtes Hyper-V \(mis à jour\)

Depuis la version 8, ordinateurs virtuels de génération 2 peuvent utiliser beaucoup plus de mémoire et de processeurs virtuels. Hôtes peuvent être configurés avec beaucoup plus de mémoire et de processeurs virtuels qu’étaient précédemment prises en charge. Ces modifications prennent en charge les nouveaux scénarios telles que l’exécution des bases de données volumineuses en mémoire pour le traitement transactionnel en ligne (OLTP) et d’entreposage de données de commerce électronique. Le blog de Windows Server a récemment publié les résultats des performances d’une machine virtuelle avec 5.5 téraoctets de mémoire et 128 processeurs virtuels en cours d’exécution de base de données en mémoire de 4 To. Performances étaient supérieure à 95 % des performances d’un serveur physique. Pour plus d’informations, consultez [performances des machines virtuelles à grande échelle Windows Server 2016 Hyper-V pour le traitement de transactions en mémoire](https://blogs.technet.microsoft.com/windowsserver/2016/09/28/windows-server-2016-hyper-v-large-scale-vm-performance-for-in-memory-transaction-processing/). Pour plus d’informations sur les versions de la machine virtuelle, consultez [version de mise à niveau machine virtuelle dans Hyper-V sur Windows 10 ou Windows Server 2016](./deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md). Pour obtenir la liste complète des configurations maximales prises en charge, consultez [planifier extensibilité Hyper-V dans Windows Server 2016](./plan/plan-hyper-v-scalability-in-windows-server.md). 

### <a name="nested-virtualization-new"></a>Virtualisation imbriquée \(nouveau\)

Cette fonctionnalité vous permet d’utiliser une machine virtuelle comme un hôte Hyper-V et créer des machines virtuelles au sein de cet hôte virtualisé. Cela peut être particulièrement utile pour les environnements de développement et test. Pour utiliser la virtualisation imbriquée, vous devez :  
  
-   Pour exécuter au moins Windows Server 2019, Windows Server 2016 ou Windows 10 sur l’hôte Hyper-V physique et l’hôte virtualisé.  
  
-   Un processeur Intel VT-x (virtualisation imbriquée est disponible uniquement pour les processeurs Intel pour l’instant).  
  
Pour plus d’informations et des instructions, consultez [exécuter Hyper-V dans une Machine virtuelle avec la virtualisation imbriquée](https://docs.microsoft.com/virtualization/hyper-v-on-windows/user-guide/nested-virtualization).  
  
### <a name="networking-features-new"></a>Fonctionnalités de mise en réseau \(nouveau\)

Nouvelles fonctionnalités de mise en réseau :  
  
-   **À distance, accès direct en mémoire (RDMA) et basculer association incorporée (SET)** . Vous pouvez configurer RDMA sur les cartes réseau liées à un commutateur virtuel Hyper-V, indépendamment de si SET est également utilisé. ENSEMBLE fournit un commutateur virtuel avec certaines des mêmes fonctionnalités que l’association de cartes réseau. Pour plus d’informations, consultez [accès de mémoire Direct à distance (RDMA) et l’association de cartes SET (Switch Embedded)](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming).  
  
-   **Files d’attente de plusieurs machines virtuelles (VMMQ)** . Améliore le débit VMQ en allouant plusieurs files d’attente de matériel par machine virtuelle.  La file d’attente par défaut devienne un ensemble de files d’attente pour une machine virtuelle, et le trafic est réparti entre les files d’attente.  
  
-   **Qualité de service (QoS) pour les réseaux défini par logiciel**. Gère la classe par défaut de trafic via le commutateur virtuel au sein de la bande passante de classe par défaut.  
  
Pour plus d’informations sur les nouvelles fonctionnalités de mise en réseau, consultez [Nouveautés de la mise en réseau](../../networking/What-s-New-in-Networking.md).  
  
### <a name="production-checkpoints-new"></a>Points de contrôle de production \(nouveau\)

Points de contrôle de production sont des images de « point-à-temps » d’une machine virtuelle. Ils vous donnent un moyen d’appliquer un point de contrôle qui est conforme aux stratégies de prise en charge lorsqu’une machine virtuelle exécute une charge de travail de production. Points de contrôle de production sont basées sur la technologie de sauvegarde à l’intérieur de l’invité au lieu d’un état enregistré. Pour les machines virtuelles Windows, le Service de capture instantanée de Volume (VSS) est utilisé. Pour les machines virtuelles Linux, les tampons sont vidées pour créer un point de contrôle qui est cohérente avec le système de fichiers. Si vous préférez utiliser les points de contrôle en fonction des États enregistrés, choisir à la place des points de contrôle standard. Pour plus d’informations, consultez [choisir entre les points de contrôle standard ou de production dans Hyper-V](manage/Choose-between-standard-or-production-checkpoints-in-Hyper-V.md).  
  
> [!IMPORTANT]  
> Nouveaux ordinateurs virtuels utilisent des points de contrôle de production en tant que la valeur par défaut.  
  
### <a name="rolling-hyper-v-cluster-upgrade-new"></a>Mise à niveau de Cluster Hyper-V propagée \(nouveau\)

Vous pouvez maintenant ajouter un nœud qui exécute Windows Server 2019 ou Windows Server 2016 à un Cluster Hyper-V avec des nœuds exécutant Windows Server 2012 R2. Cela vous permet de mettre à niveau le cluster sans temps d’arrêt. Le cluster s’exécute à un niveau de fonctionnalité de Windows Server 2012 R2 jusqu'à ce que vous mettez à niveau tous les nœuds du cluster et mettre à jour le niveau fonctionnel du cluster avec l’applet de commande Windows PowerShell, [Update-ClusterFunctionalLevel](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel).  
  
> [!IMPORTANT]  
> Une fois que vous mettez à jour le niveau fonctionnel du cluster, vous ne pouvez pas lui rendre Windows Server 2012 R2.  
  
Pour un cluster Hyper-V avec un niveau fonctionnel de Windows Server 2012 R2 avec des nœuds exécutant Windows Server 2012 R2, Windows Server 2019 et Windows Server 2016, notez les points suivants :  
  
-   Gérer le cluster, Hyper-V et les machines virtuelles à partir d’un nœud qui exécute Windows Server 2016 ou Windows 10.  
  
-   Vous pouvez déplacer des machines virtuelles entre tous les nœuds du cluster Hyper-V.  
  
-   Pour utiliser les nouvelles fonctionnalités Hyper-V, tous les nœuds doivent exécuter Windows Server 2016 ou et le niveau fonctionnel du cluster doit être mis à jour.  
  
-   La version de configuration de machine virtuelle pour les machines virtuelles existantes n’est pas mis à niveau. Vous pouvez mettre à niveau la version de configuration uniquement une fois que vous mettez à niveau le niveau fonctionnel du cluster.  
  
-   Les machines virtuelles que vous créez sont compatibles avec Windows Server 2012 R2, le niveau de configuration de machine virtuelle 5.  
  
Une fois que vous mettez à jour le niveau fonctionnel du cluster :  
  
-   Vous pouvez activer les nouvelles fonctionnalités Hyper-V.  
  
-   Pour rendre les nouvelles fonctionnalités de machine virtuelle, utilisez l’applet de commande Update-VmConfigurationVersion mettre à jour manuellement le niveau de la configuration de machine virtuelle. Pour obtenir des instructions, consultez [version mise à niveau virtual machine](deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md).    
-   Vous ne pouvez pas ajouter un nœud au Cluster Hyper-V qui exécute Windows Server 2012 R2.  
  
> [!NOTE]  
> Hyper-V sur Windows 10 ne prend pas en charge le clustering de basculement.  
  
Pour plus d’informations et des instructions, consultez le [niveau propagée de Cluster système d’exploitation](https://technet.microsoft.com/library/dn850430.aspx).  

### <a name="shared-virtual-hard-disks-updated"></a>Des disques dur virtuels partagés \(mis à jour\)
Vous pouvez désormais redimensionner les disques durs (fichiers .vhdx) de virtuels partagés utilisés pour le clustering invité, sans temps d’arrêt. Disques durs virtuels partagés peut être augmentés ou réduits lorsque la machine virtuelle est en ligne. Les clusters invités peuvent maintenant protéger également des disques durs virtuels partagés à l’aide de réplica Hyper-V pour la récupération d’urgence.

Activer la réplication sur la collection. Activer la réplication sur une collection est **exposées uniquement via l’interface WMI**. Consultez la documentation de [Msvm_CollectionReplicationService classe](https://msdn.microsoft.com/library/mt167787%28v=vs.85%29.aspx) pour plus d’informations. **Vous ne pouvez pas gérer la réplication d’une collection via l’applet de commande PowerShell ou l’interface utilisateur.** Les machines virtuelles doivent être sur des hôtes qui font partie d’un cluster Hyper-V pour accéder aux fonctionnalités qui sont spécifiques à une collection. Cela inclut le disque dur virtuel partagé - partagés disques durs virtuels sur des ordinateurs hôtes autonomes ne sont pas pris en charge par le réplica Hyper-V.

Suivez les instructions pour les disques durs virtuels partagés dans [Virtual Hard Disk Sharing Overview](https://technet.microsoft.com/library/dn281956.aspx)et n’oubliez pas que vos disques durs virtuels partagés font partie d’un cluster invité. 

Une collection avec un disque dur virtuel partagé, mais aucun cluster invité associé ne peut pas créer des points de référence pour la collection (le disque dur virtuel partagé est qu’inclus ou non dans la création de point de référence ou non). 

### <a name="virtual-machine-backupnew"></a>Sauvegarde de machines virtuelles\(nouveau\)

Si vous sauvegardez une seule machine virtuelle (indépendamment de si hôte est mis en cluster ou non), vous ne devez pas utiliser un groupe de machines virtuelles.  Ni vous devez utiliser une collecte de captures instantanées. Groupes de machines virtuelles et de collecte de captures instantanées sont destinés à être utilisés uniquement pour la sauvegarde des clusters invités qui sont à l’aide de vhdx partagé. Au lieu de cela, vous devez prendre un instantané à l’aide de la [fournisseur WMI d’Hyper-V v2](https://msdn.microsoft.com/library/windows/desktop/hh850319(v=vs.85).aspx). De même, n’utilisez pas le [fournisseur WMI de Cluster de basculement](https://msdn.microsoft.com/library/windows/desktop/mt167750(v=vs.85).aspx).

### <a name="shielded-virtual-machines-new"></a>Des machines virtuelles protégées \(nouveau\)

Protégées utilisation de machines virtuelles à plusieurs fonctionnalités qui rendent plus difficile pour les administrateurs Hyper-V et les programmes malveillants sur l’ordinateur hôte inspecter, de falsifier ou de voler des données à partir de l’état d’une machine virtuelle protégée. Données et l’état est chiffré, administrateurs Hyper-V ne peut pas voir la sortie vidéo et les disques et les machines virtuelles peuvent être limitées à exécuter uniquement sur des hôtes connus, sains, tel que déterminé par un hôte de surveillance de serveur. Pour plus d’informations, consultez [infrastructure service Guardian et des machines virtuelles protégées](../../security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms.md).
  
> [!NOTE]  
> Des machines virtuelles protégées sont compatibles avec Hyper-V Replica. Pour répliquer une machine virtuelle protégée, l’hôte que vous souhaitez effectuer la réplication doit être autorisé à exécuter cette machine virtuelle protégée.  

### <a name="start-order-priority-for-clustered-virtual-machines-new"></a>Démarrer la priorité de commande pour les ordinateurs virtuels en cluster \(nouveau\)

Cette fonctionnalité vous donne davantage de contrôle sur lequel les ordinateurs virtuels en cluster sont démarrés ou d’abord redémarrés. Cela rend plus facile à démarrer les ordinateurs virtuels qui fournissent des services avant les ordinateurs virtuels qui utilisent ces services. Définir des jeux, placer des machines virtuelles dans des jeux et spécifier les dépendances. Utiliser les applets de commande Windows PowerShell pour gérer les ensembles, telles que [New-ClusterGroupSet](https://docs.microsoft.com/powershell/module/failoverclusters/new-clustergroupset), [Get-ClusterGroupSet](https://docs.microsoft.com/powershell/module/failoverclusters/get-clustergroupset), et [Add-ClusterGroupSetDependency](https://docs.microsoft.com/powershell/module/failoverclusters/add-clustergroupsetdependency).
.  
### <a name="storage-quality-of-service-qos-updated"></a>Qualité de service (QoS) de stockage \(mis à jour\)

Vous pouvez maintenant créer des stratégies QoS de stockage sur un serveur de fichiers avec montée en puissance parallèle et les affecter à un ou plusieurs disques virtuels sur des machines virtuelles Hyper-V. Les performances de stockage sont automatiquement réajustées pour répondre aux stratégies quand la charge de stockage varie. Pour plus d’informations, consultez [qualité de Service de stockage](../../storage/storage-qos/storage-qos-overview.md).  
  
### <a name="virtual-machine-configuration-file-format-updated"></a>Format de fichier de configuration de machine virtuelle \(mis à jour\)

Fichiers de configuration de machine virtuelle utilisent un nouveau format qui rend la lecture et écriture de données de configuration plus efficaces. Le format rend également une altération des données moins susceptibles de cas de défaillance de stockage. Fichiers de données de configuration de machine virtuelle utiliser une extension de nom de fichier .vmcx et fichiers de données d’état runtime utilisent une extension de nom de fichier .vmrs.  
  
> [!IMPORTANT]  
> L’extension de nom de fichier .vmcx indique un fichier binaire. Modification des fichiers .vmcx ou .vmrs n’est pas prise en charge.  
  
### <a name="virtual-machine-configuration-version-updated"></a>Version de configuration de machine virtuelle \(mis à jour\)

La version représente la compatibilité de la configuration de la machine virtuelle, enregistré l’état et les fichiers de capture instantanée avec la version d’Hyper-V. Machines virtuelles avec la version 5 sont compatibles avec Windows Server 2012 R2 et peuvent s’exécuter sur Windows Server 2012 R2 et Windows Server 2016. Machines virtuelles avec les versions introduites dans Windows Server 2016 et et Windows Server 2019 ne s’exécute pas dans Hyper-V sur Windows Server 2012 R2.   
  
Si vous déplacez ou importez un ordinateur virtuel sur un serveur qui exécute Hyper-V sur Windows Server 2016 ou Windows Server 2019 à partir de Windows Server 2012 R2, configuration de la machine virtuelle n’est pas automatiquement mis à jour. Cela signifie que vous pouvez ramener l’ordinateur virtuel vers un serveur qui exécute Windows Server 2012 R2. Toutefois, cela signifie également que vous ne pouvez pas utiliser les nouvelles fonctionnalités de machine virtuelle jusqu'à ce que vous mettez à jour la version de la configuration de machine virtuelle manuellement.  
  
Pour obtenir des instructions sur la vérification et mise à niveau de la version, consultez [version mise à niveau virtual machine](deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md). Cet article répertorie également la version dans laquelle certaines fonctionnalités ont été introduites.   
  
> [!IMPORTANT]  
> -   Une fois que vous mettez à jour la version, vous ne pouvez pas déplacer l’ordinateur virtuel à un serveur qui exécute Windows Server 2012 R2.  
> -   Vous ne pouvez pas rétrograder la configuration à une version antérieure.  
> -   Le [mise à jour-VMVersion](https://docs.microsoft.com/powershell/module/hyper-v/update-vmversion) applet de commande est bloqué sur un Cluster Hyper-V lorsque le niveau fonctionnel est Windows Server 2012 R2.  

### <a name="virtualization-based-security-for-generation-2-virtual-machines-new"></a>Sécurité Virtualization-based pour les machines virtuelles de génération 2 \(nouveau)

Sécurité basée sur la virtualisation alimente des fonctionnalités telles que Device Guard et Credential Guard, offrant une protection accrue du système d’exploitation contre les attaques contre les programmes malveillants. Sécurité basée sur la virtualisation est disponible dans les ordinateurs virtuels de génération 2 invité depuis la version 8. Pour plus d’informations sur la version de la machine virtuelle, consultez [version de mise à niveau machine virtuelle dans Hyper-V sur Windows 10 ou Windows Server 2016](./deploy/upgrade-virtual-machine-version-in-hyper-v-on-windows-or-windows-server.md).

### <a name="windows-containers-new"></a>Les conteneurs Windows \(nouveau\)

Les conteneurs Windows permettent de nombreuses applications isolées pour s’exécuter sur un système d’ordinateur. Ils sont rapides à créer et sont hautement évolutives et portable. Deux types de runtime de conteneurs sont disponibles, chacune avec un degré différent d’isolation d’application. Conteneurs Windows Server utilisent l’isolation de processus et d’espace de noms. Conteneurs Hyper-V utilisent une machine virtuelle de léger pour chaque conteneur.  
  
Les principales fonctionnalités sont les suivantes :  
  
-   Prise en charge des sites web et applications à l’aide de HTTPS  
  
-   NANO server peut héberger les conteneurs Windows Server et Hyper-V  
  
-   Possibilité de gérer des données via des dossiers partagés de conteneur  
  
-   Possibilité de limiter les ressources de conteneur  
  
Pour plus d’informations, y compris les guides de démarrage rapide, consultez le [documentation sur les conteneurs Windows](https://docs.microsoft.com/virtualization/windowscontainers/index).  
  
### <a name="windows-powershell-direct-new"></a>Windows PowerShell Direct \(nouveau\)

Cela vous donne un moyen d’exécuter des commandes Windows PowerShell dans une machine virtuelle à partir de l’hôte. Windows PowerShell Direct s’exécute entre l’hôte et la machine virtuelle. Cela signifie qu’il ne nécessite pas mise en réseau ou la configuration requise du pare-feu, et il fonctionne indépendamment de votre configuration de gestion à distance.  
  
Windows PowerShell Direct est une alternative aux outils existants par les administrateurs Hyper-V pour se connecter à une machine virtuelle sur un ordinateur hôte Hyper-V :  
  
-   Outils de gestion à distance tels que PowerShell ou Bureau à distance  
  
-   Connexion à une Machine virtuelle Hyper-V (VMConnect)  
  
Ces outils fonctionnent bien, mais implique des compromis : VMConnect est fiable, mais il peut être difficile à automatiser. PowerShell à distance est puissant, mais il peut être difficile à configurer et maintenir. Ces compromis peuvent devenir plus importants que votre déploiement de Hyper-V se développe. Windows PowerShell Direct résoudre ce problème en fournissant de puissantes fonctionnalités de script et d’automatisation est aussi simple que d’à l’aide de VMConnect.
  
Pour la configuration requise et des instructions, consultez [Windows de gérer les machines virtuelles avec PowerShell Direct](manage/Manage-Windows-virtual-machines-with-PowerShell-Direct.md).  
