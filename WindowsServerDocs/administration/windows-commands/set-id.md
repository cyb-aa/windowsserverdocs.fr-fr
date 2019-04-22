---
title: Id de jeu
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5793d7ad-827e-4285-b2c6-ae60eeb0e886
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f95490850acd263fb0b34007ac64a84c9a374865
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822050"
---
# <a name="set-id"></a>Id de jeu

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La commande de définir l’ID Diskpart modifie le champ de type de partition pour la partition avec le focus.  
  
> [!IMPORTANT]  
> Cette commande est conçue pour une utilisation par les fabricants \(OEM\) uniquement. Modification des champs de type de partition avec ce paramètre l’ordinateur risque d’échouer ou d’être impossible de démarrage. Sauf si vous êtes fabricant ou expérimenté en disques gpt, vous ne devez pas modifier les champs de type de partition sur les disques gpt à l’aide de ce paramètre. Au lieu de cela, utilisez toujours le [créer la partition efi](create-partition-efi.md) commande pour créer des partitions système EFI, le [créer de partition msr](create-partition-msr.md) commande pour créer des partitions Microsoft Reserved et le [créer partition principale](create-partition-primary.md) commande sans le paramètre ID pour créer des partitions principales sur les disques gpt.  
  
  
  
## <a name="syntax"></a>Syntaxe  
  
```  
set id={ <byte> | <GUID> } [override] [noerr]  
```  
  
## <a name="parameters"></a>Paramètres  
  
|Paramètre|Description|  
|-------|--------|  
|<byte>|pour l’enregistrement de démarrage principal \(MBR\) disques, spécifie la nouvelle valeur pour le champ de type, au format hexadécimal, pour la partition. N’importe quel octet de type de partition peut être spécifié avec ce paramètre à l’exception de type 0 x 42, qui spécifie une partition LDM. Notez que le x 0 non significatif est omis lorsque vous spécifiez le type de partition hexadécimal.|  
|<GUID>|pour la table de partition GUID \(gpt\) disques, spécifie la nouvelle valeur GUID pour le champ de type pour la partition. GUID reconnus est les suivants :<br /><br />-Partition système EFI : c12a7328\-f81f\-11d 2\-ba4b\-00a0c93ec93b<br />-Partition de de base de données : ebd0a0a2\-b9e5\-4433\-c 87 0\-68b6b72699c7<br /><br />N’importe quel type de partition GUID peut être spécifié avec ce paramètre à l’exception de ce qui suit :<br /><br />-Microsoft Reserved partition : e3c9e316\-0b5c\-4db8\-d 817\-f92df00215ae<br />-Partition de métadonnées LDM sur un disque dynamique : 5808c8aa\-7e8f\-42e0\-85d 2\-e1e90434cfb3<br />-Partition de données LDM sur un disque dynamique : af9b60a0\-1431\-4f62\-bc68\-3311714a69ad<br />-Partition de métadonnées de cluster : db97dba9\-0840\-4bae\-97f0\-ffb9a327c7e1|  
|override|force le système de fichiers sur le volume à démonter avant de modifier le type de partition. Lorsque vous exécutez le **id de jeu** DiskPart tente de verrouiller et de démonter le système de fichiers sur le volume de la commande. Si **remplacer** n’est pas spécifié, et l’appel de verrouiller le système de fichiers échoue \(, par exemple, étant donné qu’un handle ouvert\), l’opération échoue. Lorsque **remplacer** est spécifié, DiskPart force le démontage, même si l’appel de verrouiller le système de fichiers échoue, et tous les descripteurs ouverts sur le volume ne seront plus valides.<br /><br />Cette commande est uniquement disponible pour Windows 7 et Windows Server 2008 R2.|  
|NOERR|Utilisé pour les scripts uniquement. Lorsqu’une erreur est rencontrée, DiskPart continue à traiter les commandes comme si l’erreur ne s’est pas produite. Sans ce paramètre, une erreur provoque la fermeture avec un code d’erreur de DiskPart.|  
  
## <a name="remarks"></a>Notes  
  
-   Autres que les restrictions mentionnées précédemment, DiskPart ne vérifie pas la validité de la valeur que vous spécifiez \(, sauf pour vous assurer qu’il s’agit d’un octet dans un format hexadécimal ou d’un GUID\).  
  
-   Cette commande ne fonctionne pas sur des disques dynamiques ou sur Microsoft Reserved partitions.  
  
## <a name="BKMK_examples"></a>Exemples  
Pour le champ type de la valeur est 0 x 07 et forcer le démontage du système de fichiers, tapez :  
  
```  
set id=0x07 override  
```  
  
Pour définir le champ de type soit une partition de base de données, tapez :  
  
```  
set id=ebd0a0a2-b9e5-4433-87c0-68b6b72699c7  
```  
  
#### <a name="additional-references"></a>Références supplémentaires  
[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)  
  

  

