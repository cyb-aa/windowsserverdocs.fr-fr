---
title: DiskPart
description: La rubrique commandes Windows pour **diskpart**, qui vous permet de gérer les lecteurs de votre ordinateur.
ms.prod: windows-server
ms.technology: storage
author: jasongerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: eb2921d8da4a4a29c4f700107ef5b6d7bfb41481
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122536"
---
# <a name="diskpart"></a>DiskPart

>S’applique à : Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 et Windows Server 2008 R2, Windows Server 2008

Les commandes DiskPart vous aident à gérer les lecteurs de votre ordinateur (disques, partitions, volumes ou disques durs virtuels).

Avant de pouvoir utiliser les commandes DiskPart, vous devez d’abord Lister, puis sélectionner un objet pour lui attribuer le focus. Quand un objet a le focus, toutes les commandes DiskPart que vous tapez agiront sur cet objet.

## <a name="list-available-objects"></a>Répertorier les objets disponibles

Vous pouvez répertorier les objets disponibles et déterminer le numéro ou la lettre de lecteur d’un objet à l’aide de :

- `list disk` : affiche tous les disques de l’ordinateur.

- `list volume` : affiche tous les volumes sur l’ordinateur.

- `list partition`-affiche les partitions sur le disque qui a le focus sur l’ordinateur.

- `list vdisk` : affiche tous les disques virtuels sur l’ordinateur.

Lorsque vous utilisez les commandes de **liste** , un astérisque (*) s’affiche en regard de l’objet qui a le focus.

## <a name="determine-focus"></a>Déterminer le focus

Lorsque vous sélectionnez un objet, le focus reste sur cet objet jusqu’à ce que vous sélectionnez un autre objet. Par exemple, si le focus est défini sur le disque 0 et que vous sélectionnez Volume 8 sur le disque 2, le focus passe du disque 0 au disque 2, volume 8.

Certaines commandes modifient automatiquement le focus. Par exemple, lorsque vous créez une nouvelle partition, le focus bascule automatiquement vers la nouvelle partition.

Vous ne pouvez attribuer le focus qu’à une partition sur le disque sélectionné. Lorsqu’une partition a le focus, le volume associé (le cas échéant) a également le focus. Quand un volume a le focus, le disque et la partition associés ont également le focus si le volume est mappé à une partition spécifique unique. Si ce n’est pas le cas, le fait de se concentrer sur le disque et la partition est perdu.

## <a name="diskpart-commands"></a>Commandes DiskPart

Pour démarrer l’interpréteur de commandes DiskPart, à l’invite de commandes, tapez :

```
diskpart
```

> [!IMPORTANT]
> Pour exécuter DiskPart, vous devez être dans le groupe **administrateurs** local ou un groupe disposant d’autorisations similaires.

Vous pouvez exécuter les commandes suivantes à partir de l’interpréteur de commandes DiskPart :

