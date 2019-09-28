---
title: Commandes DiskPart
ms.prod: windows-server
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 0826b773927f09cc846fb1cfdf4d5dfbf75d5cca
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377827"
---
# <a name="diskpart-commands"></a>Commandes DiskPart

S'applique à : Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 et Windows Server 2008 R2, Windows Server 2008

Les commandes DiskPart vous aident à gérer les lecteurs de votre PC (disques, partitions, volumes ou disques durs virtuels). Avant de pouvoir utiliser les commandes DiskPart, vous devez d’abord Lister, puis sélectionner un objet pour lui attribuer le focus. Quand un objet a le focus, toutes les commandes DiskPart que vous tapez agiront sur cet objet.

Vous pouvez répertorier les objets disponibles et déterminer le numéro ou la lettre de lecteur d’un objet à l’aide des commandes **List Disk, List volume, List partition**et **List vdisk** . Les commandes **List Disk, List vdisk** et **List volume** affichent tous les disques et volumes présents sur l’ordinateur. Toutefois, la commande **List partition** affiche uniquement les partitions sur le disque qui a le focus. Lorsque vous utilisez les commandes de **liste** , un astérisque (\*) s’affiche en regard de l’objet qui a le focus.

Lorsque vous sélectionnez un objet, le focus reste sur cet objet jusqu’à ce que vous sélectionnez un autre objet. Par exemple, si le focus est défini sur le disque 0 et que vous sélectionnez Volume 8 sur le disque 2, le focus passe du disque 0 au disque 2, volume 8. Certaines commandes modifient automatiquement le focus. Par exemple, lorsque vous créez une nouvelle partition, le focus bascule automatiquement vers la nouvelle partition.

Vous ne pouvez attribuer le focus qu’à une partition sur le disque sélectionné. Lorsqu’une partition a le focus, le volume associé (le cas échéant) a également le focus. Quand un volume a le focus, le disque et la partition associés ont également le focus si le volume est mappé à une partition spécifique unique. Si ce n’est pas le cas, le fait de se concentrer sur le disque et la partition est perdu.

## <a name="diskpart-commands"></a>Commandes DiskPart

Pour démarrer l’interpréteur de commandes DiskPart, à l’invite de commandes, tapez :

`diskpart`

> [!IMPORTANT]
> L’appartenance au groupe **administrateurs** local, ou équivalent, est la condition minimale requise pour exécuter Diskpart. 

Vous pouvez exécuter les commandes suivantes dans l’interpréteur de commandes DiskPart :

  - [Proactive](active.md)  
      
  - [Ajouter](add.md)  
      
  - [Assignés](assign.md)  
      
  - [Attacher vdisk](attach-vdisk.md)  
      
  - [Attributs](attributes.md)  
      
  - [Montage automatique](automount.md)  
      
  - [Saut](break.md)  
      
  - [Claire](clean.md)  
      
  - [Compact vdisk](compact-vdisk.md)  
      
  - [Passer](convert.md)  
      
  - [Créés](create.md)  
      
  - [Supprimer](delete.md)  
      
  - [Détacher vdisk](detach-vdisk.md)  
      
  - [Détail](detail.md)  
      
  - [Quitter](exit.md)  
      
  - [Développer vdisk](expand-vdisk.md)  
      
  - [Étendre](extend.md)  
      
  - [Systèmes](filesystems.md)  
      
  - [Format](format.md)  
      
  - [GPT](gpt.md)  
      
  - [Aide](help.md)  
      
  - [Importer](import.md)  
      
  - [Inactive](inactive.md)  
      
  - [Tarifs](list.md)  
      
  - [Merge vdisk](merge-vdisk.md)  
      
  - [Hors connexion](offline.md)  
      
  - [Service](online.md)  
      
  - [Récupérer](recover.md)  
      
  - [Livr](rem.md)  
      
  - [Supprimer](remove.md)  
      
  - [Résolution](repair.md)  
      
  - [Relancer](rescan.md)  
      
  - [Maintien](retain.md)  
      
  - [Système](san.md)  
      
  - [Sélectionné](select.md)  
      
  - [ID d’ensemble](set-id.md)  
      
  - [Réduire](shrink.md)  
      
  - [Quei](uniqueid.md)  
      

## <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

[Applets de commande de stockage dans Windows PowerShell](https://docs.microsoft.com/powershell/module/storage/)
