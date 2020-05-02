---
title: bootcfg copy
description: Rubrique de référence pour la commande bootcfg Copy, qui fait une copie d’une entrée de démarrage existante, à laquelle vous pouvez ajouter des options de ligne de commande.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2a236c2a-8675-444d-b695-9cbc9aff643b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 033227ab1d9efcdf1d58708a75085067766a6af2
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82709686"
---
# <a name="bootcfg-copy"></a>bootcfg copy

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Effectue une copie d’une entrée de démarrage existante, à laquelle vous pouvez ajouter des options de ligne de commande.

## <a name="syntax"></a>Syntaxe

```
bootcfg /copy [/s <computer> [/u <domain>\<user> /p <password>]] [/d <description>] [/id <osentrylinenum>]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| `/s <computer>` | Spécifie le nom ou l’adresse IP d’un ordinateur distant (n’utilisez pas de barres obliques inverses). La valeur par défaut est l'ordinateur local. |
| `/u <domain>\<user>`  | Exécute la commande avec les autorisations de compte de l’utilisateur spécifié `<user>` par `<domain>\<user>`ou. Par défaut, il s’agit des autorisations de l’utilisateur actuellement connecté sur l’ordinateur qui émet la commande. |
| `/p <password>` | Spécifie le mot de passe du compte d’utilisateur spécifié dans le paramètre **/u** . |
| `/d <description>` | Spécifie la description de la nouvelle entrée du système d’exploitation. |
| `/id <osentrylinenum>` | Spécifie le numéro de ligne d’entrée du système d’exploitation dans la section [Operating Systems] du fichier Boot. ini auquel les options de chargement du système d’exploitation sont ajoutées. La première ligne après l’en-tête de la section [Operating Systems] est 1. |
| /? | Affiche l'aide à l'invite de commandes. |

## <a name="examples"></a>Exemples

Pour copier l’entrée de démarrage 1 et entrez \ABC Server \ comme Description :

```
bootcfg /copy /d \ABC Server\ /id 1
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande bootcfg](bootcfg.md)
