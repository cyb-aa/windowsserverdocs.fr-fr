---
title: Machines virtuelles FreeBSD prises en charge sur Hyper-V
description: Répertorie les fonctionnalités incluses dans chaque version et les services d’intégration Linux
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 930e758f-bd50-46b4-a3a4-9857110f17b4
author: shirgall
ms.author: kathydav
ms.date: 08/30/2017
ms.openlocfilehash: 013328953321bc66b3fd30759e5be321eea32dde
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824230"
---
# <a name="supported-freebsd-virtual-machines-on-hyper-v"></a>Machines virtuelles FreeBSD prises en charge sur Hyper-V

>S'applique à : Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7.1, Windows 7

Le plan de distribution de fonctionnalité suivant indique les fonctionnalités de chaque version. Les problèmes connus et les solutions de contournement pour chaque distribution sont répertoriées après le tableau.

## <a name="table-legend"></a>Légende du tableau

* **Intégrées** -BIS (Service d’intégration de FreeBSD) sont inclus dans le cadre de cette version de FreeBSD.

* &#10004;-Fonctionnalité disponible

* (*vide*)-fonctionnalité non disponible

|**Fonctionnalité**|**Version du système d’exploitation Windows Server**|**11.1/11.2**|**11.0**|**10.3**|**10.2**|**10.0 - 10.1**|**9.1 - 9.3, 8.4**|
|-|-|-|-|-|-|-|-|
|**Disponibilité**||Intégré|Intégré|Intégré|Intégré|Intégré|[Ports](https://svnweb.freebsd.org/ports/branches/2015Q1/emulators/hyperv-is/) |
|**[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_core)**|2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004; |
|Heure précise de Windows Server 2016|2016|&#10004;||||||
|**[Mise en réseau](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Networking)**||||||||
|Trames Jumbo|2016, 2012 R2, 2012, 2008 R2|&#10004; Note 3|&#10004; Note 3|&#10004; Note 3|&#10004; Note 3|&#10004; Note 3|&#10004; Note 3|
|Un marquage VLAN et trunking|2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Migration en direct|2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Injection de l’adresse IP statique|2016, 2012 R2, 2012|&#10004; Note 4|&#10004; Note 4|&#10004; Note 4|&#10004; Note 4|&#10004; Note 4|&#10004;|
|vRSS|2016, 2012 R2|&#10004;|&#10004;|||||
|Segmentation de TCP et les déchargements de somme de contrôle|2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|||
|Grande (LRO) le déchargement de réception|2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;||||
|SR-IOV|2016|||||||
|**[Stockage](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Storage)**||Remarque 1|Remarque 1|Remarque 1|Remarque 1|Remarque 1, 2|Remarque 1, 2|
|Redimensionnement VHDX|2016, 2012 R2|&#10004; Note 7|&#10004; Note 7|||||
|Fibre Channel virtuel|2016, 2012 R2|||||||
|Sauvegarde de machine virtuelle active|2016, 2012 R2|&#10004;||||||
|Prise en charge de TRIM|2016, 2012 R2|&#10004;||||||
|SCSI WWN|2016, 2012 R2|||||||
|**[Mémoire](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Memory)**||||||||
|Prise en charge PAE du noyau|2016, 2012 R2, 2012, 2008 R2|||||||
|Configuration de l’écart MMIO|2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Mémoire dynamique - ajout à chaud|2016, 2012 R2, 2012|||||||
|Mémoire dynamique - augmentation de la capacité|2016, 2012 R2, 2012|||||||
|Redimensionnement de la mémoire du runtime|2016|||||||
|**[Video](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Video)**||||||||
|Périphérique vidéo spécifique Hyper-V|2016, 2012 R2, 2012, 2008 R2|||||||
|**[Divers](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Misc)**||||||||
|Paire clé/valeur|2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004; Note 6|&#10004; Note 5, 6|&#10004; Note 6|
|Interruption non masquable|2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Copie des fichiers d’hôte à invité|2016, 2012 R2|||||||
|commande de lsvmbus|2016, 2012 R2, 2012, 2008 R2|||||||
|Sockets Hyper-V|2016|||||||
|La norme PCI Passthrough/DDA|2016|&#10004;||||||
|**[Ordinateurs virtuels de génération 2](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_gen2)**||||||||
|Démarrage à l’aide d’UEFI|2016, 2012 R2|&#10004;||||||
|Démarrage sécurisé|2016|||||||

## <a name="BKMK_notes"></a>Notes de publication

1. Suggère à [périphériques de disque étiquette]( https://www.freebsd.org/doc/handbook/geom-glabel.html) afin d’éviter l’erreur de montage racine lors du démarrage.

2. Le lecteur de DVD virtuel ne peut pas être reconnu lorsque les pilotes BIS sont chargés sur FreeBSD 8.x et 9.x, sauf si vous activez le pilote ATA hérité via la commande suivante.
    ```sh
    # echo ‘hw.ata.disk_enable=1’ >> /boot/loader.conf
    # shutdown -r now
    ```

3. 9126 est que la valeur maximale prise en charge la taille MTU.

4. Impossible de définir une adresse IPv6 statique dans le serveur de réplication dans un scénario de basculement. Utilisez une adresse IPv4 à la place.

5. Paire clé/valeur est fournie par les ports sur FreeBSD 10.0. Consultez le [FreeBSD 10.0 ports](https://svnweb.freebsd.org/ports/branches/2015Q1/emulators/hyperv-is/) sur FreeBSD.org pour plus d’informations.

6. Paire clé/valeur peut ne pas fonctionne sur Windows Server 2008 R2.

7. Pour rendre le travail de redimensionnement en ligne VHDX correctement dans FreeBSD 11.0, une étape manuelle spéciale est nécessaire pour contourner un bogue de géométrie qui est résolu dans 11.0 +, une fois que l’hôte est redimensionné le disque VHDX : ouvrez le disque pour l’écriture et exécutez « recover gpart » comme suit.
    ```sh
    # dd if=/dev/da1 of=/dev/da1 count=0
    # gpart recover da1
    ```
**Remarques supplémentaires**: La matrice des fonctionnalités de stable 10 et 11 stable est le même avec la version de FreeBSD 11.1. En outre, FreeBSD 10.2 et les versions précédentes (10.1, 10.0, 9.x, 8.x) sont la fin de vie. Reportez-vous [ici](https://security.freebsd.org/) pour obtenir une liste des versions prises en charge et les avis de sécurité la plus récente.

**Remarques supplémentaires**: La matrice des fonctionnalités de stable 10 et 11 stable est le même avec la version de FreeBSD 11.1. En outre, FreeBSD 10.2 et les versions précédentes (10.1, 10.0, 9.x, 8.x) sont la fin de vie. Reportez-vous [ici](https://security.freebsd.org/) pour obtenir une liste des versions prises en charge et les avis de sécurité la plus récente.

## <a name="see-also"></a>Voir aussi

* [Descriptions des fonctionnalités pour les machines virtuelles Linux et FreeBSD sur Hyper-V](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)
* [Meilleures pratiques pour l’exécution de FreeBSD sur Hyper-V](Best-practices-for-running-FreeBSD-on-Hyper-V.md)
