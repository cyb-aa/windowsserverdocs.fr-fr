---
title: Machines virtuelles Ubuntu prises en charge sur Hyper-V
description: Répertorie les fonctionnalités et les services d’intégration Linux inclus dans chaque version
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.topic: article
ms.assetid: 95ea5f7c-25c6-494b-8ffd-2a77f631ee94
author: shirgall
ms.author: shirgall
ms.date: 04/08/2020
ms.openlocfilehash: 541f34e11146715fc54017dc3fb0d831cb4e078e
ms.sourcegitcommit: 7b1ebc4934998af2472962ca8cce1c872f39946f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80994497"
---
# <a name="supported-ubuntu-virtual-machines-on-hyper-v"></a>Machines virtuelles Ubuntu prises en charge sur Hyper-V

>S’applique à : Windows Server 2019, Hyper-V Server 2019, Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows 10, Windows 8.1

La carte de distribution des fonctionnalités suivante indique les fonctionnalités de chaque version. Les problèmes connus et les solutions de contournement pour chaque distribution sont répertoriés après le tableau.

## <a name="table-legend"></a>Légende de la table

* Les lis intégrés sont inclus **dans** le cadre de cette distribution Linux. Le package de téléchargement de LIS fourni par Microsoft ne fonctionne pas pour cette distribution. vous ne devez donc pas l’installer. Les numéros de version du module de noyau pour la LIS intégrée (comme indiqué par **lsmod**, par exemple) sont différents du numéro de version du package de téléchargement lis fourni par Microsoft. Une incompatibilité n’indique pas que la LIS intégrée est obsolète.

* &#10004;-Fonctionnalité disponible

* (*vide*)-fonctionnalité non disponible

