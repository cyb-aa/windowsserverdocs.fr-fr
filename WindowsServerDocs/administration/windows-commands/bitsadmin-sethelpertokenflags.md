---
title: Bitsadmin sethelpertokenflags
description: La rubrique commandes Windows pour **Bitsadmin sethelpertokenflags**, qui définit les indicateurs d’utilisation d’un jeton d’assistance associé à une tâche de transfert bits.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 77fac03dba2bb0686a98206405622e2eb398953e
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122973"
---
# <a name="bitsadmin-sethelpertokenflags"></a>Bitsadmin sethelpertokenflags

Définit les indicateurs d’utilisation d’un [jeton d’assistance](https://docs.microsoft.com/windows/win32/bits/helper-tokens-for-bits-transfer-jobs) associé à une tâche de transfert bits.

> [!NOTE]
> Cette commande n’est pas prise en charge par BITS 3,0 et versions antérieures.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /sethelpertokenflags <job> <flags>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| le travail | Nom complet ou GUID du travail. |
| flags | Valeurs de jeton d’assistance possibles, notamment :<ul><li>**0x0001.** Permet d’ouvrir le fichier local d’un travail de chargement, de créer ou de renommer le fichier temporaire d’un travail de téléchargement, ou de créer ou de renommer le fichier de réponse d’une tâche de chargement-réponse.</li><li>**0x0002.** Permet d’ouvrir le fichier distant d’un travail de chargement ou de téléchargement SMB (Server Message Block), ou en réponse à un serveur HTTP ou à un problème de proxy pour les informations d’identification NTLM ou Kerberos implicites.</li></ul>Vous devez appeler `/setcredentialsjob targetscheme null null` pour envoyer les informations d’identification via HTTP. |

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
