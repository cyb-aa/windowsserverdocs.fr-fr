---
title: Meilleures pratiques pour l’exécution de Linux sur Hyper-V
description: Fournit des recommandations pour l’exécution de Linux sur une machine virtuelle
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.topic: article
ms.assetid: a08648eb-eea0-4e2b-87fb-52bfe8953491
author: shirgall
ms.author: kathydav
ms.date: 04/15/2020
ms.openlocfilehash: d8861369abe24ea0d34dce209a5d98e854c4c95d
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82072235"
---
# <a name="best-practices-for-running-linux-on-hyper-v"></a>Meilleures pratiques pour l’exécution de Linux sur Hyper-V

>S’applique à : Windows Server 2019, Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7,1, Windows 7

Cette rubrique contient une liste de recommandations relatives à l’exécution d’une machine virtuelle Linux sur Hyper-V.

## <a name="tuning-linux-file-systems-on-dynamic-vhdx-files"></a>Paramétrage des systèmes de fichiers Linux sur des fichiers VHDX dynamiques

Certains systèmes de fichiers Linux peuvent consommer une quantité importante d’espace disque réel même si le système de fichiers est pratiquement vide. Pour réduire la quantité d’espace disque réellement utilisé pour les fichiers VHDX dynamiques, tenez compte des recommandations suivantes :

* Lors de la création du VHDX, utilisez 1 Mo BlockSizeBytes (à partir de la valeur par défaut 32 Mo) dans PowerShell, par exemple :

```Powershell
PS > New-VHD -Path C:\MyVHDs\test.vhdx -SizeBytes 127GB -Dynamic -BlockSizeBytes 1MB
```

* Le format ext4 est préféré à ext3, car ext4 est plus efficace que ext3 lorsqu’il est utilisé avec des fichiers VHDX dynamiques.

* Lorsque vous créez le système de fichiers, spécifiez le nombre de groupes à 4096, par exemple :

```bash
# mkfs.ext4 -G 4096 /dev/sdX1

```

## <a name="grub-menu-timeout-on-generation-2-virtual-machines"></a>Délai d’attente du menu GRUB sur les ordinateurs virtuels de 2e génération

