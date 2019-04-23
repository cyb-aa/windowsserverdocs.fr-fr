---
title: nslookup set timeout
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 07afdaf4-ffec-496f-a188-4e91cf1a28f8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5b7a35a4a8e9e9cc10ea5548875f710fb5a86036
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837810"
---
# <a name="nslookup-set-timeout"></a>nslookup set timeout

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

modifie le nombre initial de secondes à attendre une réponse à une demande de recherche.
## <a name="syntax"></a>Syntaxe
```
set timeout=<Number>
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|<Number>|Spécifie le nombre de secondes à attendre une réponse. Le nombre de secondes d’attente par défaut est 5.|
|{aide &#124; ?}|Affiche un résumé de **nslookup** sous-commandes.|
## <a name="remarks"></a>Notes
-   Une réponse à une demande n’est pas reçue dans la période spécifiée, le délai d’attente est doublée et la demande est envoyée à nouveau. Vous pouvez utiliser la **nouvelle tentative ensemble** commande pour contrôler le nombre de tentatives.
## <a name="BKMK_examples"></a>Exemples
L’exemple suivant définit le délai d’attente pour l’obtention d’une réponse à 2 secondes :
```
set timeout=2
```
## <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[nslookup définie de nouvelles tentatives](nslookup-set-retry.md)
