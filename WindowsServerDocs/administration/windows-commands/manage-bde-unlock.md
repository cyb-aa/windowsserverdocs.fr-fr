---
title: gérer-déverrouiller BDE
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7852bf7d-9102-40be-adcb-71e8f4dfde72
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3fd303116e101c8c8503d220ba382d6cdd994ad3
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724066"
---
# <a name="manage-bde-unlock"></a>Manage-bde : Unlock



Déverrouille un lecteur protégé par BitLocker à l’aide d’un mot de passe de récupération ou d’une clé de récupération.

## <a name="syntax"></a>Syntaxe

```
manage-bde -unlock {-recoverypassword <Password>|-recoverykey <PathToExternalKeyFile>} <Drive> [-certificate {-cf PathToCertificateFile | -ct CertificateThumbprint} {-pin}] [-password] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

#### <a name="parameters"></a>Paramètres

|Paramètre|Valeur|Description|
|---------|-----|-----------|
|-RecoveryPassword||Spécifie qu’un mot de passe de récupération sera utilisé pour déverrouiller le lecteur. Abréviation :-RP|
||\<Mot de passe>|Représente le mot de passe de récupération qui peut être utilisé pour déverrouiller le lecteur.|
|-recoverykey||Spécifie qu’un fichier de clé de récupération externe sera utilisé pour déverrouiller le lecteur. Abréviation :-RK|
||\<PathToExternalKeyFile>|Représente le fichier de clé de récupération externe qui peut être utilisé pour déverrouiller le lecteur.|
||\<Lecteur>|Représente une lettre de lecteur suivie par un signe deux-points.|
|-certificat||Le certificat de l’utilisateur local pour un certificat BitLocker pour désallouer le volume se trouve dans le magasin de certificats de l’utilisateur. Abréviation :-CERT|
||<-CF PathToCertificateFile>|Chemin d’accès au fichier cerficate|
||<-CT CertificateThumbprint>|Empreinte numérique de certificat qui peut éventuellement inclure le code confidentiel (-pin).|
|-password||Affiche une invite pour le mot de passe pour déverrouiller le volume. Abréviation :-PW|
|-ComputerName||Spécifie que Manage-bde. exe sera utilisé pour modifier la protection BitLocker sur un autre ordinateur. Abréviation :-CN|
||\<Name>|Représente le nom de l’ordinateur sur lequel modifier la protection BitLocker. Les valeurs acceptées incluent le nom NetBIOS de l’ordinateur et l’adresse IP de l’ordinateur.|
|-? ou /?||Affiche une brève aide à l’invite de commandes.|
|-Help ou-h||Affiche l’aide complète à l’invite de commandes.|

## <a name="examples"></a>Exemples

Pour illustrer l’utilisation de la commande **-Unlock** pour déverrouiller le lecteur E avec un fichier de clé de récupération qui a été enregistré dans un dossier de sauvegarde sur un autre lecteur.
```
manage-bde –unlock E: -recoverykey F:\Backupkeys\recoverykey.bek
```

## <a name="additional-references"></a>Références supplémentaires

-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Gérer-bde](manage-bde.md)