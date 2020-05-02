---
title: verbose
description: Rubrique de référence pour verbose, qui affiche la sortie détaillée pour une commande spécifiée.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fcf30ae7-818f-4e7e-a083-a1812682032b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3563673d1f80167e469d98a664a6f96ca49815a1
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721372"
---
# <a name="verbose"></a>verbose

Affiche la sortie détaillée d’une commande spécifiée. Vous pouvez utiliser **/Verbose** avec les autres commandes WDSUTIL que vous exécutez. Notez que vous devez spécifier **/Verbose** et **/Progress** directement après **WDSUTIL**.

## <a name="syntax"></a>Syntaxe

```
WDSUTIL /verbose <commands>
```

## <a name="examples"></a>Exemples

Pour supprimer des ordinateurs approuvés de la base de données d’ajout automatique et afficher la sortie détaillée, tapez :
```
WDSUTIL /Verbose /progress /Delete-AutoAddDevices /Server:MyWDSServer /DeviceType:ApprovedDevices
```