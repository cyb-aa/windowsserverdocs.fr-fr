---
title: Limites d’évolutivité du serveur cible iSCSI
TOCTitle: iSCSI Target Server Scalability Limits
ms.prod: windows-server
ms.technology: storage-iscsi
ms.topic: article
author: JasonGerend
manager: dougkim
ms.author: jgerend
ms.date: 09/11/2018
ms.openlocfilehash: d92ed347288bc9a0dd3893148a31152ae8b8a313
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393993"
---
# <a name="iscsi-target-server-scalability-limits"></a>Limites d’évolutivité du serveur cible iSCSI

S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette rubrique fournit les limites de serveur cible Microsoft iSCSI prises en charge et testées sur Windows Server. Les tableaux suivants affichent les limites de prise en charge testées et, le cas échéant, si les limites sont appliquées.

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
<th><p>Appliquée?</p></th>
<th><p>Commentaire</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>instances de cibles iSCSI par serveur cible iSCSI</p></td>
<td><p>256</p></td>
<td><p>Non</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>unités logiques iSCSI (lu) ou disques virtuels par serveur cible iSCSI</p></td>
<td><p>512</p></td>
<td><p>Non</p></td>
<td><p>Configurations de test incluses : 8 unités logiques par instance cible avec une moyenne sur 64 cibles et 256 instances cibles avec une unité logique par cible.</p></td>
</tr>
<tr class="odd">
<td><p>Unités logiques ou disques virtuels par instance de cible iSCSI</p></td>
<td><p>256 (128 sur Windows Server 2012)</p></td>
<td><p>Oui</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Sessions qui peuvent se connecter simultanément à une instance cible iSCSI</p></td>
<td><p>544 (512 sur Windows Server 2012)</p></td>
<td><p>Oui</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Instantanés par LU</p></td>
<td><p>512</p></td>
<td><p>Oui</p></td>
<td><p>Il existe une limite de 512 instantanés par volume d’application iSCSI indépendant.</p></td>
</tr>
<tr class="even">
<td><p>Disques virtuels montés localement ou instantanés par dispositif de stockage</p></td>
<td><p>32</p></td>
<td><p>Oui</p></td>
<td><p>Les disques virtuels montés&#39;localement don t offrent des fonctionnalités iSCSI et sont dépréciées-pour plus d’informations, voir <a href="https://technet.microsoft.com/library/dn303411.aspx">fonctionnalités supprimées ou déconseillées dans Windows Server 2012 R2</a>.</p></td>
</tr>
</tbody>
</table>

## <a name="fault-tolerance-limits"></a>Limites de tolérance de panne

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
<th><p>Appliquée?</p></th>
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
<td><p>Nœuds de cluster actifs multiples</p></td>
<td><p>Prise en charge</p></td>
<td> 
<p>N/A</p></td>
<td><p>Chaque nœud actif dans le cluster de basculement est propriétaire d’une instance en cluster de serveur cible iSCSI différente avec d’autres nœuds agissant comme des nœuds propriétaires possibles.</p></td>
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
<td><p>Sessions qui peuvent se connecter simultanément à une instance cible iSCSI</p></td>
<td><p>544 (512 sur Windows Server 2012)</p></td>
<td><p>Non</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Entrées/sorties multivoie (MPIO)</p></td>
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
<td><p>Conversion d’un serveur cible iSCSI autonome en serveur cible iSCSI en cluster ou vice versa</p></td>
<td><p>Non pris en charge</p></td>
<td><p>Non</p></td>
<td><p>Les données de configuration de l’instance cible iSCSI et du disque virtuel, y compris les métadonnées d’instantané, sont perdues lors de la conversion.</p></td>
</tr>
</tbody>
</table>

## <a name="network-limits"></a>Limites du réseau

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
<th><p>Appliquée?</p></th>
<th><p>Commentaire</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Nombre maximal de cartes réseau actives</p></td>
<td><p>8</p></td>
<td><p>Non</p></td>
<td><p>S’applique aux cartes réseau dédiées au trafic iSCSI, plutôt qu’au nombre total de cartes réseau dans l’appliance.</p></td>
</tr>
<tr class="even">
<td><p>Portail (adresses IP) pris en charge</p></td>
<td><p>64</p></td>
<td><p>Oui</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Vitesse du port réseau</p></td>
<td><p>1Gbps, 10 Gbits/s, 40Gbps, 56 Gbits/s (Windows Server 2012 R2 et versions ultérieures uniquement)</p></td>
<td><p>Non</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>IPv4/IPv6</p></td>
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
<td><p>Tirez parti d’un grand nombre d’envois (segmentation), d’une somme de contrôle, d’une modération et d’un déchargement RSS</p></td>
</tr>
<tr class="odd">
<td><p>déchargement iSCSI</p></td>
<td><p>Non pris en charge</p></td>
<td><br/><p>N/A</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Trames Jumbo</p></td>
<td><p>Prise en charge</p></td>
<td><p>N/A</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Sécurité</p></td>
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

