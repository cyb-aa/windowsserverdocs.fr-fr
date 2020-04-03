---
title: Ordinateurs virtuels Debian pris en charge sur Hyper-V
description: Répertorie les fonctionnalités et les services d’intégration Linux inclus dans chaque version
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3cc62c10-02a3-4633-960c-23bf91a45bd5
author: shirgall
ms.author: kathydav
ms.date: 10/03/2016
ms.openlocfilehash: 7a717acf5c132d68d6ee041aeb5af5a430aa171b
ms.sourcegitcommit: 9f7cc76b8c9add44dcbbd97f77b4f881d5a2c073
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80613008"
---
# <a name="supported-debian-virtual-machines-on-hyper-v"></a>Ordinateurs virtuels Debian pris en charge sur Hyper-V

>S’applique à : Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7,1, Windows 7

La carte de distribution des fonctionnalités suivante indique les fonctionnalités qui sont présentes dans chaque version. Les problèmes connus et les solutions de contournement pour chaque distribution sont répertoriés après le tableau.

## <a name="table-legend"></a>Légende de la table

* Les lis intégrés sont inclus **dans** le cadre de cette distribution Linux. Le package de téléchargement LIS fourni par Microsoft ne fonctionne pas pour cette distribution. ne l’installez donc pas. Les numéros de version du module de noyau pour la LIS intégrée (comme indiqué par **lsmod**, par exemple) sont différents du numéro de version du package de téléchargement lis fourni par Microsoft. Une incompatibilité n’indique pas que la LIS intégrée est obsolète.

* &#10004;-Fonctionnalité disponible

* (*vide*)-fonctionnalité non disponible

