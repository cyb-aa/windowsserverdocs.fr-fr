---
title: bootcfg raw
description: Rubrique de référence pour la commande bootcfg RAW, qui ajoute des options de chargement du système d’exploitation, spécifiées sous forme de chaîne, à une entrée du système d’exploitation dans la section système d’exploitation du fichier Boot. ini.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e3458749-b0a0-460f-a022-3ff199a71f27
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b9127b278a41bb8f1f36f7b45c544bf09f5c4ff7
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82708953"
---
# <a name="bootcfg-raw"></a>bootcfg raw

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ajoute des options de chargement du système d’exploitation spécifiées sous forme de chaîne à une entrée du système d’exploitation dans la section [Operating Systems] du fichier Boot. ini. Cette commande remplace toutes les options d’entrée existantes du système d’exploitation.

## <a name="syntax"></a>Syntaxe

```
bootcfg /raw [/s <computer> [/u <domain>\<user> /p <password>]] <osloadoptionsstring> [/id <osentrylinenum>] [/a]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| `/s <computer>` | Spécifie le nom ou l’adresse IP d’un ordinateur distant (n’utilisez pas de barres obliques inverses). La valeur par défaut est l'ordinateur local. |
| `/u <domain>\<user>`  | Exécute la commande avec les autorisations de compte de l’utilisateur spécifié `<user>` par `<domain>\<user>`ou. Par défaut, il s’agit des autorisations de l’utilisateur actuellement connecté sur l’ordinateur qui émet la commande. |
| `/p <password>` | Spécifie le mot de passe du compte d’utilisateur spécifié dans le paramètre **/u** . |
| `<osloadoptionsstring>` | Spécifie les options de chargement du système d’exploitation à ajouter à l’entrée du système d’exploitation. Ces options de chargement remplacent les options de chargement existantes associées à l’entrée du système d’exploitation. Il n’y a aucune validation `<osloadoptions>` par rapport au paramètre.
| `/id <osentrylinenum>` | Spécifie le numéro de ligne d’entrée du système d’exploitation dans la section [Operating Systems] du fichier Boot. ini auquel les options de chargement du système d’exploitation sont ajoutées. La première ligne après l’en-tête de la section [Operating Systems] est 1. |
| /a | Spécifie les options de système d’exploitation qui doivent être ajoutées aux options existantes du système d’exploitation. |
| /? | Affiche l'aide à l'invite de commandes. |

## <a name="examples"></a>Exemples

Ce texte doit contenir des options de chargement de système d’exploitation valides telles que **/Debug**, **/fastdetect**, **/nodebug**, **/baudrate**, **/crashdebug**et **/SOS**.

Pour ajouter **/debug/fastdetect** à la fin de la première entrée du système d’exploitation, en remplaçant toutes les options d’entrée précédentes du système d’exploitation :

```
bootcfg /raw /debug /fastdetect /id 1
```

Pour utiliser la commande **bootcfg/RAW** :

```
bootcfg /raw /debug /sos /id 2
bootcfg /raw /s srvmain /u maindom\hiropln /p p@ssW23 /crashdebug  /id 2
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande bootcfg](bootcfg.md)
