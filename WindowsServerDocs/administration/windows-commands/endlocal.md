---
title: endlocal
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 765fae3c-0c0a-4639-99a4-cf613489b949
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4958c5419ed4f6374f7c6ecf09bdf67f61134d93
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80845112"
---
# <a name="endlocal"></a>endlocal



Met fin à la localisation des modifications de l’environnement dans un fichier de commandes et restaure les variables d’environnement à leurs valeurs avant l’exécution de la commande **setlocal** correspondante.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
endlocal
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   La commande **endlocal** n’a aucun effet en dehors d’un script ou d’un fichier de commandes.
-   Il existe une commande **endlocal** implicite à la fin d’un fichier de commandes.
-   Si les extensions de commande sont activées (les extensions de commande sont activées par défaut), la commande **endlocal** restaure l’état des extensions de commande (c’est-à-dire activées ou désactivées) à ce qu’elles étaient avant l’exécution de la commande **setlocal** correspondante.

> [!NOTE]
> Pour plus d’informations sur l’activation et la désactivation des extensions de commande, consultez [cmd](cmd.md).

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

Vous pouvez localiser des variables d’environnement dans un fichier de commandes. Par exemple, le programme suivant démarre le programme de traitement par lots superapp sur le réseau, dirige la sortie vers un fichier et affiche le fichier dans le bloc-notes :
```
@echo off
setlocal
path=g:\programs\superapp;%path%
call superapp>c:\superapp.out
endlocal
start notepad c:\superapp.out
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)