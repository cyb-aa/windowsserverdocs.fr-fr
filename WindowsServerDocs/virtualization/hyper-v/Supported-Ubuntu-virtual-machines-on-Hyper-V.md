---
title: Machines virtuelles Ubuntu prises en charge sur Hyper-V
description: Répertorie les fonctionnalités incluses dans chaque version et les services d’intégration Linux
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 95ea5f7c-25c6-494b-8ffd-2a77f631ee94
author: shirgall
ms.author: shirgall
ms.date: 11/19/2018
ms.openlocfilehash: b58193ec570cf0d94b6c95018b8c00c813331986
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222640"
---
# <a name="supported-ubuntu-virtual-machines-on-hyper-v"></a>Machines virtuelles Ubuntu prises en charge sur Hyper-V

>S'applique à : Windows Server 2019, 2016, Hyper-V Server 2019, 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7.1, Windows 7

À compter de Ubuntu 12.04, le chargement du package « linux virtuels » installe un noyau pouvant être utilisée comme une machine virtuelle invitée. Ce package dépend toujours de la dernière image minimal de noyaux générique et les en-têtes utilisés pour les machines virtuelles. Même si son utilisation est facultative, le noyau de l’ordinateur virtuel linux charge moins de pilotes et peut démarrer plus rapidement et ont moins de mémoire de traitement qu’une image générique.

Pour bénéficier pleinement de Hyper-V, installez les linux-outils appropriés et les packages de cloud-linux-tools pour installer des outils et processus pour une utilisation avec des machines virtuelles. Lorsque vous utilisez le noyau linux-virtuel, charger virtuelles-linux-outils et linux-cloud-tools-virtuel.

Le plan de distribution de fonctionnalité suivant indique les fonctionnalités de chaque version. Les problèmes connus et les solutions de contournement pour chaque distribution sont répertoriées après le tableau.

## <a name="table-legend"></a>Légende du tableau

* **Intégrées** -LIS sont inclus dans le cadre de cette distribution Linux. Le package de téléchargement LIS fournie par Microsoft ne fonctionne pas pour cette distribution, afin de ne pas l’installer. Les numéros de version de module de noyau pour le LIS intégré (comme indiqué par **lsmod**, par exemple) sont différents du numéro de version sur le package de téléchargement LIS fournie par Microsoft. Une incompatibilité n’indique pas qu’intégrée dans les LIS est obsolète.

* &#10004;-Fonctionnalité disponible

* (*vide*)-fonctionnalité non disponible

