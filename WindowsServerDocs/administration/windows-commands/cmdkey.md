---
title: cmdkey
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5fcd68ee-a14a-4b71-9300-c3f5c5d31e8e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7080a3ae28fed722f17cf8311aa1e2fd3bece41f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854610"
---
# <a name="cmdkey"></a>cmdkey

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crée, répertorie et supprime les noms d’utilisateurs et les mots de passe ou informations d’identification stockés.

## <a name="syntax"></a>Syntaxe
```
cmdkey [{/add:<TargetName>|/generic:<TargetName>}] {/smartcard|/user:<UserName> [/pass:<Password>]} [/delete{:<TargetName>|/ras}] /list:<TargetName>
```
## <a name="parameters"></a>Paramètres
|Paramètres|Description|
|-------|--------|
|/ ajouter :<TargetName>|Ajoute un nom d’utilisateur et le mot de passe à la liste.<br /><br />Requiert que le paramètre de <TargetName> qui identifie le nom d’ordinateur ou le domaine auquel cette entrée sera associée.|
|/ générique :<TargetName>|Ajoute des informations d’identification génériques à la liste.<br /><br />Requiert que le paramètre de <TargetName> qui identifie le nom d’ordinateur ou le domaine auquel cette entrée sera associée.|
|/smartcard|Récupère les informations d’identification à partir d’une carte à puce.|
|/ User :<UserName>|Spécifie le nom d’utilisateur ou compte à stocker avec cette entrée. Si *nom d’utilisateur* est ne pas fourni, il vous sera demandé.|
|/pass :<Password>|Spécifie le mot de passe à stocker avec cette entrée. Si *mot de passe* est ne pas fourni, il vous sera demandé.|
|/delete{:<TargetName> &#124; /ras}|Supprime un nom d’utilisateur et le mot de passe dans la liste. Si *TargetName* est spécifié, que sera supprimé. Si /ras est spécifié, l’entrée d’accès à distance stockée est supprimée.|
|/List :<TargetName>|Affiche la liste des noms d’utilisateurs et les informations d’identification. Si *TargetName* n’est pas les noms d’utilisateur spécifié, tous stockés et informations d’identification sont répertoriées.|
|/?|Affiche l'aide à l'invite de commandes.|
## <a name="remarks"></a>Notes
-   Si plus d’une carte à puce se trouve sur le système lorsque l’option de ligne de commande /smartcard est utilisée, **cmdkey** affiche des informations sur toutes les cartes à puce disponibles et puis inviter l’utilisateur à spécifier l’application à utiliser.
-   Les mots de passe s’affichera pas une fois qu’ils sont stockés.
## <a name="BKMK_examples"></a>Exemples
Pour afficher une liste de tous les noms d’utilisateur et les informations d’identification qui sont stockées, tapez :
```
cmdkey /list
```
Pour ajouter un nom d’utilisateur et le mot de passe utilisateur MarcelRug à accès ordinateur serveur01 avec le mot de passe Kleo, tapez :
```
cmdkey /add:server01 /user:mikedan /pass:Kleo
```
Pour ajouter un nom d’utilisateur et le mot de passe utilisateur MarcelRug pour accéder à l’ordinateur Server01 et l’invite pour le mot de passe lors de chaque accès Server01, tapez :
```
cmdkey /add:server01 /user:mikedan
```
Pour supprimer les informations d’identification stockées par l’accès à distance, tapez :
```
cmdkey /delete /ras
```
Pour supprimer les informations d’identification stockées pour Server01, tapez :
```
cmdkey /delete:Server01
```
## <a name="additional-references"></a>Références supplémentaires
[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
