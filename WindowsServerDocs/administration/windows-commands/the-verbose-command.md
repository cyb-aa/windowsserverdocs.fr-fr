---
title: commentaires
description: Rubrique relative aux commandes Windows pour verbose, qui affiche la sortie détaillée pour une commande spécifiée.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fcf30ae7-818f-4e7e-a083-a1812682032b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f67a4fe99a5051e8844e6386d87911946a808c1e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80832842"
---
# <a name="verbose"></a>commentaires

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