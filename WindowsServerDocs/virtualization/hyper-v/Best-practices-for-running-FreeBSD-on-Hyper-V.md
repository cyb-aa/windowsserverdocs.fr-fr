---
title: Meilleures pratiques pour l’exécution de FreeBSD sur Hyper-V
description: Fournit des recommandations pour l’exécution de FreeBSD sur des machines virtuelles
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0c66f1c8-2606-43a3-b4cc-166acaaf2d2a
author: shirgall
ms.author: kathydav
ms.date: 01/09/2017
ms.openlocfilehash: 1a8efb4a991169258d6a11999513ce97d98130eb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816950"
---
# <a name="best-practices-for-running-freebsd-on-hyper-v"></a>Meilleures pratiques pour l’exécution de FreeBSD sur Hyper-V

>S'applique à : Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7.1, Windows 7

Cette rubrique contient une liste de recommandations pour l’exécution de FreeBSD comme système d’exploitation invité sur un ordinateur virtuel Hyper-V.

## <a name="enable-carp-in-freebsd-102-on-hyper-v"></a>Activer le protocole CARP dans FreeBSD 10.2 sur Hyper-V

Le protocole CARP (Common adresse redondance Protocol) permet à plusieurs hôtes de partagent la même adresse IP et l’ID d’hôte virtuel (VHID) afin d’offrir une haute disponibilité pour un ou plusieurs services. Si un ou plusieurs hôtes échouent, les autres hôtes en toute transparence prennent pour les utilisateurs ne remarqueront un échec du service. Pour utiliser le protocole CARP dans FreeBSD 10.2, suivez les instructions de la [manuel de FreeBSD](https://www.freebsd.org/doc/en/books/handbook/carp.html) et procédez comme suit dans le Gestionnaire Hyper-V.

* Vérifiez que l’ordinateur virtuel possède une carte réseau et il a attribué un commutateur virtuel. Sélectionnez la machine virtuelle et sélectionnez **Actions** > **paramètres**.

![Capture d’écran de paramètres de machine virtuelle avec la carte réseau sélectionnée](media/Hyper-V_Settings_NetworkAdapter.png)

* Activer l’usurpation des adresses MAC. Pour ce faire,

   1. Sélectionnez la machine virtuelle et sélectionnez **Actions** > **paramètres**.

   2. Développez **carte réseau** et sélectionnez **fonctionnalités avancées**.

   3. Sélectionnez **l’usurpation d’activer les adresses MAC**.

## <a name="create-labels-for-disk-devices"></a>Créer des étiquettes pour les périphériques de disque

Lors du démarrage, les nœuds de l’appareil sont créés que de nouveaux appareils sont découverts. Cela peut signifier que les noms de périphérique peuvent changer lors de l’ajout de nouveaux périphériques. Si vous obtenez une erreur de montage racine lors du démarrage, vous devez créer des étiquettes pour chaque partition de l’IDE éviter les conflits et les modifications. Pour savoir comment procéder, consultez [l’étiquetage des périphériques de disque](https://www.freebsd.org/doc/handbook/geom-glabel.html). Vous trouverez ci-dessous des exemples. 

> [!IMPORTANT]
> Faites une copie de sauvegarde de votre fstab avant d’apporter des modifications.

1. Redémarrez le système en mode mono-utilisateur. Cela est possible en sélectionnant l’option de menu de démarrage 2 pour FreeBSD 10.3 + (option de 4 pour FreeBSD 8.x), ou l’exécution d’un « s - amorcer » à partir de l’invite de démarrage.

2. En mode mono-utilisateur, créez des étiquettes de la géométrie pour chacune des partitions de disque IDE répertoriées dans votre fstab (racine et échange). Voici un exemple de FreeBSD 10.3.

   ```bash
   # cat  /etc/fstab
   # Device           Mountpoint      FStype  Options   Dump   Pass#
   /dev/da0p2         /               ufs     rw        1       1
   /dev/da0p3         none            swap    sw        0       0

   # glabel  label rootfs  /dev/da0p2
   # glabel  label swap   /dev/da0p3
   # exit
   ```

   Vous trouverez des informations supplémentaires sur les étiquettes de la géométrie à : [L’étiquetage des périphériques de disque](https://www.freebsd.org/doc/handbook/geom-glabel.html).

3. Le système continue avec le démarrage de plusieurs utilisateur. Après le démarrage, modifier/etc/fstab et remplacez les noms de périphérique conventionnel, leurs étiquettes respectifs. Le/etc/fstab finale doit ressembler à ceci :

   ```
   # Device                Mountpoint      FStype  Options         Dump    Pass#
   /dev/label/rootfs       /               ufs     rw              1       1
   /dev/label/swap         none            swap    sw              0       0

   ```

4.  Le système peut maintenant être redémarré. Si tout s’est bien passé, il s’allumera normalement et montage s’affichera :
   
   ```
   # mount
   /dev/label/rootfs on / (ufs, local, journaled soft-updates)
   devfs on /dev (devfs, local, mutilabel)
   ```

## <a name="use-a-wireless-network-adapter-as-the-virtual-switch"></a>Utiliser une carte réseau sans fil en tant que le commutateur virtuel

Si le commutateur virtuel sur l’ordinateur hôte est basé sur la carte réseau sans fil, vous pouvez réduire le délai d’expiration de ARP à 60 secondes par la commande suivante. Sinon, la mise en réseau de la machine virtuelle peut-être cesser de fonctionner après un certain temps.


```
   # sysctl net.link.ether.inet.max_age=60
```


Voir aussi

* [Machines virtuelles FreeBSD prises en charge sur Hyper-V](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)
