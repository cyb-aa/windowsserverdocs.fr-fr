---
title: Ordinateurs virtuels SUSE pris en charge sur Hyper-V
description: Répertorie les fonctionnalités et les services d’intégration Linux inclus dans chaque version
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.topic: article
ms.assetid: 7ec0e14c-4498-4bd9-8fe6-b94260198efc
author: shirgall
ms.author: kathydav
ms.date: 10/03/2016
ms.openlocfilehash: c5daa73e2e0c59a262565237d979d2e1e544ae4f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858002"
---
# <a name="supported-suse-virtual-machines-on-hyper-v"></a>Ordinateurs virtuels SUSE pris en charge sur Hyper-V

>S’applique à : Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7,1, Windows 7

Voici un plan de distribution de fonctionnalités qui indique les fonctionnalités de chaque version. Les problèmes connus et les solutions de contournement pour chaque distribution sont répertoriés après le tableau.

Les pilotes intégrés SUSE Linux Enterprise service pour Hyper-V sont certifiés par SUSE. Vous pouvez consulter un exemple de configuration dans ce bulletin : [Bulletin de certification SUSE Yes](https://www.suse.com/nbswebapp/yesBulletin.jsp?bulletinNumber=144176).

## <a name="table-legend"></a>Légende de la table

* Les lis intégrés sont inclus **dans** le cadre de cette distribution Linux. Le package de téléchargement de LIS fourni par Microsoft ne fonctionne pas pour cette distribution. ne l’installez donc pas. Les numéros de version du module de noyau pour la LIS intégrée (comme indiqué par **lsmod**, par exemple) sont différents du numéro de version du package de téléchargement lis fourni par Microsoft. Une incompatibilité n’indique pas que la LIS intégrée est obsolète.

* &#10004;-Fonctionnalité disponible

* (*vide*)-fonctionnalité non disponible

SLES12 + est 64-bit uniquement.

|**Fonctionnalité**|**Version du système d’exploitation Windows Server**|**SLES 15**|**SLES 12 SP3/SP4**|**SLES 12 SP2**|**SLES 12 SP1**|**SLES 11 SP4**|**SLES 11 SP3**|
|-|-|-|-|-|-|-|-|
|**Disponibilité**||Intégrée|Intégrée|Intégrée|Intégrée|Intégrée|Intégrée|
|**[Ebauche](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Heure précise de Windows Server 2016|2019, 2016|&#10004;|&#10004;|&#10004;||||
|**[Mise](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**||||||||
|Trames Jumbo|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Balisage et Trunking VLAN|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Migration en direct|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Injection d’adresses IP statiques|2019, 2016, 2012 R2, 2012|&#10004;Remarque 1|&#10004;Remarque 1|&#10004;Remarque 1|&#10004;Remarque 1|&#10004;Remarque 1|&#10004;Remarque 1|
|vRSS|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|||
|Segmentation TCP et déchargements de somme de contrôle|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|SR-IOV|2019, 2016|&#10004;|&#10004;|&#10004;||||
|**[Rangement](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**||||||||
|Redimensionnement VHDX|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Fibre Channel virtuel|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Sauvegarde de machine virtuelle en direct|2019, 2016, 2012 R2|&#10004;Remarque 2, 3, 8|&#10004;Remarque 2, 3, 8|&#10004;Remarque 2, 3, 8|&#10004;Remarque 2, 3, 8|&#10004;Remarque 2, 3, 8|&#10004;Remarque 2, 3, 8|
|SUPPRIMER la prise en charge|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|WWN SCSI|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;||||
|**[Capacité](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**||||||||
|Prise en charge du noyau PAE|2019, 2016, 2012 R2, 2012, 2008 R2|N/A|N/A|N/A|N/A|&#10004;|&#10004;|
|Configuration de l’intervalle MMIO|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Mémoire dynamique-ajout à chaud|2019, 2016, 2012 R2, 2012|&#10004;Remarque : 5, 6|&#10004;Remarque : 5, 6|&#10004;Remarque : 5, 6|&#10004;Remarque : 5, 6|&#10004;Remarque 4, 5, 6|&#10004;Remarque 4, 5, 6|
|Mémoire dynamique-bulles|2019, 2016, 2012 R2, 2012|&#10004;Remarque : 5, 6|&#10004;Remarque : 5, 6|&#10004;Remarque : 5, 6|&#10004;Remarque : 5, 6|&#10004;Remarque 4, 5, 6|&#10004;Remarque 4, 5, 6|
|Redimensionnement de la mémoire d’exécution|2019, 2016|&#10004;Remarque : 5, 6|&#10004;Remarque : 5, 6|&#10004;Remarque : 5, 6||||
|**[Vidéosurveillance](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**||||||||
|Périphérique vidéo spécifique à Hyper-V|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|**[Diverses](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**||||||||
|Paire clé/valeur|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;Remarque 7|&#10004;Remarque 7|
|Interruption non masquable|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Copie de fichiers de l’hôte vers l’invité|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|commande lsvmbus|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;||||
|Sockets Hyper-V|2019, 2016|&#10004;|&#10004;|||||
|Passthrough/DDA PCI|2019, 2016|&#10004;|&#10004;|&#10004;|&#10004;|||
|**[Ordinateurs virtuels de 2e génération](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)**||||||||
|Démarrer à l’aide d’UEFI|2019, 2016, 2012 R2|&#10004;Remarque 9|&#10004;Remarque 9|&#10004;Remarque 9|&#10004;Remarque 9|&#10004;Remarque 9||
|Démarrage sécurisé|2019, 2016|&#10004;|&#10004;|&#10004;|&#10004;|||

## <a name="notes"></a><a name="BKMK_notes"></a>Notes

1. L’injection d’adresses IP statiques peut ne pas fonctionner si le **Gestionnaire de réseau** a été configuré pour une carte réseau spécifique à Hyper-V sur la machine virtuelle. Pour garantir le bon fonctionnement de l’injection d’adresses IP statiques, assurez-vous que le gestionnaire de réseau est entièrement désactivé ou a été désactivé pour une carte réseau spécifique via son fichier **ifcfg-ethx** .

2. Si des descripteurs de fichiers sont ouverts pendant une opération de sauvegarde de machine virtuelle en direct, dans certains cas, les VHD sauvegardés devront peut-être subir une vérification de cohérence du système de fichiers (fsck) lors de la restauration.

3. Les opérations de sauvegarde en direct peuvent échouer en mode silencieux si l’ordinateur virtuel dispose d’un périphérique iSCSI attaché ou d’un stockage en attachement direct (également appelé disque direct).

4. Les opérations de mémoire dynamique peuvent échouer si le système d’exploitation invité s’exécute trop peu de mémoire. Voici quelques-unes des meilleures pratiques :

   * La mémoire de démarrage et la mémoire minimale doivent être supérieures ou égales à la quantité de mémoire recommandée par le fournisseur de distribution.

   * Les applications qui ont tendance à consommer toute la mémoire disponible sur un système se limitent à consommer jusqu’à 80% de la mémoire RAM disponible.

5. La prise en charge de la mémoire dynamique est disponible uniquement sur les ordinateurs virtuels 64 bits.

6. Si vous utilisez Mémoire dynamique sur les systèmes d’exploitation Windows Server 2016 ou Windows Server 2012, spécifiez les paramètres de **mémoire de démarrage**, de **mémoire minimale**et de **mémoire maximale** en multiples de 128 mégaoctets (Mo). Si vous ne le faites pas, vous risquez d’obtenir des erreurs d’ajout à chaud. vous ne verrez peut-être aucune augmentation de la mémoire dans un système d’exploitation invité.

7. Dans Windows Server 2016 ou Windows Server 2012 R2, l’infrastructure de paires clé/valeur peut ne pas fonctionner correctement sans mise à jour logicielle Linux. Contactez votre fournisseur de distribution pour obtenir la mise à jour logicielle en cas de problème avec cette fonctionnalité.

8. La sauvegarde VSS échoue si une partition unique est montée plusieurs fois.

9. Sur Windows Server 2012 R2, le démarrage sécurisé est activé par défaut pour les ordinateurs virtuels de génération 2, et les ordinateurs virtuels Linux de génération 2 ne démarrent pas, sauf si l’option de démarrage sécurisé est désactivée. Vous pouvez désactiver le démarrage sécurisé dans la section **Microprogramme** des paramètres de l'ordinateur virtuel dans le Gestionnaire Hyper-V ou à l'aide de Powershell :

   ```Powershell
   Set-VMFirmware -VMName "VMname" -EnableSecureBoot Off

   ```

## <a name="see-also"></a>Voir aussi

* [Set-VMFirmware](https://technet.microsoft.com/library/dn464287.aspx)

* [Ordinateurs virtuels CentOS et Red Hat Enterprise Linux pris en charge sur Hyper-V](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Machines virtuelles Debian prises en charge sur Hyper-V](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [Ordinateurs virtuels Oracle Linux pris en charge sur Hyper-V](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [Machines virtuelles Ubuntu prises en charge sur Hyper-V](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [Ordinateurs virtuels FreeBSD pris en charge sur Hyper-V](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Descriptions des fonctionnalités pour les machines virtuelles Linux et FreeBSD sur Hyper-V](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Meilleures pratiques pour l’exécution de Linux sur Hyper-V](Best-Practices-for-running-Linux-on-Hyper-V.md)
