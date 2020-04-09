---
title: bitsadmin getreplyfilename
description: La rubrique commandes Windows pour **Bitsadmin getreplyfilename**, qui obtient le chemin d’accès du fichier qui contient la réponse de chargement du serveur pour le travail.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 85447184-1732-4816-a365-2e3599551bf8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 541a6e60d641405b5da2e65fecbbbe87468c8702
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850492"
---
# <a name="bitsadmin-getreplyfilename"></a>bitsadmin getreplyfilename

Obtient le chemin d’accès du fichier qui contient la réponse de chargement du serveur pour le travail.

> [!NOTE]
> Cette commande n’est pas prise en charge par BITS 1,2 et versions antérieures.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /getreplyfilename <job>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| le travail | Nom complet ou GUID du travail. |


## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant récupère le nom de fichier de réponse de chargement pour la tâche nommée *myDownloadJob*.

```
C:\>bitsadmin /getreplyfilename myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)