En raison de la suppression du matériel hérité de l’émulation dans les ordinateurs virtuels de génération 2, le minuteur de compte à rebours du menu GRUB est trop rapide pour que le menu GRUB s’affiche, ce qui charge immédiatement l’entrée par défaut. Jusqu’à ce que grub soit résolu pour utiliser le minuteur pris en charge par EFI, modifiez **/boot/grub/grub.conf**,/**etc/default/grub**, ou équivalent à « Timeout = 100000 » au lieu de « timeout = 5 » par défaut.

## <a name="pxe-boot-on-generation-2-virtual-machines"></a>Démarrage PxE sur des ordinateurs virtuels de génération 2

Étant donné que la minuterie PIT n’est pas présente dans les ordinateurs virtuels de 2e génération, les connexions réseau au serveur TFTP PxE peuvent être prématurées et empêcher le bootloader de lire la configuration GRUB et de charger un noyau à partir du serveur.

Sur RHEL 6. x, vous pouvez utiliser le chargeur d’initialisation EFI v 0.97 EFI hérité à la place de grub2, comme décrit ici :[https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/6/html/Installation_Guide/s1-netboot-pxe-config-efi.html](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/6/html/Installation_Guide/s1-netboot-pxe-config-efi.html)

Sur les distributions Linux autres que RHEL 6. x, vous pouvez suivre des étapes similaires pour configurer grub v 0.97 pour charger les noyaux Linux à partir d’un serveur PxE.

En outre, sur les entrées de clavier et de souris RHEL/CentOS 6,6 ne fonctionneront pas avec le noyau de préinstallation qui empêche de spécifier les options d’installation dans le menu. Une console série doit être configurée pour autoriser le choix des options d’installation.

* Dans le fichier **efidefault** sur le serveur PxE, ajoutez le paramètre de noyau suivant **« console = ttyS1 »** .

* Sur la machine virtuelle dans Hyper-V, configurez un port COM à l’aide de cette applet de commande PowerShell :

```Powershell
Set-VMComPort -VMName <Name> -Number 2 -Path \\.\pipe\dbg1

```

La spécification d’un fichier bakickstart pour le noyau de préinstallation évite également d’avoir à utiliser le clavier et la souris pendant l’installation.

## <a name="use-static-mac-addresses-with-failover-clustering"></a>Utiliser des adresses MAC statiques avec le clustering de basculement

Les machines virtuelles Linux qui seront déployées à l’aide du clustering de basculement doivent être configurées avec une adresse MAC (Media Access Control) statique pour chaque carte réseau virtuelle. Dans certaines versions de Linux, la configuration de mise en réseau peut être perdue après le basculement, car une nouvelle adresse MAC est affectée à la carte réseau virtuelle. Pour éviter de perdre la configuration du réseau, assurez-vous que chaque carte réseau virtuelle a une adresse MAC statique. Vous pouvez configurer l’adresse MAC en modifiant les paramètres de l’ordinateur virtuel dans le Gestionnaire Hyper-V ou Gestionnaire du cluster de basculement.

## <a name="use-hyper-v-specific-network-adapters-not-the-legacy-network-adapter"></a>Utiliser des cartes réseau spécifiques à Hyper-V, et non la carte réseau héritée

Configurez et utilisez la carte Ethernet virtuelle, qui est une carte réseau spécifique à Hyper-V avec des performances améliorées. Si des cartes réseau spécifiques à l’ancien et à Hyper-V sont attachées à une machine virtuelle, les noms de réseau dans la sortie de **ifconfig-a** peuvent afficher des valeurs aléatoires comme **_tmp12000801310**. Pour éviter ce problème, supprimez toutes les cartes réseau héritées lors de l’utilisation de cartes réseau spécifiques à Hyper-V dans une machine virtuelle Linux.

## <a name="use-io-scheduler-noopnone-for-better-disk-io-performance"></a>Utiliser le planificateur d’e/s NOOP/None pour améliorer les performances d’e/s disque

Le noyau Linux offre deux ensembles de planificateurs d’e/s disque pour réorganiser les demandes.  L’un est pour l’ancien sous-système « BLK » et l’autre pour le sous-système « BLK-MQ » plus récent. Dans les deux cas, avec les disques SSD actuels, il est recommandé d’utiliser un planificateur qui transmet les décisions de planification à l’hyperviseur Hyper-V sous-jacent. Pour les noyaux Linux utilisant le sous-système « BLK », il s’agit du planificateur « NOOP ». Pour les noyaux Linux utilisant le sous-système « BLK-MQ », il s’agit du planificateur « None ».

Pour un disque particulier, les planificateurs disponibles peuvent être consultés à cet emplacement du système de`<diskname>`fichiers:/sys/Class/Block//queue/Scheduler, avec le planificateur actuellement sélectionné entre crochets. Vous pouvez modifier le planificateur en écrivant à cet emplacement de système de fichiers. La modification doit être ajoutée à un script d’initialisation pour pouvoir être conservée entre les redémarrages. Pour plus d’informations, consultez la documentation de votre distribution Linux.

## <a name="numa"></a>NUMA

Les versions du noyau Linux antérieures à la version 2.6.37 ne prennent pas en charge NUMA sur Hyper-V avec des machines virtuelles de taille supérieure. Ce problème concerne principalement les distributions antérieures utilisant le noyau Red Hat 2.6.32 en amont ; il a été corrigé dans Red Hat Enterprise Linux (RHEL) 6.6 (kernel-2.6.32-504). Pour les systèmes exécutant des noyaux personnalisés dont la version est antérieure à la version 2.6.37 ou des noyaux basés sur RHEL antérieurs à la version 2.6.32-504, le paramètre de démarrage `numa=off` doit être défini sur la ligne de commande du noyau dans grub.conf. Pour plus d’informations, consultez l’article [KB 436883](https://access.redhat.com/solutions/436883) sur Red Hat.

## <a name="reserve-more-memory-for-kdump"></a>Réserver plus de mémoire pour kdump

Au cas où le noyau de capture de vidage finit par une panique au démarrage, Réservez plus de mémoire pour le noyau. Par exemple, remplacez le paramètre **crashkernel = 384 Mo- : 128M** par **crashkernel = 384 Mo- : 256M** dans le fichier de configuration d’Ubuntu grub.

## <a name="shrinking-vhdx-or-expanding-vhd-and-vhdx-files-can-result-in-erroneous-gpt-partition-tables"></a>La réduction des fichiers VHDX ou de disque dur virtuel et VHDX de développement peut entraîner des tables de partition GPT erronées

Hyper-V permet de réduire les fichiers de disque virtuel (VHDX) sans tenir compte de la partition, du volume ou des structures de données du système de fichiers qui peuvent exister sur le disque. Si le VHDX est réduit à là où la fin du VHDX vient avant la fin d’une partition, les données peuvent être perdues, cette partition peut être endommagée ou des données non valides peuvent être retournées lors de la lecture de la partition.

Après le redimensionnement d’un disque dur virtuel ou d’un VHDX, les administrateurs doivent utiliser un utilitaire comme fdisk ou une partie pour mettre à jour la partition, le volume et les structures du système de fichiers afin de refléter la modification de la taille du disque. La réduction ou l’augmentation de la taille d’un disque dur virtuel ou d’un VHDX qui a une table de partition GUID (GPT) génère un avertissement quand un outil de gestion de partition est utilisé pour vérifier la disposition de la partition, et l’administrateur est averti pour corriger les en-têtes GPT principaux et secondaires. Cette étape manuelle peut s’effectuer sans risque de perte de données.

## <a name="see-also"></a>Voir aussi

* [Machines virtuelles Linux et FreeBSD prises en charge pour Hyper-V sur Windows](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)

* [Meilleures pratiques pour exécuter FreeBSD sur Hyper-V](Best-practices-for-running-FreeBSD-on-Hyper-V.md)

* [Déployer un cluster Hyper-V](https://technet.microsoft.com/library/jj863389.aspx)

* [Créer des images Linux pour Azure](https://docs.microsoft.com/azure/virtual-machines/linux/create-upload-generic)

* [Optimiser votre machine virtuelle Linux sur Azure](https://docs.microsoft.com/azure/virtual-machines/linux/optimization)
