---
title: repair-bde
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 534dca1a-05f7-4ea8-ac24-4fe5f14f988a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9e2bce0c0a0f12a3a171c161a669c903044e8b4b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813540"
---
# <a name="repair-bde"></a>repair-bde



Accède à des données chiffrées sur un disque dur gravement endommagé si le lecteur a été chiffré à l’aide de BitLocker. Repair-bde peut reconstruire les parties critiques du lecteur et récupérer les données récupérables dès lors qu'un mot de passe ou une clé de récupération est utilisée pour déchiffrer les données. Si les données de métadonnées BitLocker sur le lecteur sont endommagées, vous devez être en mesure de fournir un package de clés de sauvegarde en plus du mot de passe de récupération ou de la clé de récupération. Ce package de clés est sauvegardé dans les Services de domaine Active Directory (AD DS) si vous avez utilisé le paramètre par défaut pour la sauvegarde des services AD DS. Avec ce package de clés et le mot de passe de récupération ou clé de récupération, vous pouvez déchiffrer des parties d’un lecteur protégé par BitLocker si le disque est endommagé. Chaque package de clés fonctionnera uniquement pour un lecteur qui dispose de l’identificateur de lecteur correspondante. Vous pouvez utiliser la [visionneuse de mot de passe de récupération BitLocker pour Active Directory](https://technet.microsoft.com/library/dd875531(v=ws.10).aspx) pour obtenir ce package de clés à partir d’AD DS.

> [!NOTE]
> La visionneuse de mot de passe de récupération BitLocker est incluse comme l’une des fonctionnalités de gestion facultatif pouvant être installées à l’aide de gestion de serveur sur Windows Server 2012.

Les limitations suivantes existent pour l’outil de ligne de commande Repair-bde :
-   Repair-bde ne peut pas réparer un lecteur qui a échoué pendant le processus de chiffrement ou déchiffrement.
-   Repair-bde suppose que si le lecteur dispose d’un chiffrement, puis le lecteur a été entièrement chiffré.

Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
repair-bde <InputVolume> <OutputVolumeorImage> [-rk] [–rp] [-pw] [–kp] [–lf] [-f] [{-?|/?}]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<InputVolume>|Identifie la lettre de lecteur du lecteur chiffré par BitLocker que vous souhaitez réparer. La lettre de lecteur doit inclure un signe deux-points ; par exemple : **C:**.|
|\<OutputVolumeorImage>|Identifie le lecteur sur lequel stocker le contenu du lecteur réparé. Toutes les informations sur le lecteur de sortie seront remplacées.|
|-rk|Identifie l’emplacement de la clé de récupération doit être utilisée pour déverrouiller le volume. Cette commande peut également être spécifiée en tant que **- recoverykey**.|
|-rp|Identifie le mot de passe de récupération numérique doit être utilisé pour déverrouiller le volume. Cette commande peut également être spécifiée en tant que **- recoverypassword**.|
|-pw|Identifie le mot de passe qui doit être utilisé pour déverrouiller le volume. Cette commande peut également être spécifiée en tant que **-mot de passe**|
|-kp|Identifie le package de clé de récupération peut être utilisé pour déverrouiller le volume. Cette commande peut également être spécifiée en tant que **- keypackage**.|
|-lf|Spécifie le chemin d’accès au fichier qui stockera Repair-bde messages d’erreur, avertissement et informations. Cette commande peut également être spécifiée en tant que **- logfile**.|
|-f|Force un volume à démonter même s’il ne peut pas être verrouillé. Cette commande peut également être spécifiée en tant que **-forcer**.|
|-? ou /?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

Si le chemin d’accès à un package de clés n’est pas spécifié, **repair-bde** recherchera le lecteur pour un package de clés. Toutefois, si le disque dur a été endommagé, **repair-bde** ne peut pas être en mesure de trouver le package et vous invitera à fournir le chemin d’accès.

## <a name="BKMK_Examples"></a>Exemples

L’exemple suivant tente de réparer le lecteur C et écrire le contenu du lecteur C sur le lecteur D en utilisant le fichier clé de récupération (RecoveryKey.bek) stocké sur le lecteur F et écrit les résultats de cette tentative dans le fichier journal (log.txt) sur le lecteur Z.
```
repair-bde C: D: -rk F:\RecoveryKey.bek –lf Z:\log.txt
```
L’exemple suivant tente de réparer le lecteur C et écrire le contenu sur le lecteur C sur le lecteur D avec le mot de passe de récupération de 48 chiffres spécifié. Le mot de passe de récupération doit être tapé dans les huit blocs de six chiffres avec un trait d’union en séparant chaque bloc.
```
repair-bde C: D: -rp 111111-222222-333333-444444-555555-666666-777777-888888
```
L’exemple suivant force à démonter le lecteur C, puis tente de réparer le lecteur C et écrire le contenu sur le lecteur C sur le lecteur D en utilisant le package de clés de récupération et le fichier de clé de récupération (RecoveryKey.bek) stockées sur le lecteur F.
```
repair-bde C: D: -kp F:\RecoveryKeyPackage -rk F:\RecoveryKey.bek -f
```
L’exemple suivant tente de réparer le lecteur C et écrire le contenu du lecteur C sur le lecteur D et que vous devez taper un mot de passe pour déverrouiller le lecteur C: lorsque vous y êtes invité :
```
repair-bde C: D: -pw
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)