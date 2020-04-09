---
title: charge reg
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3b0b2b1b-f510-4108-9e9d-7057e924aa6e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 140c6b51b9f88081a8686ebebbc9400f241b5ef6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836382"
---
# <a name="reg-load"></a>charge reg



Écrit les sous-clés et les entrées enregistrées dans une autre sous-clé du Registre. Destiné à être utilisé avec les fichiers temporaires utilisés pour la résolution des problèmes ou la modification des entrées de registre.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
reg load KeyName FileName
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<KeyName >|Spécifie le chemin d’accès complet de la sous-clé à charger. Pour spécifier des ordinateurs distants, incluez le nom de l’ordinateur (au format \\\\ComputerName\) dans le *nom*de l’ordinateur. Si vous omettez \\\\ComputerName \, l’opération est effectuée par défaut sur l’ordinateur local. Le *keyName* doit inclure une clé racine valide. Les clés racines valides pour l’ordinateur local sont : HKLM, HKCU, HKCR, HKU et HKCC. Si un ordinateur distant est spécifié, les clés racines valides sont les suivantes : HKLM et HKU.|
|Nom de fichier \<>|Spécifie le nom et le chemin d’accès du fichier à charger. Ce fichier doit être créé à l’avance à l’aide de l’opération **reg save** et de l’extension. HIV.|
|/?|Affiche l’aide pour **reg Load** à l’invite de commandes.|

## <a name="remarks"></a>Notes

Le tableau suivant répertorie les valeurs renvoyées pour l’opération de **chargement de Reg** .

|Valeur|Description|
|-----|-----------|
|0|Opération réussie|
|1|Échec|

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

Pour charger le fichier nommé TempHive. HIV dans la clé HKLM\TempHive, tapez :
```
REG LOAD HKLM\TempHive TempHive.hiv
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)