---
title: Bitsadmin gethelpertokenflags
description: La rubrique commandes Windows pour **Bitsadmin gethelpertokenflags** -retourne les indicateurs d’utilisation d’un jeton d’assistance associé à une tâche de transfert bits.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 25d667736d5fdcd018f557b2a5565b94898f6e51
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381569"
---
# <a name="bitsadmin-gethelpertokenflags"></a>Bitsadmin gethelpertokenflags

Retourne les indicateurs d’utilisation d’un [jeton d’assistance](/windows/desktop/bits/helper-tokens-for-bits-transfer-jobs)  que est associé à une tâche de transfert bits.

**BITS 3,0 et versions antérieures**: Non pris en charge.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /GetHelperTokenFlags <Job>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|

## <a name="remarks"></a>Notes

Les valeurs de retour possibles sont les suivantes.

- 0x0001. Le jeton d’assistance permet d’ouvrir le fichier local d’un travail de chargement, de créer ou de renommer le fichier temporaire d’un travail de téléchargement, ou de créer ou de renommer le fichier de réponse d’une tâche de chargement-réponse.
- 0x0002. Le jeton d’assistance permet d’ouvrir le fichier distant d’un travail de chargement ou de téléchargement SMB (Server Message Block), ou en réponse à un serveur HTTP ou à un problème de proxy pour les informations d’identification NTLM ou Kerberos implicites. Vous devez appeler/SetCredentialsJob TargetScheme NULL NULL pour permettre l’envoi des informations d’identification via HTTP.

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
