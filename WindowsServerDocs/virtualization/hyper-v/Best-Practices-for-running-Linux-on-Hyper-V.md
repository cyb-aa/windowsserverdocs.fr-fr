---
title: Meilleures pratiques pour l’exécution de Linux sur Hyper-V
description: Fournit des recommandations pour l’exécution de Linux sur une machine virtuelle
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a08648eb-eea0-4e2b-87fb-52bfe8953491
author: shirgall
ms.author: kathydav
ms.date: 3/1/2019
ms.openlocfilehash: 190a5e5d32140d6fa688bb9de98d05ec2f9783c3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838680"
---
# <a name="best-practices-for-running-linux-on-hyper-v"></a>Meilleures pratiques pour l’exécution de Linux sur Hyper-V

>S'applique à : Windows Server 2019, Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7.1, Windows 7

Cette rubrique contient une liste de recommandations pour l’exécution de machine virtuelle Linux sur Hyper-V.

## <a name="tuning-linux-file-systems-on-dynamic-vhdx-files"></a>Réglage des systèmes de fichiers de Linux sur des fichiers VHDX dynamiques

Certains systèmes de fichiers Linux peuvent consommer des quantités importantes de l’espace disque réel même lorsque le système de fichiers est pratiquement vide. Pour réduire la quantité d’utilisation de l’espace disque réel des fichiers VHDX dynamiques, tenez compte des recommandations suivantes :

* Lors de la création du disque VHDX, utilisez 1 BlockSizeBytes Mo (à partir de la valeur par défaut 32 Mo) dans PowerShell, par exemple :

```Powershell
PS > New-VHD -Path C:\MyVHDs\test.vhdx -SizeBytes 127GB -Dynamic -BlockSizeBytes 1MB
```

* Le format ext4 est préférable à ext3 car ext4 est plus efficace d’espace qu’ext3 lorsqu’il est utilisé avec les fichiers VHDX dynamiques.

* Lorsque la création du système de fichiers spécifiez le nombre de groupes de 4096, par exemple :

```bash
# mkfs.ext4 -G 4096 /dev/sdX1

```

## <a name="grub-menu-timeout-on-generation-2-virtual-machines"></a>Délai d’attente du Menu GRUB sur les ordinateurs virtuels de génération 2

En raison de matériel hérité en cours de suppression de l’émulation dans les machines virtuelles de génération 2, le compte à rebours grub menu compte vers le bas trop rapidement pour le menu grub s’affiche, chargement immédiatement l’entrée par défaut. Jusqu'à ce que grub est fixé à utiliser le chronomètre pris en charge EFI, modifier **/boot/grub/grub.conf**, /**par défaut/etc/grub**, ou équivalent à avoir « délai d’attente = 100000 » au lieu de la valeur par défaut « délai d’attente = 5 ».

## <a name="pxe-boot-on-generation-2-virtual-machines"></a>Démarrage PxE sur des Machines virtuelles de génération 2

Étant donné que le minuteur de la table d’importation principale n’est pas présent dans Generation 2 Virtual Machines, les connexions réseau sur le serveur PxE TFTP peuvent être arrêtées prématurément et empêchent le chargeur de démarrage à partir de la lecture de la configuration de Grub et le chargement d’un noyau à partir du serveur.

Sur RHEL 6.x, le chargeur de démarrage grub hérité v0.97 EFI peut être utilisé au lieu de grub2, comme indiqué ici : [https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/6/html/Installation_Guide/s1-netboot-pxe-config-efi.html](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/6/html/Installation_Guide/s1-netboot-pxe-config-efi.html)

Sur les distributions Linux autres que RHEL 6.x, des étapes similaires peuvent être suivies pour configurer v0.97 grub pour charger des noyaux Linux à partir d’un serveur PxE.

En outre, sur RHEL/CentOS 6.6 clavier et souris entrée ne fonctionne pas avec le noyau de préinstallation, ce qui empêche spécifiant les options d’installation dans le menu. Une console série doit être configurée pour autoriser le choix d’options d’installation.

* Dans le **efidefault** sur le serveur PxE, ajoutez le paramètre de noyau suivant **« console = ttyS1 »**

* Sur la machine virtuelle dans Hyper-V, configurer un port COM à l’aide de cette applet de commande PowerShell :

```Powershell
Set-VMComPort -VMName <Name> -Number 2 -Path \\.\pipe\dbg1

```

Spécification d’un fichier kickstart au noyau préinstallation également évitant ainsi la nécessité de clavier et souris pendant l’installation.

## <a name="use-static-mac-addresses-with-failover-clustering"></a>Utiliser des adresses MAC statiques avec le clustering de basculement

