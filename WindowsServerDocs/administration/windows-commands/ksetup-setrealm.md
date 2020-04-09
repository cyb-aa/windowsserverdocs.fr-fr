---
title: 'Ksetup : setrealm'
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ab268c40-276b-46ef-ab16-d5ce7667fbed
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: acdbfaabe341c8efb19c6e9d183022375f679de7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841312"
---
# <a name="ksetupsetrealm"></a>Ksetup : setrealm



Définit le nom d’un domaine Kerberos. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
ksetup /setrealm <DNSDomainName>
```

#### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<nom_domaine_dns >|Le nom de domaine DNS peut se présenter sous la forme d’un nom de domaine complet ou d’un nom de domaine simple.|

## <a name="remarks"></a>Notes

Le paramètre de nom de domaine DNS doit être entré en majuscules. Dans le cas contraire, la commande **Ksetup** vous demandera de poursuivre la vérification.

La définition du domaine Kerberos sur un contrôleur de domaine n’est pas prise en charge. Si vous tentez de le faire, vous obtiendrez un avertissement et un échec de la commande.

## <a name="examples"></a><a name=BKMK_Examples></a>Illustre

Définissez le domaine de cet ordinateur sur un nom de domaine spécifique pour limiter l’accès par un contrôleur non-domaine uniquement au domaine Kerberos CONTOSO :
```
ksetup /setrealm CONTOSO
```

## <a name="additional-references"></a>Références supplémentaires

-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Ksetup](ksetup.md)
-   [Ksetup:removerealm](ksetup-removerealm.md)