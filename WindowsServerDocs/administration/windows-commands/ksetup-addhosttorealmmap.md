---
title: ksetup:addhosttorealmmap
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 237742d5-fa68-466c-b97e-636f489248ea
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 25cf258309c94f0efde980018dd5dcf3c7df4d60
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837500"
---
# <a name="ksetupaddhosttorealmmap"></a>ksetup:addhosttorealmmap



Ajoute un mappage de nom principal (SPN) de service entre l’hôte indiqué et le domaine. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
ksetup /addhosttorealmmap <HostName> <RealmName>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<HostName>|Le nom d’hôte est le nom d’ordinateur, et il peut être établi comme nom de domaine complet de l’ordinateur.|
|\<RealmName>|Le nom de domaine est indiqué comme un nom DNS en majuscules, telles que CORP. CONTOSO.COM.|

## <a name="remarks"></a>Notes

Cette commande vous permet de mapper un ou plusieurs hôtes qui partagent le même suffixe DNS pour le domaine.

Le mappage est enregistré dans le Registre **HKEY_LOCAL_MACHINE\SYSTEM\CurrentContolSet\Lsa\Kerberos\HostToRealm**.

## <a name="BKMK_Examples"></a>Exemples

Dans le cadre de la configuration du domaine CONTOSO, mappez l’ordinateur hôte IPops897 au domaine :
```
ksetup /addhosttorealmmap IPops897 CONTOSO
```
Dans le Registre, vérifiez que le mappage est comme prévu.

#### <a name="additional-references"></a>Références supplémentaires

-   [Ksetup:delhosttorealmmap](ksetup-delhosttorealmmap.md)
-   [Ksetup](ksetup.md)
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)