## <a name="iscsi-virtual-disk-limits"></a>limites des disques virtuels iSCSI

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
<th><p>Appliquée?</p></th>
<th><p>Commentaire</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>À partir d’un initiateur iSCSI convertissant le disque virtuel d’un disque de base en disque dynamique </p></td>
<td><p>Oui</p></td>
<td><p>Non</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Format de disque dur virtuel</p></td>
<td><p>. vhdx (Windows Server 2012 R2 et versions ultérieures uniquement)</p>
<p>.vhd</p></td>
<td></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Taille de format minimale du disque dur virtuel</p></td>
<td><p>vhdx 3 Mo</p>
<p>virtuels 8 MO</p></td>
<td><p>Oui</p></td>
<td><p>S’applique à tous les types de disques durs virtuels pris en charge : parent, différenciation et fixe.</p></td>
</tr>
<tr class="even">
<td><p>Taille maximale du disque dur virtuel parent</p></td>
<td><p>vhdx 64 To</p>
<p>virtuels 2 TO</p></td>
<td><p>Oui</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Taille maximale du VHD fixe</p></td>
<td><p>vhdx 64 To</p>
<p>virtuels 16 TO</p></td>
<td><p>Oui</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Taille maximale du disque dur virtuel de différenciation</p></td>
<td><p>vhdx 64 To</p>
<p>virtuels 2 TO</p></td>
<td><p>Oui</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Format fixe de disque dur virtuel</p></td>
<td><p>Prise en charge</p></td>
<td><p>Non</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Format de différenciation VHD</p></td>
<td><p>Prise en charge</p></td>
<td><p>Non</p></td>
<td><p>Les instantanés ne peuvent pas être pris en compte sur des disques virtuels iSCSI basés sur un disque dur virtuel de différenciation.</p></td>
</tr>
<tr class="odd">
<td><p>Nombre de disques durs virtuels de différenciation par disque dur virtuel parent</p></td>
<td><p>256</p></td>
<td><p>Non (Oui sur Windows Server 2012)</p></td>
<td><p>Deux niveaux de profondeur (fichiers. vhdx enfants) sont le maximum pour les fichiers. vhdx ; un niveau de profondeur (fichiers. vhd enfants) est le maximum pour les fichiers. vhd.</p></td>
</tr>
<tr class="even">
<td><p>Format dynamique VHD</p></td>
<td><p>vhdx Oui</p>
<p>virtuels Oui (non sur Windows Server 2012)</p></td>
<td><p>Oui</p></td>
<td><p>Annuler la&#39;prise en charge de UNT.</p></td>
</tr>
<tr class="odd">
<td><p>exFAT/FAT32/FAT (volume d’hébergement du disque dur virtuel)</p></td>
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
<td><p>Non-Microsoft CFS</p></td>
<td><p>Non pris en charge</p></td>
<td><p>Oui</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Allocation dynamique</p></td>
<td><p>Non</p></td>
<td><p>N/A</p></td>
<td><p>Les disques durs virtuels dynamiques sont pris en&#39;charge, mais ne sont pas pris en charge.</p></td>
</tr>
<tr class="odd">
<td><p>Réduction d’unité logique</p></td>
<td><p>Oui (Windows Server 2012 R2 et versions ultérieures uniquement)</p></td>
<td><p>N/A</p></td>
<td><p>Utilisez <a href="https://docs.microsoft.com/powershell/module/iscsitarget/resize-iscsivirtualdisk">Resize-iSCSIVirtualDisk</a> pour réduire un numéro d’unité logique.</p></td>
</tr>
<tr class="even">
<td><p>Clonage d’unité logique</p></td>
<td><p>Non pris en charge</p></td>
<td><p>N/A</p></td>
<td><p>Vous pouvez cloner rapidement les données de disque à l’aide de disques durs virtuels de différenciation.</p></td>
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
<td><p>Captures instantanées accessibles en écriture</p></td>
<td><p>Non pris en charge</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Instantané : convertir en valeur complète</p></td>
<td><p>Non pris en charge</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Instantané – restauration en ligne</p></td>
<td><p>Non pris en charge</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Instantané : conversion en écriture</p></td>
<td><p>Non pris en charge</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Redirection d’instantané</p></td>
<td><p>Non pris en charge</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Épinglage d’instantané</p></td>
<td><p>Non pris en charge</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Montage local</p></td>
<td><p>Prise en charge</p></td>
<td><p>Les disques virtuels iSCSI montés localement sont déconseillés-pour plus d’informations, voir <a href="https://technet.microsoft.com/library/dn303411.aspx">fonctionnalités supprimées ou déconseillées dans Windows Server 2012 R2</a>. Les instantanés de disque dynamique ne peuvent pas être montés localement.</p></td>
</tr>
</tbody>
</table>

