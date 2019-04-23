---
title: Option SET
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4d8d4921-9fdd-4a3c-bb0f-9df5458c4b84
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 81678768bb2b5fcfd7f2f2d067562e78e93dc549
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848850"
---
# <a name="set-option"></a>Option SET



Définit les options de création de clichés instantanés. Si utilisée sans paramètres, **définir option** affiche l’aide à l’invite de commandes.

## <a name="syntax"></a>Syntaxe

```
set option {[differential | plex] [transportable] [[rollbackrecover] [txfrecover] | [noautorecover]]}
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|[différentielle | plex]|Spécifie le type de cliché instantané à créer pour le fournisseur.|
|[transportable]|Spécifie que le cliché instantané doit ne pas être a encore été importé. Le fichier .cab de métadonnées peut être utilisé ultérieurement pour importer le cliché instantané sur le même ou un autre ordinateur.|
|[rollbackrecover]|Signale des writers à utiliser *récupération automatique* pendant la **PostSnapshot** événement. Cela est utile si le cliché instantané doit être utilisé pour la restauration (par exemple, avec l’exploration de données).|
|[txfrecover]|Demandes VSS pour rendre le cliché instantané transactionnellement cohérente lors de la création.|
|[noautorecover]|Les enregistreurs s’arrête et le système de fichiers à partir d’apporter toute modification de récupération pour le cliché instantané à un état transactionnellement cohérent. **Noautorecover** ne peut pas être utilisé avec **txfrecover** ou **rollbackrecover**.|

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)