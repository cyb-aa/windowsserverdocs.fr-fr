---
title: bitsadmin replaceremoteprefix
description: 'Rubrique relative aux commandes Windows pour **Bitsadmin REPLACEREMOTEPREFIX** : tous les fichiers du travail dont l’URL distante commence par *OldPrefix* sont modifiés pour utiliser *NewPrefix*.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d0e0abb1-bdb4-4c74-abbc-16c809f5fd81
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ee896a337b571487797967d3ce0bf1f1b17e7507
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380799"
---
# <a name="bitsadmin-replaceremoteprefix"></a>bitsadmin replaceremoteprefix

Tous les fichiers du travail dont l’URL distante commence par *OldPrefix* sont modifiés pour utiliser *NewPrefix*.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /ReplaceRemotePrefix <Job> <OldPrefix> <NewPrefix
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|
|OldPrefix|Préfixe d’URL existant|
|NewPrefix|Nouveau préfixe d’URL|

## <a name="examples"></a>Exemples

L’exemple suivant modifie tous les fichiers du travail nommé *myDownloadJob* dont l’URL distante commence par *http://stageserver* à *http://prodserver* .

```
C:\>bitsadmin /ReplaceRemotePrefix myDownloadJob http://stageserver http://prodserver
```

## <a name="additional-information"></a>Informations complémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)