## <a name="iscsi-target-server-manageability-and-backup"></a>gestion et sauvegarde du serveur cible iSCSI

Si vous souhaitez créer des clichés instantanés de volume (instantanés Open-file) de données sur des disques virtuels iSCSI à partir d’un serveur d’applications, ou si vous souhaitez gérer des disques virtuels iSCSI avec une application plus ancienne (telle que la commande DiskRAID) qui requiert un matériel VDS (Virtual Disk Service) Installez le fournisseur de stockage cible iSCSI sur le serveur à partir duquel vous souhaitez prendre un instantané ou utiliser une application de gestion VDS.

Le fournisseur de stockage cible iSCSI est un service de rôle dans Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012. vous pouvez également télécharger et installer des [fournisseurs de stockage cible iSCSI (VDS/VSS) pour les serveurs d’applications de niveau](http://www.microsoft.com/download/details.aspx?id=34759) supérieur sur les systèmes d’exploitation suivants, à condition que le serveur cible iSCSI s’exécute sur Windows Server 2012 :

  - Windows Storage Server 2008 R2

  - Windows Server 2008 R2

  - Windows HPC Server 2008 R2

  - Windows HPC Server 2008

Notez que si le serveur cible iSCSI est hébergé par un serveur exécutant Windows Server 2012 R2 ou version ultérieure et que vous souhaitez utiliser VSS ou VDS à partir d’un serveur distant, le serveur distant doit également exécuter la même version de Windows Server et disposer du rôle de fournisseur de stockage cible iSCSI. e installé. Notez également que, sur toutes les versions de Windows, vous devez installer une seule version du service de rôle fournisseur de stockage cible iSCSI.

Pour plus d’informations sur le fournisseur de stockage cible iSCSI, consultez [fournisseur de stockage cible iSCSI (VDS/VSS)](http://blogs.technet.com/b/filecab/archive/2012/10/08/iscsi-target-storage-vds-vss-provider.aspx).

## <a name="tested-compatibility-with-iscsi-initiators"></a>Compatibilité testée avec les initiateurs iSCSI

Nous avons testé le logiciel du serveur cible iSCSI avec les initiateurs iSCSI suivants :

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
<td><p>VMWare ESXi 5,0</p></td>
<td><p>Validé</p></td>
<td></td>
<td></td>
</tr>
<tr class="even">
<td><p>VMWare ESX 4,1</p></td>
<td><p>Validé</p></td>
<td></td>
<td></td>
</tr>
<tr class="odd">
<td><p>CentOS 6. x</p></td>
<td><p>Validé</p></td>
<td></td>
<td><p>Doit déconnecter une session et se reconnecter pour détecter un disque virtuel redimensionné.</p></td>
</tr>
<tr class="even">
<td><p>Red Hat Enterprise Linux 6</p></td>
<td><p>Validé</p></td>
<td></td>
<td></td>
</tr>
<tr class="odd">
<td><p>RedHat Enterprise Linux 5 et 5</p></td>
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
<td><p>Oracle Solaris 11. x</p></td>
<td><p>Validé</p></td>
<td></td>
<td></td>
</tr>
</tbody>
</table>

Nous avons également testé les initiateurs iSCSI suivants effectuant un démarrage sans disque à partir de disques virtuels hébergés par un serveur cible iSCSI :

  - Windows Server 2012 R2

  - Windows Server 2012

  - Carte réseau PCIe avec iPXE

  - CD ou disque USB avec iPXE

## <a name="see-also"></a>Voir aussi

La liste suivante propose des ressources supplémentaires sur le serveur cible iSCSI et les technologies connexes.

- [Vue d’ensemble du stockage par blocs de cibles iSCSI](iscsi-target-server.md)

- [Vue d’ensemble du démarrage de cible iSCSI](iscsi-boot-overview.md)

- [Storage](../storage.md) (Stockage)

