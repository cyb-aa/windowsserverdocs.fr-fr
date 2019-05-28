---
title: Machines virtuelles Oracle Linux prises en charge sur Hyper-V
description: Répertorie les fonctionnalités incluses dans chaque version et les services d’intégration Linux
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c02fdb5b-62f3-43cb-a190-ab74b3ebcf77
author: shirgall
ms.author: kathydav
ms.date: 06/01/2017
ms.openlocfilehash: c72fd2c3a72a304fe8372afb93468fc451b3f2bc
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222661"
---
# <a name="supported-oracle-linux-virtual-machines-on-hyper-v"></a>Machines virtuelles Oracle Linux prises en charge sur Hyper-V

>S'applique à : Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7.1, Windows 7

Le plan de distribution de fonctionnalité suivant indique les fonctionnalités qui sont présentes dans chaque version. Les problèmes connus et les solutions de contournement pour chaque distribution sont répertoriées après le tableau.

Dans cette section :

* [Série de noyau Compatible Red Hat](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md#BKMK_rhc)

* [Série de noyau Unbreakable Enterprise](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md#BKMK_uek)

* [Notes de publication](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md#BKMK_notes)

## <a name="table-legend"></a>Légende du tableau

* **Intégrées** -LIS sont inclus dans le cadre de cette distribution Linux. Les numéros de version de module de noyau pour le LIS intégré (comme indiqué par **lsmod**, par exemple) sont différents du numéro de version sur le package de téléchargement LIS fournie par Microsoft. Une incompatibilité n’indique pas qu’intégrée dans les LIS est obsolète.

* &#10004;-Fonctionnalité disponible

* (*vide*)-fonctionnalité non disponible

* **UEK R\*x QU\*y** -Unbreakable Enterprise Kernel (UEK) où *x* est le numéro de version et *y* est la mise à jour trimestrielle.

## <a name="BKMK_rhc"></a>Série de noyau Compatible Red Hat

Le noyau 32 bits pour la série 6.x est PAE est activé. Il n’existe aucune prise en charge LIS intégrée pour Oracle Linux RHCK 6.0-6.3. Noyaux de Oracle Linux 7.x sont 64 bits uniquement.

| **Fonctionnalité**                                                                                                              | **Version de Windows server**   | **7.5-7.6**         | **7.4**             | **6.4-6.8 et 7.0 à 7.3**                                             | **6.4-6.8 et 7.0-7.2**                                             | **RHCK 7.0-7.2**         | **RHCK 6.8**             | **RHCK 6.6, 6.7**        | **RHCK 6.5**              | **RHCK6.4**               |
|--------------------------------------------------------------------------------------------------------------------------|------------------------------|---------------------|---------------------|---------------------------------------------------------------------|---------------------------------------------------------------------|--------------------------|--------------------------|--------------------------|---------------------------|---------------------------|
| **Disponibilité**                                                                                                         |                              | Intégré            | Intégré            | [LIS 4.2](https://www.microsoft.com/download/details.aspx?id=55106) | [LIS 4.1](https://www.microsoft.com/download/details.aspx?id=51612) | Intégré                 | Intégré                 | Intégré                 | Intégré                  | Intégré                  |
| **[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**                          | 2016, 2012 R2, 2012, 2008 R2 | &#10004;            | &#10004;            | &#10004;                                                            | &#10004;                                                            | &#10004;                 | &#10004;                 | &#10004;                 | &#10004;                  |                           |
| Heure précise de Windows Server 2016                                                                                        | 2016                         |                     |                     |                                                                     |                                                                     |                          |                          |                          |                           |                           |
| **[Mise en réseau](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**              |                              |                     |                     |                                                                     |                                                                     |                          |                          |                          |                           |                           |
| Trames Jumbo                                                                                                             | 2016, 2012 R2, 2012, 2008 R2 | &#10004;            | &#10004;            | &#10004;                                                            | &#10004;                                                            | &#10004;                 | &#10004;                 | &#10004;                 | &#10004;                  | &#10004;                  |
| Un marquage VLAN et trunking                                                                                                | 2016, 2012 R2, 2012, 2008 R2 | &#10004;            | &#10004;            | &#10004;(Remarque 1 pour 6.4-6.8)                                       | &#10004;(Remarque 1 pour 6.4-6.8)                                       | &#10004;                 | &#10004; Note 1          | &#10004; Note 1          | &#10004; Note 1           | &#10004; Note 1           |
| Migration dynamique                                                                                                           | 2016, 2012 R2, 2012, 2008 R2 | &#10004;            | &#10004;            | &#10004;                                                            | &#10004;                                                            | &#10004;                 | &#10004;                 | &#10004;                 | &#10004;                  | &#10004;                  |
| Injection de l’adresse IP statique                                                                                                      | 2016, 2012 R2, 2012          | &#10004;Remarque 14    | &#10004;Remarque 14    | &#10004;                                                            | &#10004;                                                            | &#10004;                 | &#10004;                 | &#10004;                 | &#10004;                  | &#10004;                  |
| vRSS                                                                                                                     | 2016, 2012 R2                | &#10004;            | &#10004;            | &#10004;                                                            | &#10004;                                                            | &#10004;                 | &#10004;                 | &#10004;                 |                           |                           |
| Segmentation de TCP et les déchargements de somme de contrôle                                                                                   | 2016, 2012 R2, 2012, 2008 R2 | &#10004;            | &#10004;            | &#10004;                                                            | &#10004;                                                            | &#10004;                 | &#10004;                 | &#10004;                 |                           |                           |
| SR-IOV                                                                                                                   | 2016                         | &#10004;            | &#10004;            |                                                                     |                                                                     |                          |                          |                          |                           |                           |
| **[Stockage](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**                    |                              |                     |                     |                                                                     |                                                                     |                          |                          |                          |                           |                           |
| Redimensionnement VHDX                                                                                                              | 2016, 2012 R2                | &#10004;            | &#10004;            | &#10004;                                                            | &#10004;                                                            | &#10004;                 | &#10004;                 | &#10004;                 | &#10004;                  |                           |
| Fibre Channel virtuel                                                                                                    | 2016, 2012 R2                | &#10004; Note 2     | &#10004; Note 2     | &#10004; Note 2                                                     | &#10004; Note 2                                                     | &#10004; Note 2          | &#10004; Note 2          | &#10004; Note 2          | &#10004; Note 2           |                           |
| Sauvegarde de machine virtuelle active                                                                                              | 2016, 2012 R2                | &#10004;Remarque 11,3  | &#10004; Note 11, 3 | &#10004; Note 3, 4                                                  | &#10004; Note 3, 4                                                  | &#10004; Note 3, 4, 11   | &#10004; Note 3, 4, 11   | &#10004; Note 3, 4, 11   | &#10004; Note 3, 4, 5, 11 | &#10004; Note 3, 4, 5, 11 |
| Prise en charge de TRIM                                                                                                             | 2016, 2012 R2                | &#10004;            | &#10004;            | &#10004;                                                            | &#10004;                                                            | &#10004;                 | &#10004;                 |                          |                           |                           |
| SCSI WWN                                                                                                                 | 2016, 2012 R2                | &#10004;            |                     | &#10004;                                                            | &#10004;                                                            |                          |                          |                          |                           |                           |
| **[Mémoire](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**                      |                              |                     |                     |                                                                     |                                                                     |                          |                          |                          |                           |                           |
| Prise en charge PAE du noyau                                                                                                       | 2016, 2012 R2, 2012, 2008 R2 | N/A                 | N/A                 | &#10004;(6.x uniquement)                                                 | &#10004;(6.x uniquement)                                                 | N/A                      | &#10004;                 | &#10004;                 | &#10004;                  | &#10004;                  |
| Configuration de l’écart MMIO                                                                                                | 2016, 2012 R2                | &#10004;            | &#10004;            | &#10004;                                                            | &#10004;                                                            | &#10004;                 | &#10004;                 | &#10004;                 | &#10004;                  | &#10004;                  |
| Mémoire dynamique - ajout à chaud                                                                                                 | 2016, 2012 R2, 2012          | &#10004; Note 8, 9  | &#10004; Note 8, 9  | &#10004;Notez les 7, 8, 9, 10 (Remarque 6 pour 6.4-6.7)                      | &#10004;Notez les 7, 8, 9, 10 (Remarque 6 pour 6.4-6.7)                      | &#10004; Note 6, 7, 8, 9 | &#10004; Note 6, 7, 8, 9 | &#10004; Note 6, 7, 8, 9 | &#10004; Note 6, 7, 8, 9  |                           |
| Mémoire dynamique - augmentation de la capacité                                                                                              | 2016, 2012 R2, 2012          | &#10004; Note 8, 9  | &#10004; Note 8, 9  | &#10004;Notez les 7, 9, 10 (Remarque 6 pour 6.4-6.7)                         | &#10004;Notez les 7, 9, 10 (Remarque 6 pour 6.4-6.7)                         | &#10004; Note 6, 8, 9    | &#10004; Note 6, 8, 9    | &#10004; Note 6, 8, 9    | &#10004; Note 6, 8, 9     | &#10004; Note 6, 8, 9, 10 |
| Redimensionnement de la mémoire du runtime                                                                                                    | 2016                         |                     |                     |                                                                     |                                                                     |                          |                          |                          |                           |                           |
| **[Video](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**                        |                              |                     |                     |                                                                     |                                                                     |                          |                          |                          |                           |                           |
| Périphérique vidéo spécifique à Hyper-V                                                                                            | 2016,2012 R2, 2012, 2008 R2  | &#10004;            | &#10004;            | &#10004;                                                            | &#10004;                                                            | &#10004;                 | &#10004;                 | &#10004;                 | &#10004;                  |                           |
| **[Divers](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**                 |                              |                     |                     |                                                                     |                                                                     |                          |                          |                          |                           |                           |
| Paire clé-valeur                                                                                                           | 2016, 2012 R2, 2012, 2008 R2 | &#10004;            | &#10004;            | &#10004;                                                            | &#10004;                                                            | &#10004;Remarque 12         | &#10004;Remarque 12         | &#10004;Remarque 12         | &#10004;Remarque 12          | &#10004;Remarque 12          |
| Interruption non masquable                                                                                                   | 2016, 2012 R2                | &#10004;            | &#10004;            | &#10004;                                                            | &#10004;                                                            | &#10004;                 | &#10004;                 | &#10004;                 | &#10004;                  | &#10004;                  |
| Copie des fichiers d’hôte à invité                                                                                             | 2016, 2012 R2                | &#10004;            | &#10004;            | &#10004;                                                            | &#10004;                                                            |                          | &#10004;                 |                          |                           |                           |
| commande de lsvmbus                                                                                                          | 2016, 2012 R2, 2012, 2008 R2 |                     |                     | &#10004;                                                            | &#10004;                                                            |                          |                          |                          |                           |                           |
| Sockets Hyper-V                                                                                                          | 2016                         |                     |                     | &#10004;                                                            | &#10004;                                                            |                          |                          |                          |                           |                           |
| La norme PCI Passthrough/DDA                                                                                                      | 2016                         | &#10004;            | &#10004;            |                                                                     |                                                                     |                          |                          |                          |                           |                           |
| **[Ordinateurs virtuels de génération 2](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)** |                              |                     |                     |                                                                     |                                                                     |                          |                          |                          |                           |                           |
| Démarrage à l’aide d’UEFI                                                                                                          | 2016, 2012 R2                | &#10004; Note 13    | &#10004; Note 13    | &#10004; Note 13                                                    | &#10004; Note 13                                                    | &#10004; Note 13         | &#10004; Note 13         |                          |                           |                           |
| Démarrage sécurisé                                                                                                              | 2016                         | &#10004;            | &#10004;            |                                                                     |                                                                     |                          |                          |                          |                           |                           |


## <a name="BKMK_uek"></a>Série de noyau Unbreakable Enterprise

L’Oracle Linux Unbreakable Enterprise Kernel (UEK) est de 64 bits uniquement et a LIS prennent en charge intégrée.

|**Fonctionnalité**|**Version de Windows server**|**R4 D’UEK**|**UEK R3 QU3**|**UEK R3 QU2**|**UEK R3 QU1**|
|-|-|-|-|-|-|
|**Disponibilité**||Intégré|Intégré|Intégré|Intégré|
|**[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**|2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|Heure précise de Windows Server 2016|2016|||||
|**[Mise en réseau](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**||||||
|Trames Jumbo|2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|Un marquage VLAN et trunking|2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|Migration dynamique|2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|Injection de l’adresse IP statique|2016, 2012 R2, 2012|&#10004;|&#10004;|&#10004;||
|vRSS|2016, 2012 R2|&#10004;||||
|Segmentation de TCP et les déchargements de somme de contrôle|2016, 2012 R2, 2012, 2008 R2|&#10004;||||
|SR-IOV|2016|||||
|**[Stockage](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**||||||
|Redimensionnement VHDX|2016, 2012 R2|&#10004;|&#10004;|&#10004;||
|Fibre Channel virtuel|2016, 2012 R2|&#10004;|&#10004;|&#10004;||
|Sauvegarde de machine virtuelle active|2016, 2012 R2|&#10004; Note 3, 4, 5, 11|&#10004; Note 3, 4, 5, 11|&#10004; Note 3, 4, 5, 11||
|Prise en charge de TRIM|2016, 2012 R2|&#10004;||||
|SCSI WWN|2016, 2012 R2|||||
|**[Mémoire](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**|
|Prise en charge PAE du noyau|2016, 2012 R2, 2012, 2008 R2|N/A|N/A|N/A|N/A|
|Configuration de l’écart MMIO|2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|Mémoire dynamique - ajout à chaud|2016, 2012 R2, 2012|&#10004;||||
|Mémoire dynamique - augmentation de la capacité|2016, 2012 R2, 2012|&#10004;||||
|Redimensionnement de la mémoire du runtime|2016|||||
|**[Video](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**||||||
|Périphérique vidéo spécifique à Hyper-V|2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;||
|**[Divers](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**||||||
|Paire clé-valeur|2016, 2012 R2, 2012, 2008 R2|&#10004; Note 11, 12|&#10004; Note 11, 12|&#10004; Note 11, 12|&#10004; Note 11, 12|
|Interruption non masquable|2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|Copie des fichiers d’hôte à invité|2016, 2012 R2|&#10004;Remarque 11|&#10004;Remarque 11|&#10004;Remarque 11|&#10004;Remarque 11|
|commande de lsvmbus|2016, 2012 R2, 2012, 2008 R2|||||
|Sockets Hyper-V|2016|||||
|La norme PCI Passthrough/DDA|2016|||||
|**[Ordinateurs virtuels de génération 2](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)**|
|Démarrage à l’aide d’UEFI|2016, 2012 R2|&#10004;||||
|Démarrage sécurisé|2016|&#10004;||||

## <a name="BKMK_notes"></a>Notes de publication

1. Pour cette version d’Oracle Linux, étiquetage fonctionne mais le trunking VLAN ne fait pas.

2. Lors de l’utilisation de périphériques virtuels fibre channel, assurez-vous que le numéro d’unité logique (LUN 0) de 0 a été remplie. Si le LUN 0 n’a pas été rempli, une machine virtuelle Linux ne peut pas être en mesure de monter les périphériques fibre channel en mode natif.

3. S’il existe des descripteurs de fichiers ouverts pendant une opération de sauvegarde de machine virtuelle active, puis, dans certains cas extrêmes, les disques durs virtuels sauvegardés peut-être subir une fichier système de cohérence (fsck) lors de la restauration.

4. Les opérations de sauvegarde en direct peuvent échouer en mode silencieux si l’ordinateur virtuel possède un périphérique iSCSI attaché ou le stockage en attachement direct (également appelé un disque direct).

5. Support de sauvegarde Live pour Oracle Linux 6.4/6.5/UEKR3 QU2 et QU3 sont disponibles via [Essentials de sauvegarde Hyper-V pour Linux](https://github.com/LIS/backupessentials/tree/1.0).

6. Prise en charge de la mémoire dynamique est uniquement disponible sur les ordinateurs virtuels 64 bits.

7. Prise en charge de l’ajout à chaud n’est pas activée par défaut dans cette distribution. Pour activer la prise en charge de l’ajout à chaud, vous devez ajouter une règle d’udev sous /etc/udev/rules.d/ comme suit :

   1. Créez un fichier **/etc/udev/rules.d/100-balloon.rules**. Vous pouvez utiliser n’importe quel autre nom souhaité pour le fichier.

   2. Ajoutez le contenu suivant au fichier : `SUBSYSTEM=="memory", ACTION=="add", ATTR{state}="online"`

   3. Redémarrez le système pour activer la prise en charge de l’ajout à chaud.

   Pendant que le téléchargement de Linux Integration Services crée cette règle sur l’installation, la règle est également supprimée lors de la désinstallation de LIS, donc la règle doit être recréée si la mémoire dynamique est nécessaire après la désinstallation.

8. Opérations de mémoire dynamique peuvent échouer si le système d’exploitation invité s’exécute trop faible de la mémoire. Voici quelques-unes des meilleures pratiques :

   * Mémoire de démarrage et la quantité minimale de mémoire doivent être égale ou supérieure à la quantité de mémoire qui recommande le fournisseur de distribution.

   * Les applications qui ont tendance à consommer la totalité de la mémoire disponible sur un système sont limitées à consommer jusqu'à 80 % de mémoire vive disponible.

9. Si vous utilisez la mémoire dynamique sur un système d’exploitation Windows Server 2016 ou Windows Server 2012 R2, spécifiez **mémoire de démarrage**, **mémoire minimale**, et **mémoire maximale** paramètres par multiples de 128 mégaoctets (Mo). Cela peut entraîner à chaud de défaillances, et vous ne voyiez pas de mémoire augmenter dans un système d’exploitation.

10. Certaines distributions, y compris ceux qui utilisent les LIS 3.5 ou 4.0 LIS, augmentation prennent en charge uniquement et ne fournissent pas de prise en charge de l’ajout à chaud. Dans ce scénario, la fonctionnalité mémoire dynamique peut être utilisée en définissant le paramètre de mémoire de démarrage pour une valeur qui est égale au paramètre mémoire Maximum. Il en résulte toute la mémoire requise n’est ajouté à chaud à la machine virtuelle au moment du démarrage et ensuite selon les besoins en mémoire de l’hôte, Hyper-V peut librement allouer ou libérer la mémoire de l’invité à l’aide d’augmentation. Veuillez configurer **mémoire de démarrage** et **mémoire minimale** supérieure ou égale à la valeur recommandée pour la distribution.

11. Les démons Linux Hyper-V Oracle ne sont pas installés par défaut. Pour utiliser ces processus, installez le package de démons de Hyper-v. Conflits de ce package avec téléchargement Linux Integration Services et ne doivent pas être installé sur les systèmes avec LIS téléchargé.

12. L’infrastructure de (KVP) de paire clé/valeur ne peut pas fonctionner correctement sans une mise à jour de logiciels Linux. Contactez votre fournisseur de distribution pour obtenir la mise à jour logicielle dans le cas où vous voyez des problèmes liés à cette fonctionnalité.

13. Machines virtuelles de vos serveurs Windows Server 2012 R2Generation 2 ont le démarrage sécurisé est activé par défaut et des machines virtuelles de Linux ne démarrera pas, sauf si l’option de démarrage sécurisé est désactivée. Vous pouvez désactiver le démarrage sécurisé dans le **microprogramme** section des paramètres de la machine virtuelle dans **Gestionnaire Hyper-V** ou vous pouvez le désactiver à l’aide de Powershell :

   ```Powershell
   Set-VMFirmware -VMName "VMname" -EnableSecureBoot Off

   ```

    Le téléchargement de Services d’intégration Linux peut être appliqué aux machines virtuelles de 2 génération existantes, mais n’octroie pas de fonctionnalité de génération 2.

14. Injection d’adresse IP statique peut ne pas fonctionne si le Gestionnaire de réseau a été configuré pour une carte réseau synthétique donné sur la machine virtuelle. Pour le bon fonctionnement de l’adresse IP statique injection Assurez-vous que le Gestionnaire de réseau est hors tension complètement ou a été désactivé pour une carte réseau spécifique via son fichier ifcfg-ethX.


Voir aussi

* [Set-VMFirmware](https://technet.microsoft.com/library/dn464287.aspx)

* [Prise en charge de CentOS et les machines virtuelles de Red Hat Enterprise Linux sur Hyper-V](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Machines virtuelles Debian prises en charge sur Hyper-V](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [Machines virtuelles SUSE prises en charge sur Hyper-V](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [Machines virtuelles Ubuntu prises en charge sur Hyper-V](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [Machines virtuelles FreeBSD prises en charge sur Hyper-V](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Descriptions des fonctionnalités pour les machines virtuelles Linux et FreeBSD sur Hyper-V](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Meilleures pratiques pour l’exécution de Linux sur Hyper-V](Best-Practices-for-running-Linux-on-Hyper-V.md)
