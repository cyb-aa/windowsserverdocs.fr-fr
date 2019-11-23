---
title: 'Ksetup : delhosttorealmmap'
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 70b54aaebc0b7b46c34c6f52e45f6583afd6c477
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375150"
---
# <a name="ksetupdelhosttorealmmap"></a>Ksetup : delhosttorealmmap



Supprime un mappage de nom de principal du service (SPN) entre l’hôte indiqué et le domaine. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
ksetup /delhosttorealmmap <HostName> <RealmName>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Nom d’hôte \<>|Le nom d’hôte est le nom de l’ordinateur et il peut être indiqué comme nom de domaine complet de l’ordinateur.|
|\<RealmName >|Le nom de domaine est indiqué en tant que nom DNS en majuscules, par exemple CORP. CONTOSO.COM.|

## <a name="remarks"></a>Notes

Lorsqu’un mappage hôte à domaine (ou plusieurs hôtes au domaine) existe, cette commande supprime ce mappage.

Le mappage est enregistré dans le registre dans **HKEY_LOCAL_MACHINE \system\currentcontolset\lsa\kerberos\hosttorealm**. Vous devez vérifier le mappage dans le registre après avoir utilisé cette commande.

## <a name="BKMK_Examples"></a>Illustre

La modification de la configuration du domaine CONTOSO, supprimer le mappage de l’ordinateur hôte IPops897 au domaine :
```
ksetup /delhosttorealmmap IPops897 CONTOSO
```
Après avoir exécuté cette commande, vous pouvez vérifier dans le registre que le mappage est prévu.

#### <a name="additional-references"></a>Références supplémentaires

-   [Ksetup:addhosttorealmmap](ksetup-addhosttorealmmap.md)
-   [Ksetup](ksetup.md)
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)