| Commande | Description |
| ------- | ----------- |
| [Proactive](active.md) | Marque la partition du disque avec le focus, comme étant active. |
| [Ajouter](add.md) | Met en miroir le volume simple ayant le focus sur le disque spécifié. |
| [Assignés](assign.md) | Affecte une lettre de lecteur ou un point de montage au volume qui a le focus. |
| [Attacher vdisk](attach-vdisk.md) | Joint (parfois appelé montage ou surfaces) un disque dur virtuel (VHD) afin qu’il apparaisse sur l’ordinateur hôte en tant que lecteur de disque dur local. |
| [Attributs](attributes.md) | Affiche, définit ou efface les attributs d’un disque ou d’un volume. |
| [Montage automatique](automount.md) | Active ou désactive la fonctionnalité de montage automatique. | 
| [Saut](break.md) | Divise le volume en miroir avec le focus en deux volumes simples. |
| [Claire](clean.md) | Supprime toute mise en forme de partition ou de volume du disque qui a le focus. |
| [Compact vdisk](compact-vdisk.md) | Réduit la taille physique d’un fichier de disque dur virtuel (VHD) de taille dynamique. |
| [Passer](convert.md) | Convertit les volumes FAT (File Allocation Table) et FAT32 dans le système de fichiers NTFS, en laissant intacts les fichiers et les répertoires existants. |
| [Créés](create.md) | Crée une partition sur un disque, un volume sur un ou plusieurs disques ou un disque dur virtuel (VHD). |
| [Supprimer](delete.md) | Supprime une partition ou un volume. |
| [Détacher vdisk](detach-vdisk.md) | Arrête l’affichage du disque dur virtuel sélectionné en tant que lecteur de disque dur local sur l’ordinateur hôte. |
| [Détail](detail.md) | Affiche des informations sur le disque, la partition, le volume ou le disque dur virtuel sélectionné (VHD). |
| [Quitter](exit.md) | Quitte l’interpréteur de commandes DiskPart. |
| [Développer vdisk](expand-vdisk.md) | développe un disque dur virtuel (VHD) à la taille que vous spécifiez. |
| [Étendre](extend.md) | Étend le volume ou la partition avec le focus, ainsi que son système de fichiers, à l’espace libre (non alloué) sur un disque. |
| [Systèmes](filesystems.md) | Affiche des informations sur le système de fichiers actuel du volume qui a le focus et répertorie les systèmes de fichiers pris en charge pour la mise en forme du volume. |
| [Format](format.md) | Met en forme un disque pour accepter des fichiers Windows. |
| [GPT](gpt.md) | Attribue le ou les attributs GPT à la partition avec le focus sur des disques GPT (GUID partition table) de base. |
| [Aide](help.md) | Affiche la liste des commandes disponibles ou des informations d’aide détaillées sur une commande spécifiée. |
| [Importer](import.md) | Importe un groupe de disques étrangers dans le groupe de disques de l’ordinateur local. |
| [Inactive](inactive.md) | Marque la partition système ou la partition de démarrage qui a le focus comme étant inactive sur les disques MBR (Master Boot Record) de base. |
| [Tarifs](list.md) | Affiche la liste des disques, des partitions sur un disque, des volumes d’un disque ou des disques durs virtuels (VHD). |
| [Merge vdisk](merge-vdisk.md) | Fusionne un disque dur virtuel de différenciation avec son disque dur virtuel parent correspondant. |
| [Hors connexion](offline.md) | Met un disque ou un volume en ligne à l’état hors connexion. |
| [Service](online.md) | Met un disque ou un volume hors connexion à l’État en ligne. |
| [Récupérer](recover.md) | Actualise l’état de tous les disques d’un groupe de disques, tente de récupérer les disques d’un groupe de disques non valide et resynchronise les volumes en miroir et les volumes RAID-5 qui contiennent des données obsolètes. |
| [Livr](rem.md) | Fournit un moyen d’ajouter des commentaires à un script. |
| [Supprimer](remove.md) | Supprime une lettre de lecteur ou un point de montage d’un volume. |
| [Résolution](repair.md) | Répare le volume RAID-5 actif en remplaçant la région du disque défaillant par le disque dynamique spécifié. |
| [Relancer](rescan.md) | Localise les nouveaux disques qui ont pu être ajoutés à l’ordinateur. |
| [Maintien](retain.md) | Prépare un volume simple dynamique existant à utiliser comme volume de démarrage ou de système. |
| [Système](san.md) | Affiche ou définit la stratégie de réseau de zone de stockage (San) pour le système d’exploitation. |
| [Sélectionné](select.md) | Déplace le focus sur un disque, une partition, un volume ou un disque dur virtuel (VHD). |
| [ID d’ensemble](set-id.md) | Modifie le champ type de partition de la partition qui a le focus. |
| [Réduire](shrink.md) | Réduit la taille du volume sélectionné en spécifiant la quantité spécifiée. |
| [Quei](uniqueid.md) | Affiche ou définit l’identificateur de table de partition GUID (GPT) ou la signature d’enregistrement de démarrage principal (MBR) pour le disque qui a le focus. |

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [Vue d’ensemble de la gestion des disques](https://docs.microsoft.com/windows-server/storage/disk-management/overview-of-disk-management)

- [Applets de commande de stockage dans Windows PowerShell](https://docs.microsoft.com/powershell/module/storage/)
