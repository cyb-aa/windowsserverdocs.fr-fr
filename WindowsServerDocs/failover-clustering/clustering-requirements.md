---
title: Configuration matérielle requise clustering avec basculement et options de stockage
description: Configuration matérielle requise et les options de stockage pour la création d’un cluster de basculement.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 04/26/2018
ms.localizationpriority: medium
ms.openlocfilehash: e6eb6f2acd420ae657a5c1b698e9733751378552
ms.sourcegitcommit: ed27ddbe316d543b7865bc10590b238290a2a1ad
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65476083"
---
# <a name="failover-clustering-hardware-requirements-and-storage-options"></a>Configuration matérielle requise clustering avec basculement et options de stockage

S’applique à : Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vous avez besoin du matériel suivant pour créer un cluster de basculement. Pour être pris en charge par Microsoft, tout le matériel doit être certifié pour la version de Windows Server que vous exécutez, et la solution de cluster de basculement complète doit réussir tous les tests de l'Assistant Validation d'une configuration. Pour plus d'informations sur la validation d'un cluster de basculement, voir [Valider le matériel pour un cluster de basculement](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134244(v%3dws.11)>).

- **Serveurs** : il est recommandé d'utiliser un ensemble d'ordinateurs compatibles et qui contiennent des composants similaires ou identiques.
- **Câble et cartes réseau (pour la communication réseau)**  : si vous utilisez la technologie iSCSI, chaque carte réseau doit être dédiée à la communication réseau ou à iSCSI, mais pas les deux à la fois.

    Dans l’infrastructure réseau qui connecte vos nœuds de cluster, évitez d’avoir des points de défaillance uniques. Par exemple, vous pouvez connecter vos nœuds de cluster à l’aide de plusieurs réseaux distincts. Vous pouvez également connecter vos nœuds de cluster avec un réseau qui est construit avec les cartes réseau, commutateurs redondants, des routeurs redondants ou tout autre matériel similaire qui supprime les points de défaillance uniques.

    >[!NOTE]
    >Si vous connectez des nœuds de cluster à un réseau unique, le réseau transmet les exigences de redondance à l’Assistant Validation d’une configuration. Toutefois, le rapport de l’Assistant inclut un avertissement indiquant que le réseau ne doit pas comporter de points de défaillance uniques.

- **Contrôleurs de périphériques ou adaptateurs appropriés pour le stockage** :

  - **SAS (Serial Attached SCSI) ou Fibre Channel** : si vous utilisez SAS (Serial Attached SCSI) ou Fibre Channel, dans l'ensemble des serveurs en cluster, tous les éléments de la pile de stockage doivent être identiques. Il est nécessaire que le logiciel multichemin d’e/s (MPIO) soit identique et que le logiciel du Module spécifique au périphérique (DSM) soient identiques. Il est recommandé que les contrôleurs de périphérique de stockage de masse — le de bus hôte (HBA) d’adaptateur, les pilotes HBA et le microprogramme HBA, qui sont joints à stockage de cluster être identiques. Si vous utilisez des adaptateurs de bus hôte non similaires, vous devez vérifier auprès du fournisseur de stockage que vous respectez les configurations prises en charge ou recommandées.
  - **iSCSI** : si vous utilisez la norme iSCSI, chaque serveur en cluster doit avoir une ou plusieurs cartes réseau, ou un ou plusieurs adaptateurs de bus hôte dédiés à l'espace de stockage en cluster. Le réseau que vous utilisez pour iSCSI ne doit pas être utilisé pour la communication réseau. Dans tous les serveurs en cluster, les cartes réseau que vous utilisez pour la connexion à la cible de stockage iSCSI doivent être identiques ; en outre, il est recommandé d’utiliser des cartes Gigabit Ethernet ou une connexion plus rapide.