|**Fonctionnalité**|**Version du système d’exploitation Windows Server**|**19,10**|**18,04 LTS**|**16,04 LTS**|**14,04 LTS**|
|-|-|-|-|-|-|
|**Disponibilité**||Intégrée|Intégrée|Intégrée|Intégrée|
|**[Ebauche](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|Heure précise de Windows Server 2016|2019, 2016|&#10004;|&#10004;|&#10004;||
|**[Mise](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**||||||
|Trames Jumbo|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|Balisage et Trunking VLAN|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|Migration dynamique|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|Injection d’adresses IP statiques|2019, 2016, 2012 R2|&#10004;Remarque 1|&#10004;Remarque 1|&#10004;Remarque 1|&#10004;Remarque 1|
|vRSS|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|Segmentation TCP et déchargements de somme de contrôle|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|SR-IOV|2019, 2016|&#10004;|&#10004;|&#10004;||
|**[Rangement](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**|||||
|Redimensionnement VHDX|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|Fibre Channel virtuel|2019, 2016, 2012 R2|&#10004;Remarque 2|&#10004;Remarque 2|&#10004;Remarque 2|&#10004;Remarque 2|
|Sauvegarde de machine virtuelle en direct|2019, 2016, 2012 R2|&#10004;Remarque 3, 4, 6|&#10004;Remarque 3, 4, 5|&#10004;Remarque 3, 4, 5|&#10004;Remarque 3, 4, 5|
|SUPPRIMER la prise en charge|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|WWN SCSI|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|**[Capacité](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**|||||
|Prise en charge du noyau PAE|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|Configuration de l’intervalle MMIO|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|Mémoire dynamique-ajout à chaud|2019, 2016, 2012 R2|&#10004;Remarque 7, 8, 9|&#10004;Remarque 7, 8, 9|&#10004;Remarque 7, 8, 9|&#10004;Remarque 7, 8, 9|
|Mémoire dynamique-bulles|2019, 2016, 2012 R2|&#10004;Remarque 7, 8, 9|&#10004;Remarque 7, 8, 9|&#10004;Remarque 7, 8, 9|&#10004;Remarque 7, 8, 9|
|Redimensionnement de la mémoire d’exécution|2019, 2016|&#10004;|&#10004;|&#10004;|&#10004;|
|**[Vidéosurveillance](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**||||||
|Périphérique vidéo spécifique à Hyper-V|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|**[Diverses](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**|||||
|Paire clé/valeur|2019, 2016, 2012 R2|&#10004;Remarque 6, 10|&#10004;Remarque : 5, 10|&#10004;Remarque : 5, 10|&#10004;Remarque : 5, 10|
|Interruption non masquable|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|Copie de fichiers de l’hôte vers l’invité|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|commande lsvmbus|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|Sockets Hyper-V|2019, 2016|||||
|Passthrough/DDA PCI|2019, 2016|&#10004;|&#10004;|&#10004;|&#10004;|
|**[Ordinateurs virtuels de 2e génération](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)**|||||
|Démarrer à l’aide d’UEFI|2019, 2016, 2012 R2|&#10004;Remarque 11, 12|&#10004;Remarque 11, 12|&#10004;Remarque 11, 12|&#10004;Remarque 11, 12|
|Démarrage sécurisé|2019, 2016|&#10004;|&#10004;|&#10004;|&#10004;|

## <a name="notes"></a>Remarques

1. L’injection d’adresses IP statiques peut ne pas fonctionner si le **Gestionnaire de réseau** a été configuré pour une carte réseau spécifique à Hyper-V sur la machine virtuelle. Pour garantir le bon fonctionnement de l’injection d’adresses IP statiques, assurez-vous que le gestionnaire de réseau est entièrement désactivé ou a été désactivé pour une carte réseau spécifique via son fichier **ifcfg-ethx** .

2. Quand vous utilisez des appareils Fibre Channel virtuels, assurez-vous que le numéro d’unité logique 0 (LUN 0) a été rempli. Si le LUN 0 n’a pas été rempli, une machine virtuelle Linux peut ne pas être en mesure de monter des appareils Fibre Channel en mode natif.

3. Si des descripteurs de fichiers sont ouverts pendant une opération de sauvegarde de machine virtuelle en direct, dans certains cas, les VHD sauvegardés devront peut-être subir une vérification de cohérence du système de fichiers (`fsck`) lors de la restauration.

4. Les opérations de sauvegarde en direct peuvent échouer en mode silencieux si l’ordinateur virtuel dispose d’un périphérique iSCSI attaché ou d’un stockage en attachement direct (également appelé disque direct).

5. Sur les versions de support à long terme (LTS), utilisez le dernier noyau d’activation du matériel virtuel (HWE) pour les Integration Services Linux à jour.

   Pour installer le noyau optimisé pour Azure sur 14,04, 16,04 et 18,04, exécutez les commandes suivantes en tant que root (ou sudo) :

   ```bash
   # apt-get update
   # apt-get install linux-azure
   ```

6. Sur Ubuntu 19,10, utilisez le dernier noyau virtuel pour disposer de fonctionnalités Hyper-V à jour.

   Pour installer le noyau virtuel sur 19,10, exécutez les commandes suivantes en tant que root (ou sudo) :

   ```bash
   # apt-get update
   # apt-get install linux-azure
   ```

   Chaque fois que le noyau est mis à jour, l’ordinateur virtuel doit être redémarré pour pouvoir être utilisé.

7. La prise en charge de la mémoire dynamique est disponible uniquement sur les ordinateurs virtuels 64 bits.

8. Les opérations de Mémoire dynamique peuvent échouer si le système d’exploitation invité s’exécute trop peu de mémoire. Voici quelques-unes des meilleures pratiques :

   * La mémoire de démarrage et la mémoire minimale doivent être supérieures ou égales à la quantité de mémoire recommandée par le fournisseur de distribution.

   * Les applications qui ont tendance à consommer toute la mémoire disponible sur un système se limitent à consommer jusqu’à 80% de la mémoire RAM disponible.

9. Si vous utilisez Mémoire dynamique sur les systèmes d’exploitation Windows Server 2019, Windows Server 2016 ou Windows Server 2012/2012 R2, spécifiez la **mémoire de démarrage**, la **mémoire minimale**et les paramètres de **mémoire Maximum** , en multiples de 128 mégaoctets (Mo). Si vous ne le faites pas, vous risquez d’obtenir des erreurs d’ajout à chaud et vous ne verrez peut-être aucune augmentation de la mémoire sur un système d’exploitation invité.

10. Dans Windows Server 2019, Windows Server 2016 ou Windows Server 2012 R2, l’infrastructure de paires clé/valeur peut ne pas fonctionner correctement sans mise à jour logicielle Linux. Contactez votre fournisseur de distribution pour obtenir la mise à jour logicielle en cas de problème avec cette fonctionnalité.

11. Sur Windows Server 2012 R2, le démarrage sécurisé est activé par défaut pour les ordinateurs virtuels de génération 2, et certaines machines virtuelles Linux ne démarrent que si l’option de démarrage sécurisé est désactivée. Vous pouvez désactiver le démarrage sécurisé dans la section **microprogramme** des paramètres de l’ordinateur virtuel dans le **Gestionnaire Hyper-V** , ou vous pouvez le désactiver à l’aide de PowerShell :

    ```Powershell
    Set-VMFirmware -VMName "VMname" -EnableSecureBoot Off
    ```

12. Avant de tenter de copier le disque dur virtuel d’une machine virtuelle de génération 2 VHD existante pour créer des ordinateurs virtuels de 2e génération, procédez comme suit :

    1. Connectez-vous à la machine virtuelle de génération 2 existante.

    2. Accédez au répertoire EFI de démarrage :

       ```bash
       # cd /boot/efi/EFI
       ```

    3. Copiez le répertoire Ubuntu dans dans un nouveau répertoire nommé boot :

       ```bash
       # sudo cp -r ubuntu/ boot
       ```

    4. Accédez au répertoire de démarrage nouvellement créé :

       ```bash
       # cd boot
       ```

    5. Renommez le fichier shimx64. EFI :

       ```bash
       # sudo mv shimx64.efi bootx64.efi
       ```

## <a name="see-also"></a>Voir aussi

* [Ordinateurs virtuels CentOS et Red Hat Enterprise Linux pris en charge sur Hyper-V](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Machines virtuelles Debian prises en charge sur Hyper-V](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [Ordinateurs virtuels Oracle Linux pris en charge sur Hyper-V](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [Ordinateurs virtuels SUSE pris en charge sur Hyper-V](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [Descriptions des fonctionnalités pour les machines virtuelles Linux et FreeBSD sur Hyper-V](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Meilleures pratiques pour l’exécution de Linux sur Hyper-V](Best-Practices-for-running-Linux-on-Hyper-V.md)

* [Set-VMFirmware](https://technet.microsoft.com/library/dn464287.aspx)

* [Ubuntu 14,04 sur une machine virtuelle de génération 2-blog de virtualisation de Ben Armstrong](https://blogs.msdn.com/b/virtual_pc_guy/archive/2014/06/09/ubuntu-14-04-in-a-generation-2-vm.aspx)
