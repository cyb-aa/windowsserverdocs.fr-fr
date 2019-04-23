---
title: ksetup:changepassword
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: f629c6c7930777583df38f5af900ed380ec60f9c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878530"
---
# <a name="ksetupchangepassword"></a>ksetup:changepassword



Utilise la valeur de mot de passe (kpasswd) du centre de Distribution de clés (KDC) pour modifier le mot de passe de l’utilisateur connecté. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
ksetup /changepassword <OldPasswd> <NewPasswd>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<OldPasswd>|Indique le mot de passe existant connecté à l’utilisateur.|
|\<NewPasswd>|États connecté sur le nouveau mot de passe de l’utilisateur.|

## <a name="remarks"></a>Notes

Cette commande utilise la valeur de mot de passe (kpasswd) KDC pour modifier le mot de passe de l’utilisateur connecté. Le kpasswd, si défini, s’affiche dans la sortie en exécutant la **ksetup /dumpstate** commande.

Nouveau mot de passe de l’utilisateur doit répondre à toutes les exigences de mot de passe qui sont définies sur cet ordinateur.

Si le compte d’utilisateur est introuvable dans le domaine actuel, le système vous demandera de fournir le nom de domaine où réside le compte d’utilisateur.

Si vous souhaitez forcer une modification de mot de passe à la prochaine ouverture de session, cette commande permet l’utilisation de l’astérisque (*), l’utilisateur sera invité pour un nouveau mot de passe.

La sortie de la commande vous informe de l’état de réussite ou l’échec.

## <a name="BKMK_Examples"></a>Exemples

Modifier le mot de passe d’un utilisateur actuellement connecté à cet ordinateur dans ce domaine :
```
ksetup /changepassword Pas$w0rd Pa$$w0rd
```
Modifier le mot de passe d’un utilisateur actuellement connecté dans le domaine Contoso :
```
ksetup /domain CONTOSO /changepassword Pas$w0rd Pa$$w0rd
```
Force l’utilisateur actuellement connecté pour modifier le mot de passe à la prochaine ouverture de session :
```
ksetup /changepassword Pas$w0rd *
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)