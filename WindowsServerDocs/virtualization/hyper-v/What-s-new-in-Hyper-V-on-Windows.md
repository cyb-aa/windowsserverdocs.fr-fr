---
title: Nouveautés d’Hyper-V sur Windows Server 2016
description: Fournit un résumé des nouvelles fonctionnalités d’Hyper-V
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1a65a98e-54b6-4c41-9732-1e3d32fe3a5f
author: KBDAzure
ms.author: kathydav
ms.date: 09/21/2017
ms.openlocfilehash: 195d78ff8de75ca9e3a88d4300bb2f52cd45632f
ms.sourcegitcommit: 06ae7c34c648538e15c4d9fe330668e7df32fbba
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78371761"
---
# <a name="whats-new-in-hyper-v-on-windows-server"></a>Nouveautés d’Hyper-V sur Windows Server

>S’applique à : Windows Server 2019, Microsoft Hyper-V Server 2016, Windows Server 2016
  
Cet article explique les fonctionnalités nouvelles et modifiées d’Hyper-V sur Windows Server 2019, Windows Server 2016 et Microsoft Hyper-V Server 2016. Pour utiliser les nouvelles fonctionnalités sur des machines virtuelles créées avec Windows Server 2012 R2 et déplacées ou importées vers un serveur qui exécute Hyper-V sur Windows Server 2019 ou Windows Server 2016, vous devez mettre à niveau manuellement la version de configuration de la machine virtuelle. Pour obtenir des instructions, consultez [mettre à niveau la version de la machine virtuelle](deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md).  
  
Voici ce qui est inclus dans cet article et si les fonctionnalités sont nouvelles ou mises à jour.  

## <a name="windows-server-version-1903"></a>Windows Server, version 1903

### <a name="add-hyper-v-manager-to-server-core-installations-updated"></a>Ajouter le Gestionnaire Hyper-V aux installations minimales (mises à jour)

Comme vous le savez peut-être, nous recommandons d’utiliser l’option d’installation Server Core quand vous utilisez le canal semi-annuel de Windows Server en production. Toutefois, Server Core omet par défaut un certain nombre d’outils de gestion utiles. Vous pouvez ajouter un grand nombre des outils les plus couramment utilisés en installant la fonctionnalité de compatibilité des applications, mais certains outils sont toujours manquants.

Par conséquent, en fonction des commentaires des clients, nous avons ajouté un plus grand nombre d’outils à la fonctionnalité de compatibilité des applications dans cette version : le Gestionnaire Hyper-V (virtmgmt. msc).

Pour plus d’informations, consultez [Fonctionnalité de compatibilité des applications Server Core](../../get-started-19/install-fod-19.md).

## <a name="windows-server-2019"></a>Windows Server 2019

### <a name="security-shielded-virtual-machines-improvements-new"></a>Sécurité : améliorations des machines virtuelles protégées (nouveauté)

