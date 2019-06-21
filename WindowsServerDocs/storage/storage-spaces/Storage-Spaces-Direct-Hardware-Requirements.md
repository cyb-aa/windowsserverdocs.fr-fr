---
title: Configuration matérielle requise pour les espaces de stockage direct
ms.prod: windows-server-threshold
description: Configuration matérielle minimale requise pour tester les espaces de stockage direct
ms.author: eldenc
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: eldenchristensen
ms.date: 06/13/2019
ms.localizationpriority: medium
ms.openlocfilehash: 7fa4560e0c050c8decbcb4e9456a884976e447e2
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284416"
---
# <a name="storage-spaces-direct-hardware-requirements"></a>Configuration matérielle requise pour les espaces de stockage direct

> S’applique à : Windows Server 2019, Windows Server 2016

Cette rubrique décrit la configuration matérielle minimale requise pour les espaces de stockage Direct.

Pour la production, Microsoft vous recommande d’acheter une solution matérielle/logicielle validé à partir de nos partenaires, qui incluent des procédures et des outils de déploiement. Ces solutions sont conçues, assemblées et validées par rapport à notre architecture de référence pour garantir la compatibilité et la fiabilité, afin de vous être opérationnel rapidement opérationnel. Pour les solutions Windows Server 2019, visitez le [site Web de solutions Azure Stack HCL](https://azure.microsoft.com/overview/azure-stack/hci). Pour les solutions Windows Server 2016, en savoir plus sur [défini par le logiciel Windows Server](https://microsoft.com/wssd).

   > [!TIP]
   > À évaluer les espaces de stockage Direct, mais n’ai pas matériel ? Utilisez des machines virtuelles Hyper-V ou Azure comme décrit dans [à l’aide de Storage Spaces Direct dans les clusters d’ordinateurs virtuels invités](storage-spaces-direct-in-vm.md).

## <a name="base-requirements"></a>Exigences de base

Systèmes, les composants, les périphériques et les pilotes doivent être **certifié de Windows Server 2016** par le [catalogue Windows Server](https://www.windowsservercatalog.com). En outre, nous recommandons que les serveurs, les lecteurs, les adaptateurs de bus hôte et les cartes réseau ont le **Software-Defined Data Center (SDDC) Standard** et/ou **Software-Defined Data Center (SDDC) Premium** qualifications supplémentaires (AQs), comme illustré ci-dessous. Il existe plus de 1 000 composants avec le AQs SDDC.

![capture d’écran du catalogue Windows Server montrant le AQs SDDC](media/hardware-requirements/sddc-aqs.png)

Le cluster complètement configuré (serveurs, réseau et stockage) doit réussir tous les [les tests de validation de cluster](https://technet.microsoft.com/library/cc732035(v=ws.10).aspx) par l’Assistant dans le Gestionnaire de Cluster de basculement ou avec le `Test-Cluster` [applet de commande](https://docs.microsoft.com/powershell/module/failoverclusters/test-cluster?view=win10-ps) dans PowerShell.

En outre, les conditions suivantes s’appliquent :

## <a name="servers"></a>Serveurs

- Deux serveurs minimum, 16 serveurs maximum
- Recommandé que tous les serveurs du même fabricant et modèle

## <a name="cpu"></a>Processeur

- Intel Nehalem ou version ultérieure compatible ; ou
- AMD EPYC ou version ultérieure compatible

## <a name="memory"></a>Mémoire

- Mémoire pour Windows Server, les machines virtuelles et d’autres applications ou charges de travail ; signe plus
- 4 Go de RAM par téraoctet (To) de capacité de disque de cache sur chaque serveur, pour les métadonnées d’espaces de stockage Direct

## <a name="boot"></a>Démarrage

- N’importe quel périphérique de démarrage pris en charge par Windows Server, lequel [inclut désormais SATADOM](https://cloudblogs.microsoft.com/windowsserver/2017/08/30/announcing-support-for-satadom-boot-drives-in-windows-server-2016/)
- RAID 1 est mise en miroir **pas** requis, mais est pris en charge pour le démarrage
- Recommandé : Taille minimale de 200 Go

## <a name="networking"></a>Mise en réseau

Minimum (pour le nœud d’à petite échelle 2-3)
- Interface réseau de 10 Gbits/s
- À connexion directe (DAS) est pris en charge avec 2 nœuds

Recommandé (pour de hautes performances, à l’échelle, ni les déploiements de nœuds 4 +)
- Cartes réseau qui est les accès à la mémoire directs à distance (RDMA) compatible, iWARP (recommandé) ou RoCE
- Deux ou plusieurs cartes d’interface réseau pour la redondance et performances
- Interface réseau de 25 Gbits/s ou version ultérieure

## <a name="drives"></a>Lecteurs

Espaces de stockage Direct fonctionne avec DAS SATA, SAS ou NVMe des lecteurs sont physiquement attachés à un seul serveur. Pour plus d'informations sur le choix des disques, voir la rubrique [Choix des disques](choosing-drives.md).

- Les lecteurs SATA, SAS ou NVMe (M.2 U.2 et ajouter de carte) sont toutes prises en charge
- 512n, émulation de 512 octets et 4K natif lecteurs sont toutes prises en charge
- Disques SSD doivent fournir [protection contre la perte d’alimentation](https://blogs.technet.microsoft.com/filecab/2016/11/18/dont-do-it-consumer-ssd/)
- Même nombre et types de lecteurs dans chaque serveur – consultez [considérations relatives à la symétrie de lecteur](drive-symmetry-considerations.md)
- Les appareils de cache doivent être 32 Go ou plus
- Lorsque vous utilisez des dispositifs de mémoire persistante en tant que périphériques cache, vous devez utiliser des appareils de capacité, ou SSD NVMe (vous ne pouvez pas utiliser des disques durs)
- NVMe pilote est l’emploi de Microsoft ou NVMe pilote mis à jour.
- Recommandé : Nombre de lecteurs de capacité est un multiple entier du nombre de lecteurs de cache
- Recommandé : Lecteurs de cache doivent avoir la résistance de l’écriture élevée : au moins 3 lecteur écritures par jour (DWPD) ou au moins 4 téraoctets écrits (TBW) par jour – consultez [lecteur de présentation écrit par jour (DWPD), téraoctets écrite (TBW) et la valeur minimale recommandée pour le stockage Espaces Direct](https://blogs.technet.microsoft.com/filecab/2017/08/11/understanding-dwpd-tbw/)

Voici comment les lecteurs peuvent être connectés pour les espaces de stockage Direct :

- Attachement direct des disques SATA
- Lecteurs NVMe connectés directement
- Adaptateur de bus hôte (HBA) SAS avec des disques SAS
- Adaptateur de bus hôte (HBA) SAS avec des disques SATA
- **NON PRIS EN CHARGE :** RAID SAN (Fibre Channel, iSCSI, FCoE) ou cartes contrôleurs de stockage. Cartes de bus hôte (HBA) doivent implémenter le mode Pass-Through simple.

![interconnexions de diagramme de disque pris en charge](media/hardware-requirements/drive-interconnect-support-1.png)

Les lecteurs peuvent être internes au serveur, ou dans un boîtier externe qui est connecté à un seul serveur. Services SES (SCSI Enclosure) est requis pour l’identification et de mappage d’emplacement. Chaque boîtier externe doit présenter un identificateur unique (ID Unique).

- Lecteurs internes au serveur
- Les disques dans un boîtier externe « JBOD ( ») connecté à un seul serveur
- **NON PRIS EN CHARGE :** Les boîtiers SAS partagés connecté à plusieurs serveurs ou de toute forme de chemins d’accès multiples d’e/s (MPIO) où les lecteurs sont accessibles par plusieurs chemins d’accès.

![interconnexions de diagramme de disque pris en charge](media/hardware-requirements/drive-interconnect-support-2.png)

### <a name="minimum-number-of-drives-excludes-boot-drive"></a>Nombre minimal de disques (exclut le lecteur de démarrage)

- Si des disques sont utilisés en cache, il doit y en avoir au moins deux par serveur.
- Au moins quatre disques de capacité (non utilisés en cache) doivent être utilisés par serveur

| Types de disques présents   | Nombre minimal requis |
|-----------------------|-------------------------|
| Toute la mémoire persistante (même modèle) | mémoire persistante 4 |
| Tous NVMe (même modèle) | 4 NVMe                  |
| Tous SSD (même modèle)  | 4 SSD                   |
| Mémoire persistante + NVMe ou SSD | mémoire persistante 2 + 4 NVMe ou SSD |
| NVMe + SSD            | 2 NVMe + 4 SSD          |
| NVMe + HDD            | 2 NVMe + 4 HDD          |
| SSD + HDD             | 2 SSD + 4 HDD           |
| NVMe + SSD + HDD      | 2 NVMe + 4 autres       |

   >[!NOTE]
   > Ce tableau fournit la valeur minimale pour les déploiements de matériel. Si votre déploiement avec des machines virtuelles et virtualisé stockage, comme Microsoft Azure, consultez [à l’aide de Storage Spaces Direct dans les clusters d’ordinateurs virtuels invités](storage-spaces-direct-in-vm.md).

### <a name="maximum-capacity"></a>Capacité maximale

| Maximums                | Windows Server 2019  | Windows Server 2016  |
| ---                     | ---------            | ---------            |
| Capacité brute par serveur | 100 TO               | 100 TO               |
| Capacité du pool           | 4 GO (4 000 TO)      | 1 PO                 |
