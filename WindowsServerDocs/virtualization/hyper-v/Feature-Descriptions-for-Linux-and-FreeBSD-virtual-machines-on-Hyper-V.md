---
title: Descriptions des fonctionnalités pour les machines virtuelles Linux et FreeBSD sur Hyper-V
description: Décrit les fonctionnalités qui affectent les composants principaux tels que la mise en réseau, le stockage, la mémoire lors de l’utilisation de Linux et de FreeBSD sur une machine virtuelle
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a9ee931d-91fc-40cf-9a15-ed6fa6965cb6
author: shirgall
ms.author: kathydav
ms.date: 10/03/2016
ms.openlocfilehash: 1690230d326d7e32175ccde5da1e5fae421a76d0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366797"
---
# <a name="feature-descriptions-for-linux-and-freebsd-virtual-machines-on-hyper-v"></a>Descriptions des fonctionnalités pour les machines virtuelles Linux et FreeBSD sur Hyper-V

>S’applique à : Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7,1, Windows 7

Cet article décrit les fonctionnalités disponibles dans les composants tels que le noyau, la mise en réseau, le stockage et la mémoire lors de l’utilisation de Linux et de FreeBSD sur une machine virtuelle.

## <a name="core"></a>Standard

|**Fonctionnalité**|**Description**|
|-|-|
|Arrêt intégré|Avec cette fonctionnalité, un administrateur peut arrêter des machines virtuelles à partir du Gestionnaire Hyper-V. Pour plus d’informations, consultez [arrêt du système d’exploitation](https://technet.microsoft.com/library/dn798297(WS.11).aspx#BKMK_Shutdown).|
|Synchronisation de l’heure|Cette fonctionnalité garantit que la durée de conservation au sein d’une machine virtuelle reste synchronisée avec l’heure de la maintenance de l’ordinateur hôte. Pour plus d’informations, consultez synchronisation de l' [heure](https://technet.microsoft.com/library/dn798297(WS.11).aspx#BKMK_time).|
|Heure précise de Windows Server 2016|Cette fonctionnalité permet à l’invité d’utiliser la fonctionnalité de temps précis de Windows Server 2016, ce qui améliore la synchronisation de l’heure avec l’hôte à 1 ms de précision. Pour plus d’informations, consultez l' [heure précise de Windows Server 2016](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-2016-accurate-time)|
|Prise en charge du multitraitement|Avec cette fonctionnalité, un ordinateur virtuel peut utiliser plusieurs processeurs sur l’ordinateur hôte en configurant plusieurs processeurs virtuels.|
|Pulsations|Avec cette fonctionnalité, l’hôte peut suivre l’état de l’ordinateur virtuel. Pour plus d’informations, consultez [pulsation](https://technet.microsoft.com/library/dn798297(WS.11).aspx#BKMK_heartbeat).|
|Prise en charge intégrée de la souris|Avec cette fonctionnalité, vous pouvez utiliser une souris sur le Bureau d’un ordinateur virtuel et utiliser la souris de manière transparente entre le bureau Windows Server et la console Hyper-V pour la machine virtuelle.|
|Périphérique de stockage spécifique à Hyper-V|Cette fonctionnalité accorde un accès à hautes performances aux périphériques de stockage attachés à une machine virtuelle.|
|Périphérique réseau spécifique à Hyper-V|Cette fonctionnalité accorde un accès à hautes performances aux cartes réseau attachées à une machine virtuelle.|

## <a name="networking"></a>Mise en réseau

|**Fonctionnalité**|**Description**|
|-|-|
|Trames Jumbo|Avec cette fonctionnalité, un administrateur peut augmenter la taille des trames réseau au-delà de 1500 octets, ce qui augmente considérablement les performances du réseau.|
|Balisage et Trunking VLAN|Cette fonctionnalité vous permet de configurer le trafic de réseau local virtuel (VLAN) pour les machines virtuelles.|
|Migration dynamique|Avec cette fonctionnalité, vous pouvez migrer un ordinateur virtuel d’un ordinateur hôte vers un autre. Pour plus d’informations, voir [vue d’ensemble de la migration dynamique d’une machine virtuelle](https://technet.microsoft.com/library/hh831435.aspx).|
|Injection d’adresses IP statiques|Avec cette fonctionnalité, vous pouvez répliquer l’adresse IP statique d’une machine virtuelle après qu’elle a été basculée vers son réplica sur un autre hôte. Cette réplication IP garantit que les charges de travail réseau continuent de fonctionner de manière transparente après un événement de basculement.|
|vRSS (mise à l’échelle côté réception virtuelle)|Répartit la charge à partir d’une carte réseau virtuelle sur plusieurs processeurs virtuels dans un ordinateur virtuel. Pour plus d’informations, consultez [mise à l’échelle côté réception virtuelle dans Windows Server 2012 R2](https://technet.microsoft.com/library/dn383582.aspx).|
|Segmentation TCP et déchargements de somme de contrôle|Transfère le travail de segmentation et de somme de contrôle de l’UC invité au commutateur virtuel ou à la carte réseau de l’hôte pendant les transferts de données réseau.|
|Déchargement de réception volumineux (LRO)|Augmente le débit entrant des connexions à bande passante élevée en regroupant plusieurs paquets dans une mémoire tampon plus grande, ce qui réduit la surcharge du processeur.|
|SR-IOV|Les périphériques d’e/s d’une racine unique utilisent DDA pour permettre aux invités d’accéder à des portions de cartes réseau spécifiques, ce qui permet une latence réduite et un débit accru. SR-IOV nécessite des pilotes de fonction physique à jour (PF) sur l’hôte et les pilotes de fonction virtuelle (VF) sur l’invité.|

## <a name="storage"></a>Stockage

|**Fonctionnalité**|**Description**|
|-|-|
|Redimensionnement VHDX|Avec cette fonctionnalité, un administrateur peut redimensionner un fichier. vhdx de taille fixe attaché à un ordinateur virtuel. Pour plus d’informations, consultez [vue d’ensemble du redimensionnement de disque dur virtuel en ligne](https://technet.microsoft.com/library/dn282286.aspx).|
|Fibre Channel virtuel|Avec cette fonctionnalité, les machines virtuelles peuvent reconnaître un appareil Fiber Channel et le monter en mode natif. Pour plus d’informations, voir [Vue d’ensemble de Fibre Channel virtuel Hyper-V](https://technet.microsoft.com/library/hh831413.aspx).|
|Sauvegarde de machine virtuelle en direct|Cette fonctionnalité facilite la sauvegarde de zéro des machines virtuelles en temps réel.<br /><br />Notez que l’opération de sauvegarde échoue si l’ordinateur virtuel possède des disques durs virtuels (VHD) qui sont hébergés sur un stockage distant, tel qu’un partage SMB (Server Message Block) ou un volume iSCSI. En outre, assurez-vous que la cible de sauvegarde ne se trouve pas sur le même volume que le volume que vous sauvegardez.|
|SUPPRIMER la prise en charge|Les indicateurs de découpage informent le lecteur que certains secteurs précédemment alloués ne sont plus requis par l’application et peuvent être purgés. Ce processus est généralement utilisé quand une application effectue des allocations d’espace importantes par le biais d’un fichier, puis gère automatiquement les allocations dans le fichier, par exemple, pour les fichiers de disque dur virtuel.|
|WWN SCSI|Le pilote storvsc extrait les informations WWN du port et du nœud des appareils attachés à l’ordinateur virtuel et crée les fichiers sysfs appropriés. |

## <a name="memory"></a>Mémoire

|**Fonctionnalité**|**Description**|
|-|-|
|Prise en charge du noyau PAE|La technologie d’extension d’adresse physique (PAE) permet à un noyau 32 bits d’accéder à un espace d’adressage physique supérieur à 4 Go. Les distributions Linux plus anciennes, telles que RHEL 5. x, ont été utilisées pour livrer un noyau distinct pour lequel PAE est activé. Les distributions plus récentes, telles que RHEL 6. x, prennent en charge les PAE prédéfinies.|
|Configuration de l’intervalle MMIO|Avec cette fonctionnalité, les fabricants d’appliances peuvent configurer l’emplacement de l’écart d’e/s mappées en mémoire (MMIO). L’intervalle MMIO est généralement utilisé pour diviser la mémoire physique disponible entre les systèmes d’exploitation (JeOS) d’une appliance et l’infrastructure logicielle réelle qui alimente l’appliance.|
|Mémoire dynamique-ajout à chaud|L’hôte peut augmenter ou diminuer dynamiquement la quantité de mémoire disponible sur un ordinateur virtuel pendant qu’il est en cours d’exécution. Avant l’approvisionnement, l’administrateur Active Mémoire dynamique dans le panneau Paramètres de l’ordinateur virtuel et spécifier la mémoire de démarrage, la mémoire minimale et la mémoire maximale pour l’ordinateur virtuel. Lorsque la machine virtuelle est en cours d’opération Mémoire dynamique ne peut pas être désactivée et seuls les paramètres minimal et maximal peuvent être modifiés. (Il est recommandé de spécifier ces tailles de mémoire en tant que multiples de 128 Mo.)<br /><br />Lorsque la machine virtuelle est démarrée pour la première fois, la mémoire disponible est égale à la **mémoire de démarrage**. À mesure que la demande de mémoire augmente en raison des charges de travail d’application, Hyper-V peut allouer dynamiquement plus de mémoire à la machine virtuelle via le mécanisme d’ajout à chaud, s’il est pris en charge par cette version du noyau. La quantité maximale de mémoire allouée est limitée par la valeur du paramètre de **mémoire maximale** .<br /><br />L’onglet mémoire du Gestionnaire Hyper-V affiche la quantité de mémoire affectée à l’ordinateur virtuel, mais les statistiques de mémoire de l’ordinateur virtuel affichent la quantité de mémoire maximale allouée.<br /><br />Pour plus d’informations, consultez [vue d’ensemble d’Hyper-V mémoire dynamique](https://technet.microsoft.com/library/hh831766.aspx).<br /><br />|
|Mémoire dynamique-bulles|L’hôte peut augmenter ou diminuer dynamiquement la quantité de mémoire disponible sur un ordinateur virtuel pendant qu’il est en cours d’exécution. Avant l’approvisionnement, l’administrateur Active Mémoire dynamique dans le panneau Paramètres de l’ordinateur virtuel et spécifier la mémoire de démarrage, la mémoire minimale et la mémoire maximale pour l’ordinateur virtuel. Lorsque la machine virtuelle est en cours d’opération Mémoire dynamique ne peut pas être désactivée et seuls les paramètres minimal et maximal peuvent être modifiés. (Il est recommandé de spécifier ces tailles de mémoire en tant que multiples de 128 Mo.)<br /><br />Lorsque la machine virtuelle est démarrée pour la première fois, la mémoire disponible est égale à la **mémoire de démarrage**. À mesure que la demande de mémoire augmente en raison des charges de travail d’application, Hyper-V peut allouer dynamiquement plus de mémoire à la machine virtuelle via le mécanisme d’ajout à chaud (ci-dessus). À mesure que la demande de mémoire diminue, Hyper-V peut annuler automatiquement la configuration de la mémoire à partir de la machine virtuelle via le mécanisme de bulle. Hyper-V n’annule pas l’approvisionnement de la mémoire sous le paramètre de **mémoire minimal** .<br /><br />L’onglet mémoire du Gestionnaire Hyper-V affiche la quantité de mémoire affectée à l’ordinateur virtuel, mais les statistiques de mémoire de l’ordinateur virtuel affichent la quantité de mémoire maximale allouée.<br /><br />Pour plus d’informations, consultez [vue d’ensemble d’Hyper-V mémoire dynamique](https://technet.microsoft.com/library/hh831766.aspx).<br /><br />|
|Redimensionnement de la mémoire d’exécution|Un administrateur peut définir la quantité de mémoire disponible pour un ordinateur virtuel pendant qu’il est en cours d’exécution, en renforçant la mémoire (« ajout à chaud ») ou en le diminuant (« suppression à chaud »). La mémoire est retournée à Hyper-V par le biais du pilote de ballonnet (voir « Mémoire dynamique-bulles »). Le pilote de la bulle maintient une quantité minimale de mémoire disponible après la bulle, appelée « plancher », de sorte que la mémoire affectée ne peut pas être réduite en dessous de la demande actuelle plus cette quantité d’étage. L’onglet mémoire du Gestionnaire Hyper-V affiche la quantité de mémoire affectée à l’ordinateur virtuel, mais les statistiques de mémoire de l’ordinateur virtuel affichent la quantité de mémoire maximale allouée. (Il est recommandé de spécifier des valeurs de mémoire en tant que multiples de 128 Mo.)|

## <a name="video"></a>Video

|**Fonctionnalité**|**Description**|
|-|-|
|Périphérique vidéo spécifique à Hyper-V|Cette fonctionnalité offre des fonctionnalités graphiques hautes performances et une résolution supérieure pour les machines virtuelles. Cet appareil ne fournit pas de mode de session étendu ni de fonctionnalités RemoteFX.|

## <a name="miscellaneous"></a>Divers

|**Fonctionnalité**|**Description**|
|-|-|
|Exchange KVP (paire clé-valeur)|Cette fonctionnalité fournit un service d’échange de paires clé/valeur (KVP) pour les ordinateurs virtuels. En général, les administrateurs utilisent le mécanisme KVP pour effectuer des opérations de lecture et d’écriture de données personnalisées sur un ordinateur virtuel. Pour plus d’informations, consultez [échange de données : utilisation de paires clé-valeur pour partager des informations entre l’hôte et l’invité sur Hyper-V](https://technet.microsoft.com/library/dn798287.aspx).|
|Interruption non masquable|Avec cette fonctionnalité, un administrateur peut émettre des interruptions non masquables (NMI) sur un ordinateur virtuel. Les NMIs sont utiles pour obtenir des vidages sur incident des systèmes d’exploitation qui ont cessé de répondre en raison de bogues d’application. Ces vidages sur incident peuvent être diagnostiqués après le redémarrage.|
|Copie de fichiers de l’hôte vers l’invité|Cette fonctionnalité permet de copier des fichiers de l’ordinateur physique hôte vers les machines virtuelles invitées sans utiliser la carte réseau. Pour plus d’informations, consultez [services invités](https://technet.microsoft.com/library/dn798297(WS.11).aspx#BKMK_guest).|
|commande lsvmbus|Cette commande obtient des informations sur les appareils sur le bus VMBus Hyper-V, de la même façon que les commandes d’informations telles que lspci.|
|Sockets Hyper-V|Il s’agit d’un canal de communication supplémentaire entre l’hôte et le système d’exploitation invité. Pour charger et utiliser le module noyau des sockets Hyper-V, consultez [créer vos propres services d’intégration](https://msdn.microsoft.com/virtualization/hyperv_on_windows/develop/make_mgmt_service).|
|Passthrough/DDA PCI|Avec Windows Server 2016, les administrateurs peuvent passer par des périphériques PCI Express via le mécanisme d’attribution d’appareils discret. Les périphériques courants sont les cartes réseau, les cartes graphiques et les dispositifs de stockage spéciaux. L’ordinateur virtuel nécessite que le pilote approprié utilise le matériel exposé. Le matériel doit être attribué à l’ordinateur virtuel pour pouvoir être utilisé.<br /><br />Pour plus d’informations, consultez [attribution d’appareil discrète-Description et arrière-plan](https://blogs.technet.microsoft.com/virtualization/2015/11/19/discrete-device-assignment-description-and-background/).<br /><br />DDA est une condition préalable à la mise en réseau SR-IOV. Les ports virtuels doivent être affectés à la machine virtuelle et l’ordinateur virtuel doit utiliser les pilotes de fonction virtuelle (VF) appropriés pour le multiplexage de l’appareil.|

## <a name="generation-2-virtual-machines"></a>Ordinateurs virtuels de génération 2

|**Fonctionnalité**|**Description**|
|-|-|
|Démarrer à l’aide d’UEFI|Cette fonctionnalité permet aux machines virtuelles de démarrer à l’aide d’Unified Extensible Firmware Interface (UEFI).<br /><br />Pour plus d’informations, voir [Vue d’ensemble d’un ordinateur virtuel de génération 2](https://technet.microsoft.com/library/dn282285.aspx).|
|Démarrage sécurisé|Cette fonctionnalité permet aux machines virtuelles d’utiliser le mode de démarrage sécurisé UEFI. Lorsqu’un ordinateur virtuel est démarré en mode sécurisé, différents composants du système d’exploitation sont vérifiés à l’aide des signatures présentes dans le magasin de données UEFI.<br /><br />Pour plus d'informations, voir [Démarrage sécurisé](https://technet.microsoft.com/library/dn486875.aspx).|

## <a name="see-also"></a>Voir aussi

* [Ordinateurs virtuels CentOS et Red Hat Enterprise Linux pris en charge sur Hyper-V](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Machines virtuelles Debian prises en charge sur Hyper-V](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [Ordinateurs virtuels Oracle Linux pris en charge sur Hyper-V](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [Ordinateurs virtuels SUSE pris en charge sur Hyper-V](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [Machines virtuelles Ubuntu prises en charge sur Hyper-V](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [Ordinateurs virtuels FreeBSD pris en charge sur Hyper-V](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Meilleures pratiques pour l’exécution de Linux sur Hyper-V](Best-Practices-for-running-Linux-on-Hyper-V.md)

* [Meilleures pratiques pour exécuter FreeBSD sur Hyper-V](Best-practices-for-running-FreeBSD-on-Hyper-V.md)