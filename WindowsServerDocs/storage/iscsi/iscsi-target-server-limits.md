---
title: iSCSI limites d’extensibilité du serveur cible
TOCTitle: iSCSI Target Server Scalability Limits
ms.prod: windows-server-threshold
ms.technology: storage-iscsi
ms.topic: article
author: JasonGerend
manager: dougkim
ms.author: jgerend
ms.date: 09/11/2018
ms.openlocfilehash: 9514392da133911c900f68fc8f1be260b6c91138
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873030"
---
# <a name="iscsi-target-server-scalability-limits"></a>iSCSI limites d’extensibilité du serveur cible

S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette rubrique fournit des limites du serveur cible Microsoft iSCSI pris en charge et testé sur Windows Server. Les tableaux suivants affichent les limites de prise en charge testé et, le cas échéant, si les limites sont appliquées.

## <a name="general-limits"></a>Limites générales

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Élément</p></th>
<th><p>Limite de prise en charge</p></th>
<th><p>Appliquées ?</p></th>
<th><p>Commentaire</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>instances de cibles iSCSI par le serveur cible iSCSI</p></td>
<td><p>256</p></td>
<td><p>Non</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>iSCSI, unités logiques (lu) ou des disques virtuels par le serveur cible iSCSI</p></td>
<td><p>512</p></td>
<td><p>Non</p></td>
<td><p>Configurations de tests inclus : Unités 8 logiques par instance cible avec une cible de plus de 64 moyenne et les instances cibles 256 avec une seule unité logique par cible.</p></td>
</tr>
<tr class="odd">
<td><p>unités logiques iSCSI ou des disques virtuels par iSCSI ciblent l’instance</p></td>
<td><p>256 (128 sur Windows Server 2012)</p></td>
<td><p>Oui</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Sessions qui peuvent se connecter simultanément à une instance de la cible iSCSI</p></td>
<td><p>544 (512 sur Windows Server 2012)</p></td>
<td><p>Oui</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Captures instantanées par %lu</p></td>
<td><p>512</p></td>
<td><p>Oui</p></td>
<td><p>Il existe une limite de 512 instantanés par volume d’application indépendants iSCSI.</p></td>
</tr>
<tr class="even">
<td><p>Monté localement des disques virtuels ou des captures instantanées par dispositif de stockage</p></td>
<td><p>32</p></td>
<td><p>Oui</p></td>
<td><p>Les disques virtuels montés localement n’offrent pas toutes les fonctionnalités spécifiques à iSCSI et sont obsolètes. pour plus d’informations, consultez <a href="https://technet.microsoft.com/library/dn303411.aspx">fonctionnalités supprimées ou déconseillées dans Windows Server 2012 R2</a>.</p></td>
</tr>
</tbody>
</table>

## <a name="fault-tolerance-limits"></a>Limite d’une tolérance de panne

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Élément</p></th>
<th><p>Limite de prise en charge</p></th>
<th><p>Appliquées ?</p></th>
<th><p>Commentaire</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Nœuds de cluster de basculement</p></td>
<td><p>8 (5 sur Windows Server 2012)</p></td>
<td><p>Non</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Plusieurs nœuds actifs du cluster</p></td>
<td><p>Prise en charge</p></td>
<td> 
<p>N/A</p></td>
<td><p>Chaque nœud actif du cluster de basculement possède une instance en cluster du serveur cible iSCSI différents avec d’autres nœuds agissant en tant que nœuds propriétaires possibles.</p></td>
</tr>
<tr class="odd">
<td><p>Niveau de récupération d’erreur (ERL)</p></td>
<td><p>0</p></td>
<td><p>Oui</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Connexions par session</p></td>
<td><p>1</p></td>
<td><p>Oui</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Sessions qui peuvent se connecter simultanément à une instance de la cible iSCSI</p></td>
<td><p>544 (512 sur Windows Server 2012)</p></td>
<td><p>Non</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>D’entrée/sortie en chemins d’accès multiples (MPIO)</p></td>
<td><p>Prise en charge</p></td>
<td><p>N/A</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Chemins d’accès MPIO</p></td>
<td><p>4</p></td>
<td><p>Non</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Conversion d’un serveur cible iSCSI autonome à un cluster iSCSI serveur cible et inversement</p></td>
<td><p>Non pris en charge</p></td>
<td><p>Non</p></td>
<td><p>L’instance de cible iSCSI et disque virtuel des données de configuration, notamment les métadonnées de capture instantanée, seront perdues pendant la conversion.</p></td>
</tr>
</tbody>
</table>

