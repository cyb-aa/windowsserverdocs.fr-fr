---
title: Vssadmin redimensionner ShadowStorage
description: Description de la commande vssadmin Resize ShadowStorage.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 03/05/2020
ms.localizationpriority: medium
ms.openlocfilehash: 8cd3b5e5629e41702126fb42b815931ff0aea6fa
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830022"
---
# <a name="vssadmin-resize-shadowstorage"></a>Vssadmin redimensionner ShadowStorage

>S’applique à : Windows 10, Windows 8.1, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Redimensionne la quantité maximale d’espace de stockage qui peut être utilisée pour le stockage de clichés instantanés.

La quantité minimale d’espace de stockage pouvant être utilisée pour le stockage de clichés instantanés peut être spécifiée à l’aide de la valeur de Registre **MinDiffAreaFileSize** . Pour plus d’informations, consultez [MinDiffAreaFileSize](https://docs.microsoft.com/windows/win32/backup/registry-keys-for-backup-and-restore#mindiffareafilesize).

> [!WARNING]
> Le redimensionnement de l’Association de stockage peut entraîner la disparition des clichés instantanés.

## <a name="syntax"></a>Syntaxe

```cmd
vssadmin resize shadowstorage /for=<ForVolumeSpec> /on=<OnVolumeSpec> [/maxsize=<MaxSizeSpec>]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---|---|
`/for=<ForVolumeSpec>`  | Spécifie le volume pour lequel la quantité maximale d’espace de stockage doit être redimensionnée.
`/on=<OnVolumeSpec>` | Spécifie le volume de stockage.
`[/maxsize=<MaxSizeSpec>]` |  Spécifie la quantité d’espace maximale pouvant être utilisée pour stocker les clichés instantanés. Si aucune valeur n’est spécifiée pour/MaxSize, il n’y a aucune limite placée sur la quantité d’espace de stockage qui peut être utilisée.  <br> <br> La valeur MaxSizeSpec doit être supérieure ou égale à 1 Mo et doit être exprimée dans l’une des unités suivantes : KB, MB, Go, to, PB ou EB. Si aucune unité n’est spécifiée, MaxSizeSpec utilise des octets par défaut.

## <a name="examples"></a>Exemples

```cmd
vssadmin Resize ShadowStorage /For=C: /On=D: /MaxSize=900MB
vssadmin Resize ShadowStorage /For=C: /On=D: /MaxSize=UNBOUNDED
vssadmin Resize ShadowStorage /For=C: /On=C: /MaxSize=20%
```

## <a name="additional-references"></a>Références supplémentaires

* [Clé de syntaxe de ligne de commande](https://docs.microsoft.com/windows-server/administration/windows-commands/command-line-syntax-key)
* [Vssadmin](vssadmin.md)
