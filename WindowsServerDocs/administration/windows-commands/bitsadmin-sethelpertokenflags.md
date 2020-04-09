---
title: Bitsadmin sethelpertokenflags
description: La rubrique commandes Windows pour Bitsadmin sethelpertokenflags, qui définit les indicateurs d’utilisation d’un jeton d’assistance associé à une tâche de transfert BITS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: c644e82026cfc1d62f3fb5d20e3925002b871036
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849492"
---
# <a name="bitsadmin-sethelpertokenflags"></a>Bitsadmin sethelpertokenflags

Définit les indicateurs d’utilisation d’un [jeton d’assistance](/windows/desktop/bits/helper-tokens-for-bits-transfer-jobs) associé à une tâche de transfert bits.

**BITS 3,0 et versions antérieures**: non pris en charge.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /SetHelperTokenFlags <Job> <Flags>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail.|
|Flags|Les valeurs possibles sont les suivantes. 0x0001&mdash;le jeton d’assistance permet d’ouvrir le fichier local d’un travail de chargement, de créer ou de renommer le fichier temporaire d’un travail de téléchargement, ou de créer ou de renommer le fichier de réponse d’une tâche de chargement-réponse. 0x0002&mdash;le jeton d’assistance est utilisé pour ouvrir le fichier distant d’un travail de chargement ou de téléchargement SMB (Server Message Block), ou en réponse à un serveur HTTP ou à un problème de proxy pour les informations d’identification NTLM ou Kerberos implicites. Vous devez appeler `/SetCredentialsJob TargetScheme NULL NULL` pour permettre l’envoi des informations d’identification via HTTP.|

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
