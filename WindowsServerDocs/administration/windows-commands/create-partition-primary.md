---
title: create partition primary
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6d652d8e-3935-4a91-8ced-b17c0e7937be
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bbbb4b89540b7264fe907f79ca003117429da08e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881710"
---
# <a name="create-partition-primary"></a>create partition primary

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crée une partition principale sur le disque de base avec le focus.  
  
> [!CAUTION]  
  
  
  
## <a name="syntax"></a>Syntaxe  
  
```  
create partition primary [size=<n>] [offset=<n>] [id={ <byte> | <guid> }] [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Paramètres  
  
|Paramètre|Description|  
|-------|--------|  
|size\=<n>|Spécifie la taille de la partition en mégaoctets \(Mo\). Si aucune taille n’est donnée, la partition se poursuit jusqu'à ce que plus aucun espace non alloué dans la région actuelle.|  
|offset\=<n>|Le décalage en kilo-octets \(Ko\), à laquelle la partition est créée. Si aucun décalage n’est fourni, la partition démarre au début de la plus grande étendue de disque qui est suffisamment grande pour contenir l’il.|  
|align\=<n>|Aligne toutes les étendues de partition à la limite d’alignement le plus proche. Généralement utilisé avec le matériel RAID numéro d’unité logique \(LUN\) tableaux pour améliorer les performances. <n> est le nombre de kilo-octets \(Ko\) à partir du début du disque à la limite d’alignement le plus proche.|  
|id\={ <byte> &#124; <guid> }|Spécifie le type de partition. Ce paramètre est destiné au fabricant \(OEM\) utiliser uniquement. N’importe quel octet de type de partition ou le GUID peut être spécifié avec ce paramètre. DiskPart ne vérifie pas le type de partition pour la validité, sauf pour vous assurer qu’il s’agit d’un octet dans un format hexadécimal ou d’un GUID. **Attention :** Création de partitions avec ce paramètre peut provoquer l’échec ou Impossible de démarrer. Sauf si vous êtes un fabricant OEM ou un INFORMATICIEN professionnel expérimenté en disques gpt, ne créez pas de partitions sur les disques gpt à l’aide de ce paramètre. Au lieu de cela, utilisez toujours le **créer la partition efi** commande pour créer des partitions système EFI, le **créer de partition msr** commande pour créer des partitions Microsoft Reserved et le **créer partition principale** commande \(sans le **id\={octets&#124;guid}** paramètre\) pour créer des partitions principales sur les disques gpt.<br /><br />**Disques d’enregistrement de démarrage principal**<br /><br />pour l’enregistrement de démarrage principal \(MBR\) disques, vous spécifiez un octet de type de partition, au format hexadécimal, pour la partition. Si ce paramètre n’est pas spécifié pour un disque MBR, la commande crée une partition de type 0 x 06, ce qui indique qu’un système de fichiers n’est pas installé. Par exemple :<br /><br />-Partition de données LDM : 0 x 42<br />-partition de récupération : 0x27<br />-Reconnue partition OEM : 0x12, 0x84, 0xDE, 0xFE, 0xA0<br /><br />**Disques de table de partition GUID**<br /><br />pour la table de partition GUID \(gpt\) disques, vous pouvez spécifier un type de partition GUID pour la partition que vous souhaitez créer. GUID reconnus est les suivants :<br /><br />-Partition système EFI : c12a7328\-f81f\-11d 2\-ba4b\-00a0c93ec93b<br />-Microsoft Reserved partition : e3c9e316\-0b5c\-4db8\-d 817\-f92df00215ae<br />-Partition de de base de données : ebd0a0a2\-b9e5\-4433\-c 87 0\-68b6b72699c7<br />-Partition de métadonnées LDM sur un disque dynamique : 5808c8aa\-7e8f\-42e0\-85d2\-e1e90434cfb3<br />-Partition de données LDM sur un disque dynamique : af9b60a0\-1431\-4f62\-bc68\-3311714a69ad<br />-   recovery partition: de94bba4\-06d1\-4d40\-a16a\-bfd50179d6ac<br /><br />Si ce paramètre n’est pas spécifié pour un disque gpt, la commande crée une partition de base de données.|  
|NOERR|Pour les scripts uniquement. Lorsqu’une erreur est rencontrée, DiskPart continue à traiter les commandes comme si l’erreur ne s’est pas produite. Sans le paramètre noerr, une erreur provoque la fermeture avec un code d’erreur de DiskPart.|  
  
## <a name="remarks"></a>Notes  
  
-   Après avoir créé la partition, le focus passe automatiquement à la nouvelle partition.  
  
-   La partition ne reçoit pas une lettre de lecteur. Vous devez utiliser le **affecter** commande DiskPart pour attribuer une lettre de lecteur à la partition.  
  
-   Un disque de base doit être sélectionné pour cette opération réussisse. Utilisez le **sélectionnez disque** commande pour sélectionner un disque de base et de déplacer le focus vers elle.  
  
## <a name="BKMK_examples"></a>Exemples  
Pour créer une partition principale de 1 000 mégaoctets taille, tapez :  
  
```  
create partition primary size=1000  
```  
  
#### <a name="additional-references"></a>Références supplémentaires  
[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)  
  

  

