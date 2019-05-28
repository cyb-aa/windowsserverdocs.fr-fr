---
Title: Commandes DiskPart
ms.prod: windows-server-threshold
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 3cc0667b54dba75d892795f6520664ce7a7a62a5
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192648"
---
# <a name="diskpart-commands"></a>Commandes DiskPart

S'applique à : Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 et Windows Server 2008 R2, Windows Server 2008

Commandes DiskPart vous aident à gérer les lecteurs de votre PC (disques, partitions, volumes ou disques durs virtuels). Avant de pouvoir utiliser les commandes DiskPart, vous devez tout d’abord la liste, puis sélectionnez un objet afin de lui donner le focus. Quand un objet a le focus, toutes les commandes DiskPart que vous tapez seront appliquent à cet objet.

Vous pouvez répertorier les objets disponibles et déterminer la lettre de lecteur ou le nombre d’un objet à l’aide de la **disque de la liste, le volume de la liste, partition de liste**, et **liste vdisk** commandes. Le **disque de la liste, liste vdisk** et **liste volume** commandes affichent tous les disques et volumes sur l’ordinateur. Toutefois, le **liste partition** commande affiche uniquement les partitions sur le disque qui a le focus. Lorsque vous utilisez le **liste** des commandes, un astérisque (\*) s’affiche en regard de l’objet ayant le focus.

Lorsque vous sélectionnez un objet, le focus reste sur cet objet jusqu'à ce que vous sélectionnez un autre objet. Par exemple, si le focus est défini sur le disque 0 et que vous sélectionnez le volume 8 sur le disque 2, le focus passe à partir du disque 0 sur le disque 2, volume 8. Certaines commandes changent automatiquement le focus. Par exemple, lorsque vous créez une nouvelle partition, le focus passe automatiquement à la nouvelle partition.

Vous pouvez uniquement donner le focus à une partition sur le disque sélectionné. Lorsqu’une partition a le focus, le volume associé (le cas échéant) a également le focus. Lorsqu’un volume a le focus, disque et partition ont également le focus si le volume est mappé à une partition spécifique unique. Si ce n’est pas le cas, le focus sur le disque et partition est perdue.

## <a name="diskpart-commands"></a>Commandes DiskPart

Pour démarrer l’interpréteur de commande DiskPart, à l’invite de commandes, tapez :

`diskpart`

> [!IMPORTANT]
> L’appartenance au local **administrateurs** , ou équivalent, est la condition minimale requise pour exécuter DiskPart. 

Vous pouvez exécuter les commandes suivantes dans l’interpréteur de commande Diskpart :

  - [Active](active.md)  
      
  - [Ajouter](add.md)  
      
  - [Assign](assign.md)  
      
  - [attacher vdisk](attach-vdisk.md)  
      
  - [Attributs](attributes.md)  
      
  - [Automount](automount.md)  
      
  - [break](break.md)  
      
  - [Clean](clean.md)  
      
  - [Compact vdisk](compact-vdisk.md)  
      
  - [Convert](convert.md)  
      
  - [Create](create.md)  
      
  - [Supprimer](delete.md)  
      
  - [Détacher vdisk](detach-vdisk.md)  
      
  - [Détail](detail.md)  
      
  - [Quitter](exit.md)  
      
  - [Développez vdisk](expand-vdisk.md)  
      
  - [Étendre](extend.md)  
      
  - [systèmes de fichiers](filesystems.md)  
      
  - [Format](format.md)  
      
  - [GPT](gpt.md)  
      
  - [Aide](help.md)  
      
  - [Importer](import.md)  
      
  - [Inactive](inactive.md)  
      
  - [List](list.md)  
      
  - [Fusion vdisk](merge-vdisk.md)  
      
  - [Offline](offline.md)  
      
  - [Online](online.md)  
      
  - [récupérer](recover.md)  
      
  - [REM](rem.md)  
      
  - [Supprimer](remove.md)  
      
  - [Réparation](repair.md)  
      
  - [Rescan](rescan.md)  
      
  - [Conserver](retain.md)  
      
  - [San](san.md)  
      
  - [Select](select.md)  
      
  - [Id de jeu](set-id.md)  
      
  - [réduction](shrink.md)  
      
  - [Uniqueid](uniqueid.md)  
      

## <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

[Applets de commande de stockage dans Windows PowerShell](https://docs.microsoft.com/en-us/powershell/module/storage/)