## <a name="network-limits"></a>Limites de réseau

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Élément</p></th>
<th><p>Limite de prise en charge</p></th>
<th><p>Appliquées ?</p></th>
<th><p>Commentaire</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Nombre maximal de cartes réseau actives</p></td>
<td><p>8</p></td>
<td><p>Non</p></td>
<td><p>S’applique aux cartes réseau qui sont dédiés au trafic iSCSI, plutôt que le nombre total de cartes réseau dans l’appliance.</p></td>
</tr>
<tr class="even">
<td><p>Portail (adresses IP) pris en charge</p></td>
<td><p>64</p></td>
<td><p>Oui</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Vitesse du port réseau</p></td>
<td><p>1 Gbit/s, 10 Gbits/s, 40Gbps, 56 Gbits/s (Windows Server 2012 R2 et versions ultérieures uniquement)</p></td>
<td><p>Non</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>IPv4</p></td>
<td><p>Prise en charge</p></td>
<td><p>N/A</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>IPv6</p></td>
<td><p>Prise en charge</p></td>
<td><p>N/A</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Déchargement TCP</p></td>
<td><p>Prise en charge</p></td>
<td><p>N/A</p></td>
<td><p>Tirer parti d’envoi de grande taille (segmentation), de somme de contrôle, de modération de l’interruption et de déchargement de RSS</p></td>
</tr>
<tr class="odd">
<td><p>déchargement iSCSI</p></td>
<td><p>Non pris en charge</p></td>
<td>              
<p>N/A</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Trames Jumbo</p></td>
<td><p>Prise en charge</p></td>
<td><p>N/A</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>IPSec</p></td>
<td><p>Prise en charge</p></td>
<td><p>N/A</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Déchargement CRC</p></td>
<td><p>Prise en charge</p></td>
<td><p>N/A</p></td>
<td></td>
</tr>
</tbody>
</table>

