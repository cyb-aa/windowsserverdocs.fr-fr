---
title: Machines virtuelles SUSE prises en charge sur Hyper-V
description: Répertorie les fonctionnalités incluses dans chaque version et les services d’intégration Linux
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7ec0e14c-4498-4bd9-8fe6-b94260198efc
author: shirgall
ms.author: kathydav
ms.date: 10/03/2016
ms.openlocfilehash: 3506c00651951aa2a62637cae6cc4989f9edf1fc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819000"
---
# <a name="supported-suse-virtual-machines-on-hyper-v"></a>Machines virtuelles SUSE prises en charge sur Hyper-V

>S'applique à : Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7.1, Windows 7

Voici une carte de distribution de fonctionnalité qui indique les fonctionnalités de chaque version. Les problèmes connus et les solutions de contournement pour chaque distribution sont répertoriées après le tableau.

Les pilotes de SUSE Linux Enterprise Service intégrés pour Hyper-V sont certifiées par SUSE. Un exemple de configuration peut être affichée dans ce bulletin : [Bulletin de Certification SUSE Oui](https://www.suse.com/nbswebapp/yesBulletin.jsp?bulletinNumber=144176).

## <a name="table-legend"></a>Légende du tableau

* **Intégrées** -LIS sont inclus dans le cadre de cette distribution Linux. Le package de téléchargement LIS fournie par Microsoft ne fonctionne pas pour cette distribution, afin de ne pas l’installer. Les numéros de version de module de noyau pour le LIS intégré (comme indiqué par **lsmod**, par exemple) sont différents du numéro de version sur le package de téléchargement LIS fournie par Microsoft. Une incompatibilité n’indique pas qu’intégrée dans les LIS est obsolète.

* &#10004;-Fonctionnalité disponible

* (*vide*)-fonctionnalité non disponible

Sous SLES12 + est 64 bits uniquement.

|**Fonctionnalité**|**Version du système d’exploitation Windows Server**|**SLES 15**|**SLES 12 SP3/SP4**|**SLES 12 SP2**|**SLES 12 SP1**|**SLES 11 SP4**|**SLES 11 SP3**|
|-|-|-|-|-|-|-|-|
|**Disponibilité**||Intégrée|Intégrée|Intégrée|Intégrée|Intégrée|Intégrée|
|**[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_core)**|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Heure précise de Windows Server 2016|2019, 2016|&#10004;|&#10004;|&#10004;||||
|**[Mise en réseau](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Networking)**||||||||
|Trames Jumbo|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Un marquage VLAN et trunking|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Migration en direct|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Injection de l’adresse IP statique|2019, 2016, 2012 R2, 2012|&#10004;Note 1|&#10004;Note 1|&#10004;Note 1|&#10004;Note 1|&#10004;Note 1|&#10004;Note 1|
|vRSS|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|||
|Segmentation de TCP et les déchargements de somme de contrôle|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|SR-IOV|2019, 2016|&#10004;|&#10004;|&#10004;||||
|**[Stockage](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Storage)**||||||||
|Redimensionnement VHDX|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Fibre Channel virtuel|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Sauvegarde de machine virtuelle active|2019, 2016, 2012 R2|&#10004; Note 2, 3, 8|&#10004; Note 2, 3, 8|&#10004; Note 2, 3, 8|&#10004; Note 2, 3, 8|&#10004; Note 2, 3, 8|&#10004; Note 2, 3, 8|
|Prise en charge de TRIM|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|SCSI WWN|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;||||
|**[Mémoire](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Memory)**||||||||
|Prise en charge PAE du noyau|2019, 2016, 2012 R2, 2012, 2008 R2|N/A|N/A|N/A|N/A|&#10004;|&#10004;|
|Configuration de l’écart MMIO|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Mémoire dynamique - ajout à chaud|2019, 2016, 2012 R2, 2012|&#10004; Note 5, 6|&#10004; Note 5, 6|&#10004; Note 5, 6|&#10004; Note 5, 6|&#10004; Note 4, 5, 6|&#10004; Note 4, 5, 6|
|Mémoire dynamique - augmentation de la capacité|2019, 2016, 2012 R2, 2012|&#10004; Note 5, 6|&#10004; Note 5, 6|&#10004; Note 5, 6|&#10004; Note 5, 6|&#10004; Note 4, 5, 6|&#10004; Note 4, 5, 6|
|Redimensionnement de la mémoire du runtime|2019, 2016|&#10004; Note 5, 6|&#10004; Note 5, 6|&#10004; Note 5, 6||||
|**[Video](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Video)**||||||||
|Périphérique vidéo spécifique à Hyper-V|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|**[Divers](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Misc)**||||||||
|Paire clé/valeur|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004; Note 7|&#10004; Note 7|
|Interruption non masquable|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Copie des fichiers d’hôte à invité|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|commande de lsvmbus|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;||||
|Sockets Hyper-V|2019, 2016|&#10004;|&#10004;|||||
|La norme PCI Passthrough/DDA|2019, 2016|&#10004;|&#10004;|&#10004;|&#10004;|||
|**[Ordinateurs virtuels de génération 2](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_gen2)**||||||||
|Démarrage à l’aide d’UEFI|2019, 2016, 2012 R2|&#10004; Note 9|&#10004; Note 9|&#10004; Note 9|&#10004; Note 9|&#10004; Note 9||
|Démarrage sécurisé|2019, 2016|&#10004;|&#10004;|&#10004;|&#10004;|||

## <a name="BKMK_notes"></a>Notes de publication

1. Injection d’adresse IP statique peut ne pas fonctionne si **Gestionnaire de réseau** a été configuré pour une carte de réseau spécifiques à Hyper-V donnée sur la machine virtuelle. Pour garantir le bon fonctionnement de l’adresse IP statique injection Veuillez vous assurer que le Gestionnaire de réseau est complètement arrêtée ou a été désactivé pour une carte réseau spécifique via son **ifcfg-ethX** fichier.

2. S’il existe des descripteurs de fichiers ouverts pendant une opération de sauvegarde de machine virtuelle active, puis, dans certains cas extrêmes, les disques durs virtuels sauvegardés peut-être subir une fichier système de cohérence (fsck) lors de la restauration.

3. Les opérations de sauvegarde en direct peuvent échouer en mode silencieux si l’ordinateur virtuel possède un périphérique iSCSI attaché ou le stockage en attachement direct (également appelé un disque direct).

4. Opérations de mémoire dynamique peuvent échouer si le système d’exploitation invité s’exécute trop faible de la mémoire. Voici quelques-unes des meilleures pratiques :

   * Mémoire de démarrage et la quantité minimale de mémoire doivent être égale ou supérieure à la quantité de mémoire qui recommande le fournisseur de distribution.

   * Les applications qui ont tendance à consommer la totalité de la mémoire disponible sur un système sont limitées à consommer jusqu'à 80 % de mémoire vive disponible.

5. Prise en charge de la mémoire dynamique est uniquement disponible sur les ordinateurs virtuels 64 bits.

6. Si vous utilisez la mémoire dynamique sur les systèmes d’exploitation Windows Server 2016 ou Windows Server 2012, spécifier **mémoire de démarrage**, **mémoire minimale**, et **mémoire maximale** paramètres par multiples de 128 mégaoctets (Mo). Cela peut entraîner des échecs ajout à chaud, et vous ne voyiez pas de mémoire augmenter dans un système d’exploitation.

7. Dans Windows Server 2016 ou Windows Server 2012 R2, l’infrastructure de la paire clé/valeur ne pas fonctionne correctement sans une mise à jour de logiciels Linux. Contactez votre fournisseur de distribution pour obtenir la mise à jour logicielle dans le cas où vous voyez des problèmes liés à cette fonctionnalité.

8. Sauvegarde VSS échouera si une seule partition est chargée plusieurs fois.

9. Sur Windows Server 2012 R2, les machines virtuelles ont le démarrage sécurisé est activé par défaut et les machines virtuelles de génération 2 Linux de génération 2 ne démarre pas, sauf si l’option de démarrage sécurisé est désactivée. Vous pouvez désactiver le démarrage sécurisé dans la section **Microprogramme** des paramètres de l'ordinateur virtuel dans le Gestionnaire Hyper-V ou à l'aide de Powershell :

   ```Powershell
   Set-VMFirmware -VMName "VMname" -EnableSecureBoot Off

   ```

## <a name="see-also"></a>Voir aussi

* [Set-VMFirmware](https://technet.microsoft.com/library/dn464287.aspx)

* [Prise en charge de CentOS et les machines virtuelles de Red Hat Enterprise Linux sur Hyper-V](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Prise en charge des machines virtuelles Debian sur Hyper-V](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [Machines virtuelles Oracle Linux prises en charge sur Hyper-V](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [Machines virtuelles Ubuntu prises en charge sur Hyper-V](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [Machines virtuelles FreeBSD prises en charge sur Hyper-V](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Descriptions des fonctionnalités pour les machines virtuelles Linux et FreeBSD sur Hyper-V](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Meilleures pratiques pour l’exécution de Linux sur Hyper-V](Best-Practices-for-running-Linux-on-Hyper-V.md)
