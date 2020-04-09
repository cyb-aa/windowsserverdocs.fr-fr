---
title: Meilleures pratiques pour exécuter FreeBSD sur Hyper-V
description: Fournit des recommandations pour l’exécution de FreeBSD sur des machines virtuelles
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.topic: article
ms.assetid: 0c66f1c8-2606-43a3-b4cc-166acaaf2d2a
author: shirgall
ms.author: kathydav
ms.date: 01/09/2017
ms.openlocfilehash: 18f59020ed4878e9a54150dcda18bca3da1dd614
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853282"
---
# <a name="best-practices-for-running-freebsd-on-hyper-v"></a>Meilleures pratiques pour exécuter FreeBSD sur Hyper-V

>S’applique à : Windows Server 2019, Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7,1, Windows 7

Cette rubrique contient une liste de recommandations pour l’exécution de FreeBSD en tant que système d’exploitation invité sur un ordinateur virtuel Hyper-V.

## <a name="enable-carp-in-freebsd-102-on-hyper-v"></a>Activer le protocole CARP dans FreeBSD 10,2 sur Hyper-V

Le protocole CARP (Common Address Redundancy Protocol) permet à plusieurs hôtes de partager la même adresse IP et l’ID d’hôte virtuel (VHID) pour fournir une haute disponibilité pour un ou plusieurs services. Si un ou plusieurs ordinateurs hôtes échouent, les autres hôtes prennent le relais de manière transparente afin que les utilisateurs ne remarquent pas un échec du service. Pour utiliser le protocole CARP dans FreeBSD 10,2, suivez les instructions du [Manuel FreeBSD](https://www.freebsd.org/doc/en/books/handbook/carp.html) et procédez comme suit dans le Gestionnaire Hyper-V.

* Vérifiez que la machine virtuelle dispose d’une carte réseau et qu’elle est dotée d’un commutateur virtuel. Sélectionnez la machine virtuelle et sélectionnez **Actions** > **paramètres**.

![Capture d’écran des paramètres de machine virtuelle avec la carte réseau sélectionnée](media/Hyper-V_Settings_NetworkAdapter.png)

* Activez l’usurpation d’adresses MAC. Pour ce faire,

   1. Sélectionnez la machine virtuelle et sélectionnez **Actions** > **paramètres**.

   2. Développez **carte réseau** et sélectionnez **fonctionnalités avancées**.

   3. Sélectionnez **activer l’usurpation d’adresses Mac**.

## <a name="create-labels-for-disk-devices"></a>Créer des étiquettes pour les périphériques de disque

Au démarrage, les nœuds d’appareil sont créés à mesure que de nouveaux périphériques sont découverts. Cela peut signifier que les noms d’appareils peuvent changer lors de l’ajout de nouveaux appareils. Si vous recevez une erreur de montage racine au démarrage, vous devez créer des étiquettes pour chaque partition IDE afin d’éviter les conflits et les modifications. Pour savoir comment procéder, consultez [étiquetage des périphériques de disque](https://www.freebsd.org/doc/handbook/geom-glabel.html). Vous trouverez ci-dessous des exemples. 

> [!IMPORTANT]
> Effectuez une copie de sauvegarde de votre fstab avant d’apporter des modifications.

1. Redémarrez le système en mode mono-utilisateur. Pour ce faire, vous pouvez sélectionner l’option de menu de démarrage 2 pour FreeBSD 10.3 + (option 4 pour FreeBSD 8. x) ou exécuter un’boot-s’à partir de l’invite de démarrage.

2. En mode mono-utilisateur, créez des étiquettes GEOM pour chacune des partitions de disque IDE listées dans votre fstab (à la fois racine et échange). Vous trouverez ci-dessous un exemple de FreeBSD 10,3.

   ```bash
   # cat  /etc/fstab
   # Device           Mountpoint      FStype  Options   Dump   Pass#
   /dev/da0p2         /               ufs     rw        1       1
   /dev/da0p3         none            swap    sw        0       0

   # glabel  label rootfs  /dev/da0p2
   # glabel  label swap   /dev/da0p3
   # exit
   ```

   Vous trouverez des informations supplémentaires sur les étiquettes GEOM à l’adresse suivante : [étiquetage des périphériques de disque](https://www.freebsd.org/doc/handbook/geom-glabel.html).

3. Le système se poursuit avec le démarrage multi-utilisateur. Une fois le démarrage terminé, modifiez/etc/fstab et remplacez les noms des appareils conventionnels par leurs étiquettes respectives. Le/etc/fstab final se présente comme suit :

   ```
   # Device                Mountpoint      FStype  Options         Dump    Pass#
   /dev/label/rootfs       /               ufs     rw              1       1
   /dev/label/swap         none            swap    sw              0       0
   ```

4. Le système peut maintenant être redémarré. Si tout s’est bien passé, le montage sera normal et le montage affichera les éléments suivants :

   ```
   # mount
   /dev/label/rootfs on / (ufs, local, journaled soft-updates)
   devfs on /dev (devfs, local, mutilabel)
   ```

## <a name="use-a-wireless-network-adapter-as-the-virtual-switch"></a>Utiliser une carte réseau sans fil comme commutateur virtuel

Si le commutateur virtuel sur l’ordinateur hôte est basé sur une carte réseau sans fil, réduisez le délai d’expiration ARP à 60 secondes à l’aide de la commande suivante. Sinon, la mise en réseau de la machine virtuelle peut cesser de fonctionner après un certain temps.


```
   # sysctl net.link.ether.inet.max_age=60
```


Voir aussi

* [Ordinateurs virtuels FreeBSD pris en charge sur Hyper-V](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)
