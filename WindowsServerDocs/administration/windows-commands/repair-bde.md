---
title: repair-bde
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 534dca1a-05f7-4ea8-ac24-4fe5f14f988a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 501ab801e76980f7e94e88213dd3aa42ee04d4d7
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722396"
---
# <a name="repair-bde"></a>repair-bde



Accède à des données chiffrées sur un disque dur gravement endommagé si le lecteur a été chiffré à l’aide de BitLocker. Repair-bde peut reconstruire les parties critiques du lecteur et récupérer les données récupérables dès lors qu'un mot de passe ou une clé de récupération est utilisée pour déchiffrer les données. Si les données de métadonnées BitLocker sur le lecteur sont endommagées, vous devez être en mesure de fournir un package de clés de sauvegarde en plus du mot de passe de récupération ou de la clé de récupération. Ce package de clé est sauvegardé dans Active Directory Domain Services (AD DS) si vous avez utilisé le paramètre par défaut pour AD DS sauvegarde. Avec ce package de clé et le mot de passe de récupération ou la clé de récupération, vous pouvez déchiffrer des parties d’un lecteur protégé par BitLocker si le disque est endommagé. Chaque package de clé fonctionne uniquement pour un lecteur qui a l’identificateur de lecteur correspondant. Vous pouvez utiliser la [visionneuse de mot de passe de récupération BitLocker pour Active Directory](https://technet.microsoft.com/library/dd875531(v=ws.10).aspx) pour obtenir ce package de clés à partir de AD DS.

> [!NOTE]
> La visionneuse de mot de passe de récupération BitLocker est incluse sous la forme d’une des fonctionnalités de gestion facultatives installables à l’aide de Server Manage sur Windows Server 2012.

Les limitations suivantes existent pour l’outil en ligne de commande Repair-bde :
-   Repair-bde ne peut pas réparer un lecteur qui a échoué lors du processus de chiffrement ou de déchiffrement.
-   Repair-bde suppose que si le lecteur possède un chiffrement, le lecteur a été entièrement chiffré.



## <a name="syntax"></a>Syntaxe

```
repair-bde <InputVolume> <OutputVolumeorImage> [-rk] [–rp] [-pw] [–kp] [–lf] [-f] [{-?|/?}]
```

#### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<InputVolume>|Identifie la lettre de lecteur du lecteur chiffré par BitLocker que vous souhaitez réparer. La lettre de lecteur doit inclure un signe deux-points ; par exemple : **C :**.|
|\<OutputVolumeorImage>|Identifie le lecteur sur lequel stocker le contenu du lecteur réparé. Toutes les informations sur le lecteur de sortie seront remplacées.|
|-RK|Identifie l’emplacement de la clé de récupération à utiliser pour déverrouiller le volume. Cette commande peut également être spécifiée en tant que **-RecoveryKey**.|
|-RP|Identifie le mot de passe de récupération numérique qui doit être utilisé pour déverrouiller le volume. Cette commande peut également être spécifiée en tant que **-RecoveryPassword**.|
|-PW|Identifie le mot de passe qui doit être utilisé pour déverrouiller le volume. Cette commande peut également être spécifiée en tant que **-Password**|
|-KP|Identifie le package de clé de récupération qui peut être utilisé pour déverrouiller le volume. Cette commande peut également être spécifiée en tant que **-keypackage**.|
|-lf|Spécifie le chemin d’accès au fichier qui stocke les messages d’erreur, d’avertissement et d’information de Repair-bde. Cette commande peut également être spécifiée sous la forme **-logfile**.|
|-f|Force le démontage d’un volume, même s’il ne peut pas être verrouillé. Cette commande peut également être spécifiée en tant que **force**.|
|-? ou /?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes 

Si le chemin d’accès à un package de clé n’est pas spécifié, la commande **Repair-bde** recherche un package de clés sur le lecteur. Toutefois, si le disque dur a été endommagé, **Repair-bde** peut ne pas être en mesure de trouver le package et vous invitera à fournir le chemin d’accès.

## <a name="examples"></a>Exemples

Pour tenter de réparer le lecteur C et d’écrire le contenu du lecteur C sur le lecteur D à l’aide du fichier de clé de récupération (RecoveryKey. bek) stocké sur le lecteur F, puis écrit les résultats de cette tentative dans le fichier journal (log. txt) sur le lecteur Z.
```
repair-bde C: D: -rk F:\RecoveryKey.bek –lf Z:\log.txt
```
Pour tenter de réparer le lecteur C et d’écrire le contenu sur le lecteur C sur le lecteur D en utilisant le mot de passe de récupération à 48 chiffres spécifié. Le mot de passe de récupération doit être tapé dans huit blocs de six chiffres, un trait d’Union séparant chaque bloc.
```
repair-bde C: D: -rp 111111-222222-333333-444444-555555-666666-777777-888888
```
Pour forcer le démontage du lecteur C, puis tente de réparer le lecteur C et d’écrire le contenu sur le lecteur C sur le lecteur D à l’aide du package de clé de récupération et du fichier de clé de récupération (RecoveryKey. bek) stockés sur le lecteur F.
```
repair-bde C: D: -kp F:\RecoveryKeyPackage -rk F:\RecoveryKey.bek -f
```
Pour tenter de réparer le lecteur C et d’écrire le contenu du lecteur C sur le lecteur D, vous devez taper un mot de passe pour déverrouiller le lecteur C : lorsque vous y êtes invité :
```
repair-bde C: D: -pw
```

## <a name="additional-references"></a>Références supplémentaires

-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)