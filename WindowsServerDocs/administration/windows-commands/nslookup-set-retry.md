---
title: nslookup set retry
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 615fdfa2-fa29-47a8-8c9e-a6c5b45b3b71
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 63d72a45c33da099c5936d625b27aa71ef002280
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857660"
---
# <a name="nslookup-set-retry"></a>nslookup set retry

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Définit le nombre de tentatives.
## <a name="syntax"></a>Syntaxe
```
set retry=<Number>
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|<Number>|Spécifie la nouvelle valeur pour le nombre de tentatives. Le nombre de nouvelles tentatives par défaut est 4.|
|{aide &#124; ?}|Affiche un résumé de **nslookup** sous-commandes.|
## <a name="remarks"></a>Notes
-   Une réponse à une demande n’est pas reçue dans un certain laps de temps, le délai d’attente est doublé et la demande est renvoyée. La valeur de nouvelle tentative contrôle le nombre de fois où une demande est renvoyée avant d’abandonner. Vous pouvez modifier le délai d’attente avec le **définir le délai d’attente** sous-commande.
## <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[nslookup définie le délai d’expiration](nslookup-set-timeout.md)
