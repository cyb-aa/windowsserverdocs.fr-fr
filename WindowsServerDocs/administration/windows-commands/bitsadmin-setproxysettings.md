---
title: bitsadmin setproxysettings
description: Rubrique de référence pour la commande Bitsadmin setproxysettings n', qui définit les paramètres de proxy pour le travail spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: be8edb1b-614e-4d0b-a8f8-64b4bde3e64b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7f7c54b3081c85756735d921fb70f726ba60d833
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720490"
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
| travail | Nom complet ou GUID du travail. |
| usage | Définit l’utilisation du proxy, y compris :<ul><li>**PRECONFIG.** Utilisez les paramètres par défaut d’Internet Explorer du propriétaire.</li><li>**NO_PROXY.** N’utilisez pas de serveur proxy.</li><li>**Remplacer.** Utilisez une liste de proxy explicite et une liste de contournement. La liste de proxy et les informations de contournement du proxy doivent suivre.</li><li>**Détection automatique.** Détecte automatiquement les paramètres du proxy.</li></ul> |
| list | Utilisé lorsque le paramètre *usage* est défini sur override. Doit contenir une liste délimitée par des virgules de serveurs proxy à utiliser. |
| ignorer | Utilisé lorsque le paramètre *usage* est défini sur override. Doit contenir une liste délimitée par des espaces de noms d’hôtes ou d’adresses IP, ou les deux, pour lesquels les transferts ne doivent pas être routés via un proxy. Cela peut consister `<local>` à faire référence à tous les serveurs sur le même réseau local. Les valeurs NULL peuvent être utilisées pour une liste de contournement vide du proxy. |

## <a name="examples"></a>Exemples

Pour définir les paramètres de proxy à l’aide des différentes options d’utilisation de la tâche nommée *myDownloadJob*:

```
bitsadmin /setproxysettings myDownloadJob PRECONFIG
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

- [commande Bitsadmin](bitsadmin.md)
