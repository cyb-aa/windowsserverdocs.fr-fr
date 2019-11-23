---
title: ID d’ensemble
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 5b48cc701716412c4a79cedddb4458c57ba25ad5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384073"
---
# <a name="set-id"></a>ID d’ensemble

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La commande DiskPart Set ID modifie le champ type de partition pour la partition qui a le focus.  
  
> [!IMPORTANT]  
> Cette commande est destinée à être utilisée par les fabricants d’équipements d’origine \(OEM\) uniquement. La modification des champs de type de partition à l’aide de ce paramètre peut entraîner l’échec de votre ordinateur ou l’impossibilité de démarrer. À moins que vous ne soyez un fabricant d’ordinateurs OEM ou expérimenté avec des disques GPT, vous ne devez pas modifier les champs de type de partition sur les disques GPT à l’aide de ce paramètre. Au lieu de cela, utilisez toujours la commande [Create partition EFI](create-partition-efi.md) pour créer des partitions système EFI, la commande [Create partition MSR](create-partition-msr.md) pour créer des partitions réservées Microsoft et la commande [Create partition Primary](create-partition-primary.md) sans le paramètre ID pour créer des partitions principales sur des disques GPT.  
  
  
  
## <a name="syntax"></a>Syntaxe  
  
```  
set id={ <byte> | <GUID> } [override] [noerr]  
```  
  
## <a name="parameters"></a>Paramètres  
  
| Paramètre |                                                                                                                                                                                                                                                                                                                                                                   Description                                                                                                                                                                                                                                                                                                                                                                   |
|-----------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  <byte>   |                                                                                                                                                                                                       pour l’enregistrement de démarrage principal \(disques MBR\), spécifie la nouvelle valeur du champ type, au format hexadécimal, pour la partition. Tous les octets de type de partition peuvent être spécifiés avec ce paramètre, à l’exception du type 0x42, qui spécifie une partition LDM. Notez que le 0x de début est omis lors de la spécification du type de partition hexadécimale.                                                                                                                                                                                                       |
|  <GUID>   | pour la table de partition GUID \(disques GPT\), spécifie la nouvelle valeur GUID du champ type pour la partition. Les GUID reconnus sont les suivants :<br /><br />-Partition système EFI : C12A7328\-F81F\-11D2\-BA4B\-00A0C93EC93B<br />-Partition de données de base : ebd0a0a2\-b9e5\-4433\-87C0\-68b6b72699c7<br /><br />Tout GUID de type de partition peut être spécifié à l’aide de ce paramètre, à l’exception des éléments suivants :<br /><br />-Partition réservée Microsoft : e3c9e316\-0b5c\-4db8\-817d\-f92df00215ae<br />-Partition de métadonnées LDM sur un disque dynamique : 5808c8aa\-7e8f\-42e0\-85d2\-e1e90434cfb3<br />-Partition de données LDM sur un disque dynamique : af9b60a0\-1431\-4f62\-bc68\-3311714a69ad<br />-Partition des métadonnées du cluster : db97dba9\-0840\-4BAE\-97f0\-ffb9a327c7e1 |
| remplacer  |                                                                force le démontage du système de fichiers sur le volume avant de modifier le type de partition. Lorsque vous exécutez la commande **Set ID** , DiskPart tente de verrouiller et démonter le système de fichiers sur le volume. Si la **substitution** n’est pas spécifiée et que l’appel de verrouillage du système de fichiers échoue \(par exemple, comme il existe un handle ouvert\), l’opération échoue. Lorsque **override** est spécifié, DiskPart force le démontage même si l’appel de verrouillage du système de fichiers échoue et si les descripteurs ouverts sur le volume deviennent non valides.<br /><br />Cette commande est disponible uniquement pour Windows 7 et Windows Server 2008 R2.                                                                 |
|   noerr   |                                                                                                                                                                                                                                                                    Utilisé uniquement pour les scripts. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur.                                                                                                                                                                                                                                                                    |
  
## <a name="remarks"></a>Notes  
  
-   Hormis les limitations mentionnées précédemment, DiskPart ne vérifie pas la validité de la valeur que vous spécifiez \(sauf pour s’assurer qu’il s’agit d’un octet au format hexadécimal ou d’un GUID\).  
  
-   Cette commande ne fonctionne pas sur les disques dynamiques ou sur les partitions réservées Microsoft.  
  
## <a name="BKMK_examples"></a>Illustre  
Pour définir le champ de type sur 0x07 et forcer le démontage du système de fichiers, tapez :  
  
```  
set id=0x07 override  
```  
  
Pour définir le champ type sur une partition de données de base, tapez :  
  
```  
set id=ebd0a0a2-b9e5-4433-87c0-68b6b72699c7  
```  
  
#### <a name="additional-references"></a>Références supplémentaires  
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
  

  

