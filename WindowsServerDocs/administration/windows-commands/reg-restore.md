---
title: restauration de reg
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a51f1c0c-969b-4b76-930a-c8bb14dea26e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0025a37ed8ca50b47e7750501a7362659b500537
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858770"
---
# <a name="reg-restore"></a>restauration de reg



Entrées et écrit enregistré les sous-clés de nouveau dans le Registre.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
Reg restore <KeyName> <FileName>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<KeyName>|Spécifie le chemin d’accès complet de la sous-clé à restaurer. L’opération de restauration fonctionne uniquement avec l’ordinateur local. KeyName doit inclure une clé racine valide. Clés racines valides sont : HKLM, HKCU, HKCR, HKU et HKCC.|
|\<FileName>|Spécifie le nom et le chemin d’accès du fichier avec le contenu à écrire dans le Registre. Ce fichier doit être créé à l’avance avec le **reg enregistrer** opération à l’aide d’une extension .hiv.|
|/?|Affiche l’aide de **reg restauration** à l’invite de commandes.|

## <a name="remarks"></a>Notes

-   Avant de modifier les entrées de Registre, enregistrez la sous-clé parente avec la **reg enregistrer** opération. Si la modification échoue, restaurez la sous-clé d’origine avec le **reg restauration** opération.
-   Le tableau suivant répertorie les valeurs de retour pour la **reg restauration** opération.

|Value|Description|
|-----|-----------|
|0|Opération réussie|
|1|Échec|

## <a name="BKMK_examples"></a>Exemples

Pour restaurer le fichier nommé NTRKBkUp.hiv dans la clé HKLM\Software\Microsoft\ResKit et remplacer le contenu existant de la clé, tapez :
```
REG RESTORE HKLM\Software\Microsoft\ResKit NTRKBkUp.hiv
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)