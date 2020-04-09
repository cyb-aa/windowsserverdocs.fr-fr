---
title: 'Ksetup : addhosttorealmmap'
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 237742d5-fa68-466c-b97e-636f489248ea
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1ee8f434482b0658194daed46b62f6f7f70abae1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841842"
---
# <a name="ksetupaddhosttorealmmap"></a>Ksetup : addhosttorealmmap



Ajoute un mappage de nom de principal du service (SPN) entre l’hôte indiqué et le domaine. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
ksetup /addhosttorealmmap <HostName> <RealmName>
```

#### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Nom d’hôte \<>|Le nom d’hôte est le nom de l’ordinateur et il peut être indiqué comme nom de domaine complet de l’ordinateur.|
|\<RealmName >|Le nom de domaine est indiqué en tant que nom DNS en majuscules, par exemple CORP. CONTOSO.COM.|

## <a name="remarks"></a>Notes

Cette commande vous permet de mapper un hôte ou plusieurs hôtes qui partagent le même suffixe DNS au domaine.

Le mappage est enregistré dans le registre dans **HKEY_LOCAL_MACHINE \system\currentcontolset\lsa\kerberos\hosttorealm**.

## <a name="examples"></a><a name=BKMK_Examples></a>Illustre

Dans le cadre de la configuration du domaine CONTOSO, mappez l’ordinateur hôte IPops897 au domaine :
```
ksetup /addhosttorealmmap IPops897 CONTOSO
```
Dans le registre, vérifiez que le mappage est le même que prévu.

## <a name="additional-references"></a>Références supplémentaires

-   [Ksetup:delhosttorealmmap](ksetup-delhosttorealmmap.md)
-   [Ksetup](ksetup.md)
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)