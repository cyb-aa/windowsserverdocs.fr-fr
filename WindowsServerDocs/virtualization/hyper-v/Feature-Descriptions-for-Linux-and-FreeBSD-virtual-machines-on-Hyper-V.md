---
title: Descriptions des fonctionnalités pour les machines virtuelles Linux et FreeBSD sur Hyper-V
description: Décrit les fonctionnalités qui affectent les principaux composants tels que la mise en réseau, stockage, la mémoire lors de l’utilisation de Linux et FreeBSD sur une machine virtuelle
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a9ee931d-91fc-40cf-9a15-ed6fa6965cb6
author: shirgall
ms.author: kathydav
ms.date: 10/03/2016
ms.openlocfilehash: 944f8e9d902953ab4d6da0750603a2c40fa9e96d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844890"
---
# <a name="feature-descriptions-for-linux-and-freebsd-virtual-machines-on-hyper-v"></a>Descriptions des fonctionnalités pour les machines virtuelles Linux et FreeBSD sur Hyper-V

>S'applique à : Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7.1, Windows 7

Cet article décrit les fonctionnalités disponibles dans les composants tels que core, mise en réseau, stockage et la mémoire lors de l’utilisation de Linux et FreeBSD sur une machine virtuelle.

## <a name="BKMK_core"></a>Core

