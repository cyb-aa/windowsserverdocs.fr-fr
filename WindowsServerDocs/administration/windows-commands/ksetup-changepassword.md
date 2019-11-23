---
title: 'Ksetup : ChangePassword'
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 283078e7-a88f-4875-90e6-f8605e6b7ea7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 51be9e71c2b290e6346d23144543e0eec29f9d07
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375181"
---
# <a name="ksetupchangepassword"></a>Ksetup : ChangePassword



Utilise la valeur de mot de passe du centre de distribution de clés (kpasswd) pour modifier le mot de passe de l’utilisateur connecté. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
ksetup /changepassword <OldPasswd> <NewPasswd>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<OldPasswd >|Indique le mot de passe existant de l’utilisateur connecté.|
|\<NewPasswd >|Indique le nouveau mot de passe de l’utilisateur connecté.|

## <a name="remarks"></a>Notes

Cette commande utilise la valeur de mot de passe KDC (kpasswd) pour modifier le mot de passe de l’utilisateur connecté. Le kpasswd, s’il est défini, s’affiche dans la sortie en exécutant la commande **Ksetup/dumpstate** .

Le nouveau mot de passe de l’utilisateur doit satisfaire à toutes les exigences de mot de passe définies sur cet ordinateur.

Si le compte d’utilisateur est introuvable dans le domaine actuel, le système vous demande de fournir le nom de domaine où se trouve le compte d’utilisateur.

Si vous souhaitez forcer une modification de mot de passe lors de la prochaine ouverture de session, cette commande autorise l’utilisation de l’astérisque (*) pour que l’utilisateur soit invité à entrer un nouveau mot de passe.

La sortie de la commande vous informe de l’état de réussite ou d’échec.

## <a name="BKMK_Examples"></a>Illustre

Modifier le mot de passe d’un utilisateur actuellement connecté à cet ordinateur dans ce domaine :
```
ksetup /changepassword Pas$w0rd Pa$$w0rd
```
Modifiez le mot de passe d’un utilisateur actuellement connecté dans le domaine contoso :
```
ksetup /domain CONTOSO /changepassword Pas$w0rd Pa$$w0rd
```
Forcer l’utilisateur actuellement connecté à modifier le mot de passe à la prochaine ouverture de session :
```
ksetup /changepassword Pas$w0rd *
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)