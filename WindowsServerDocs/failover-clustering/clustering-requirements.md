---
title: Configuration matérielle le clustering avec basculement et les options de stockage
description: Configuration matérielle requise et les options de stockage pour la création d’un cluster de basculement.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 04/26/2018
ms.localizationpriority: medium
ms.openlocfilehash: 4706372b06d0554196b692c3ddcda145dee5bae5
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/22/2018
ms.locfileid: "2082099"
---
# <a name="failover-clustering-hardware-requirements-and-storage-options"></a>Configuration matérielle le clustering avec basculement et les options de stockage

S’applique à: Windows Server 2012 R2, Windows Server 2012, Windows Server 2016

Vous devez le matériel suivant pour créer un cluster de basculement. Pour être pris en charge par Microsoft, tout le matériel doit être certifié pour la version de Windows Server que vous exécutez, et la solution de cluster de basculement complète doit passer tous les tests de la rubrique valider un Assistant de Configuration. Pour plus d’informations sur la validation d’un cluster de basculement, voir [Valider le matériel pour un Cluster de basculement](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134244(v%3dws.11)>).

- **Serveurs**: nous vous conseillons d’utiliser un ensemble d’ordinateurs contenant les composants similaires ou identiques de correspondance.
- **Cartes réseau et câble (pour la communication réseau)**: Si vous utilisez iSCSI, chaque carte réseau doit être dédiée à la communication réseau ou iSCSI, pas les deux.

    Dans l’infrastructure réseau qui connecte les nœuds du cluster, évitez de points de défaillance uniques. Par exemple, vous pouvez vous connecter vos nœuds de cluster par plusieurs réseaux distincts. Autrement, vous pouvez connecter les nœuds du cluster avec un réseau qui est construit avec des cartes réseau associées, des commutateurs redondants, des routeurs redondants ou matériel similaire qui supprime les points de défaillance uniques.

    >[!NOTE]
    >Si vous vous connectez les nœuds du cluster avec un seul réseau, le réseau passera l’exigence de redondance de la rubrique valider un Assistant de Configuration. Toutefois, le rapport à partir de l’Assistant inclura un avertissement indiquant que le réseau ne doit pas comporter de points de défaillance uniques.

- **Contrôleurs de périphérique ou cartes appropriées pour le stockage**:

  - **Serial Attached SCSI ou Fibre Channel**: Si vous utilisez Serial Attached SCSI ou Fibre Channel, dans les serveurs en cluster, tous les éléments de la pile de stockage doivent être identiques. Il est nécessaire que le logiciel d’e/s (MPIO) plusieurs chemins soient identiques et que le logiciel du Module DSM (Device Specific) soit identique. Il est recommandé que les contrôleurs de périphérique de stockage de masse — l’hôte du bus adaptateur (HBA), les pilotes HBA et microprogrammes HBA — qui sont attachées à stockage de cluster soit identique. Si vous utilisez HBA différents, vous devez vérifier avec le fournisseur de stockage de suivre leurs configurations prises en charge ou recommandées.
  - **iSCSI**: Si vous utilisez iSCSI, chaque serveur en cluster doit avoir un ou plusieurs cartes réseau ou HBA qui est dédiés au stockage de cluster. Le réseau que vous utilisez pour iSCSI ne doit pas être utilisé pour la communication réseau. Dans les serveurs en cluster, les cartes réseau que vous utilisez pour vous connecter à la cible de stockage iSCSI doivent être identiques, et nous vous conseillons d’utiliser Ethernet Gigabit ou supérieur.
- **Stockage**: vous devez utiliser [Les espaces de stockage Direct](../storage/storage-spaces/storage-spaces-direct-overview.md) ou partagé de stockage qui est compatible avec Windows Server 2012 R2 ou Windows Server 2012. Vous pouvez utiliser un stockage partagé est attaché, et vous pouvez également utiliser des partages de fichiers SMB 3.0 en tant que stockage partagé pour les serveurs qui exécutent Hyper-V qui sont configurés dans un cluster de basculement. Pour plus d’informations, voir [Déploiement d’Hyper-V sur SMB](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>).

    Dans la plupart des cas, stockage doit contenir plusieurs, en séparant les disques (numéros d’unité logique ou LUN, Logical Unit) qui sont configurés au niveau du matériel. Pour certains clusters, un seul disque fonctionne en tant que le témoin de disque (décrit à la fin de cette section). Autres disques contiennent les fichiers requis pour les rôles en cluster (anciennement appelés services en cluster ou applications). Exigences de stockage sont les suivantes:

  - Pour utiliser la prise en charge des disques natifs incluse dans le clustering avec basculement, utilisez des disques de base, non dynamiques.
  - Nous vous recommandons de formater les partitions NTFS. Si vous utilisez Cluster partagés Volumes (CSV), la partition de chacune d’elles doit être NTFS.

    >[!NOTE]
    >Si vous avez un témoin de disque pour votre configuration de quorum, vous pouvez mettre en forme le disque avec NTFS ou le système de fichiers résilient (ReFS).

  - Pour le style de partition du disque, vous pouvez utiliser l’enregistrement de démarrage principal (MBR) ou table de partition GUID (GPT).

    Un témoin de disque est un disque de stockage du cluster qui a désigné pour stocker une copie de la base de données de configuration de cluster. Un cluster de basculement possède un témoin de disque uniquement si cela est spécifié dans le cadre de la configuration de quorum. Pour plus d’informations, voir [Understanding Quorum dans les espaces de stockage Direct](../storage/storage-spaces/understand-quorum.md).

