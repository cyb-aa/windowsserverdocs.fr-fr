---
title: Prise en charge de CentOS et les machines virtuelles de Red Hat Enterprise Linux sur Hyper-V
description: Répertorie les versions de Linux integration services pour la prise en charge de CentOS et Red Hat Enterprise distributions
ms.prod: windows-server-threshold
ms.service: na
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4bf8783d-dee5-4b3e-8cce-2b11b117c189
author: danihalfin
ms.author: daniha
ms.date: 12/20/2017
ms.openlocfilehash: 6c9ed85c2249a4671e52eb7d512298a75f53b309
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222672"
---
# <a name="supported-centos-and-red-hat-enterprise-linux-virtual-machines-on-hyper-v"></a>Prise en charge de CentOS et les machines virtuelles de Red Hat Enterprise Linux sur Hyper-V

>S'applique à : Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7.1, Windows 7

Les mappages de distribution de fonctionnalité suivants indiquent les fonctionnalités qui sont présentes dans les versions de Linux Integration Services intégrées et téléchargeables. Les problèmes connus et les solutions de contournement pour chaque distribution sont répertoriées après les tables.

Les pilotes de Red Hat Enterprise Linux Integration Services intégrés pour Hyper-V (disponible à partir de Red Hat Enterprise Linux 6.4) sont suffisantes pour les visiteurs de Red Hat Enterprise Linux à l’exécuter avec les périphériques synthétiques hautes performances sur les ordinateurs hôtes Hyper-V. Ces pilotes intégrés sont certifiées par Red Hat pour cette utilisation. Des configurations certifiées peuvent être affichées sur cette page web de Red Hat : [Catalogue de Certification de Red Hat](https://access.redhat.com/ecosystem/search/#/ecosystem/Red%20Hat%20Enterprise%20Linux?sort=sortTitle%20asc&vendors=Microsoft&category=Server). Il n’est pas nécessaire télécharger et installer des packages Linux Integration Services à partir du Microsoft Download Center, et cela par conséquent, peut limiter votre prise en charge de Red Hat comme décrit dans l’article de la base de connaissances Red Hat 1067 : [Base de connaissances Red Hat 1067](https://access.redhat.com/articles/1067).

En raison de conflits potentiels entre la prise en charge intégrée de LIS et la prise en charge LIS téléchargeable lorsque vous mettez à niveau le noyau, désactiver les mises à jour automatiques, désinstallez les packages téléchargeables LIS, mettez à jour le noyau, redémarrage et puis installer la dernière version de LIS mise en production, et redémarrez à nouveau.

>[!NOTE]
>Les informations de certification officielles de Red Hat Enterprise Linux soient disponibles via le [portail client Red Hat](https://access.redhat.com/ecosystem/search/#/category/Server?sort=sortTitle%20asc&query=windows%20server&ecosystem=Red%20Hat%20Enterprise%20Linux).

Dans cette section :

* [RHEL/CentOS 7.x série](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md#BKMK_7x)

* [RHEL/CentOS 6.x série](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md#BKMK_6x)

* [RHEL/CentOS 5.x série](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md#BKMK_5x)

* [Notes de publication](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md#BKMK_notes)

## <a name="table-legend"></a>Légende du tableau

* **Intégrées** -LIS sont inclus dans le cadre de cette distribution Linux. Les numéros de version de module de noyau pour le LIS intégré (comme indiqué par **lsmod**, par exemple) sont différents du numéro de version sur le package de téléchargement LIS fournie par Microsoft. Une incompatibilité n’indique pas qu’intégrée dans les LIS est obsolète.

* &#10004;-Fonctionnalité disponible

* (*vide*)-fonctionnalité non disponible

## <a name="BKMK_7x"></a>RHEL/CentOS 7.x série

Cette série a uniquement les noyaux de 64 bits.

|**Fonctionnalité**|**Version de Windows Server**|**7.5-7.6**|**7.3-7.4**|**7.0-7.2**|**7.5-7.6**|**7.4**|**7.3**|**7.2**|**7.1**|**7.0**|
|-|-|-|-|-|-|-|-|-|-|-|
|**Disponibilité**||[LIS 4.2](https://www.microsoft.com/download/details.aspx?id=55106)|[LIS 4.2](https://www.microsoft.com/download/details.aspx?id=55106)|[LIS 4.2](https://www.microsoft.com/download/details.aspx?id=55106)|Intégré|Intégré|Intégré|Intégré|Intégré|Intégré||
|**[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Heure précise de Windows Server 2016|2019, 2016|&#10004;|&#10004;|||||||
|**[Mise en réseau](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**|||||||
|Trames Jumbo|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Un marquage VLAN et trunking|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Migration dynamique|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Injection de l’adresse IP statique|2019, 2016, 2012 R2, 2012|&#10004; Note 2|&#10004; Note 2|&#10004; Note 2|&#10004; Note 2|&#10004; Note 2|&#10004; Note 2|&#10004; Note 2|&#10004; Note 2|&#10004; Note 2|
|vRSS|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|Segmentation de TCP et les déchargements de somme de contrôle|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|SR-IOV|2019, 2016|&#10004;|&#10004;||&#10004;|&#10004;|&#10004;|||
|**[Stockage](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**||||
|Redimensionnement VHDX|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Fibre Channel virtuel|2019, 2016, 2012 R2|&#10004; Note 3|&#10004; Note 3|&#10004; Note 3|&#10004; Note 3|&#10004; Note 3|&#10004; Note 3|&#10004; Note 3|&#10004; Note 3|&#10004; Note 3|
|Sauvegarde de machine virtuelle active|2019, 2016, 2012 R2|&#10004; Note 5|&#10004; Note 5|&#10004; Note 5|&#10004; Note 4,5|&#10004; Note 4, 5|&#10004; Note 4, 5|&#10004; Note 4, 5|&#10004; Note 4, 5|&#10004; Note 4, 5|
|Prise en charge de TRIM|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|||
|SCSI WWN|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|||||
|**[Mémoire](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**||||
|Prise en charge PAE du noyau|2019, 2016, 2012 R2, 2012, 2008 R2|N/A|N/A|N/A|N/A|N/A|N/A|N/A|N/A|N/A|
|Configuration de l’écart MMIO|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Mémoire dynamique - ajout à chaud|2019, 2016, 2012 R2, 2012|&#10004; Note 8, 9, 10|&#10004; Note 8, 9, 10|&#10004; Note 8, 9, 10|&#10004; Note 9, 10|&#10004; Note 9, 10|&#10004; Note 9, 10|&#10004; Note 9, 10|&#10004; Note 9, 10|&#10004; Note 8, 9, 10|
|Mémoire dynamique - augmentation de la capacité|2019, 2016, 2012 R2, 2012|&#10004; Note 8, 9, 10|&#10004; Note 8, 9, 10|&#10004; Note 8, 9, 10|&#10004; Note 9, 10|&#10004; Note 9, 10|&#10004; Note 9, 10|&#10004; Note 9, 10|&#10004; Note 9, 10|&#10004; Note 8, 9, 10|
|Redimensionnement de la mémoire du runtime|2019, 2016|&#10004;|&#10004;|&#10004;|||||||
|**[Video](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**||||
|Périphérique vidéo spécifique à Hyper-V|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|**[Divers](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**||||
|Paire clé-valeur|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Interruption non masquable|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Copie des fichiers d’hôte à invité|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|commande de lsvmbus|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|||||||
|Sockets Hyper-V|2019, 2016|&#10004;|&#10004;|&#10004;|||||||
|La norme PCI Passthrough/DDA|2019, 2016|&#10004;|&#10004;||&#10004;|&#10004;|&#10004;||||
|**[Ordinateurs virtuels de génération 2](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)**|||||
|Démarrage à l’aide d’UEFI|2019, 2016, 2012 R2|&#10004;Remarque 14|&#10004;Remarque 14|&#10004;Remarque 14|&#10004;Remarque 14|&#10004;Remarque 14|&#10004;Remarque 14|&#10004;Remarque 14|&#10004;Remarque 14|&#10004;Remarque 14|
|Démarrage sécurisé|2019, 2016|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|

## <a name="BKMK_6x"></a>RHEL/CentOS 6.x série

Le noyau 32 bits pour cette série est PAE est activé. Il n’existe aucune prise en charge LIS intégrée pour RHEL/CentOS 6.0 – 6.3.

|**Fonctionnalité**|**Version de Windows Server**|**6.4-6.10**|**6.0-6.3**|**6.10, 6.9, 6.8**|**6.6, 6.7**|**6.5**|**6.4**|
|-|-|-|-|-|-|-|-|
|**Disponibilité**||[LIS 4.2](https://www.microsoft.com/download/details.aspx?id=55106)|[LIS 4.2](https://www.microsoft.com/download/details.aspx?id=55106)|Intégré|Intégré|Intégré|Intégré|
|**[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Heure précise de Windows Server 2016|2019, 2016||||||
|**[Mise en réseau](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**|
|Trames Jumbo|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Un marquage VLAN et trunking|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004; Note 1|&#10004; Note 1|&#10004; Note 1|&#10004; Note 1|&#10004; Note 1|&#10004; Note 1|
|Migration dynamique|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Injection de l’adresse IP statique|2019, 2016, 2012 R2, 2012|&#10004; Note 2|&#10004; Note 2|&#10004; Note 2|&#10004; Note 2|&#10004; Note 2|&#10004; Note 2|
|vRSS|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|||
|Segmentation de TCP et les déchargements de somme de contrôle|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|||
|SR-IOV|2019, 2016|||||||
|**[Stockage](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**|
|Redimensionnement VHDX|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|Fibre Channel virtuel|2019, 2016, 2012 R2|&#10004; Note 3|&#10004; Note 3|&#10004; Note 3|&#10004; Note 3|&#10004; Note 3||
|Sauvegarde de machine virtuelle active|2019, 2016, 2012 R2|&#10004; Note 5|&#10004; Note 5|&#10004; Note 4, 5|&#10004; Note 4, 5|&#10004; Note 4, 5, 6|&#10004; Note 4, 5, 6|
|Prise en charge de TRIM|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;||||
|SCSI WWN|2019, 2016, 2012 R2|&#10004;|&#10004;|||||
|**[Mémoire](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**|
|Prise en charge PAE du noyau|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Configuration de l’écart MMIO|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Mémoire dynamique - ajout à chaud|2019, 2016, 2012 R2, 2012|&#10004; Note 7, 9, 10|&#10004; Note 7, 9, 10|&#10004; Note 7, 9, 10|&#10004; Note 7, 8, 9, 10|&#10004; Note 7, 8, 9, 10||
|Mémoire dynamique - augmentation de la capacité|2019, 2016, 2012 R2, 2012|&#10004; Note 7, 9, 10|&#10004; Note 7, 9, 10|&#10004; Note 7, 9, 10|&#10004; Note 7, 9, 10|&#10004; Note 7, 9, 10|&#10004; Note 7, 9, 10, 11|
|Redimensionnement de la mémoire du runtime|2019, 2016|||||||
|**[Video](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**|
|Périphérique vidéo spécifique à Hyper-V|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|**[Divers](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**|
|Paire clé-valeur|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;Remarque 12|&#10004;Remarque 12|&#10004; Note 12, 13|&#10004; Note 12, 13|
|Interruption non masquable|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Copie des fichiers d’hôte à invité|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|||
|commande de lsvmbus|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;||||||
|Sockets Hyper-V|2019, 2016|&#10004;|&#10004;|||||
|La norme PCI Passthrough/DDA|2019, 2016|||||||
|**[Ordinateurs virtuels de génération 2](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)**|
|Démarrage à l’aide d’UEFI|2012 R2|||||||
||2019, 2016|&#10004;Remarque 14|&#10004;Remarque 14|&#10004;Remarque 14||||
|Démarrage sécurisé|2019, 2016||||||

## <a name="BKMK_5x"></a>RHEL/CentOS 5.x série

Cette série a un prise en charge 32 bits PAE du noyau disponible. Il n’existe aucune prise en charge LIS intégrée pour RHEL/CentOS avant 5.9.

|**Fonctionnalité**|**Version de Windows Server**|5.2 -5.11|**5.2-5.11**|**5.9 - 5.11**|
|-|-|-|-|-|
|**Disponibilité**||[LIS 4.2](https://www.microsoft.com/download/details.aspx?id=55106)|[LIS 4.1](https://www.microsoft.com/download/details.aspx?id=51612)|Intégré|
|**[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|
|Heure précise de Windows Server 2016|2019, 2016||||
|**[Mise en réseau](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**|
|Trames Jumbo|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|
|Un marquage VLAN et trunking|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004; Note 1|&#10004; Note 1|&#10004; Note 1|
|Migration dynamique|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|
|Injection de l’adresse IP statique|2019, 2016, 2012 R2, 2012|&#10004; Note 2|&#10004; Note 2|&#10004; Note 2|
|vRSS|2019, 2016, 2012 R2||||
|Segmentation de TCP et les déchargements de somme de contrôle|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;||
|SR-IOV|2019, 2016||||||
|**[Stockage](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**|
|Redimensionnement VHDX|2019, 2016, 2012 R2|&#10004;|&#10004;||
|Fibre Channel virtuel|2019, 2016, 2012 R2|&#10004; Note 3|&#10004; Note 3||
|Sauvegarde de machine virtuelle active|2019, 2016, 2012 R2|&#10004; Note 5, 15|&#10004; Note 5|&#10004; Note 4, 5, 6|
|Prise en charge de TRIM|2019, 2016, 2012 R2||||
|SCSI WWN|2019, 2016, 2012 R2||||
|**[Mémoire](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**|
|Prise en charge PAE du noyau|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|
|Configuration de l’écart MMIO|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|
|Mémoire dynamique - ajout à chaud|2019, 2016, 2012 R2, 2012||||
|Mémoire dynamique - augmentation de la capacité|2019, 2016, 2012 R2, 2012|&#10004; Note 7, 9, 10, 11|&#10004; Note 7, 9, 10, 11||
|Redimensionnement de la mémoire du runtime|2019, 2016||||
|**[Video](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**|
|Périphérique vidéo spécifique à Hyper-V|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;||
|**[Divers](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**|
|Paire clé-valeur|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;||
|Interruption non masquable|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|
|Copie des fichiers d’hôte à invité|2019, 2016, 2012 R2|&#10004;|&#10004;||
|commande de lsvmbus|2019, 2016, 2012 R2, 2012, 2008 R2||||
|Sockets Hyper-V|2019, 2016||||
|La norme PCI Passthrough/DDA|2019, 2016||||||
|**[Ordinateurs virtuels de génération 2](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)**|
|Démarrage à l’aide d’UEFI|2019, 2016, 2012 R2||||
|Démarrage sécurisé|2019, 2016||||

## <a name="BKMK_notes"></a>Notes de publication

1. Pour cette version RHEL/CentOS, VLAN balisage fonctionne mais n’est pas le cas de jonction de réseau local virtuel.

2. Injection d’adresse IP statique peut ne pas fonctionne si le Gestionnaire de réseau a été configuré pour une carte réseau synthétique donné sur la machine virtuelle. Pour le bon fonctionnement de l’adresse IP statique injection Assurez-vous que le Gestionnaire de réseau est hors tension complètement ou a été désactivé pour une carte réseau spécifique via son fichier ifcfg-ethX.

3. Sur Windows Server 2012 R2 lors de l’utilisation de périphériques virtuels fibre channel, assurez-vous que le numéro d’unité logique (LUN 0) de 0 a été remplie. Si le LUN 0 n’a pas été rempli, une machine virtuelle Linux ne peut pas être en mesure de monter les périphériques fibre channel en mode natif.

4. Pour les LIS intégrées, le package « Hyper-v-démons » doit être installé pour cette fonctionnalité.

5. S’il existe des descripteurs de fichiers ouverts pendant une opération de sauvegarde de machine virtuelle active, puis, dans certains cas extrêmes, les disques durs virtuels sauvegardés peut-être subir une fichier système de cohérence (fsck) lors de la restauration. Les opérations de sauvegarde en direct peuvent échouer en mode silencieux si l’ordinateur virtuel possède un périphérique iSCSI attaché ou le stockage en attachement direct (également appelé un disque direct).

6. Pendant le téléchargement de Linux Integration Services par défaut, live sauvegarde prise en charge pour RHEL/CentOS 5.9 - 5.11/6.4/6.5 est également disponible via [Essentials de sauvegarde Hyper-V pour Linux](https://github.com/LIS/backupessentials/tree/1.0).

7. Prise en charge de la mémoire dynamique est uniquement disponible sur les ordinateurs virtuels 64 bits.

8. Prise en charge de l’ajout à chaud n’est pas activée par défaut dans cette distribution. Pour activer la prise en charge de l’ajout à chaud, vous devez ajouter une règle d’udev sous /etc/udev/rules.d/ comme suit :

   1. Créez un fichier **/etc/udev/rules.d/100-balloon.rules**. Vous pouvez utiliser n’importe quel autre nom souhaité pour le fichier.

   2. Ajoutez le contenu suivant au fichier : `SUBSYSTEM=="memory", ACTION=="add", ATTR{state}="online"`

   3. Redémarrez le système pour activer la prise en charge de l’ajout à chaud.

   Pendant que le téléchargement de Linux Integration Services crée cette règle sur l’installation, la règle est également supprimée lors de la désinstallation de LIS, donc la règle doit être recréée si la mémoire dynamique est nécessaire après la désinstallation.

9. Opérations de mémoire dynamique peuvent échouer si le système d’exploitation invité s’exécute trop faible de la mémoire. Voici quelques-unes des meilleures pratiques :

   * Mémoire de démarrage et la quantité minimale de mémoire doivent être égale ou supérieure à la quantité de mémoire qui recommande le fournisseur de distribution.

   * Les applications qui ont tendance à consommer la totalité de la mémoire disponible sur un système sont limitées à consommer jusqu'à 80 % de mémoire vive disponible.

10. Si vous utilisez la mémoire dynamique sur un système d’exploitation Windows Server 2016 ou Windows Server 2012 R2, spécifiez **mémoire de démarrage**, **mémoire minimale**, et **mémoire maximale** paramètres par multiples de 128 mégaoctets (Mo). Cela peut entraîner à chaud de défaillances, et vous ne voyiez pas de mémoire augmenter dans un système d’exploitation.

11. Certaines distributions, y compris ceux qui utilisent les LIS 4.0 et 4.1, augmentation prennent en charge uniquement et ne fournissent pas de prise en charge de l’ajout à chaud. Dans ce scénario, la fonctionnalité mémoire dynamique peut être utilisée en définissant le paramètre de mémoire de démarrage pour une valeur qui est égale au paramètre mémoire Maximum. Il en résulte toute la mémoire requise n’est ajouté à chaud à la machine virtuelle au moment du démarrage et ensuite selon les besoins en mémoire de l’hôte, Hyper-V peut librement allouer ou libérer la mémoire de l’invité à l’aide d’augmentation. Veuillez configurer **mémoire de démarrage** et **mémoire minimale** supérieure ou égale à la valeur recommandée pour la distribution.

12. Pour activer l’infrastructure de clé/valeur paire (paire clé/valeur), installez le package rpm hypervkvpd ou les démons de Hyper-v à partir de votre ISO RHEL. Le package peut également être installé directement à partir de référentiels RHEL.

13. L’infrastructure de (KVP) de paire clé/valeur ne peut pas fonctionner correctement sans une mise à jour de logiciels Linux. Contactez votre fournisseur de distribution pour obtenir la mise à jour logicielle dans le cas où vous voyez des problèmes liés à cette fonctionnalité.

14. Sur Windows Server 2012 R2 génération 2 machines virtuelles ont le démarrage sécurisé est activé par défaut et des machines virtuelles de Linux ne démarrera pas, sauf si l’option de démarrage sécurisé est désactivée. Vous pouvez désactiver le démarrage sécurisé dans le **microprogramme** section des paramètres de la machine virtuelle dans **Gestionnaire Hyper-V** ou vous pouvez le désactiver à l’aide de Powershell :

   ```Powershell
   Set-VMFirmware -VMName "VMname" -EnableSecureBoot Off

   ```

   Le téléchargement de Services d’intégration Linux peut être appliqué aux machines virtuelles de 2 génération existantes, mais n’octroie pas de fonctionnalité de génération 2.

15. Dans Red Hat Enterprise Linux ou CentOS 5.2, 5.3 et 5.4 la fonctionnalité de blocage de système de fichiers n’est pas disponible, afin de la sauvegarde de Machine virtuelle en direct n’est pas disponible.

Voir aussi

* [Set-VMFirmware](https://technet.microsoft.com/library/dn464287.aspx)

* [Machines virtuelles Debian prises en charge sur Hyper-V](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [Machines virtuelles Oracle Linux prises en charge sur Hyper-V](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [Machines virtuelles SUSE prises en charge sur Hyper-V](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [Machines virtuelles Ubuntu prises en charge sur Hyper-V](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [Machines virtuelles FreeBSD prises en charge sur Hyper-V](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Descriptions des fonctionnalités pour les machines virtuelles Linux et FreeBSD sur Hyper-V](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Meilleures pratiques pour l’exécution de Linux sur Hyper-V](Best-Practices-for-running-Linux-on-Hyper-V.md)

* [Certification de matériel de Red Hat](https://hardware.redhat.com/&quicksearch=Microsoft)
