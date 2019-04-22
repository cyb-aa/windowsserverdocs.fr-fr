---
title: Bitsadmin sethelpertokenflags
description: Rubrique de commandes de Windows pour **bitsadmin sethelpertokenflags** -définit les indicateurs de l’utilisation d’un jeton d’assistance qui est associé à une tâche de transfert BITS.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: cc9652afe73476041aa42e64671885bfc1af9628
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813860"
---
# <a name="bitsadmin-sethelpertokenflags"></a>Bitsadmin sethelpertokenflags

Définit les indicateurs de l’utilisation d’un [jeton d’application d’assistance](/windows/desktop/bits/helper-tokens-for-bits-transfer-jobs) qui est associé à une tâche de transfert BITS.

**BITS 3.0 et versions antérieures**: Non pris en charge.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /SetHelperTokenFlags <Job> <Flags>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom d’affichage ou le GUID du travail.|
|Flags|Les valeurs possibles sont les suivantes. 0 x 0001&mdash;le jeton d’assistance est utilisé pour ouvrir le fichier local d’une tâche de téléchargement, pour créer ou de renommer le fichier temporaire d’une tâche de téléchargement, ou pour créer ou de renommer le fichier de réponse d’une tâche de chargement-réponse. 0 x 0002&mdash;le jeton d’assistance est utilisé pour ouvrir le fichier à distance d’un téléchargement de Server Message Block (SMB) ou la tâche de téléchargement, ou en réponse à une stimulation de serveur ou le proxy HTTP pour implicite NTLM ou Kerberos des informations d’identification. Vous devez appeler `/SetCredentialsJob TargetScheme NULL NULL` pour autoriser les informations d’identification d’être envoyés via HTTP.|

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
