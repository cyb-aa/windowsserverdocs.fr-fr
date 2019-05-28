---
title: Prise en charge des machines virtuelles Debian sur Hyper-V
description: Répertorie les fonctionnalités incluses dans chaque version et les services d’intégration Linux
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3cc62c10-02a3-4633-960c-23bf91a45bd5
author: shirgall
ms.author: kathydav
ms.date: 10/03/2016
ms.openlocfilehash: 129783dc980be6e471ecadb2cdbffee900e3396e
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222836"
---
# <a name="supported-debian-virtual-machines-on-hyper-v"></a>Prise en charge des machines virtuelles Debian sur Hyper-V

>S'applique à : Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7.1, Windows 7

Le plan de distribution de fonctionnalité suivant indique les fonctionnalités qui sont présentes dans chaque version. Les problèmes connus et les solutions de contournement pour chaque distribution sont répertoriées après le tableau.

## <a name="table-legend"></a>Légende du tableau

* **Intégrées** -LIS sont inclus dans le cadre de cette distribution Linux. Le package de téléchargement LIS fournie par Microsoft ne fonctionne pas pour cette distribution afin de ne pas l’installer. Les numéros de version de module de noyau pour le LIS intégré (comme indiqué par **lsmod**, par exemple) sont différents du numéro de version sur le package de téléchargement LIS fournie par Microsoft. Une incompatibilité n’indique pas qu’intégrée dans les LIS est obsolète.

* &#10004;-Fonctionnalité disponible

* (*vide*)-fonctionnalité non disponible

|**Fonctionnalité**|**Version du système d’exploitation Windows Server**|**9.0-9.6 (stretch)**|**8.0-8.11 (jessie)**|**7.0-7.11 (wheezy)**|
|-|-|-|-|-|
|**Disponibilité**||Intégré|Intégré|Intégré (Remarque 6)|
|**[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|
|Heure précise de Windows Server 2016|2019, 2016|&#10004; Note 8||
|**[Mise en réseau](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**|
|Trames Jumbo|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|
|Un marquage VLAN et trunking|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|
|Migration dynamique|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|
|Injection de l’adresse IP statique|2019, 2016, 2012 R2, 2012|||
|vRSS|2019, 2016, 2012 R2|&#10004; Note 8|||
|Segmentation de TCP et les déchargements de somme de contrôle|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004; Note 8|||
|SR-IOV|2019, 2016|&#10004; Note 8||
|**[Stockage](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**|
|Redimensionnement VHDX|2019, 2016, 2012 R2|&#10004; Note 1|&#10004; Note 1|&#10004; Note 1|
|Fibre Channel virtuel|2019, 2016, 2012 R2|||
|Sauvegarde de machine virtuelle active|2019, 2016, 2012 R2|&#10004; Note 4,5|&#10004; Note 4,5|&#10004; Note 4|
|Prise en charge de TRIM|2019, 2016, 2012 R2|&#10004; Note 8|||
|SCSI WWN|2019, 2016, 2012 R2|&#10004; Note 8||
|**[Mémoire](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**|
|Prise en charge PAE du noyau|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|
|Configuration de l’écart MMIO|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|
|Mémoire dynamique - ajout à chaud|2019, 2016, 2012 R2, 2012|&#10004; Note 8|||
|Mémoire dynamique - augmentation de la capacité|2019, 2016, 2012 R2, 2012|&#10004; Note 8|||
|Redimensionnement de la mémoire du runtime|2019, 2016|&#10004; Note 8|||
|**[Video](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**|
|Périphérique vidéo spécifique à Hyper-V|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;||
|**[Divers](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**|
|Paire clé-valeur|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004; Note 4|&#10004; Note 4||
|Interruption non masquable|2019, 2016, 2012 R2|&#10004;|&#10004;|
|Copie des fichiers d’hôte à invité|2019, 2016, 2012 R2|&#10004; Note 4|&#10004; Note 4||
|commande de lsvmbus|2019, 2016, 2012 R2, 2012, 2008 R2|||
|Sockets Hyper-V|2019, 2016|&#10004; Note 8|||
|La norme PCI Passthrough/DDA|2019, 2016|&#10004; Note 8|||
|**[Ordinateurs virtuels de génération 2](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)**|
|Démarrage à l’aide d’UEFI|2019, 2016, 2012 R2|&#10004; Note 7|&#10004; Note 7||
|Démarrage sécurisé|2019, 2016|||

## <a name="BKMK_notes"></a>Notes de publication

1. Création de systèmes de fichiers sur les disques durs virtuels supérieurs à 2 To n’est pas prise en charge.

2. Sur Windows Server 2008 R2 SCSI disques créer 8 entrées différentes dans/dev/sd *.

3. Windows Server 2012 R2 une machine virtuelle avec 8 cœurs ou plus ont toutes les interruptions acheminées vers un seul processeur virtuel.

4. À compter 8.3 Debian installés manuellement le package Debian « Hyper-v-démons » contient la paire clé-valeur, fcopy et les démons VSS. Sur Debian 7.x et 8.0-8.2 le package de démons de Hyper-v doit provenir de [backports Debian](https://wiki.debian.org/Backports).

5. Sauvegarde de machine virtuelle active ne fonctionne pas avec les systèmes de fichiers ext2. La disposition par défaut créée par le programme d’installation Debian inclut des systèmes de fichiers ext2, vous devez personnaliser la disposition pour ne pas créer ce type de système de fichiers.

6. Bien que Debian 7.x est la prise en charge et utilise un noyau plus anciens, le noyau inclus dans [backports Debian](https://wiki.debian.org/Backports) pour Debian 7.x a amélioré les fonctionnalités Hyper-V.

7. Sur Windows Server 2012 R2 génération 2 machines virtuelles ont le démarrage sécurisé est activé par défaut et des machines virtuelles de Linux ne démarrera pas, sauf si l’option de démarrage sécurisé est désactivée. Vous pouvez désactiver le démarrage sécurisé dans le **microprogramme** section des paramètres de la machine virtuelle dans **Gestionnaire Hyper-V** ou vous pouvez le désactiver à l’aide de Powershell :

   ```Powershell
   Set-VMFirmware -VMName "VMname" -EnableSecureBoot Off

   ```
8. Les dernières fonctionnalités de noyau en amont sont uniquement disponibles en utilisant le noyau inclus [backports Debian](https://wiki.debian.org/Backports).

Voir aussi

* [Prise en charge de CentOS et les machines virtuelles de Red Hat Enterprise Linux sur Hyper-V](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Machines virtuelles Oracle Linux prises en charge sur Hyper-V](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [Machines virtuelles SUSE prises en charge sur Hyper-V](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [Machines virtuelles Ubuntu prises en charge sur Hyper-V](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [Machines virtuelles FreeBSD prises en charge sur Hyper-V](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Descriptions des fonctionnalités pour les machines virtuelles Linux et FreeBSD sur Hyper-V](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Meilleures pratiques pour l’exécution de Linux sur Hyper-V](Best-Practices-for-running-Linux-on-Hyper-V.md)
