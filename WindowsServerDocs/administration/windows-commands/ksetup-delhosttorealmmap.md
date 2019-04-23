---
title: ksetup:delhosttorealmmap
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3faee482-a96c-4614-86fd-aaa446643ec4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cf01edc4932fd5ec1cf98043de04286b3a100a34
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882340"
---
# <a name="ksetupdelhosttorealmmap"></a>ksetup:delhosttorealmmap



Supprime un mappage de nom principal (SPN) de service entre l’hôte indiqué et le domaine. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
ksetup /delhosttorealmmap <HostName> <RealmName>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<HostName>|Le nom d’hôte est le nom d’ordinateur, et il peut être établi comme nom de domaine complet de l’ordinateur.|
|\<RealmName>|Le nom de domaine est indiqué comme un nom DNS en majuscules, telles que CORP. CONTOSO.COM.|

## <a name="remarks"></a>Notes

Lorsqu’un ordinateur hôte à un domaine (ou plusieurs hôtes au domaine) mappage existe, cette commande supprime ce mappage.

Le mappage est enregistré dans le Registre **HKEY_LOCAL_MACHINE\SYSTEM\CurrentContolSet\Lsa\Kerberos\HostToRealm**. Vous devez vérifier le mappage dans le Registre après l’utilisation de cette commande.

## <a name="BKMK_Examples"></a>Exemples

Modification de la configuration du domaine CONTOSO, de supprimer le mappage de l’ordinateur hôte IPops897 au domaine :
```
ksetup /delhosttorealmmap IPops897 CONTOSO
```
Après avoir exécuté cette commande, vous pouvez vérifier dans le Registre que le mappage est comme prévu.

#### <a name="additional-references"></a>Références supplémentaires

-   [Ksetup:addhosttorealmmap](ksetup-addhosttorealmmap.md)
-   [Ksetup](ksetup.md)
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)