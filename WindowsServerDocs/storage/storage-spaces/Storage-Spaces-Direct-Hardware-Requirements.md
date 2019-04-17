---
title: Configuration matérielle requise pour les espaces de stockage direct
ms.prod: windows-server-threshold
description: Configuration matérielle minimale requise pour tester les espaces de stockage direct.
ms.author: eldenc
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: eldenchristensen
ms.date: 04/12/2018
ms.localizationpriority: medium
ms.openlocfilehash: 84d10ab3e25500720dd13e2ba057dc3c5bf05a6f
ms.sourcegitcommit: 515b4fd5c40fcbae0e12a2c30090384533972353
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/29/2018
ms.locfileid: "8232528"
---
# Configuration matérielle requise pour les espaces de stockage direct

> S’applique à: Windows Server 2016, Windows Server Insider Preview

Cette rubrique décrit la configuration matérielle minimale requise pour les espaces de stockage Direct.

Pour la production, Microsoft vous recommande de ces offres matérielle/logicielle [Windows Server Software-Defined](https://microsoft.com/wssd) de nos partenaires, qui incluent des procédures et les outils de déploiement. Ils sont conçues, assemblées et validées par rapport à notre architecture de référence pour assurer la compatibilité et la fiabilité, afin de vous préparer et en cours d’exécution rapidement. En savoir plus sur dans [https://microsoft.com/wssd](https://microsoft.com/wssd).

![logos de nos partenaires Windows Server Software Defined](media/hardware-requirements/wssd-partners.png)

   > [!TIP]
   > D’évaluer les espaces de stockage Direct, mais n’avez pas matériel? Utiliser Hyper-V ou les machines virtuelles Azure comme décrit à l’aide d’espaces de [Stockage Direct dans des clusters de machine virtuelle invité](storage-spaces-direct-in-vm.md).

## Configuration requise de base

Systèmes, composants, appareils et pilotes doivent être **Windows Server 2016 certifiés** par le [Catalogue Windows Server](https://www.windowsservercatalog.com). En outre, nous vous recommandons de serveurs, les lecteurs, les adaptateurs de bus hôte et les cartes réseau les qualifications supplémentaires **Standard de centre de données Software-Defined (SDDC)** et/ou de **Centre de données Software-Defined (SDDC) Premium** (AQs), comme illustré ci-dessous ci-dessous. Il existe plus de 1 000 composants avec la AQs SDDC.

![capture d’écran du catalogue Windows Server montrant le SDDC AQs](media/hardware-requirements/sddc-aqs.png)

Le cluster entièrement configuré (serveurs, réseau et stockage) doit réussir tous les [tests de validation de cluster](https://technet.microsoft.com/library/cc732035(v=ws.10).aspx) par l’Assistant dans le Gestionnaire de Cluster de basculement ou avec le `Test-Cluster` [applet de commande](https://docs.microsoft.com/powershell/module/failoverclusters/test-cluster?view=win10-ps) dans PowerShell.

En outre, les conditions suivantes s’appliquent:

## Serveurs

- Deux serveurs minimum, 16serveurs maximum
- Recommandé que tous les serveurs du même fabricant et modèle

## CPU

- Intel Nehalem ou un processeur compatible; ou
- AMD EPYC ou un processeur compatible

## Mémoire

- Mémoire pour Windows Server, machines virtuelles et les autres applications ou les charges de travail; plus
- 4 Go de RAM par téraoctet (To) de capacité de lecteur de cache sur chaque serveur, pour les espaces de stockage Direct métadonnées

## Démarrage

- N’importe quel périphérique de démarrage pris en charge par Windows Server, qui [inclut désormais SATADOM](https://cloudblogs.microsoft.com/windowsserver/2017/08/30/announcing-support-for-satadom-boot-drives-in-windows-server-2016/)
- RAID 1 miroir n’est **pas** obligatoire, mais est pris en charge pour le démarrage
- Recommandé: taille minimale de 200 Go

## Réseaux

Minimum (pour le nœud de petite échelle 2 à 3)
- Une interface réseau 10 Gbits/s
- De connexion directe (DAS) est pris en charge avec 2 nœuds

Recommandé (pour des performances élevées, à l’échelle ou les déploiements de 4 + nœuds)
- Cartes réseau avec accès direct à distance à la mémoire (RDMA) compatible, iWARP (recommandé) ou RoCE
- Deux ou plusieurs cartes réseau pour la redondance et performances
- 25 Gbits/s, une interface réseau ou une version ultérieure

## Lecteurs

Espaces de stockage Direct fonctionne avec attachement direct SATA, SAS ou NVMe disques physiquement reliés à un seul serveur. Pour plus d'informations sur le choix des disques, voir la rubrique [Choix des disques](choosing-drives.md).

- SATA, SAS ou NVMe lecteurs (M.2, suivants: U.2 et Add-carte complémentaire) sont toutes prises en charge
- 512n, 512e et les lecteurs de 4K natifs sont toutes prises en charge
- Les disques SSD doivent fournir [une protection contre les pertes d’alimentation](https://blogs.technet.microsoft.com/filecab/2016/11/18/dont-do-it-consumer-ssd/)
- Même nombre et les types de disques dans chaque serveur – voir [Considérations relatives à la symétrie du lecteur](drive-symmetry-considerations.md)
- Pilote NVMe est intégrés de Microsoft ou NVMe pilote mis à jour.
- Recommandé: Nombre de disques de capacité est un multiple du nombre de lecteurs de cache entier
- Recommandé: Les lecteurs de Cache doivent avoir une endurance élevée en écriture: au moins 3 drive-writes-per-day (DWPD) ou au moins 4 téraoctets écrit (TBW) par jour – voir [Understanding drive écrit par jour (DWPD), téraoctets écrits (TBW) et la valeur minimale recommandée pour le stockage Espaces Direct](https://blogs.technet.microsoft.com/filecab/2017/08/11/understanding-dwpd-tbw/)

Voici comment les lecteurs peuvent être connectés pour les espaces de stockage Direct:

1. Attachement direct des disques SATA
2. Attachement direct des disques NVMe
3. Adaptateur de bus hôte (HBA) SAS avec les disques SAS
4. Adaptateur de bus hôte (HBA) SAS avec des disques SATA
5. **Non pris en charge:** RAID cartes contrôleur ou SAN (Fibre Channel, iSCSI, FCoE) stockage. Cartes de bus hôte (HBA) doivent implémenter le mode Pass-Through simple.

![diagramme du lecteur pris en charge interconnexions](media/hardware-requirements/drive-interconnect-support-1.png)

Les lecteurs peuvent être internes au serveur, ou d’un boîtier externe qui est connecté au qu’un seul serveur. SCSI Enclosure Services (SES) est requis pour l’identification et mappage d’emplacement. Chaque boîtier externe doit présenter un identificateur unique (ID Unique).

1. Lecteurs internes au serveur
2. Les disques d’un boîtier externe («JBOD») connecté à un serveur
3. **Non pris en charge:** Des boîtiers SAS partagés connecté à plusieurs serveurs ou de toute forme de plusieurs chemins d’e/s (MPIO) dans lequel les lecteurs sont accessibles par plusieurs chemins d’accès.

![diagramme du lecteur pris en charge interconnexions](media/hardware-requirements/drive-interconnect-support-2.png)

### Nombre minimal de disques (exclut lecteur de démarrage)

- Si des disques sont utilisés en cache, il doit y en avoir au moins deux par serveur.
- Au moins quatre disques de capacité (non utilisés en cache) doivent être utilisés par serveur

| Types de disques présents   | Nombre minimal requis |
|-----------------------|-------------------------|
| Tous NVMe (même modèle) | 4NVMe                  |
| Tous SSD (même modèle)  | 4SSD                   |
| NVMe + SSD            | 2NVMe + 4SSD          |
| NVMe + HDD            | 2NVMe + 4HDD          |
| SSD + HDD             | 2SSD + 4HDD           |
| NVMe + SSD + HDD      | 2NVMe + 4autres       |

   >[!NOTE]
   > Le tableau suivant fournit la valeur minimale pour les déploiements de matériel. Si vous effectuez le déploiement avec les machines virtuelles et le stockage virtualisé, comme dans Microsoft Azure, reportez-vous [à l’aide d’espaces de stockage Direct dans des clusters de machine virtuelle invité](storage-spaces-direct-in-vm.md).

### Capacité maximale

- Recommandé: La capacité de stockage brute Maximum 100 téraoctets (To) par serveur
- 1 pétaoctet (1 000 To) capacité brute maximale du pool de stockage
