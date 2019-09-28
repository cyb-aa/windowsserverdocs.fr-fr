---
title: Configuration matérielle requise pour les espaces de stockage direct
ms.prod: windows-server
description: Configuration matérielle minimale requise pour tester les espaces de stockage direct
ms.author: eldenc
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: eldenchristensen
ms.date: 08/05/2019
ms.localizationpriority: medium
ms.openlocfilehash: 63a7152ec6abb318a096ac321ae7ccfaaef4d199
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402941"
---
# <a name="storage-spaces-direct-hardware-requirements"></a>Configuration matérielle requise pour les espaces de stockage direct

> S’applique à : Windows Server 2019, Windows Server 2016

Cette rubrique décrit la configuration matérielle minimale requise pour espaces de stockage direct.

Pour la production, Microsoft recommande d’acheter une solution matérielle/logicielle validée auprès de nos partenaires, y compris des outils et des procédures de déploiement. Ces solutions sont conçues, assemblées et validées par rapport à notre architecture de référence pour garantir la compatibilité et la fiabilité, ce qui vous permet d’être opérationnel rapidement. Pour les solutions Windows Server 2019, visitez le [site web Azure Stack solutions HCI](https://azure.microsoft.com/overview/azure-stack/hci). Pour plus d’informations sur les solutions Windows Server 2016, consultez la [définition du logiciel Windows Server](https://microsoft.com/wssd).

   > [!TIP]
   > Vous souhaitez évaluer espaces de stockage direct, mais vous n’avez pas de matériel ? Utilisez des machines virtuelles Hyper-V ou Azure comme décrit dans [utilisation de espaces de stockage direct dans les clusters d’ordinateurs virtuels invités](storage-spaces-direct-in-vm.md).

## <a name="base-requirements"></a>Exigences de base

Les systèmes, les composants, les périphériques et les pilotes doivent être **Windows server 2016 certifiés** par le [Catalogue Windows Server](https://www.windowsservercatalog.com). En outre, nous recommandons que les serveurs, les lecteurs, les adaptateurs de bus hôte et les cartes réseau disposent des qualifications supplémentaires du **Centre de données défini par le logiciel (SDDC) standard** et/ou du **Centre de données défini par logiciel (SDDC) Premium** (AQS), comme indiqué dessus. Il y a plus de 1 000 composants avec le AQs SDDC.

![capture d’écran du catalogue Windows Server montrant le AQs SDDC](media/hardware-requirements/sddc-aqs.png)

Le cluster entièrement configuré (serveurs, mise en réseau et stockage) doit réussir tous les [tests de validation de cluster](https://technet.microsoft.com/library/cc732035(v=ws.10).aspx) pour l’Assistant gestionnaire du cluster de basculement ou avec l' [applet](https://docs.microsoft.com/powershell/module/failoverclusters/test-cluster?view=win10-ps) de commande `Test-Cluster` dans PowerShell.

En outre, les conditions suivantes s’appliquent :

## <a name="servers"></a>Serveurs

- Deux serveurs minimum, 16 serveurs maximum
- Il est recommandé que tous les serveurs soient les mêmes fabricant et modèle

## <a name="cpu"></a>Processeur

- Processeur Intel Nehalem ou version ultérieure compatible ; ni
- Processeur compatible AMD EPYC ou version ultérieure

## <a name="memory"></a>Mémoire

- Mémoire pour Windows Server, les machines virtuelles et d’autres applications ou charges de travail ; Protect
- 4 Go de RAM par téraoctet (to) de capacité de lecteur de cache sur chaque serveur, pour les métadonnées de espaces de stockage direct

## <a name="boot"></a>Partition

- Tout périphérique de démarrage pris en charge par Windows Server, qui [comprend désormais SATADOM](https://cloudblogs.microsoft.com/windowsserver/2017/08/30/announcing-support-for-satadom-boot-drives-in-windows-server-2016/)
- Le miroir RAID 1 n’est **pas** requis, mais il est pris en charge pour le démarrage
- Recommandé : taille minimale de 200 Go

## <a name="networking"></a>Mise en réseau

Espaces de stockage direct nécessite une bande passante élevée fiable et une connexion réseau à faible latence entre chaque nœud.  

Interconnexion minimale pour le nœud à petite échelle 2-3
- carte d’interface réseau (NIC) 10 Gbits/s ou plus rapide
- Au moins deux connexions réseau de chaque nœud sont recommandées pour la redondance et les performances

Connexion recommandée pour des performances élevées, à l’échelle ou dans des déploiements de 4 + 
- Cartes réseau prenant en charge l’accès direct à la mémoire à distance (RDMA), iWARP (recommandé) ou RoCE
- Au moins deux connexions réseau de chaque nœud sont recommandées pour la redondance et les performances
- carte réseau 25 Gbit/s ou plus rapide

Interconnexions de nœuds commutées ou non
- Mettent Les commutateurs réseau doivent être correctement configurés pour gérer la bande passante et le type de mise en réseau.  Si vous utilisez RDMA qui implémente le protocole RoCE, la configuration du commutateur et du périphérique réseau est encore plus importante. 
- Aucune commutation : Les nœuds peuvent être interconnectés à l’aide de connexions directes, évitant ainsi l’utilisation d’un commutateur.  Chaque nœud a une connexion directe avec tous les autres nœuds du cluster.


## <a name="drives"></a>Lecteurs

Espaces de stockage direct fonctionne avec les lecteurs SATA, SAS ou NVMe directement attachés physiquement à un seul serveur. Pour plus d'informations sur le choix des disques, voir la rubrique [Choix des disques](choosing-drives.md).

- Les lecteurs SATA, SAS et NVMe (M. 2, U. 2 et Add-in-Card) sont tous pris en charge
- les lecteurs natifs 512N, émulation et 4K sont tous pris en charge
- Les disques SSD doivent fournir [une protection contre la perte de puissance](https://blogs.technet.microsoft.com/filecab/2016/11/18/dont-do-it-consumer-ssd/)
- Même nombre et types de lecteurs dans chaque serveur – voir [considérations relatives](drive-symmetry-considerations.md) à la symétrie de lecteur
- Les appareils du cache doivent être de 32 Go ou plus
- Lors de l’utilisation de périphériques de mémoire persistants comme périphériques de cache, vous devez utiliser des appareils de capacité NVMe ou SSD (vous ne pouvez pas utiliser de disques durs)
- Le pilote NVMe est fourni par Microsoft et il est inclus dans Windows. (stornvme. sys)
- Recommandé : Le nombre de lecteurs de capacité est un multiple entier du nombre de lecteurs de cache
- Recommandé : Les lecteurs de cache doivent avoir une endurance en écriture élevée : au moins 3 écritures de lecteur par jour (DWPD) ou au moins 4 téraoctets écrits (TBW) par jour : consultez [la page comprendre les écritures de lecteur par jour (DWPD), téraoctets écrits (TBW) et le minimum recommandé pour espaces de stockage direct ](https://blogs.technet.microsoft.com/filecab/2017/08/11/understanding-dwpd-tbw/)

Voici comment les lecteurs peuvent être connectés pour espaces de stockage direct :

- Lecteurs SATA à connexion directe
- Lecteurs NVMe directement attachés
- Adaptateur de bus hôte SAS avec lecteurs SAS
- Adaptateur de bus hôte SAS avec lecteurs SATA
- **NON PRIS EN CHARGE :** Les cartes de contrôleur RAID ou le stockage SAN (Fibre Channel, iSCSI, FCoE). Les cartes HBA doivent implémenter le mode de transfert simple.

![diagramme des interconnexions de lecteur prises en charge](media/hardware-requirements/drive-interconnect-support-1.png)

Les lecteurs peuvent être internes au serveur ou dans un boîtier externe connecté à un seul serveur. Les services de boîtier SCSI sont requis pour le mappage et l’identification des emplacements. Chaque boîtier externe doit présenter un identificateur unique (ID unique).

- Lecteurs internes au serveur
- Lecteurs d’un boîtier externe (« JBOD ») connectés à un serveur
- **NON PRIS EN CHARGE :** Boîtiers SAS partagés connectés à plusieurs serveurs ou toute forme de MPIO (Multipath IO) où les lecteurs sont accessibles par plusieurs chemins d’accès.

![diagramme des interconnexions de lecteur prises en charge](media/hardware-requirements/drive-interconnect-support-2.png)

### <a name="minimum-number-of-drives-excludes-boot-drive"></a>Nombre minimal de lecteurs (exclut le lecteur de démarrage)

- Si des disques sont utilisés en cache, il doit y en avoir au moins deux par serveur.
- Au moins quatre disques de capacité (non utilisés en cache) doivent être utilisés par serveur

| Types de disques présents   | Nombre minimal requis |
|-----------------------|-------------------------|
| Toute la mémoire persistante (même modèle) | 4 mémoire persistante |
| Tous NVMe (même modèle) | 4 NVMe                  |
| Tous SSD (même modèle)  | 4 SSD                   |
| Mémoire persistante + NVMe ou SSD | 2 mémoire persistante + 4 NVMe ou SSD |
| NVMe + SSD            | 2 NVMe + 4 SSD          |
| NVMe + HDD            | 2 NVMe + 4 HDD          |
| SSD + HDD             | 2 SSD + 4 HDD           |
| NVMe + SSD + HDD      | 2 NVMe + 4 autres       |

   >[!NOTE]
   > Ce tableau fournit la valeur minimale pour les déploiements de matériel. Si vous effectuez un déploiement avec des machines virtuelles et un stockage virtualisé, par exemple dans Microsoft Azure, consultez [utilisation de espaces de stockage direct dans des clusters de machines virtuelles invitées](storage-spaces-direct-in-vm.md).

### <a name="maximum-capacity"></a>Capacité maximale

| Valeurs maximales                | Windows Server 2019  | Windows Server 2016  |
| ---                     | ---------            | ---------            |
| Capacité brute par serveur | 400 TO               | 100 TO               |
| Capacité du pool           | 4 PO (4 000 TO)      | 1 PO                 |