## <a name="iscsi-virtual-disk-limits"></a>limites de disque virtuel iSCSI

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Élément</p></th>
<th><p>Limite de prise en charge</p></th>
<th><p>Appliquées ?</p></th>
<th><p>Commentaire</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>À partir d’un initiateur iSCSI convertir le disque virtuel à partir d’un disque de base en disque dynamique </p></td>
<td><p>Oui</p></td>
<td><p>Non</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Format de disque dur virtuel</p></td>
<td><p>.vhdx (Windows Server 2012 R2 et versions ultérieures uniquement)</p>
<p>.vhd</p></td>
<td></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Taille minimale de format de disque dur virtuel</p></td>
<td><p>.vhdx: 3 Mo</p>
<p>.vhd: 8 Mo</p></td>
<td><p>Oui</p></td>
<td><p>S’applique à tous les types de disque dur virtuel pris en charge : parent, de différenciation et fixe.</p></td>
</tr>
<tr class="even">
<td><p>Taille maximale du disque dur virtuel parent</p></td>
<td><p>.vhdx: 64 To</p>
<p>.vhd: 2 TO</p></td>
<td><p>Oui</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Taille maximale de disque dur virtuel fixe</p></td>
<td><p>.vhdx: 64 To</p>
<p>.vhd: 16 TO</p></td>
<td><p>Oui</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Taille maximale de disque dur virtuel de différenciation</p></td>
<td><p>.vhdx: 64 To</p>
<p>.vhd: 2 TO</p></td>
<td><p>Oui</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Disque dur virtuel au format fixé</p></td>
<td><p>Prise en charge</p></td>
<td><p>Non</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Format de différenciation VHD</p></td>
<td><p>Prise en charge</p></td>
<td><p>Non</p></td>
<td><p>Ne peut pas être instantanées de disques virtuels iSCSI basées sur le disque dur virtuel de différenciation.</p></td>
</tr>
<tr class="odd">
<td><p>Nombre de disques durs virtuels de différenciation par disque dur virtuel parent</p></td>
<td><p>256</p></td>
<td><p>Non (Oui sur Windows Server 2012)</p></td>
<td><p>Deux niveaux de profondeur (fichiers .vhdx de petits-enfants) est la valeur maximale pour les fichiers .vhdx ; un seul niveau de profondeur (fichiers .vhd de l’enfant) est la valeur maximale pour les fichiers .vhd.</p></td>
</tr>
<tr class="even">
<td><p>Format de disque dur virtuel dynamique</p></td>
<td><p>.vhdx: Oui</p>
<p>.vhd: Oui (non sur Windows Server 2012)</p></td>
<td><p>Oui</p></td>
<td><p>Annuler le mappage n’est pas pris en charge.</p></td>
</tr>
<tr class="odd">
<td><p>exFAT/FAT/FAT32 (qui héberge le volume de disque dur virtuel)</p></td>
<td><p>Non pris en charge</p></td>
<td><p>Oui</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>CSV v2</p></td>
<td><p>Non pris en charge</p></td>
<td><p>Oui</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>ReFS</p></td>
<td><p>Prise en charge</p></td>
<td><p>N/A</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>NTFS</p></td>
<td><p>Prise en charge</p></td>
<td><p>N/A</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>CFS non - Microsoft</p></td>
<td><p>Non pris en charge</p></td>
<td><p>Oui</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Allocation dynamique</p></td>
<td><p>Non</p></td>
<td><p>N/A</p></td>
<td><p>Disques durs virtuels dynamiques sont pris en charge, mais Unmap n’est pas pris en charge.</p></td>
</tr>
<tr class="odd">
<td><p>Réduction d’unité logique</p></td>
<td><p>Oui (Windows Server 2012 R2 et versions ultérieures uniquement)</p></td>
<td><p>N/A</p></td>
<td><p>Utilisez <a href="https://docs.microsoft.com/powershell/module/iscsitarget/resize-iscsivirtualdisk">Resize-iSCSIVirtualDisk</a> pour réduire un numéro d’unité logique.</p></td>
</tr>
<tr class="even">
<td><p>Unité logique de clonage</p></td>
<td><p>Non pris en charge</p></td>
<td><p>N/A</p></td>
<td><p>Vous pouvez rapidement cloner les données de disque à l’aide de disques durs virtuels de différenciation.</p></td>
</tr>
</tbody>
</table>

## <a name="snapshot-limits"></a>Limites de capture instantanée

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Élément</p></th>
<th><p>Limite de prise en charge</p></th>
<th><p>Commentaire</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Création d’instantané</p></td>
<td><p>Prise en charge</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Restauration de capture instantanée</p></td>
<td><p>Prise en charge</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Captures instantanées inscriptibles</p></td>
<td><p>Non pris en charge</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Instantané : convert complet</p></td>
<td><p>Non pris en charge</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Instantané : restauration en ligne</p></td>
<td><p>Non pris en charge</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Instantané – convertir accessible en écriture</p></td>
<td><p>Non pris en charge</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Instantané - redirection</p></td>
<td><p>Non pris en charge</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Instantané - épinglage</p></td>
<td><p>Non pris en charge</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Montage local</p></td>
<td><p>Prise en charge</p></td>
<td><p>Disques virtuels iSCSI monté localement sont obsolètes. pour plus d’informations, consultez <a href="https://technet.microsoft.com/library/dn303411.aspx">fonctionnalités supprimées ou déconseillées dans Windows Server 2012 R2</a>. Captures instantanées des disques dynamiques ne peut pas être montés localement.</p></td>
</tr>
</tbody>
</table>

## <a name="iscsi-target-server-manageability-and-backup"></a>sauvegarde et la facilité de gestion de serveur cible iSCSI

Si vous souhaitez créer des clichés instantanés (instantanés du fichier ouvert VSS) des données de volume sur les disques virtuels iSCSI à partir d’un serveur d’applications, ou si vous souhaitez gérer les disques virtuels iSCSI avec une application plus anciens (par exemple, la commande Diskraid) qui requiert un matériel de Service de disque virtuel (VDS) fournisseur, installez le fournisseur de stockage cible iSCSI sur le serveur à partir duquel vous souhaitez prendre un instantané ou utiliser une application de gestion VDS.

