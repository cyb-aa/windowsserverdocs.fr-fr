---
title: gérer-bde déverrouiller
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7852bf7d-9102-40be-adcb-71e8f4dfde72
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a86267890449be2048221940e5955e49f30f99f3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814600"
---
# <a name="manage-bde-unlock"></a>gérer-bde : déverrouiller



Déverrouille un lecteur protégé par BitLocker à l’aide d’un mot de passe de récupération ou une clé de récupération. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
manage-bde -unlock {-recoverypassword <Password>|-recoverykey <PathToExternalKeyFile>} <Drive> [-certificate {-cf PathToCertificateFile | -ct CertificateThumbprint} {-pin}] [-password] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Value|Description|
|---------|-----|-----------|
|-recoverypassword||Spécifie qu’un mot de passe de récupération sera utilisée pour déverrouiller le lecteur. Abréviation : - rp|
||\<Mot de passe >|Représente le mot de passe de récupération qui peut être utilisé pour déverrouiller le lecteur.|
|-recoverykey||Spécifie qu’un fichier de clé de récupération externe sera être utilisé pour déverrouiller le lecteur. Abréviation : - rk|
||\<PathToExternalKeyFile>|Représente le fichier de clé de récupération externe qui peut être utilisé pour déverrouiller le lecteur.|
||\<Drive>|Représente une lettre de lecteur suivie par un signe deux-points.|
|-certificat||Le certificat d’utilisateur local pour un certificat de BitLocker dans unclock le volume se trouve dans le magasin de certificats d’utilisateur local. Abréviation :-cert|
||<-cf PathToCertificateFile>|Chemin d’accès au fichier cerficate|
||<-ct CertificateThumbprint >|Empreinte numérique du certificat qui peut éventuellement inclure le code confidentiel (-code confidentiel).|
|-password||Affiche une invite pour le mot de passe déverrouiller le volume. Abréviation : - mot de passe|
|-computername||Spécifie que gérer-bde.exe permet de modifier la protection BitLocker sur un autre ordinateur. Abréviation : - cn|
||\<Name>|Représente le nom de l’ordinateur sur lequel modifier la protection BitLocker. Valeurs acceptées incluent le nom NetBIOS de l’ordinateur et l’adresse IP de l’ordinateur.|
|-? ou /?||Affiche un résumé aide à l’invite de commandes.|
|-help ou-h||Affiche une aide complète à l’invite de commandes.|

## <a name="BKMK_Examples"></a>Exemples

L’exemple suivant illustre l’utilisation de la **-déverrouiller** commande pour déverrouiller le lecteur E avec un fichier de clé de récupération qui a été enregistré dans un dossier de sauvegarde sur un autre lecteur.
```
manage-bde –unlock E: -recoverykey "F:\Backupkeys\recoverykey.bek"
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Manage-bde](manage-bde.md)