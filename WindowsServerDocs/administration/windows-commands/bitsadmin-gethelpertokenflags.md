---
title: Bitsadmin gethelpertokenflags
description: La rubrique commandes Windows pour **Bitsadmin gethelpertokenflags**, qui retourne les indicateurs d’utilisation d’un jeton d’assistance associé à une tâche de transfert bits.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 37e40ea1f71795e0c975988169577deb205c3335
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850662"
---
# <a name="bitsadmin-gethelpertokenflags"></a>Bitsadmin gethelpertokenflags

Retourne les indicateurs d’utilisation d’un [jeton d’assistance](https://docs.microsoft.com/windows/win32/bits/helper-tokens-for-bits-transfer-jobs) associé à une tâche de transfert bits.

> [!NOTE]
> Cette commande n’est pas prise en charge par BITS 3,0 et versions antérieures.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /gethelpertokenflags <job>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| le travail | Nom complet ou GUID du travail. |

## <a name="remarks"></a>Notes

Valeurs de retour possibles, notamment :

- **0x0001.** Le jeton d’assistance permet d’ouvrir le fichier local d’un travail de chargement, de créer ou de renommer le fichier temporaire d’un travail de téléchargement, ou de créer ou de renommer le fichier de réponse d’une tâche de chargement-réponse.

- **0x0002.** Le jeton d’assistance permet d’ouvrir le fichier distant d’un travail de chargement ou de téléchargement SMB (Server Message Block), ou en réponse à un serveur HTTP ou à un problème de proxy pour les informations d’identification NTLM ou Kerberos implicites. Vous devez appeler/SetCredentialsJob TargetScheme NULL NULL pour permettre l’envoi des informations d’identification via HTTP.

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
