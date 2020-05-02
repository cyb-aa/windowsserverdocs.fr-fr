---
title: cmdkey
description: Rubrique de référence pour la commande cmdkey, qui crée, répertorie et supprime les noms d’utilisateurs et les mots de passe ou les informations d’identification stockés.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5fcd68ee-a14a-4b71-9300-c3f5c5d31e8e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4005707785101fcc1fb0030ffe895668bd65f730
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82712152"
---
# <a name="cmdkey"></a>cmdkey

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crée, répertorie et supprime les noms d’utilisateurs et les mots de passe ou informations d’identification stockés.

## <a name="syntax"></a>Syntaxe

```
cmdkey [{/add:<targetname>|/generic:<targetname>}] {/smartcard | /user:<username> [/pass:<password>]} [/delete{:<targetname> | /ras}] /list:<targetname>
```

### <a name="parameters"></a>Paramètres

| Paramètres | Description |
| ---------- | ----------- |
| /Add`<targetname>` | Ajoute un nom d’utilisateur et un mot de passe à la liste.<p>Requiert le paramètre de `<targetname>` qui identifie le nom de l’ordinateur ou du domaine auquel cette entrée sera associée. |
| /générique`<targetname>` | Ajoute des informations d’identification génériques à la liste.<p>Requiert le paramètre de `<targetname>` qui identifie le nom de l’ordinateur ou du domaine auquel cette entrée sera associée. |
| /Smartcard | Récupère les informations d’identification d’une carte à puce. Si plus d’une carte à puce est détectée sur le système lorsque cette option est utilisée, **cmdkey** affiche des informations sur toutes les cartes à puce disponibles, puis invite l’utilisateur à spécifier celui à utiliser. |
| /User`<username>` | Spécifie le nom d’utilisateur ou de compte à stocker avec cette entrée. Si `<username>` n’est pas fourni, il est demandé. |
|/Pass`<password>` | Spécifie le mot de passe à stocker avec cette entrée. Si `<password>` n’est pas fourni, il est demandé. Les mots de passe ne sont pas affichés une fois qu’ils sont stockés. |
| /delete{:`<targetname>` | exécutant | Supprime un nom d’utilisateur et un mot de passe de la liste. Si `<targetname>` est spécifié, cette entrée est supprimée. Si `/ras` est spécifié, l’entrée d’accès à distance stockée est supprimée. |
| /List`<targetname>` | Affiche la liste des noms d’utilisateur et des informations d’identification stockés. Si `<targetname>` n’est pas spécifié, tous les noms d’utilisateur et les informations d’identification stockés sont répertoriés. |
| /? | Affiche l'aide à l'invite de commandes. |

## <a name="examples"></a>Exemples

Pour afficher la liste de tous les noms d’utilisateur et informations d’identification stockés, tapez :

```
cmdkey /list
```

Pour ajouter un nom d’utilisateur et un mot de passe pour l’utilisateur *Mikedan* afin d’accéder à l’ordinateur *SERVEUR01* avec le mot de passe *Kleo*, tapez :

```
cmdkey /add:server01 /user:mikedan /pass:Kleo
```

Pour ajouter un nom d’utilisateur et un mot de passe pour l’utilisateur *Mikedan* pour accéder à l’ordinateur *SERVEUR01* et demander le mot de passe à chaque accès à SERVEUR01, tapez :

```
cmdkey /add:server01 /user:mikedan
```

Pour supprimer les informations d’identification stockées par l’accès à distance, tapez :

```
cmdkey /delete /ras
```

Pour supprimer les informations d’identification stockées pour *SERVEUR01*, tapez :

```
cmdkey /delete:server01
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
