---
title: Reg Restore
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a51f1c0c-969b-4b76-930a-c8bb14dea26e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e511694247c04f2befc9c0148498e43b85f664ed
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836362"
---
# <a name="reg-restore"></a>Reg Restore



Écrit les sous-clés et les entrées enregistrées dans le registre.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
Reg restore <KeyName> <FileName>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<KeyName >|Spécifie le chemin d’accès complet de la sous-clé à restaurer. L’opération de restauration fonctionne uniquement avec l’ordinateur local. Le KeyName doit inclure une clé racine valide. Les clés racines valides sont : HKLM, HKCU, HKCR, HKU et HKCC.|
|Nom de fichier \<>|Spécifie le nom et le chemin d’accès du fichier contenant le contenu à écrire dans le registre. Ce fichier doit être créé à l’avance avec l’opération d' **enregistrement reg** à l’aide d’une extension. HIV.|
|/?|Affiche l’aide de **reg Restore** à l’invite de commandes.|

## <a name="remarks"></a>Notes

-   Avant de modifier des entrées de Registre, enregistrez la sous-clé parente avec l’opération **reg save** . Si la modification échoue, restaurez la sous-clé d’origine avec l’opération **reg Restore** .
-   Le tableau suivant répertorie les valeurs renvoyées pour l’opération **reg Restore** .

|Valeur|Description|
|-----|-----------|
|0|Opération réussie|
|1|Échec|

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

Pour restaurer le fichier nommé NTRKBkUp. HIV dans la clé HKLM\Software\Microsoft\ResKit, et remplacer le contenu existant de la clé, tapez :
```
REG RESTORE HKLM\Software\Microsoft\ResKit NTRKBkUp.hiv
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)