| **Fonctionnalité**                                                                                                                                  | **Version du système d’exploitation Windows Server** | **10.0-10.3 (Buster)** | **9.0-9.12 (Stretch)** | **8.0-8.11 (Jessie)** | **7.0-7.11 (wheezy)** |
|----------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------|-----------------------|-----------------------|-----------------------|-----------------------|
| **Disponibilité**                                                                                                                             |                                             | Intégré              | Intégré              | Intégré              | Intégré (Remarque 6)     |
| **[Ebauche](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**                                                   | 2019, 2016, 2012 R2, 2012, 2008 R2          | &#10004;              | &#10004;              | &#10004;              | &#10004;              |
| Heure précise de Windows Server 2016                                                                                                            | 2019, 2016                                  | &#10004;Remarque 8       | &#10004;Remarque 8       |                       |                       |
| **[Mise](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**                                       |                                             |                       |                       |                       |                       |
| Trames Jumbo                                                                                                                                 | 2019, 2016, 2012 R2, 2012, 2008 R2          | &#10004;              | &#10004;              | &#10004;              | &#10004;              |
| Balisage et Trunking VLAN                                                                                                                    | 2019, 2016, 2012 R2, 2012, 2008 R2          | &#10004;              | &#10004;              | &#10004;              | &#10004;              |
| Migration dynamique                                                                                                                               | 2019, 2016, 2012 R2, 2012, 2008 R2          | &#10004;              | &#10004;              | &#10004;              | &#10004;              |
| Injection d’adresses IP statiques                                                                                                                          | 2019, 2016, 2012 R2, 2012                   |                       |                       |                       |                       |
| vRSS                                                                                                                                         | 2019, 2016, 2012 R2                         | &#10004;Remarque 8       | &#10004;Remarque 8       |                       |                       |
| Segmentation TCP et déchargements de somme de contrôle                                                                                                       | 2019, 2016, 2012 R2, 2012, 2008 R2          | &#10004;Remarque 8       | &#10004;Remarque 8       |                       |                       |
| SR-IOV                                                                                                                                       | 2019, 2016                                  | &#10004;Remarque 8       | &#10004;Remarque 8       |                       |                       |
| **[Rangement](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**                                             |                                             |                       |                       |                       |                       |
| Redimensionnement VHDX                                                                                                                                  | 2019, 2016, 2012 R2                         | &#10004;Remarque 1       | &#10004;Remarque 1       | &#10004;Remarque 1       | &#10004;Remarque 1       |
| Fibre Channel virtuel                                                                                                                        | 2019, 2016, 2012 R2                         |                       |                       |                       |                       |
| Sauvegarde de machine virtuelle en direct                                                                                                                  | 2019, 2016, 2012 R2                         | &#10004;Remarque 4, 5     | &#10004;Remarque 4, 5     | &#10004;Remarque 4, 5     | &#10004;Remarque 4       |
| SUPPRIMER la prise en charge                                                                                                                                 | 2019, 2016, 2012 R2                         | &#10004;Remarque 8       | &#10004;Remarque 8       |                       |                       |
| WWN SCSI                                                                                                                                     | 2019, 2016, 2012 R2                         | &#10004;Remarque 8       | &#10004;Remarque 8       |                       |                       |
| **[Capacité](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**                                               |                                             |                       |                       |                       |                       |
| Prise en charge du noyau PAE                                                                                                                           | 2019, 2016, 2012 R2, 2012, 2008 R2          | &#10004;              | &#10004;              | &#10004;              | &#10004;              |
| Configuration de l’intervalle MMIO                                                                                                                    | 2019, 2016, 2012 R2                         | &#10004;              | &#10004;              | &#10004;              | &#10004;              |
| Mémoire dynamique-ajout à chaud                                                                                                                     | 2019, 2016, 2012 R2, 2012                   | &#10004;Remarque 8       | &#10004;Remarque 8       |                       |                       |
| Mémoire dynamique-bulles                                                                                                                  | 2019, 2016, 2012 R2, 2012                   | &#10004;Remarque 8       | &#10004;Remarque 8       |                       |                       |
| Redimensionnement de la mémoire d’exécution                                                                                                                        | 2019, 2016                                  | &#10004;Remarque 8       | &#10004;Remarque 8       |                       |                       |
| **[Vidéosurveillance](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**                                                 |                                             |                       |                       |                       |                       |
| Périphérique vidéo spécifique à Hyper-V                                                                                                                | 2019, 2016, 2012 R2, 2012, 2008 R2          | &#10004;              | &#10004;              | &#10004;              |                       |
| **[Diverses](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**                                 |                                             |                       |                       |                       |                       |
| Paire clé-valeur                                                                                                                               | 2019, 2016, 2012 R2, 2012, 2008 R2          | &#10004;Remarque 4       | &#10004;Remarque 4       | &#10004;Remarque 4       |                       |
| Interruption non masquable                                                                                                                       | 2019, 2016, 2012 R2                         | &#10004;              | &#10004;              | &#10004;              |                       |
| Copie de fichiers de l’hôte vers l’invité                                                                                                                 | 2019, 2016, 2012 R2                         | &#10004;Remarque 4       | &#10004;Remarque 4       | &#10004;Remarque 4       |                       |
| commande lsvmbus                                                                                                                              | 2019, 2016, 2012 R2, 2012, 2008 R2          |                       |                       |                       |                       |
| Sockets Hyper-V                                                                                                                              | 2019, 2016                                  | &#10004;Remarque 8       | &#10004;Remarque 8       |                       |                       |
| Passthrough/DDA PCI                                                                                                                          | 2019, 2016                                  | &#10004;Remarque 8       | &#10004;Remarque 8       |                       |                       |
| **[Ordinateurs virtuels de 2e génération](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)** |                                             |                       |                       |                       |                       |
| Démarrer à l’aide d’UEFI                                                                                                                              | 2019, 2016, 2012 R2                         | &#10004;Remarque 7       | &#10004;Remarque 7       | &#10004;Remarque 7       |                       |
| Démarrage sécurisé                                                                                                                                  | 2019, 2016                                  | &#10004;              |                       |                       |                       |


## <a name="notes"></a><a name="BKMK_notes"></a>Notes

1. La création de systèmes de fichiers sur des disques durs virtuels supérieurs à 2 to n’est pas prise en charge.

2. Sur les disques SCSI Windows Server 2008 R2, créez 8 entrées différentes dans/dev/SD *.

3. Windows Server 2012 R2 une machine virtuelle avec 8 cœurs ou plus aura toutes les interruptions routées vers un seul processeur virtuel.

4. À compter de Debian 8,3, le package Debian installé manuellement « HyperV-daemons » contient la paire clé-valeur, fcopy et les démons VSS. Sur Debian 7. x et 8.0-8.2, le package HyperV-daemons doit provenir d’un [port Debian](https://wiki.debian.org/Backports).

5. La sauvegarde de machines virtuelles en temps réel ne fonctionne pas avec les systèmes de fichiers ext2. La disposition par défaut créée par le programme d’installation de Debian comprend les systèmes de fichiers ext2, vous devez personnaliser la disposition pour ne pas créer ce type de système de fichiers.

6. Si Debian 7. x n’est plus pris en charge et utilise un noyau plus ancien, le noyau inclus dans les [ports Debian](https://wiki.debian.org/Backports) de Debian pour Debian 7. x a amélioré les fonctionnalités Hyper-V.

7. Sur les ordinateurs virtuels Windows Server 2012 R2 génération 2, le démarrage sécurisé est activé par défaut et certaines machines virtuelles Linux ne démarrent pas, sauf si l’option de démarrage sécurisé est désactivée. Vous pouvez désactiver le démarrage sécurisé dans la section **microprogramme** des paramètres de l’ordinateur virtuel dans le **Gestionnaire Hyper-V** , ou vous pouvez le désactiver à l’aide de PowerShell :

   ```Powershell
   Set-VMFirmware -VMName "VMname" -EnableSecureBoot Off

   ```
8. Les dernières fonctionnalités de noyau en amont sont disponibles uniquement à l’aide du noyau des [ports Debian](https://wiki.debian.org/Backports)inclus.

Voir aussi

* [Ordinateurs virtuels CentOS et Red Hat Enterprise Linux pris en charge sur Hyper-V](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Ordinateurs virtuels Oracle Linux pris en charge sur Hyper-V](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [Ordinateurs virtuels SUSE pris en charge sur Hyper-V](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [Machines virtuelles Ubuntu prises en charge sur Hyper-V](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [Ordinateurs virtuels FreeBSD pris en charge sur Hyper-V](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Descriptions des fonctionnalités pour les machines virtuelles Linux et FreeBSD sur Hyper-V](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Meilleures pratiques pour l’exécution de Linux sur Hyper-V](Best-Practices-for-running-Linux-on-Hyper-V.md)
