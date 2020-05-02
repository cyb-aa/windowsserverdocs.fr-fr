---
title: bitsadmin gethelpertokenflags
description: Rubrique de référence pour la commande Bitsadmin gethelpertokenflags, qui retourne les indicateurs d’utilisation d’un jeton d’assistance associé à une tâche de transfert BITS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 23e93ca71915fc369a940a21ce856b14deced004
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717910"
---
# <a name="bitsadmin-gethelpertokenflags"></a>bitsadmin gethelpertokenflags

Retourne les indicateurs d’utilisation d’un  [jeton d’assistance](https://docs.microsoft.com/windows/win32/bits/helper-tokens-for-bits-transfer-jobs)associé à une tâche de transfert bits.

> [!NOTE]
> Cette commande n’est pas prise en charge par BITS 3,0 et versions antérieures.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /gethelpertokenflags <job>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| travail | Nom complet ou GUID du travail. |

### <a name="remarks"></a>Notes 

Valeurs de retour possibles, notamment :

- **0x0001.** Le jeton d’assistance permet d’ouvrir le fichier local d’un travail de chargement, de créer ou de renommer le fichier temporaire d’un travail de téléchargement, ou de créer ou de renommer le fichier de réponse d’une tâche de chargement-réponse.

- **0x0002.** Le jeton d’assistance permet d’ouvrir le fichier distant d’un travail de chargement ou de téléchargement SMB (Server Message Block), ou en réponse à un serveur HTTP ou à un problème de proxy pour les informations d’identification NTLM ou Kerberos implicites. Vous devez appeler `/SetCredentialsJob TargetScheme NULL NULL` pour autoriser l’envoi des informations d’identification via http.
  
## <a name="examples"></a>Exemples

Pour récupérer les indicateurs d’utilisation d’un jeton d’assistance associé à une tâche de transfert BITS nommée *myDownloadJob*:

```
bitsadmin /gethelpertokenflags myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin](bitsadmin.md)
