---
title: La commande verbose
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fcf30ae7-818f-4e7e-a083-a1812682032b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6a655ccdbd95b2f3523babecaa713ccdf99f9ec7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827240"
---
# <a name="the-verbose-command"></a>La commande verbose



Affiche la sortie détaillée d’une commande spécifiée. Vous pouvez utiliser **/verbose** avec toutes les autres commandes WDSUTIL que vous exécutez. Notez que vous devez spécifier **/verbose** et **/progression** directement après **WDSUTIL**.

## <a name="syntax"></a>Syntaxe

```
WDSUTIL /verbose <commands>
```

## <a name="examples"></a>Exemples

Pour supprimer des ordinateurs approuvés à partir de la base de données d’ajout automatique et afficher une sortie détaillée, tapez :
```
WDSUTIL /Verbose /progress /Delete-AutoAddDevices /Server:MyWDSServer /DeviceType:ApprovedDevices
```