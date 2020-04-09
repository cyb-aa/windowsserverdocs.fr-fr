---
title: 'Ksetup : domaine'
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2ef766e3-6071-44f2-946b-22ea5b61a508
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bfaa8a37ae4ee5c9669b09f27a73b3d016324dea
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841582"
---
# <a name="ksetupdomain"></a>Ksetup : domaine



Définit le nom de domaine pour toutes les opérations Kerberos. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
ksetup /domain <DomainName>
```

#### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<DomainName >|Nom du domaine auquel vous souhaitez établir une connexion. Utilisez le nom de domaine complet ou une forme simple du nom, par exemple contoso.com ou contoso.|

## <a name="remarks"></a>Notes

None.

## <a name="examples"></a><a name=BKMK_Examples></a>Illustre

Établissez une connexion à un domaine valide, tel que Microsoft, à l’aide de la sous-commande/mapuser :
```
ksetup /mapuser principal@realm domain-user /domain domain-name
```
Une fois la connexion établie, vous recevrez un nouveau TGT ou un ticket TGT existant sera actualisé.

## <a name="additional-references"></a>Références supplémentaires

-   [Ksetup](ksetup.md)
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)