Machines virtuelles Linux qui sera déployées à l’aide du clustering de basculement doit être configurés avec une adresse statique media access control (MAC) pour chaque carte réseau virtuelle. Dans certaines versions de Linux, la configuration du réseau peut être perdue après le basculement, car une nouvelle adresse MAC est affectée à la carte réseau virtuelle. Pour éviter de perdre la configuration du réseau, vérifiez que chaque carte réseau virtuelle a une adresse MAC statique. Vous pouvez configurer l’adresse MAC en modifiant les paramètres de la machine virtuelle dans le Gestionnaire Hyper-V ou le Gestionnaire du Cluster de basculement.

## <a name="use-hyper-v-specific-network-adapters-not-the-legacy-network-adapter"></a>Utilisez des cartes réseau spécifiques à Hyper-V, pas la carte réseau héritée

Configurer et utiliser la carte Ethernet virtuelle, qui est une carte de réseau spécifiques à Hyper-V avec des performances améliorées. Si hérité et adaptateurs de réseau spécifiques à Hyper-V sont attachés à une machine virtuelle, les noms de réseau dans la sortie de **ifconfig - a** peut afficher des valeurs aléatoires comme **_tmp12000801310**. Pour éviter ce problème, supprimez toutes les cartes réseau héritées lors de l’utilisation de cartes de réseau spécifiques à Hyper-V dans une machine virtuelle Linux.

## <a name="use-io-scheduler-noop-for-better-disk-io-performance"></a>Utilisation d’e/s planificateur NOOP pour le disque de meilleures performances d’e/s

Le noyau Linux a quatre planificateurs d’e/s différentes pour réorganiser les demandes avec différents algorithmes. NOOP est une file d’attente premier sorti qui transmet la décision de planification d’être effectuées par l’hyperviseur. Il est recommandé d’utiliser NOOP comme planificateur lors de l’exécution de machine virtuelle Linux sur Hyper-V. Modifier le planificateur pour un périphérique spécifique, dans la configuration du chargeur de démarrage (/ etc/grub.conf, par exemple), ajouter **ascenseur = noop** et les paramètres de noyau, puis redémarrez.

## <a name="numa"></a>NUMA

Versions du noyau Linux antérieures à la version 2.6.37 ne prennent en charge NUMA sur Hyper-V avec la taille de machine virtuelle. Ce problème concerne principalement les distributions antérieures à l’aide de la source amont noyau Red Hat 2.6.32 et a été corrigé dans Red Hat Enterprise Linux (RHEL) 6.6 (kernel-2.6.32-504). Les systèmes exécutant des noyaux personnalisés antérieures à la version 2.6.37 ou des noyaux basés sur RHEL que 2.6.32-504 doivent définir le paramètre de démarrage `numa=off` sur la ligne de commande du noyau dans grub.conf. Pour plus d’informations, consultez [Red Hat KB 436883](https://access.redhat.com/solutions/436883).

## <a name="reserve-more-memory-for-kdump"></a>Réserver plus de mémoire pour kdump

Dans le cas où le noyau de capture de vidage se termine avec une erreur grave au démarrage, réserver plus de mémoire pour le noyau. Par exemple, modifiez le paramètre **crashkernel = 384M-:128M** à **crashkernel = 384M-:256M** dans le fichier de configuration de grub Ubuntu.

## <a name="shrinking-vhdx-or-expanding-vhd-and-vhdx-files-can-result-in-erroneous-gpt-partition-tables"></a>Réduire VHDX ou de développer des fichiers VHD et VHDX peut entraîner des tables de partition GPT erronées

Hyper-V permet de réduire les fichiers de disque virtuel (VHDX) sans tenir compte de toute partition, le volume ou les structures de données de système de fichiers qui peuvent exister sur le disque. Si le disque VHDX est réduit à l’origine de la fin de la VHDX avant la fin d’une partition, données peuvent être perdues, que partition peut devenir endommagées ou non valides des données peut être retournée lors de la lecture de la partition.

Après le redimensionnement d’un disque dur virtuel ou un VHDX, les administrateurs doivent utiliser un utilitaire tel que fdisk ou partitionnées pour mettre à jour de la partition, volume et structures de système de fichiers pour refléter la modification de la taille du disque. Réduction ou augmentation de la taille d’un disque dur virtuel ou d’un VHDX qui a un Table de Partition GUID (GPT) entraîne un avertissement lorsqu’un outil de gestion de partition est utilisé pour vérifier la disposition de partition, et indiquera l’administrateur pour résoudre les en-têtes GPT premier et secondaires. Cette étape manuelle est possible sans effectuer sans perte de données.

## <a name="see-also"></a>Voir aussi

* [Machines virtuelles prises en charge Linux et FreeBSD pour Hyper-V sur Windows](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)

* [Meilleures pratiques pour l’exécution de FreeBSD sur Hyper-V](Best-practices-for-running-FreeBSD-on-Hyper-V.md)

* [Déployer un Cluster Hyper-V](https://technet.microsoft.com/library/jj863389.aspx)

* [Créer des Images Linux pour Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/create-upload-generic)

* [Optimiser votre machine virtuelle Linux sur Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/optimization)
