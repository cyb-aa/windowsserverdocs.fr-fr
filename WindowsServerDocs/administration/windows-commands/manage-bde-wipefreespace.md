---
title: gérer-bde WipeFreeSpace
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b8d83a2a-c5c8-4019-9041-23d1d6abf282
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8750094e7357a3aefa307d24abd1470fbf8d2a71
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867170"
---
# <a name="manage-bde-wipefreespace"></a>gérer-bde : WipeFreeSpace



Effacer le contenu de l’espace libre sur le volume de suppression de tous les fragments de données peuvent avoir existé dans l’espace. Exécution de cette commande sur un volume qui a été chiffré à l’aide de le « espace utilisé uniquement ? méthode de chiffrement fournit le même niveau de protection en tant que la « Full Volume Encryption ? méthode de chiffrement. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
manage-bde –WipeFreeSpace|-w [<Drive>] [-Cancel] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<Drive>|Représente une lettre de lecteur suivie par un signe deux-points, un chemin d’accès GUID de volume ou un volume monté.|
|-Annuler|Annule une réinitialisation d’espace libre est en cours.|
|-computername|Spécifie que gérer-bde.exe permet de modifier la protection BitLocker sur un autre ordinateur. Vous pouvez également utiliser **- cn** comme une version abrégée de cette commande.|
|\<Name>|Représente le nom de l’ordinateur sur lequel modifier la protection BitLocker. Valeurs acceptées incluent le nom NetBIOS de l’ordinateur et l’adresse IP de l’ordinateur.|
|-? ou /?|Affiche un résumé aide à l’invite de commandes.|
|-help ou-h|Affiche une aide complète à l’invite de commandes.|

## <a name="BKMK_Examples"></a>Exemples

L’exemple suivant illustre l’utilisation de la **-w** commande pour créer réinitialiser l’espace libre sur le lecteur C.
```
manage-bde -w C:
```
L’exemple suivant illustre l’utilisation de la **-w** commande avec le **– Annuler** paramètre pour annuler la réinitialisation de l’espace libre sur le lecteur C.
```
manage-bde -w -Cancel C:
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Manage-bde](manage-bde.md)