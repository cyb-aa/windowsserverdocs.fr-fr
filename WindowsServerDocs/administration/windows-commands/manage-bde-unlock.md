---
title: gérer-déverrouiller BDE
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 92ed2e00babfad890be83e45827ae8e0080cac40
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373871"
---
# <a name="manage-bde-unlock"></a>Manage-bde : Unlock



Déverrouille un lecteur protégé par BitLocker à l’aide d’un mot de passe de récupération ou d’une clé de récupération. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
manage-bde -unlock {-recoverypassword <Password>|-recoverykey <PathToExternalKeyFile>} <Drive> [-certificate {-cf PathToCertificateFile | -ct CertificateThumbprint} {-pin}] [-password] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Value|Description|
|---------|-----|-----------|
|-RecoveryPassword||Spécifie qu’un mot de passe de récupération sera utilisé pour déverrouiller le lecteur. Abréviation :-RP|
||@no__t 0Password >|Représente le mot de passe de récupération qui peut être utilisé pour déverrouiller le lecteur.|
|-recoverykey||Spécifie qu’un fichier de clé de récupération externe sera utilisé pour déverrouiller le lecteur. Abréviation :-RK|
||@no__t 0PathToExternalKeyFile >|Représente le fichier de clé de récupération externe qui peut être utilisé pour déverrouiller le lecteur.|
||@no__t 0Drive >|Représente une lettre de lecteur suivie par un signe deux-points.|
|-certificat||Le certificat de l’utilisateur local pour un certificat BitLocker pour désallouer le volume se trouve dans le magasin de certificats de l’utilisateur. Abréviation :-CERT|
||<-CF PathToCertificateFile >|Chemin d’accès au fichier cerficate|
||<-CT CertificateThumbprint >|Empreinte numérique de certificat qui peut éventuellement inclure le code confidentiel (-pin).|
|-Password||Affiche une invite pour le mot de passe pour déverrouiller le volume. Abréviation :-PW|
|-ComputerName||Spécifie que Manage-bde. exe sera utilisé pour modifier la protection BitLocker sur un autre ordinateur. Abréviation :-CN|
||\<Name>|Représente le nom de l’ordinateur sur lequel modifier la protection BitLocker. Les valeurs acceptées incluent le nom NetBIOS de l’ordinateur et l’adresse IP de l’ordinateur.|
|-? ou /?||Affiche une brève aide à l’invite de commandes.|
|-Help ou-h||Affiche l’aide complète à l’invite de commandes.|

## <a name="BKMK_Examples"></a>Illustre

L’exemple suivant illustre l’utilisation de la commande **-Unlock** pour déverrouiller le lecteur E avec un fichier de clé de récupération qui a été enregistré dans un dossier de sauvegarde sur un autre lecteur.
```
manage-bde –unlock E: -recoverykey "F:\Backupkeys\recoverykey.bek"
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Gérer-bde](manage-bde.md)