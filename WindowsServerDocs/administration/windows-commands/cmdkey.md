---
title: cmdkey
description: La rubrique commandes Windows pour cmdkey, qui crée, répertorie et supprime les noms d’utilisateurs et les mots de passe ou les informations d’identification stockés.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5fcd68ee-a14a-4b71-9300-c3f5c5d31e8e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cdb732bf95e30af012f78d1bad337d6d6d191268
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847592"
---
# <a name="cmdkey"></a>cmdkey

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crée, répertorie et supprime les noms d’utilisateurs et les mots de passe ou informations d’identification stockés.

## <a name="syntax"></a>Syntaxe
```
cmdkey [{/add:<TargetName>|/generic:<TargetName>}] {/smartcard|/user:<UserName> [/pass:<Password>]} [/delete{:<TargetName>|/ras}] /list:<TargetName>
```
### <a name="parameters"></a>Paramètres

|             Paramètres             |                                                                                    Description                                                                                     |
|------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         /Add :<TargetName>          | Ajoute un nom d’utilisateur et un mot de passe à la liste.<p>Requiert le paramètre de <TargetName> qui identifie le nom de l’ordinateur ou du domaine auquel cette entrée sera associée. |
|       /générique :<TargetName>        |   Ajoute des informations d’identification génériques à la liste.<p>Requiert le paramètre de <TargetName> qui identifie le nom de l’ordinateur ou du domaine auquel cette entrée sera associée.    |
|             /Smartcard             |                                                                    Récupère les informations d’identification d’une carte à puce.                                                                     |
|          /User :<UserName>          |                                 Spécifie le nom d’utilisateur ou de compte à stocker avec cette entrée. Si le *nom d’utilisateur* n’est pas fourni, il est demandé.                                  |
|          /Pass :<Password>          |                                       Spécifie le mot de passe à stocker avec cette entrée. Si le *mot de passe* n’est pas fourni, il est demandé.                                        |
| /Delete{ :<TargetName> &#124; /RAS} |  supprime un nom d’utilisateur et un mot de passe de la liste. Si *TargetName* est spécifié, cette entrée sera supprimée. Si/RAS est spécifié, l’entrée d’accès à distance stockée est supprimée.   |
|         /List :<TargetName>         |                  Affiche la liste des noms d’utilisateur et des informations d’identification stockés. Si *TargetName* n’est pas spécifié, tous les noms d’utilisateur et informations d’identification stockés seront listés.                   |
|                 /?                 |                                                                        Affiche l'aide à l'invite de commandes.                                                                        |

## <a name="remarks"></a>Notes
- Si plus d’une carte à puce est détectée sur le système lorsque l’option de ligne de commande/Smartcard est utilisée, **cmdkey** affiche des informations sur toutes les cartes à puce disponibles, puis invite l’utilisateur à spécifier celui à utiliser.
- Les mots de passe ne sont pas affichés une fois qu’ils sont stockés.
  ## <a name="examples"></a><a name=BKMK_examples></a>Illustre
  Pour afficher la liste de tous les noms d’utilisateur et informations d’identification stockés, tapez :
  ```
  cmdkey /list
  ```
  Pour ajouter un nom d’utilisateur et un mot de passe pour l’utilisateur Mikedan afin d’accéder à l’ordinateur SERVEUR01 avec le mot de passe Kleo, tapez :
  ```
  cmdkey /add:server01 /user:mikedan /pass:Kleo
  ```
  Pour ajouter un nom d’utilisateur et un mot de passe pour l’utilisateur Mikedan pour accéder à l’ordinateur SERVEUR01 et demander le mot de passe à chaque accès à SERVEUR01, tapez :
  ```
  cmdkey /add:server01 /user:mikedan
  ```
  Pour supprimer les informations d’identification stockées par l’accès à distance, tapez :
  ```
  cmdkey /delete /ras
  ```
  Pour supprimer les informations d’identification stockées pour Serveur01, tapez :
  ```
  cmdkey /delete:Server01
  ```
  ## <a name="additional-references"></a>Références supplémentaires
  - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