Le fournisseur de stockage cible iSCSI est un service de rôle dans Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012 ; Vous pouvez également télécharger et installer [iSCSI fournisseurs de stockage cible (VDS/VSS) pour les serveurs d’applications de bas niveau](http://www.microsoft.com/download/details.aspx?id=34759) sur les systèmes d’exploitation suivants à condition que le serveur cible iSCSI est en cours d’exécution sur Windows Server 2012 :

  - Windows Storage Server 2008 R2

  - Windows Server 2008 R2

  - Windows HPC Server 2008 R2

  - Windows HPC Server 2008

Notez que si le serveur cible iSCSI est hébergé par un serveur exécutant Windows Server 2012 R2 ou version ultérieure et que vous souhaitez utiliser VSS ou VDS à partir d’un serveur distant, le serveur distant doit également exécuter la même version de Windows Server et disposer du rôle de fournisseur de stockage cible iSCSI service e installé. Notez également que vous devez installer qu’une seule version du service de rôle de fournisseur de stockage cible iSCSI sur toutes les versions de Windows.

Pour plus d’informations sur le fournisseur de stockage cible iSCSI, consultez [fournisseur de stockage cible (VDS/VSS) iSCSI](http://blogs.technet.com/b/filecab/archive/2012/10/08/iscsi-target-storage-vds-vss-provider.aspx).

## <a name="tested-compatibility-with-iscsi-initiators"></a>Compatibilité testée avec les initiateurs iSCSI

Nous avons testé le logiciel de serveur cible avec les initiateurs iSCSI suivant iSCSI :

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p>Initiateur</p></td>
<td><p>Windows Server 2012 R2</p></td>
<td><p>Windows Server 2012</p></td>
<td><p>Commentaires</p></td>
</tr>
<tr class="even">
<td><p>Windows Server 2012 R2</p></td>
<td><p>Validé</p></td>
<td></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Windows Server 2012, Windows Server 2008 R2, Windows Server 2008, Windows Server 2003</p></td>
<td><p>Validé</p></td>
<td><p>Validé</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>VMWare vSphere 5</p></td>
<td></td>
<td><p>Validé</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>VMWare ESXi 5.0</p></td>
<td><p>Validé</p></td>
<td></td>
<td></td>
</tr>
<tr class="even">
<td><p>VMWare ESX 4.1</p></td>
<td><p>Validé</p></td>
<td></td>
<td></td>
</tr>
<tr class="odd">
<td><p>CentOS 6.x</p></td>
<td><p>Validé</p></td>
<td></td>
<td><p>Doit se déconnecter une session et reconnectez-vous pour détecter un disque virtuel redimensionné.</p></td>
</tr>
<tr class="even">
<td><p>Red Hat Enterprise Linux 6</p></td>
<td><p>Validé</p></td>
<td></td>
<td></td>
</tr>
<tr class="odd">
<td><p>RedHat Enterprise Linux 5 and 5</p></td>
<td><p>Validé</p></td>
<td><p>Validé</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>SUSE Linux Enterprise Server 10</p></td>
<td></td>
<td><p>Validé</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Oracle Solaris 11.x</p></td>
<td><p>Validé</p></td>
<td></td>
<td></td>
</tr>
</tbody>
</table>

Nous avons également testé les initiateurs iSCSI suivant effectuant un démarrage sans disque à partir de disques virtuels hébergés par le serveur cible iSCSI :

  - Windows Server 2012 R2

  - Windows Server 2012

  - NIC PCIe avec iPXE

  - Disque CD ou une clé USB avec iPXE

## <a name="see-also"></a>Voir aussi

La liste suivante propose des ressources supplémentaires sur le serveur cible iSCSI et les technologies connexes.

  - [vue d’ensemble de stockage de bloc cible iSCSI](iscsi-target-server.md)

  - [vue d’ensemble du démarrage de cible iSCSI](iscsi-boot-overview.md)

  - [Stockage dans Windows Server](..\storage.md)