## <a name="hardware-requirements-for-hyper-v"></a>Configuration matérielle requise pour Hyper-V

Si vous créez un cluster de basculement qui inclut des ordinateurs virtuels en cluster, les serveurs du cluster doivent prendre en charge la configuration matérielle requise pour le rôle Hyper-V. Hyper-V nécessite un processeur 64 bits qui inclut les éléments suivants:

- Assistance matérielle à la virtualisation Cette option est disponible dans les processeurs qui comprennent une option de virtualisation, notamment les processeurs dotés de la technologie de virtualisation Intel (VT Intel) ou la technologie de virtualisation AMD-V (AMD).
- Matérielle données (prévention de l’exécution) doit être disponible et activée. Plus précisément, vous devez activer Intel XD bit (bit de verrouillage) ou bit AMD NX (aucun bit execute).

Pour plus d’informations sur le rôle Hyper-V, voir [Vue d’ensemble d’Hyper-V](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831531(v%3dws.11)>).

## <a name="deploying-storage-area-networks-with-failover-clusters"></a>Déploiement de réseaux SAN avec des clusters de basculement

Lors du déploiement d’un réseau de stockage (SAN) avec un cluster de basculement, procédez comme suit:

- **Vérifiez la compatibilité du stockage**: confirmer avec les fabricants et fournisseurs que le stockage, y compris les pilotes, le microprogramme et logiciel utilisé pour le stockage, sont compatibles avec les clusters de basculement dans la version de Windows Server que vous exécutez.
- **Isoler les périphériques de stockage, un seul cluster par périphérique**: serveurs de clusters différents ne doivent pas être en mesure d’accéder aux même périphériques de stockage. Dans la plupart des cas, un LUN à utiliser pour un groupe de serveurs du cluster doit être isolé de tous les autres serveurs par le biais de masquage des LUN ou zonage.
- **Envisagez l’utilisation du logiciel MPIO e/s ou associées des cartes réseau**: dans une structure de stockage hautement disponible, vous pouvez déployer des clusters de basculement avec plusieurs adaptateurs de bus hôte à l’aide de plusieurs chemins logiciel d’e/s ou réseau agrégation de cartes (également appelé charge l’équilibrage et le basculement ou LBFO). Cela offre le niveau le plus élevé de redondance et de disponibilité. Pour Windows Server 2012 R2 ou Windows Server 2012, votre solution de plusieurs chemins doit être basée sur Microsoft MPIO e/s (o). Votre fournisseur de matériel fournira généralement un module spécifique au périphérique MPIO (DSM) votre matériel, bien que Windows Server inclut un ou plusieurs DSM dans le cadre du système d’exploitation.

    Pour plus d’informations sur LBFO, voir [Vue d’ensemble de la collaboration de carte réseau](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming) dans la bibliothèque technique de Windows Server.

    >[!IMPORTANT]
    >Adaptateurs de bus hôte et le logiciel d’e/s à chemins multiples peuvent être très sensibles à la version. Si vous implémentez une solution à chemins multiples pour votre cluster, travail en étroite collaboration avec votre fournisseur de matériel pour choisir les cartes correctes, microprogramme et logiciel pour la version de Windows Server que vous exécutez.

- **Envisagez l’utilisation des espaces de stockage**: Si vous prévoyez de déployer série stockage SAS (SCSI) en cluster qui est configurée à l’aide des espaces de stockage, voir [Déployer des espaces de stockage en cluster](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11)>) pour la configuration requise.

## <a name="more-information"></a>Informations supplémentaires

- [Clustering de basculement](failover-clustering.md)
- [Espaces de stockage](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831739(v%3dws.11)>)
- [Utilisation du clustering invité pour une haute disponibilité](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn440540(v%3dws.11)>)