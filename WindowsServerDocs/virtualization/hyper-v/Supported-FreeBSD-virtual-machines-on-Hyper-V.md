---
title: Ordinateurs virtuels FreeBSD pris en charge sur Hyper-V
description: Répertorie les fonctionnalités et les services d’intégration Linux inclus dans chaque version
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
ms.openlocfilehash: a6e9c6e3bec2001c73254ffd813954f04a37a714
ms.sourcegitcommit: 6f968368c12b9dd699c197afb3a3d13c2211f85b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68544712"
---
# <a name="supported-freebsd-virtual-machines-on-hyper-v"></a>Ordinateurs virtuels FreeBSD pris en charge sur Hyper-V

>S'applique à : Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7,1, Windows 7

La carte de distribution des fonctionnalités suivante indique les fonctionnalités de chaque version. Les problèmes connus et les solutions de contournement pour chaque distribution sont répertoriés après le tableau.

## <a name="table-legend"></a>Légende de la table

* **Intégré** à la version FreeBSD (service d’intégration FreeBSD) est inclus dans le cadre de cette version de FreeBSD.

* &#10004;-Fonctionnalité disponible

* (*vide*)-fonctionnalité non disponible

|**Fonctionnalité**|**Version du système d’exploitation Windows Server**|**11.1/11.2**|**11.0**|**10,3**|**10,2**|**10,0-10,1**|**9,1-9,3, 8,4**|
|-|-|-|-|-|-|-|-|
|**Disponibilité**||Intégré|Intégré|Intégré|Intégré|Intégré|[Utilis](https://svnweb.freebsd.org/ports/branches/2015Q1/emulators/hyperv-is/) |
|**[Ebauche](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004; |
|Heure précise de Windows Server 2016|2019, 2016|&#10004;||||||
|**[Mise](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**||||||||
|Trames Jumbo|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;Remarque 3|&#10004;Remarque 3|&#10004;Remarque 3|&#10004;Remarque 3|&#10004;Remarque 3|&#10004;Remarque 3|
|Balisage et Trunking VLAN|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Migration en direct|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Injection d’adresses IP statiques|2019, 2016, 2012 R2, 2012|&#10004;Remarque 4|&#10004;Remarque 4|&#10004;Remarque 4|&#10004;Remarque 4|&#10004;Remarque 4|&#10004;|
|vRSS|2019, 2016, 2012 R2|&#10004;|&#10004;|||||
|Segmentation TCP et déchargements de somme de contrôle|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|||
|Déchargement de réception volumineux (LRO)|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;||||
|SR-IOV|2019, 2016|||||||
|**[Rangement](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**||Remarque 1|Remarque 1|Remarque 1|Remarque 1|Remarque 1, 2|Remarque 1, 2|
|Redimensionnement VHDX|2019, 2016, 2012 R2|&#10004;Remarque 7|&#10004;Remarque 7|||||
|Fibre Channel virtuel|2019, 2016, 2012 R2|||||||
|Sauvegarde de machine virtuelle en direct|2019, 2016, 2012 R2|&#10004;||||||
|SUPPRIMER la prise en charge|2019, 2016, 2012 R2|&#10004;||||||
|WWN SCSI|2019, 2016, 2012 R2|||||||
|**[Capacité](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**||||||||
|Prise en charge du noyau PAE|2019, 2016, 2012 R2, 2012, 2008 R2|||||||
|Configuration de l’intervalle MMIO|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Mémoire dynamique-ajout à chaud|2019, 2016, 2012 R2, 2012|||||||
|Mémoire dynamique-bulles|2019, 2016, 2012 R2, 2012|||||||
|Redimensionnement de la mémoire d’exécution|2019, 2016|||||||
|**[Vidéosurveillance](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**||||||||
|Périphérique vidéo spécifique à Hyper-V|2019, 2016, 2012 R2, 2012, 2008 R2|||||||
|**[Diverses](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**||||||||
|Paire clé/valeur|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;Remarque 6|&#10004;Remarque: 5, 6|&#10004;Remarque 6|
|Interruption non masquable|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Copie de fichiers de l’hôte vers l’invité|2019, 2016, 2012 R2|||||||
|commande lsvmbus|2019, 2016, 2012 R2, 2012, 2008 R2|||||||
|Sockets Hyper-V|2019, 2016|||||||
|Passthrough/DDA PCI|2019, 2016|&#10004;||||||
|**[Ordinateurs virtuels de 2e génération](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)**||||||||
|Démarrer à l’aide d’UEFI|2019, 2016, 2012 R2|&#10004;||||||
|Démarrage sécurisé|2019, 2016|||||||

## <a name="BKMK_notes"></a>Notes

1. Suggérer d' [étiqueter les périphériques de disque]( https://www.freebsd.org/doc/handbook/geom-glabel.html) pour éviter une erreur de montage racine au démarrage.

2. Le lecteur de DVD virtuel n’est peut-être pas reconnu lors du chargement des pilotes BIS sur FreeBSD 8. x et 9. x, sauf si vous activez le pilote ATA hérité à l’aide de la commande suivante.
    ```sh
    # echo ‘hw.ata.disk_enable=1’ >> /boot/loader.conf
    # shutdown -r now
    ```

3. 9126 est la taille MTU maximale prise en charge.

4. Dans un scénario de basculement, vous ne pouvez pas définir une adresse IPv6 statique sur le serveur de réplication. Utilisez plutôt une adresse IPv4.

5. KVP est fourni par les ports sur FreeBSD 10,0. Pour plus d’informations, consultez [ports FreeBSD 10,0](https://svnweb.freebsd.org/ports/branches/2015Q1/emulators/hyperv-is/) sur FreeBSD.org.

6. KVP peut ne pas fonctionner sur Windows Server 2008 R2.

7. Pour que le redimensionnement VHDX en ligne fonctionne correctement dans FreeBSD 11,0, une étape manuelle spéciale est requise pour contourner un bogue GEOM qui est corrigé dans 11.0 +, après que l’hôte a redimensionné le disque VHDX-ouvrir le disque en écriture et exécuter «gpart Recover» comme suit.
    ```sh
    # dd if=/dev/da1 of=/dev/da1 count=0
    # gpart recover da1
    ```
   **Remarques supplémentaires**: La matrice de fonctionnalités de 10 stable et 11 stable est la même avec la version 11,1 de FreeBSD. En outre, FreeBSD 10,2 et versions antérieures (10,1, 10,0, 9. x, 8. x) sont en fin de vie. Pour obtenir une liste à jour des versions prises en charge et des avis de sécurité les plus récents, consultez [cette page](https://security.freebsd.org/) .

**Remarques supplémentaires**: La matrice de fonctionnalités de 10 stable et 11 stable est la même avec la version 11,1 de FreeBSD. En outre, FreeBSD 10,2 et versions antérieures (10,1, 10,0, 9. x, 8. x) sont en fin de vie. Pour obtenir une liste à jour des versions prises en charge et des avis de sécurité les plus récents, consultez [cette page](https://security.freebsd.org/) .

## <a name="see-also"></a>Voir aussi

* [Descriptions des fonctionnalités pour les machines virtuelles Linux et FreeBSD sur Hyper-V](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)
* [Meilleures pratiques pour exécuter FreeBSD sur Hyper-V](Best-practices-for-running-FreeBSD-on-Hyper-V.md)