- **Améliorations pour les filiales**

    Vous pouvez maintenant exécuter des machines virtuelles dotées d’une protection maximale sur des ordinateurs ayant une connectivité intermittente au Service Guardian hôte en tirant parti du nouveau [SGH de secours](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-manage-branch-office#fallback-configuration) et des fonctionnalités du [mode hors connexion](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-manage-branch-office#offline-mode). Le SGH de secours vous permet de configurer un deuxième ensemble d’URL qu’Hyper-V peut essayer s’il ne peut pas atteindre votre serveur SGH principal.

    Le mode hors connexion vous permet de continuer à démarrer vos machines virtuelles protégés même si le SGH n’est pas accessible, tant que la machine virtuelle a démarré avec succès une fois et que la configuration de sécurité de l’hôte n’a pas changé.

- **Améliorations de la résolution des problèmes**

    Nous avons également simplifié le [dépannage de vos machines virtuelles](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-troubleshoot-shielded-vms) en autorisant la prise en charge du Mode de Session étendu VMConnect et de PowerShell Direct. Ces outils sont particulièrement utiles si vous avez perdu la connectivité réseau à votre machine virtuelle et que vous devez mettre à jour sa configuration pour restaurer l’accès.

    Ces fonctionnalités n’ont pas besoin d’être configurées et deviennent automatiquement disponibles quand une machine virtuelle protégé est placée sur un ordinateur hôte Hyper-V exécutant Windows Server version 1803 ou ultérieur.

- **Prise en charge de Linux**

    Si vous exécutez des environnements à systèmes d’exploitation mixtes, Windows Server 2019 prend désormais en charge l’exécution d’Ubuntu, Red Hat Enterprise Linux et SUSE Linux Enterprise Server dans les machines virtuelles.

## <a name="windows-server-2016"></a>Windows Server 2016

### <a name="compatible-with-connected-standby-new"></a>Compatible avec la mise en veille connectée \(nouveau\)

Lorsque le rôle Hyper-V est installé sur un ordinateur qui utilise le modèle d’alimentation Always On/Always connected (alimentation AOAC), l’état d’alimentation de **veille connectée** est désormais disponible.  
  
### <a name="discrete-device-assignment-new"></a>Attribution d’appareil discrète \(nouveau\)

Cette fonctionnalité vous permet d’octroyer à un ordinateur virtuel un accès direct et exclusif à certains périphériques matériels PCIe. L’utilisation d’un périphérique de cette façon contourne la pile de virtualisation Hyper-V, ce qui entraîne un accès plus rapide. Pour plus d’informations sur le matériel pris en charge, voir « affectation de périphérique discrète » dans [Configuration système requise pour Hyper-V sur Windows Server 2016](System-requirements-for-Hyper-V-on-Windows.md). Pour plus d’informations, notamment sur la façon d’utiliser cette fonctionnalité et les considérations à prendre en compte, consultez la publication «[attribution de périphérique discrète — Description and background](https://blogs.technet.microsoft.com/virtualization/2015/11/19/discrete-device-assignment-description-and-background/)» dans le blog sur la virtualisation.

### <a name="encryption-support-for-the-operating-system-disk-in-generation-1-virtual-machines-new"></a>Prise en charge du chiffrement pour le disque du système d’exploitation dans les ordinateurs virtuels de génération 1 \(nouveau)

Vous pouvez maintenant protéger le disque du système d’exploitation à l’aide du chiffrement de lecteur BitLocker dans les ordinateurs virtuels de génération 1. Une nouvelle fonctionnalité, stockage de clés, crée un petit lecteur dédié pour stocker la clé BitLocker du lecteur système. Pour ce faire, vous devez utiliser un Module de plateforme sécurisée (TPM) virtuel (TPM), qui est disponible uniquement sur les ordinateurs virtuels de génération 2. Pour déchiffrer le disque et démarrer l’ordinateur virtuel, l’ordinateur hôte Hyper-V doit faire partie d’une structure protégée autorisée ou disposer de la clé privée de l’un des gardiens de la machine virtuelle. Le stockage de clés nécessite une machine virtuelle de la version 8. Pour plus d’informations sur la version de la machine virtuelle, consultez [mettre à niveau la version de la machine virtuelle dans Hyper-V sur Windows 10 ou Windows Server 2016](./deploy/upgrade-virtual-machine-version-in-hyper-v-on-windows-or-windows-server.md).  
  
### <a name="host-resource-protection-new"></a>Protection des ressources de l’hôte \(nouveau\)

Cette fonctionnalité permet d’empêcher un ordinateur virtuel d’utiliser plus que sa part de ressources système en recherchant des niveaux d’activité excessifs. Cela peut aider à empêcher l’activité excessive d’une machine virtuelle de dégrader les performances de l’ordinateur hôte ou d’autres ordinateurs virtuels. Lorsque l’analyse détecte une machine virtuelle avec une activité excessive, le nombre de ressources est réduit pour l’ordinateur virtuel. Cette surveillance et la mise en application sont désactivées par défaut. Utilisez Windows PowerShell pour l’activer ou la désactiver. Pour l’activer, exécutez la commande suivante :  
  
```
Set-VMProcessor TestVM -EnableHostResourceProtection $true
```

Pour plus d’informations sur cette applet de commande, voir [Set-VMProcessor](https://docs.microsoft.com/powershell/module/hyper-v/set-vmprocessor).

### <a name="hot-add-and-remove-for-network-adapters-and-memory-new"></a>Ajout et suppression à chaud des cartes réseau et de la mémoire \(nouveau\)

Vous pouvez maintenant ajouter ou supprimer une carte réseau pendant que l’ordinateur virtuel est en cours d’exécution, sans entraîner de temps d’arrêt. Cela fonctionne pour les ordinateurs virtuels de génération 2 qui exécutent des systèmes d’exploitation Windows ou Linux.  
  
Vous pouvez également ajuster la quantité de mémoire allouée à un ordinateur virtuel en cours d’exécution, même si vous n’avez pas activé Mémoire dynamique. Cela fonctionne pour les ordinateurs virtuels de génération 1 et de génération 2, exécutant Windows Server 2016 ou Windows 10.  

### <a name="hyper-v-manager-improvements-updated"></a>Les améliorations apportées au Gestionnaire Hyper-V \(mises à jour\) 
  
-   **Prise en charge d’autres informations d’identification** : vous pouvez désormais utiliser un jeu d’informations d’identification différent dans le Gestionnaire Hyper-V quand vous vous connectez à un autre hôte distant windows server 2016 ou Windows 10. Vous pouvez également enregistrer ces informations d’identification pour faciliter la connexion.  
  
-   **Gérer les versions antérieures** : avec le Gestionnaire Hyper-v dans windows server 2019, windows server 2016 et Windows 10, vous pouvez gérer des ordinateurs exécutant Hyper-v sur windows server 2012, Windows 8, windows server 2012 R2 et Windows 8.1.  
  
-   **Protocole de gestion mis à jour** : le Gestionnaire Hyper-v communique désormais avec les hôtes Hyper-v distants à l’aide du protocole WS-Man, qui autorise l’authentification CredSSP, Kerberos ou NTLM. Quand vous utilisez CredSSP pour vous connecter à un hôte Hyper-V distant, vous pouvez effectuer une migration dynamique sans activer la délégation avec restriction dans Active Directory. L’infrastructure basée sur WS-MAN facilite également l’activation d’un hôte pour la gestion à distance. WS-MAN se connecte par le biais du port 80 qui est ouvert par défaut.  
  
### <a name="integration-services-delivered-through-windows-update-updated"></a>Les services d’intégration fournis via Windows Update \(mis à jour\) 

Les mises à jour d’Integration Services pour les invités Windows sont distribuées via Windows Update. Pour les fournisseurs de services et les hébergeurs de cloud privé, cela donne le contrôle de l’application des mises à jour aux mains des locataires qui possèdent les machines virtuelles. Les locataires peuvent désormais mettre à jour leurs machines virtuelles Windows avec toutes les mises à jour, y compris les services d’intégration, à l’aide d’une méthode unique. Pour plus d’informations sur les services d’intégration pour les invités Linux, consultez [machines virtuelles Linux et FreeBSD sur Hyper-V](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md).  
  
> [!IMPORTANT]  
> Le fichier image vmguest. ISO n’est plus nécessaire. il n’est donc pas inclus dans Hyper-V sur Windows Server 2016.  
  
### <a name="linux-secure-boot-new"></a>Démarrage sécurisé Linux \(nouveau\) 

Les systèmes d’exploitation Linux exécutés sur des ordinateurs virtuels de génération 2 peuvent désormais démarrer avec l’option de démarrage sécurisé activée. Ubuntu 14,04 et versions ultérieures, SUSE Linux Enterprise Server 12 et versions ultérieures, Red Hat Enterprise Linux 7,0 et versions ultérieures, et CentOS 7,0 et versions ultérieures sont activés pour un démarrage sécurisé sur les ordinateurs hôtes qui exécutent Windows Server 2016. Avant de démarrer la machine virtuelle pour la première fois, vous devez configurer l’ordinateur virtuel pour qu’il utilise l’autorité de certification Microsoft UEFI. Vous pouvez le faire à partir du Gestionnaire Hyper-V, Virtual Machine Manager ou une session Windows PowerShell avec élévation de privilèges. Pour Windows PowerShell, exécutez la commande suivante :  
  
```  
Set-VMFirmware TestVM -SecureBootTemplate MicrosoftUEFICertificateAuthority  
```  
  
Pour plus d’informations sur les machines virtuelles Linux sur Hyper-V, consultez [machines virtuelles Linux et FreeBSD sur Hyper-v](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md). Pour plus d’informations sur l’applet de commande, voir [Set-VMFirmware](https://docs.microsoft.com/powershell/module/hyper-v/set-vmfirmware).

### <a name="more-memory-and-processors-for-generation-2-virtual-machines-and-hyper-v-hosts-updated"></a>Plus de mémoire et de processeurs pour les ordinateurs virtuels de 2e génération et les hôtes Hyper-V \(mis à jour\)

Depuis la version 8, les machines virtuelles de génération 2 peuvent utiliser beaucoup plus de mémoire et de processeurs virtuels. Les ordinateurs hôtes peuvent également être configurés avec beaucoup plus de mémoire et de processeurs virtuels que ceux précédemment pris en charge. Ces modifications prennent en charge de nouveaux scénarios, tels que l’exécution de bases de données en mémoire volumineuses de commerce électronique pour le traitement transactionnel en ligne (OLTP) et l’entreposage de données (DW). Le blog de Windows Server a récemment publié les résultats de performances d’un ordinateur virtuel avec 5,5 téraoctets de mémoire et 128 processeurs virtuels exécutant une base de données en mémoire de 4 to. Les performances étaient supérieures à 95% des performances d’un serveur physique. Pour plus d’informations, consultez [performances des machines virtuelles Hyper-V Windows Server 2016 Hyper-V pour le traitement des transactions en mémoire](https://blogs.technet.microsoft.com/windowsserver/2016/09/28/windows-server-2016-hyper-v-large-scale-vm-performance-for-in-memory-transaction-processing/). Pour plus d’informations sur les versions des machines virtuelles, consultez [mettre à niveau la version de machine virtuelle dans Hyper-V sur Windows 10 ou Windows Server 2016](./deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md). Pour obtenir la liste complète des configurations maximales prises en charge, consultez [planifier l’extensibilité d’Hyper-V dans Windows Server 2016](./plan/plan-hyper-v-scalability-in-windows-server.md). 

### <a name="nested-virtualization-new"></a>Virtualisation imbriquée \(nouvelle\)

Cette fonctionnalité vous permet d’utiliser une machine virtuelle en tant qu’ordinateur hôte Hyper-V et de créer des machines virtuelles au sein de cet hôte virtualisé. Cela peut s’avérer particulièrement utile pour les environnements de développement et de test. Pour utiliser la virtualisation imbriquée, vous devez disposer des éléments suivants :  
  
-   Pour exécuter au moins Windows Server 2019, Windows Server 2016 ou Windows 10 sur l’hôte Hyper-V physique et l’ordinateur hôte virtualisé.  
  
-   Un processeur avec Intel VT-x (la virtualisation imbriquée est disponible uniquement pour les processeurs Intel pour le moment).  
  
Pour plus d’informations et pour obtenir des instructions, consultez [exécuter Hyper-V sur une machine virtuelle avec la virtualisation imbriquée](https://docs.microsoft.com/virtualization/hyper-v-on-windows/user-guide/nested-virtualization).  
  
### <a name="networking-features-new"></a>Fonctionnalités de mise en réseau \(nouveau\)

Les nouvelles fonctionnalités de mise en réseau sont les suivantes :  
  
-   **Accès direct à la mémoire à distance (RDMA) et Switch Embedded Teaming (Set)** . Vous pouvez configurer RDMA sur des cartes réseau liées à un commutateur virtuel Hyper-V, que l’option définir soit également utilisée ou non. SET fournit un commutateur virtuel avec les mêmes fonctionnalités que l’Association de cartes réseau. Pour plus d’informations, consultez [accès direct à la mémoire à distance (RDMA) et Switch Embedded Teaming (Set)](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming).  
  
-   **Plusieurs files d’attente d’ordinateurs virtuels (VMMQ)** . Améliore le débit de l’ordinateur virtuel en allouant plusieurs files d’attente matérielles par machine virtuelle.  La file d’attente par défaut devient un ensemble de files d’attente pour un ordinateur virtuel et le trafic est réparti entre les files d’attente.  
  
-   **Qualité de service (QoS) pour les réseaux définis par logiciel**. Gère la classe par défaut du trafic via le commutateur virtuel au sein de la bande passante de classe par défaut.  
  
Pour en savoir plus sur les nouvelles fonctionnalités de mise en réseau, consultez [Nouveautés en matière de mise en réseau](../../networking/What-s-New-in-Networking.md).  
  
### <a name="production-checkpoints-new"></a>Points de contrôle de production \(nouveaux\)

Les points de contrôle de production sont des images « ponctuelles » d’une machine virtuelle. Elles vous permettent d’appliquer un point de contrôle qui respecte les stratégies de prise en charge lorsqu’une machine virtuelle exécute une charge de travail de production. Les points de contrôle de production sont basés sur la technologie de sauvegarde au sein de l’invité plutôt que sur un état enregistré. Pour les machines virtuelles Windows, le service d’instantané du volume (VSS) est utilisé. Pour les machines virtuelles Linux, les mémoires tampons du système de fichiers sont vidées pour créer un point de contrôle cohérent avec le système de fichiers. Si vous préférez utiliser des points de contrôle en fonction des États enregistrés, choisissez plutôt points de contrôle standard. Pour plus d’informations, consultez [choisir entre des points de contrôle standard ou de production dans Hyper-V](manage/Choose-between-standard-or-production-checkpoints-in-Hyper-V.md).  
  
> [!IMPORTANT]  
> Les nouvelles machines virtuelles utilisent les points de contrôle de production comme valeur par défaut.  
  
### <a name="rolling-hyper-v-cluster-upgrade-new"></a>Mise à niveau propagée du cluster Hyper-V \(nouveau\)

Vous pouvez maintenant ajouter un nœud exécutant Windows Server 2019 ou Windows Server 2016 à un cluster Hyper-V avec des nœuds exécutant Windows Server 2012 R2. Cela vous permet de mettre à niveau le cluster sans temps d’arrêt. Le cluster s’exécute au niveau de la fonctionnalité Windows Server 2012 R2 jusqu’à ce que vous procédiez à la mise à niveau de tous les nœuds du cluster et que vous mettez à jour le niveau fonctionnel du cluster avec l’applet de commande Windows PowerShell [Update-ClusterFunctionalLevel](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel).  
  
> [!IMPORTANT]  
> Après avoir mis à jour le niveau fonctionnel du cluster, vous ne pouvez pas le retourner à Windows Server 2012 R2.  
  
Pour un cluster Hyper-V avec un niveau fonctionnel de Windows Server 2012 R2 avec des nœuds exécutant Windows Server 2012 R2, Windows Server 2019 et Windows Server 2016, notez les points suivants :  
  
-   Gérer le cluster, Hyper-V et les machines virtuelles à partir d’un nœud exécutant Windows Server 2016 ou Windows 10.  
  
-   Vous pouvez déplacer des ordinateurs virtuels entre tous les nœuds du cluster Hyper-V.  
  
-   Pour utiliser les nouvelles fonctionnalités Hyper-V, tous les nœuds doivent exécuter Windows Server 2016 ou et le niveau fonctionnel du cluster doit être mis à jour.  
  
-   La version de configuration de l’ordinateur virtuel pour les ordinateurs virtuels existants n’est pas mise à niveau. Vous pouvez mettre à niveau la version de configuration uniquement après avoir mis à niveau le niveau fonctionnel du cluster.  
  
-   Les machines virtuelles que vous créez sont compatibles avec Windows Server 2012 R2, niveau de configuration de machine virtuelle 5.  
  
Après avoir mis à jour le niveau fonctionnel du cluster :  
  
-   Vous pouvez activer les nouvelles fonctionnalités Hyper-V.  
  
-   Pour mettre à disposition de nouvelles fonctionnalités de machine virtuelle, utilisez l’applet de commande Update-VmConfigurationVersion pour mettre à jour manuellement le niveau de configuration de l’ordinateur virtuel. Pour obtenir des instructions, consultez [mettre à niveau la version de la machine virtuelle](deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md).    
-   Vous ne pouvez pas ajouter un nœud au cluster Hyper-V qui exécute Windows Server 2012 R2.  
  
> [!NOTE]  
> Hyper-V sur Windows 10 ne prend pas en charge le clustering de basculement.  
  
Pour plus d’informations et pour obtenir des instructions, consultez [mise à niveau propagée du système d’exploitation de cluster](https://technet.microsoft.com/library/dn850430.aspx).  

### <a name="shared-virtual-hard-disks-updated"></a>Les disques durs virtuels partagés \(mis à jour\)
Vous pouvez maintenant redimensionner les disques durs virtuels partagés (fichiers. vhdx) utilisés pour le clustering invité, sans temps d’arrêt. Les disques durs virtuels partagés peuvent être augmentés ou réduits lorsque l’ordinateur virtuel est en ligne. Les clusters invités peuvent désormais également protéger les disques durs virtuels partagés à l’aide de la réplication Hyper-V pour la récupération d’urgence.

Activez la réplication sur la collection. L’activation de la réplication sur une collection est **exposée uniquement via l’interface WMI**. Pour plus d’informations, consultez la documentation relative à la [classe Msvm_CollectionReplicationService](https://msdn.microsoft.com/library/mt167787%28v=vs.85%29.aspx) . **Vous ne pouvez pas gérer la réplication d’une collection via l’applet de commande ou l’interface utilisateur PowerShell.** Les machines virtuelles doivent se trouver sur des hôtes qui font partie d’un cluster Hyper-V pour accéder aux fonctionnalités spécifiques à un regroupement. Cela comprend les VHD partagés partagés partagés sur les ordinateurs hôtes autonomes qui ne sont pas pris en charge par le réplica Hyper-V.

Suivez les instructions relatives aux VHD partagés dans [vue d’ensemble du partage de disque dur virtuel](https://technet.microsoft.com/library/dn281956.aspx)et assurez-vous que vos VHD partagés font partie d’un cluster invité. 

Une collection avec un VHD partagé, mais aucun cluster invité associé ne peut pas créer de points de référence pour la collection (que le VHD partagé soit inclus ou non dans la création du point de référence). 

### <a name="virtual-machine-backupnew"></a>Sauvegarde de la machine virtuelle\(nouveau\)

Si vous sauvegardez une seule machine virtuelle (qu’elle soit ou non en cluster), vous ne devez pas utiliser de groupe de machines virtuelles.  Vous ne devez pas non plus utiliser une collection d’instantanés. Les groupes de machines virtuelles et la collection de captures instantanées sont destinés à être utilisés uniquement pour la sauvegarde des clusters invités qui utilisent un vhdx partagé. Au lieu de cela, vous devez prendre un instantané à l’aide du [fournisseur de Hyper-V WMI v2](https://msdn.microsoft.com/library/windows/desktop/hh850319(v=vs.85).aspx). De même, n’utilisez pas le [fournisseur WMI du cluster de basculement](https://msdn.microsoft.com/library/windows/desktop/mt167750(v=vs.85).aspx).

### <a name="shielded-virtual-machines-new"></a>Machines virtuelles protégées \(nouveaux\)

Les machines virtuelles protégées utilisent plusieurs fonctionnalités pour compliquer l’inspection, la falsification ou le vol des données de l’état d’une machine virtuelle protégée pour les administrateurs et les logiciels malveillants Hyper-V sur l’ordinateur hôte. Les données et l’État sont chiffrés, les administrateurs Hyper-V ne peuvent pas voir la sortie vidéo et les disques, et les ordinateurs virtuels peuvent être limités pour s’exécuter uniquement sur des ordinateurs hôtes connus et sains, comme déterminé par un serveur Guardian hôte. Pour plus d’informations, consultez [infrastructure protégée et machines virtuelles protégées](../../security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms.md).
  
> [!NOTE]  
> Les machines virtuelles protégées sont compatibles avec la réplication Hyper-V. Pour répliquer un ordinateur virtuel protégé, l’ordinateur hôte sur lequel vous souhaitez effectuer la réplication doit être autorisé à exécuter cet ordinateur virtuel protégé.  

### <a name="start-order-priority-for-clustered-virtual-machines-new"></a>Début de la priorité de l’ordre des machines virtuelles en cluster \(nouveau\)

Cette fonctionnalité vous permet de mieux contrôler les machines virtuelles en cluster qui sont démarrées ou redémarrées en premier. Cela facilite le démarrage des ordinateurs virtuels qui fournissent des services avant les ordinateurs virtuels qui utilisent ces services. Définir des ensembles, placer des machines virtuelles dans des ensembles et spécifier des dépendances. Utilisez les applets de commande Windows PowerShell pour gérer les ensembles, tels que [New-ClusterGroupSet](https://docs.microsoft.com/powershell/module/failoverclusters/new-clustergroupset), [ClusterGroupSet](https://docs.microsoft.com/powershell/module/failoverclusters/get-clustergroupset)et [Add-ClusterGroupSetDependency](https://docs.microsoft.com/powershell/module/failoverclusters/add-clustergroupsetdependency).
.  
### <a name="storage-quality-of-service-qos-updated"></a>La qualité de service (QoS) de stockage \(mise à jour\)

Vous pouvez maintenant créer des stratégies QoS de stockage sur un serveur de fichiers avec montée en puissance parallèle et les affecter à un ou plusieurs disques virtuels sur des machines virtuelles Hyper-V. Les performances de stockage sont automatiquement réajustées pour répondre aux stratégies quand la charge de stockage varie. Pour plus d’informations, consultez [qualité de service de stockage](../../storage/storage-qos/storage-qos-overview.md).  
  
### <a name="virtual-machine-configuration-file-format-updated"></a>Le format du fichier de configuration de l’ordinateur virtuel \(mis à jour\)

Les fichiers de configuration d’ordinateur virtuel utilisent un nouveau format qui rend la lecture et l’écriture des données de configuration plus efficace. Le format rend également les données endommagées moins probable en cas de défaillance du stockage. Les fichiers de données de configuration d’ordinateur virtuel utilisent une extension de nom de fichier. vmcx et les fichiers de données d’état d’exécution utilisent une extension de nom de fichier. VMRS.  
  
> [!IMPORTANT]  
> L’extension de nom de fichier. vmcx indique un fichier binaire. La modification des fichiers. vmcx ou. VMRS n’est pas prise en charge.  
  
### <a name="virtual-machine-configuration-version-updated"></a>La version de configuration de l’ordinateur virtuel \(mise à jour\)

La version représente la compatibilité de la configuration de l’ordinateur virtuel, de l’état enregistré et des fichiers d’instantanés avec la version d’Hyper-V. Les machines virtuelles avec la version 5 sont compatibles avec Windows Server 2012 R2 et peuvent s’exécuter sur Windows Server 2012 R2 et Windows Server 2016. Les machines virtuelles avec des versions introduites dans Windows Server 2016 et Windows Server 2019 ne s’exécutent pas dans Hyper-V sur Windows Server 2012 R2.   
  
Si vous déplacez ou importez un ordinateur virtuel sur un serveur exécutant Hyper-V sur Windows Server 2016 ou Windows Server 2019 à partir de Windows Server 2012 R2, la configuration de la machine virtuelle n’est pas automatiquement mise à jour. Cela signifie que vous pouvez déplacer la machine virtuelle vers un serveur qui exécute Windows Server 2012 R2. Mais cela signifie également que vous ne pouvez pas utiliser les nouvelles fonctionnalités de la machine virtuelle tant que vous n’avez pas mis à jour manuellement la version de la configuration de la machine virtuelle.  
  
Pour obtenir des instructions sur la vérification et la mise à niveau de la version, consultez [mettre à niveau la version de la machine virtuelle](deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md). Cet article répertorie également la version dans laquelle certaines fonctionnalités ont été introduites.   
  
> [!IMPORTANT]  
> -   Après avoir mis à jour la version, vous ne pouvez pas déplacer la machine virtuelle vers un serveur qui exécute Windows Server 2012 R2.  
> -   Vous ne pouvez pas rétrograder la configuration à une version antérieure.  
> -   L’applet de commande [Update-VMVersion](https://docs.microsoft.com/powershell/module/hyper-v/update-vmversion) est bloquée sur un cluster Hyper-V lorsque le niveau fonctionnel du cluster est Windows Server 2012 R2.  

### <a name="virtualization-based-security-for-generation-2-virtual-machines-new"></a>Sécurité basée sur la virtualisation pour les ordinateurs virtuels de 2e génération \()

La sécurité basée sur la virtualisation met en œuvre des fonctionnalités telles que Device Guard et Credential Guard, offrant ainsi une protection accrue du système d’exploitation contre les attaques de logiciels malveillants. La sécurité basée sur la virtualisation est disponible dans les machines virtuelles invitées de génération 2 à partir de la version 8. Pour plus d’informations sur la version de la machine virtuelle, consultez [mettre à niveau la version de la machine virtuelle dans Hyper-V sur Windows 10 ou Windows Server 2016](./deploy/upgrade-virtual-machine-version-in-hyper-v-on-windows-or-windows-server.md).

### <a name="windows-containers-new"></a>Conteneurs Windows \(nouveau\)

Les conteneurs Windows permettent à de nombreuses applications isolées de s’exécuter sur un système informatique. Ils sont rapides à créer et sont hautement évolutifs et portables. Deux types de Runtime de conteneur sont disponibles, chacun avec un degré d’isolation d’application différent. Les conteneurs Windows Server utilisent l’espace de noms et l’isolation des processus. Les conteneurs Hyper-V utilisent une machine virtuelle légère pour chaque conteneur.  
  
Les principales fonctionnalités sont les suivantes :  
  
-   Prise en charge des sites Web et des applications à l’aide de HTTPs  
  
-   Nano Server peut héberger à la fois les conteneurs Windows Server et Hyper-V  
  
-   Possibilité de gérer des données via des dossiers partagés de conteneur  
  
-   Possibilité de limiter les ressources de conteneur  
  
Pour plus d’informations, y compris des guides de démarrage rapide, consultez la [documentation relative aux conteneurs Windows](https://docs.microsoft.com/virtualization/windowscontainers/index).  
  
### <a name="windows-powershell-direct-new"></a>Windows PowerShell direct \(nouveau\)

Cela vous permet d’exécuter des commandes Windows PowerShell sur un ordinateur virtuel à partir de l’ordinateur hôte. Windows PowerShell direct s’exécute entre l’hôte et la machine virtuelle. Cela signifie qu’elle ne nécessite pas de réseau ou de pare-feu, et fonctionne indépendamment de la configuration de votre administration à distance.  
  
Windows PowerShell direct est une alternative aux outils existants que les administrateurs Hyper-V utilisent pour se connecter à un ordinateur virtuel sur un ordinateur hôte Hyper-V :  
  
-   Outils de gestion à distance tels que PowerShell ou Bureau à distance  
  
-   Connexion à un ordinateur virtuel Hyper-V (VMConnect)  
  
Ces outils fonctionnent bien, mais ont des compromis : VMConnect est fiable, mais peut être difficile à automatiser. PowerShell à distance est puissant, mais il peut être difficile à configurer et à entretenir. Ces compromis peuvent devenir plus importants à mesure que votre déploiement Hyper-V augmente. Windows PowerShell direct répond à cela en fournissant une expérience de script et d’automatisation puissante, aussi simple que l’utilisation de VMConnect.
  
Pour plus d’informations sur les spécifications et les instructions, consultez [gérer des machines virtuelles Windows avec PowerShell direct](manage/Manage-Windows-virtual-machines-with-PowerShell-Direct.md).  
