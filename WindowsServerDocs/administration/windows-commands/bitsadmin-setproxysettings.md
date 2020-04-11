---
title: bitsadmin setproxysettings
description: La rubrique commandes Windows pour **Bitsadmin setproxysettings n'** , qui définit les paramètres de proxy pour le travail spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: be8edb1b-614e-4d0b-a8f8-64b4bde3e64b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0ea92383d9bd09372d21d3c1da84db060b0a9958
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122754"
---
# <a name="bitsadmin-setproxysettings"></a>bitsadmin setproxysettings

Définit les paramètres de proxy pour le travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /setproxysettings <job> <usage> [list] [bypass]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| le travail | Nom complet ou GUID du travail. |
| usage | Définit l’utilisation du proxy, y compris :<ul><li>**PRECONFIG.** Utilisez les paramètres par défaut d’Internet Explorer du propriétaire.</li><li>**NO_PROXY.** N’utilisez pas de serveur proxy.</li><li>**Remplacer.** Utilisez une liste de proxy explicite et une liste de contournement. La liste de proxy et les informations de contournement du proxy doivent suivre.</li><li>**Détection automatique.** Détecte automatiquement les paramètres du proxy.</li></ul> |
| list | Utilisé lorsque le paramètre *usage* est défini sur override. Doit contenir une liste délimitée par des virgules de serveurs proxy à utiliser. |
| Omettre | Utilisé lorsque le paramètre *usage* est défini sur override. Doit contenir une liste délimitée par des espaces de noms d’hôtes ou d’adresses IP, ou les deux, pour lesquels les transferts ne doivent pas être routés via un proxy. Cela peut être `<local>` pour faire référence à tous les serveurs sur le même réseau local. Les valeurs NULL peuvent être utilisées pour une liste de contournement vide du proxy. |

## <a name="examples"></a>Exemples

L’exemple suivant définit les paramètres de proxy pour la tâche nommée *myDownloadJob*.

```
C:\>bitsadmin /setproxysettings myDownloadJob PRECONFIG
```

```
bitsadmin /setproxysettings myDownloadJob NO_PROXY
```
```
bitsadmin /setproxysettings myDownloadJob OVERRIDE proxy1:80
```

```
bitsadmin /setproxysettings myDownloadJob OVERRIDE proxy1,proxy2,proxy3 NULL
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)