|**Fonctionnalité**|**Version du système d’exploitation Windows Server**|**18.10**|**18.04 LTS**|**16.04 LTS**|**14.04 LTS**|**12.04 LTS**|
|-|-|-|-|-|-|-|
|**Disponibilité**||Intégrée|Intégrée|Intégrée|Intégrée|Intégrée|
|**[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Heure précise de Windows Server 2016|2019, 2016|&#10004;|&#10004;|&#10004;|||
|**[Mise en réseau](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**|||||||
|Trames Jumbo|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Un marquage VLAN et trunking|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Migration en direct|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Injection de l’adresse IP statique|2019, 2016, 2012 R2, 2012|&#10004; Note 1|&#10004; Note 1|&#10004; Note 1|&#10004; Note 1|&#10004; Note 1|
|vRSS|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|Segmentation de TCP et les déchargements de somme de contrôle|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|SR-IOV|2019, 2016|&#10004;|&#10004;|&#10004;|||
|**[Stockage](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**||||||
|Redimensionnement VHDX|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|Fibre Channel virtuel|2019, 2016, 2012 R2|&#10004; Note 2|&#10004; Note 2|&#10004; Note 2|&#10004; Note 2||
|Sauvegarde de machine virtuelle active|2019, 2016, 2012 R2|&#10004; Note 3, 4, 6|&#10004; Note 3, 4, 5|&#10004; Note 3, 4, 5|&#10004; Note 3, 4, 5||
|Prise en charge de TRIM|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|SCSI WWN|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|**[Mémoire](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**||||||
|Prise en charge PAE du noyau|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Configuration de l’écart MMIO|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Mémoire dynamique - ajout à chaud|2019, 2016, 2012 R2, 2012|&#10004; Note 7, 8, 9|&#10004; Note 7, 8, 9|&#10004; Note 7, 8, 9|&#10004; Note 7, 8, 9||
|Mémoire dynamique - augmentation de la capacité|2019, 2016, 2012 R2, 2012|&#10004; Note 7, 8, 9|&#10004; Note 7, 8, 9|&#10004; Note 7, 8, 9|&#10004; Note 7, 8, 9||
|Redimensionnement de la mémoire du runtime|2019, 2016|&#10004;|&#10004;|&#10004;|&#10004;||
|**[Video](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**|||||||
|Périphérique vidéo spécifique Hyper-V|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|**[Divers](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**||||||
|Paire clé/valeur|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004; Note 6, 10|&#10004; Note 5, 10|&#10004; Note 5, 10|&#10004; Note 5, 10|&#10004; Note 5, 10|
|Interruption non masquable|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Copie des fichiers d’hôte à invité|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|commande de lsvmbus|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|Sockets Hyper-V|2019, 2016||||||
|La norme PCI Passthrough/DDA|2019, 2016|&#10004;|&#10004;|&#10004;|&#10004;||
|**[Ordinateurs virtuels de génération 2](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)**||||||
|Démarrage à l’aide d’UEFI|2019, 2016, 2012 R2|&#10004; Note 11, 12|&#10004; Note 11, 12|&#10004; Note 11, 12|&#10004; Note 11, 12||
|Démarrage sécurisé|2019, 2016|&#10004;|&#10004;|&#10004;|&#10004;||

## <a name="notes"></a>Notes

1. Injection d’adresse IP statique peut ne pas fonctionne si **Gestionnaire de réseau** a été configuré pour une carte de réseau spécifiques à Hyper-V donnée sur la machine virtuelle. Pour garantir le bon fonctionnement de l’adresse IP statique injection Veuillez vous assurer que le Gestionnaire de réseau est complètement arrêtée ou a été désactivé pour une carte réseau spécifique via son **ifcfg-ethX** fichier.

2. Lors de l’utilisation de dispositifs de virtual Fibre channel, assurez-vous que le numéro d’unité logique (LUN 0) de 0 a été remplie. Si le LUN 0 n’a pas été rempli, une machine virtuelle Linux ne peut pas être en mesure de monter les périphériques Fibre channel en mode natif.

3. Si sont ouverts descripteurs de fichiers lors d’une opération de sauvegarde de machine virtuelle active, dans certains cas extrêmes, les disques durs virtuels sauvegardés peuvent devoir être soumis à une vérification de cohérence de système de fichier (`fsck`) lors de la restauration.

4. Les opérations de sauvegarde en direct peuvent échouer en mode silencieux si l’ordinateur virtuel possède un périphérique iSCSI attaché ou le stockage en attachement direct (également appelé un disque direct).

5. Prise en charge à long terme de versions (LTS) destiné à Linux Integration Services à jour plus récente du noyau activation de matériel (Hardware Enablement) virtuel.

   Pour installer le noyau Azure réglée sur 14.04, 16.04 et 18.04, exécutez les commandes suivantes en tant que racine (ou sudo) :

   ```bash
   # apt-get update
   # apt-get install linux-azure

   ```

   12.04 n’a pas d’un noyau virtuel distinct. Pour installer le noyau Enablement générique sur 12.04, exécutez les commandes suivantes en tant que racine (ou sudo) :

   ```bash
   # apt-get update
   # apt-get install linux-generic-lts-trusty

   ```

   Sur Ubuntu 12.04 les démons Hyper-V suivants sont dans un package installé séparément :

   * **Démon d’instantané VSS** -ce démon est nécessaire pour créer des sauvegardes dynamiques de la machine virtuelle Linux.
   * **Démon KVP** -ce démon permet de définir et d’interroger les paires clé-valeur intrinsèques et extrinsèques.
   * **fcopy démon** -ce démon implémente un service entre l’hôte et l’invité de copie de fichiers.

   Pour installer le démon KVP sur 12.04, exécutez les commandes suivantes en tant que racine (ou sudo).

   ```bash
   # apt-get install hv-kvp-daemon-init linux-tools-lts-trusty linux-cloud-tools-generic-lts-trusty

   ```

   Chaque fois que le noyau est mis à jour, la machine virtuelle doit être redémarrée pour l’utiliser.

6. Sur Ubuntu 18.10, utilisez le dernier noyau virtuel pour disposer de fonctionnalités d’Hyper-V à jour.

   Pour installer le noyau virtual sur 18.10, exécutez les commandes suivantes en tant que racine (ou sudo) :

   ```bash
   # apt-get update
   # apt-get install linux-azure

   ```

   Chaque fois que le noyau est mis à jour, la machine virtuelle doit être redémarrée pour l’utiliser.

7. Prise en charge de la mémoire dynamique est uniquement disponible sur les ordinateurs virtuels 64 bits.

8. Les opérations de mémoire dynamiques peuvent échouer si le système d’exploitation invité s’exécute trop faible de la mémoire. Voici quelques-unes des meilleures pratiques :

   * Mémoire de démarrage et la quantité minimale de mémoire doivent être égale ou supérieure à la quantité de mémoire qui recommande le fournisseur de distribution.

   * Les applications qui ont tendance à consommer la totalité de la mémoire disponible sur un système sont limitées à consommer jusqu'à 80 % de mémoire vive disponible.

9. Si vous utilisez la mémoire dynamique sur Windows Server 2019, Windows Server 2016 ou systèmes d’exploitation Windows Server 2012/2012 R2, spécifiez **mémoire de démarrage**, **mémoire minimale**, et **maximale mémoire** paramètres en multiples de 128 mégaoctets (Mo). Cela peut entraîner des échecs ajout à chaud, et vous verrez ne peut-être pas de mémoire augmenter sur un système d’exploitation.

10. Dans Windows Server 2019, Windows Server 2016 ou Windows Server 2012 R2, l’infrastructure de la paire clé/valeur ne pas fonctionne correctement sans une mise à jour de logiciels Linux. Contactez votre fournisseur de distribution pour obtenir la mise à jour logicielle dans le cas où vous voyez des problèmes liés à cette fonctionnalité.

11. Sur Windows Server 2012 R2, les machines virtuelles de génération 2 ont le démarrage sécurisé est activé par défaut et certains Linux de machines virtuelles ne démarre pas, sauf si l’option de démarrage sécurisé est désactivée. Vous pouvez désactiver le démarrage sécurisé dans le **microprogramme** section des paramètres de la machine virtuelle dans **Gestionnaire Hyper-V** ou vous pouvez le désactiver à l’aide de Powershell :

   ```Powershell
   Set-VMFirmware -VMName "VMname" -EnableSecureBoot Off

   ```

12. Avant de tenter de copier le disque dur virtuel d’une machine virtuelle de disque dur virtuel de génération 2 pour créer des machines virtuelles de génération 2, procédez comme suit :

   1. Connectez-vous à la machine virtuelle de génération 2.

   2. Basculez vers le répertoire EFI de démarrage :

      ```bash
      # cd /boot/efi/EFI

      ```

   3. Copiez le répertoire d’ubuntu dans vers un répertoire nommé démarrage :

      ```bash
      # sudo cp -r ubuntu/ boot

      ```

   4. Accédez au répertoire dans le répertoire de démarrage nouvellement créée :

      ```bash
      # cd boot

      ```

   5. Renommez le fichier shimx64.efi :

      ```bash
      # sudo mv shimx64.efi bootx64.efi

      ```

## <a name="see-also"></a>Voir aussi

* [Prise en charge de CentOS et les machines virtuelles de Red Hat Enterprise Linux sur Hyper-V](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Machines virtuelles Debian prises en charge sur Hyper-V](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [Machines virtuelles Oracle Linux prises en charge sur Hyper-V](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [Machines virtuelles SUSE prises en charge sur Hyper-V](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [Descriptions des fonctionnalités pour les machines virtuelles Linux et FreeBSD sur Hyper-V](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Meilleures pratiques pour l’exécution de Linux sur Hyper-V](Best-Practices-for-running-Linux-on-Hyper-V.md)

* [Set-VMFirmware](https://technet.microsoft.com/library/dn464287.aspx)

* [Ubuntu 14.04 dans une génération 2 machines virtuelles - Blog de virtualisation de Ben Armstrong](https://blogs.msdn.com/b/virtual_pc_guy/archive/2014/06/09/ubuntu-14-04-in-a-generation-2-vm.aspx)