- **Stockage** : Vous devez utiliser [espaces de stockage Direct](../storage/storage-spaces/storage-spaces-direct-overview.md) ou partagé de stockage qui est compatible avec Windows Server 2012 R2 ou Windows Server 2012. Vous pouvez utiliser un stockage partagé qui est attaché, et vous pouvez également utiliser des partages de fichiers SMB 3.0 comme stockage partagé pour les serveurs qui exécutent Hyper-V qui sont configurés dans un cluster de basculement. Pour plus d’informations, voir [Déployer Hyper-V sur SMB](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>).

    Dans la plupart des cas, le stockage attaché doit contenir plusieurs disques distincts (numéros d'unité logique) configurés au niveau du matériel. Pour certains clusters, l’un des disques sert de témoin de disque (description à la fin de cette sous-section). D’autres disques contiennent les fichiers requis pour les rôles en cluster (anciennement appelés « services ou applications en cluster »). Les conditions requises pour le stockage incluent les éléments suivants :

  - Pour utiliser la prise en charge de disque native incluse dans le clustering de basculement, utilisez des disques de base au lieu de disques dynamiques.
  - Il est recommandé de formater les partitions en NTFS. Si vous utilisez des volumes partagés de cluster, la partition de chacun d'eux doit être au format NTFS.

    >[!NOTE]
    >Si vous disposez d'un témoin de disque pour votre configuration de quorum, vous pouvez formater le disque en NTFS ou ReFS (Resilient File System).

  - Pour le style de partition du disque, vous pouvez choisir entre une partition MBR (Master Boot Record) ou une partition GPT (table de partition GUID).

    Un témoin de disque est un disque de stockage en cluster qui est désigné pour conserver une copie de la base de données de configuration de cluster. Un cluster de basculement possède un témoin de disque uniquement si cela est spécifié dans la configuration de quorum. Pour plus d’informations, consultez [Quorum de présentation dans les espaces de stockage Direct](../storage/storage-spaces/understand-quorum.md).

## <a name="hardware-requirements-for-hyper-v"></a>Configuration matérielle requise pour Hyper-V

Si vous créez un cluster de basculement qui comporte des ordinateurs virtuels en cluster, les serveurs de cluster doivent prendre en charge la configuration matérielle requise pour le rôle Hyper-V. Hyper-V nécessite un processeur 64 bits qui comprend les éléments suivants :

- Virtualisation à assistance matérielle. Elle est disponible dans les processeurs dotés d’une option de virtualisation, et plus précisément les processeurs dotés de la technologie Intel VT (Intel Virtualization Technology) ou AMD-V (AMD Virtualization).
- La prévention de l’exécution des données par le matériel doit être disponible et activée. Plus spécifiquement, vous devez activer le bit Intel XD (bit de désactivation d’exécution) ou le bit AMD NX (bit de non-exécution).

Pour plus d’informations sur le rôle Hyper-V, consultez [vue d’ensemble d’Hyper-V](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831531(v%3dws.11)>).

## <a name="deploying-storage-area-networks-with-failover-clusters"></a>Déploiement de réseaux SAN avec des clusters de basculement

Lorsque vous déployez un réseau de zone de stockage (SAN) avec un cluster de basculement, suivez ces instructions :

- **Vérifiez la compatibilité du stockage** : vérifiez auprès des fabricants et des fournisseurs que le stockage, y compris les pilotes, les microprogrammes et les logiciels utilisés pour le stockage, sont compatibles avec les clusters de basculement dans la version de Windows Server que vous exécutez.
- **Isolez les dispositifs de stockage, avec un cluster par périphérique** : les serveurs de clusters distincts ne doivent pas être en mesure d'accéder aux mêmes dispositifs de stockage. Dans la plupart des cas, un numéro d’unité logique utilisé pour un seul ensemble de serveurs de clusters doit être isolé de tous les autres serveurs via un masquage ou une segmentation des numéros d’unité logique.
- **Envisagez d'utiliser un logiciel MPIO (Multipath I/O) ou des cartes réseau associées** : dans une structure de stockage hautement disponible, vous pouvez déployer des clusters de basculement dotés de plusieurs adaptateurs de bus hôte à l'aide d'un logiciel MPIO (Multipath I/O) ou d'une association de cartes réseau (aussi appelé « équilibrage de charge et basculement » ou « LBFO »). Cela fournit le niveau le plus élevé de redondance et de disponibilité. Pour Windows Server 2012 R2 ou Windows Server 2012, votre solution multivoie doit être basée sur Microsoft Multipath i/o d’e/s (MPIO). Votre fournisseur de matériel proposera généralement un module propre au périphérique (DSM, Device-Specific Module) MPIO pour votre matériel, même si le système d'exploitation Windows Server intègre un ou plusieurs modules DSM.

    Pour plus d’informations sur LBFO, voir [vue d’ensemble de l’association de cartes réseau](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming) dans la bibliothèque technique Windows Server.

    >[!IMPORTANT]
    >Les adaptateurs de bus hôte et les logiciels MPIO (Multipath I/O) peuvent être très sensibles aux versions. Si vous implémentez une solution multivoie pour votre cluster, rapprochez-vous de votre fournisseur de matériel pour choisir les adaptateurs, microprogrammes et logiciels adaptés à la version de Windows Server que vous exécutez.

- **Envisagez d'utiliser des espaces de stockage** : Si vous envisagez de déployer la série (stockage SAS attached SCSI) en cluster qui est configuré à l’aide des espaces de stockage, consultez [déployer des espaces de stockage en cluster](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11)>) pour la configuration requise.

## <a name="more-information"></a>Informations supplémentaires

- [Clustering de basculement](failover-clustering.md)
- [Espaces de stockage](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831739(v%3dws.11)>)
- [À l’aide du Clustering invité pour la haute disponibilité](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn440540(v%3dws.11)>)