---
title: Reg Import
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0be103de-08fc-4f02-b590-361782680b3e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0816297e837bbce91ca069e3506405cbdb53c51a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836422"
---
# <a name="reg-import"></a>Reg Import



Copie le contenu d’un fichier qui contient des sous-clés, des entrées et des valeurs de Registre exportées dans le registre de l’ordinateur local.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
Reg import FileName
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Nom de fichier \<>|Spécifie le nom et le chemin d’accès du fichier dont le contenu doit être copié dans le registre de l’ordinateur local. Ce fichier doit être créé à l’avance à l’aide de **reg Export**.|
|/?|Affiche l’aide pour **reg Import** à l’invite de commandes.|

## <a name="remarks"></a>Notes

Le tableau suivant répertorie les valeurs renvoyées pour l’opération **reg Import** .

|Valeur|Description|
|-----|-----------|
|0|Opération réussie|
|1|Échec|

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

Pour importer des entrées de Registre à partir du fichier nommé AppBkUp. reg, tapez :
```
reg import AppBkUp.reg
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)