|**Fonctionnalité**|**Description**|
|-|-|
|Arrêt intégré|Avec cette fonctionnalité, un administrateur peut arrêter les machines virtuelles à partir du Gestionnaire Hyper-V. Pour plus d’informations, consultez [arrêt du système d’exploitation](https://technet.microsoft.com/library/dn798297(WS.11).aspx#BKMK_Shutdown).|
|Synchronisation date / heure|Cette fonctionnalité garantit que l’heure de mise à jour à l’intérieur d’une machine virtuelle reste synchronisé avec l’heure de mise à jour sur l’ordinateur hôte. Pour plus d’informations, consultez [synchronisation date/heure](https://technet.microsoft.com/library/dn798297(WS.11).aspx#BKMK_time).|
|Heure précise de Windows Server 2016|Cette fonctionnalité autorise l’invité à utiliser la fonctionnalité de l’heure précise de Windows Server 2016, ce qui améliore la synchronisation de l’heure avec l’hôte à 1 MS précision. Pour plus d’informations, consultez [heure précise de Windows Server 2016](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-2016-accurate-time)|
|Prise en charge de multitraitement|Avec cette fonctionnalité, un ordinateur virtuel peut utiliser plusieurs processeurs sur l’ordinateur hôte en configurant plusieurs processeurs virtuels.|
|Pulsations|Avec cette fonctionnalité, l’hôte peut suivre l’état de la machine virtuelle. Pour plus d’informations, consultez [pulsation](https://technet.microsoft.com/library/dn798297(WS.11).aspx#BKMK_heartbeat).|
|Prise en charge intégrée de la souris|Avec cette fonctionnalité, vous pouvez utiliser la souris sur le bureau d’un ordinateur virtuel et également utiliser la souris en toute transparence entre le bureau de Windows Server et la console Hyper-V pour la machine virtuelle.|
|Périphérique de stockage spécifique de Hyper-V|Cette fonctionnalité octroie l’accès hautes performances aux dispositifs de stockage qui sont attachés à une machine virtuelle.|
|Périphérique de réseau Hyper-V spécifique|Cette fonctionnalité octroie l’accès hautes performances pour les cartes réseau qui sont attachés à une machine virtuelle.|

## <a name="BKMK_Networking"></a>Mise en réseau

|**Fonctionnalité**|**Description**|
|-|-|
|Trames Jumbo|Avec cette fonctionnalité, un administrateur peut augmenter la taille de trames réseau au-delà de 1 500 octets, ce qui entraîne une augmentation significative des performances du réseau.|
|Un marquage VLAN et trunking|Cette fonctionnalité vous permet de configurer le trafic LAN (VLAN) virtuel pour les machines virtuelles.|
|Migration dynamique|Avec cette fonctionnalité, vous pouvez migrer une machine virtuelle à partir d’un ordinateur hôte vers un autre hôte. Pour plus d’informations, consultez [présentation de la Machine virtuelle Live Migration](https://technet.microsoft.com/library/hh831435.aspx).|
|Injection de l’adresse IP statique|Avec cette fonctionnalité, vous pouvez répliquer l’adresse IP statique d’une machine virtuelle une fois qu’il a été basculé vers son réplica sur un hôte différent. Cette réplication IP permet de s’assurer que les charges de travail réseau continuent de fonctionner en toute transparence après un événement de basculement.|
|vRSS (Virtual Receive Side Scaling)|Permet de répartir la charge à partir d’une carte réseau virtuelle entre plusieurs processeurs virtuels dans une machine virtuelle. Pour plus d’informations, consultez [Virtual Receive-side Scaling dans Windows Server 2012 R2](https://technet.microsoft.com/library/dn383582.aspx).|
|Segmentation de TCP et les déchargements de somme de contrôle|Segmentation de transferts et de somme de contrôle fonctionnent à partir de l’UC de l’invité à la carte réseau ou de commutateur virtuelle hôte lors des transferts de données réseau.|
|Grande (LRO) le déchargement de réception|Augmente le débit entrant de connexions haut débit en regroupant plusieurs paquets dans une mémoire tampon plus grande, réduisant la surcharge du processeur.|
|SR-IOV|Les unités d’e/s de racine unique utilisent DDA pour autoriser l’accès des invités à des parties des cartes NIC spécifiques permettant de réduction de la latence et un débit plus élevé. SR-IOV nécessite des pilotes à jour fonction physique (PF) sur l’ordinateur hôte et fonction virtuelle (VF) sur l’invité.|

## <a name="BKMK_Storage"></a>Stockage

|**Fonctionnalité**|**Description**|
|-|-|
|Redimensionnement VHDX|Avec cette fonctionnalité, un administrateur peut redimensionner un fichier .vhdx de taille fixe qui est attaché à une machine virtuelle. Pour plus d’informations, consultez [Online Virtual Hard Disk redimensionnement Overview](https://technet.microsoft.com/library/dn282286.aspx).|
|Fibre Channel virtuel|Avec cette fonctionnalité, les machines virtuelles peut reconnaître un périphérique de canal fibre optique et montez-le en mode natif. Pour plus d’informations, voir [Vue d’ensemble de Fibre Channel virtuel Hyper-V](https://technet.microsoft.com/library/hh831413.aspx).|
|Sauvegarde de machine virtuelle active|Cette fonctionnalité facilite le zéro de la sauvegarde des machines virtuelles.<br /><br />Notez que l’opération de sauvegarde échoue si l’ordinateur virtuel possède des disques durs virtuels (VHD) qui sont hébergés sur le stockage à distance comme un partage de Server Message Block (SMB) ou un volume iSCSI. En outre, assurez-vous que la cible de sauvegarde ne réside pas sur le même volume que le volume que vous sauvegardez.|
|Prise en charge de TRIM|Indicateurs TRIM notifier le lecteur qui ne sont plus requis par l’application de certains secteurs qui ont été précédemment alloués et peuvent être purgées. Ce processus est généralement utilisé lorsqu’une application apporte des allocations d’espace grand via un fichier, puis gère lui-même les allocations dans le fichier, par exemple, aux fichiers de disque dur virtuel.|
|SCSI WWN|Le pilote storvsc extrait des informations de nom WWN (World Wide) à partir du port et le nœud des périphériques attachés à la machine virtuelle et crée les fichiers sysfs approprié. |

## <a name="BKMK_Memory"></a>Mémoire

|**Fonctionnalité**|**Description**|
|-|-|
|Prise en charge PAE du noyau|Technologie d’Extension d’adresse (PAE) physique permet un noyau 32 bits accéder à un espace d’adressage physique qui est supérieur à 4 Go. Les distributions Linux antérieures tels que RHEL 5.x utilisé pour expédier un noyau distinct qui a été PAE est activé. Nouvelles distributions tels que RHEL 6.x ont prédéfinis de prise en charge PAE.|
|Configuration de l’écart MMIO|Avec cette fonctionnalité, les fabricants de matériel peuvent configurer l’emplacement de l’écart de mémoire mappés d’e/s (MMIO). L’écart MMIO est généralement utilisé pour diviser la mémoire physique disponible entre juste assez systèmes d’exploitation d’une appliance (JeOS) et l’infrastructure logicielle réelle qui alimente l’appliance.|
|Mémoire dynamique - ajout à chaud|L’hôte peut dynamiquement augmenter ou diminuer la quantité de mémoire disponible pour une machine virtuelle en cours d’opération. Avant la configuration, l’administrateur Active la mémoire dynamique dans le panneau Paramètres de la Machine virtuelle et spécifiez la mémoire de démarrage, la mémoire minimale et la mémoire maximale pour la machine virtuelle. Lorsque la machine virtuelle est dans l’opération de que mémoire dynamique ne peut pas être désactivée et uniquement les paramètres Minimum et Maximum peut être modifié. (Il est recommandé de spécifier ces tailles de mémoire en multiples de 128 Mo).<br /><br />Au démarrage de la machine virtuelle est tout d’abord disponible mémoire est égale à la **mémoire de démarrage**. À mesure que la demande de mémoire augmente en raison des charges de travail Hyper-V peut allouer dynamiquement de davantage de mémoire à la machine virtuelle via le mécanisme d’ajout à chaud, si pris en charge par cette version du noyau. La quantité maximale de mémoire allouée est limitée par la valeur de la **mémoire maximale** paramètre.<br /><br />L’onglet de la mémoire du Gestionnaire Hyper-V Affiche la quantité de mémoire attribuée à la machine virtuelle, mais les statistiques de la mémoire au sein de la machine virtuelle affiche la quantité maximale de mémoire allouée.<br /><br />Pour plus d’informations, consultez [Hyper-V Dynamic Memory Overview](https://technet.microsoft.com/library/hh831766.aspx).<br /><br />|
|Mémoire dynamique - augmentation de la capacité|L’hôte peut dynamiquement augmenter ou diminuer la quantité de mémoire disponible pour une machine virtuelle en cours d’opération. Avant la configuration, l’administrateur Active la mémoire dynamique dans le panneau Paramètres de la Machine virtuelle et spécifiez la mémoire de démarrage, la mémoire minimale et la mémoire maximale pour la machine virtuelle. Lorsque la machine virtuelle est dans l’opération de que mémoire dynamique ne peut pas être désactivée et uniquement les paramètres Minimum et Maximum peut être modifié. (Il est recommandé de spécifier ces tailles de mémoire en multiples de 128 Mo).<br /><br />Au démarrage de la machine virtuelle est tout d’abord disponible mémoire est égale à la **mémoire de démarrage**. À mesure que la demande de mémoire augmente en raison des charges de travail Hyper-V peut allouer dynamiquement de davantage de mémoire à la machine virtuelle via le mécanisme d’ajout à chaud (ci-dessus). Lorsque la demande de mémoire diminue Hyper-V peut annuler automatiquement l’approvisionnement mémoire à partir de la machine virtuelle via le mécanisme d’info-bulle. Hyper-V ne va pas annuler l’approvisionnement mémoire ci-dessous le **mémoire minimale** paramètre.<br /><br />L’onglet de la mémoire du Gestionnaire Hyper-V Affiche la quantité de mémoire attribuée à la machine virtuelle, mais les statistiques de la mémoire au sein de la machine virtuelle affiche la quantité maximale de mémoire allouée.<br /><br />Pour plus d’informations, consultez [Hyper-V Dynamic Memory Overview](https://technet.microsoft.com/library/hh831766.aspx).<br /><br />|
|Redimensionnement de la mémoire du runtime|Un administrateur peut définir la quantité de mémoire disponible pour une machine virtuelle en cours d’opération, l’augmentation de la mémoire (« chaud ») ou décroissant il (« à chaud supprimer »). Mémoire est retournée pour Hyper-V via le pilote de l’info-bulle (voir « Dynamique mémoire – augmentation »). Le pilote de bulle gère une quantité minimale de mémoire disponible après que augmentation, appelée « floor », attribué par conséquent, la mémoire ne peut pas être inférieure à la demande actuelle ainsi que cette quantité étage. L’onglet de la mémoire du Gestionnaire Hyper-V Affiche la quantité de mémoire attribuée à la machine virtuelle, mais les statistiques de la mémoire au sein de la machine virtuelle affiche la quantité maximale de mémoire allouée. (Il est recommandé de spécifier des valeurs de mémoire en multiples de 128 Mo).|

## <a name="BKMK_Video"></a>Video

|**Fonctionnalité**|**Description**|
|-|-|
|Périphérique vidéo spécifique à Hyper-V|Cette fonctionnalité fournit des graphiques hautes performances et une résolution supérieure pour les machines virtuelles. Cet appareil ne fournit pas de fonctionnalités de Mode de Session étendu ou RemoteFX.|

## <a name="BKMK_Misc"></a>Divers

|**Fonctionnalité**|**Description**|
|-|-|
|Exchange de la paire clé/valeur (paire clé-valeur)|Cette fonctionnalité fournit une paire clé/valeur de service d’exchange (paire clé/valeur) pour les machines virtuelles. Généralement, les administrateurs utilisent le mécanisme de paire clé/valeur à lire et écrire des opérations de données personnalisé sur une machine virtuelle. Pour plus d’informations, consultez [échange de données : À l’aide de paires clé-valeur pour partager des informations entre l’hôte et l’invité sur Hyper-V](https://technet.microsoft.com/library/dn798287.aspx).|
|Interruption non masquable|Avec cette fonctionnalité, un administrateur peut émettre Non masquable interrompt masquable à une machine virtuelle. NMI est utiles pour obtenir les vidages sur incident des systèmes d’exploitation qui sont devenus ne répond pas en raison des bogues de l’application. Ces vidages sur incident peuvent être diagnostiqués après le redémarrage.|
|Copie des fichiers d’hôte à invité|Cette fonctionnalité permet des fichiers à copier à partir de l’ordinateur physique hôte pour les machines virtuelles invitées sans utiliser la carte réseau. Pour plus d’informations, consultez [services d’invité](https://technet.microsoft.com/library/dn798297(WS.11).aspx#BKMK_guest).|
|commande de lsvmbus|Cette commande Obtient les informations sur les périphériques sur le bus d’ordinateur virtuel Hyper-V VMBus semblable aux commandes des informations telles que lspci.|
|Sockets Hyper-V|Il s’agit d’un canal de communication supplémentaire entre le système d’exploitation hôte et invité. Pour charger et utiliser le module de noyau Sockets Hyper-V, consultez [apporter vos propres services d’intégration](https://msdn.microsoft.com/virtualization/hyperv_on_windows/develop/make_mgmt_service).|
|La norme PCI Passthrough/DDA|Avec Windows Server 2016 administrateurs peuvent traverser via le mécanisme d’affectation discrète d’appareils périphériques PCI Express. Périphériques courants sont les cartes réseau, les cartes graphiques et les périphériques de stockage spécial. La machine virtuelle nécessite le pilote approprié à utiliser le matériel exposé. Le matériel doit être affecté à la machine virtuelle pour pouvoir être utilisé.<br /><br />Pour plus d’informations, consultez [discrètes affectation de périphérique - Description et arrière-plan](https://blogs.technet.microsoft.com/virtualization/2015/11/19/discrete-device-assignment-description-and-background/).<br /><br />DDA est une condition préalable pour la mise en réseau SR-IOV. Ports virtuels seront nécessaire d’attribuer à la machine virtuelle et la machine virtuelle doit utiliser les pilotes Virtual fonction (VF) appropriés de multiplexage de l’appareil.|

## <a name="BKMK_gen2"></a>Ordinateurs virtuels de génération 2

|**Fonctionnalité**|**Description**|
|-|-|
|Démarrage à l’aide d’UEFI|Cette fonctionnalité permet des machines virtuelles démarrer à l’aide de Unified Extensible Firmware Interface (UEFI).<br /><br />Pour plus d’informations, voir [Vue d’ensemble d’un ordinateur virtuel de génération 2](https://technet.microsoft.com/library/dn282285.aspx).|
|Démarrage sécurisé|Cette fonctionnalité permet des machines virtuelles utiliser le mode de démarrage sécurisé UEFI en fonction. Lorsqu’un ordinateur virtuel est démarré en mode sécurisé, différents composants du système d’exploitation sont vérifiées à l’aide de signatures présents dans le magasin de données UEFI.<br /><br />Pour plus d'informations, voir [Démarrage sécurisé](https://technet.microsoft.com/library/dn486875.aspx).|

## <a name="see-also"></a>Voir aussi

* [Prise en charge de CentOS et les machines virtuelles de Red Hat Enterprise Linux sur Hyper-V](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Prise en charge des machines virtuelles Debian sur Hyper-V](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [Machines virtuelles Oracle Linux prises en charge sur Hyper-V](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [Machines virtuelles SUSE prises en charge sur Hyper-V](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [Machines virtuelles Ubuntu prises en charge sur Hyper-V](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [Machines virtuelles FreeBSD prises en charge sur Hyper-V](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Meilleures pratiques pour l’exécution de Linux sur Hyper-V](Best-Practices-for-running-Linux-on-Hyper-V.md)

* [Meilleures pratiques pour l’exécution de FreeBSD sur Hyper-V](Best-practices-for-running-FreeBSD-on-Hyper-V.md)