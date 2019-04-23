---
title: liste de l’outil Auditpol
description: Rubrique de commandes de Windows pour **auditpol liste** - listes d’audit des catégories de stratégie et/ou des sous-catégories ou répertorie les utilisateurs pour lesquels un utilisateur par la stratégie d’audit est définie.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 45502abe-3d6e-4e13-94f0-8e6fcb6db860
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 08f524ef0aacd731f709ce7a2e17b3d831da1e5b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858580"
---
# <a name="auditpol-list"></a>liste de l’outil Auditpol

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

listes d’audit des catégories de stratégie et/ou sous-catégories ou répertorie les utilisateurs pour lesquels une stratégie d’audit par utilisateur est définie.

## <a name="syntax"></a>Syntaxe
```
auditpol /list
[/user|/category|subcategory[:<categoryname>|<{guid}>|*]]
[/v] [/r]
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|/User|Récupère tous les utilisateurs pour lesquels la stratégie d’audit par utilisateur a été définie. Si utilisé avec le paramètre /v, l’identificateur de sécurité (SID) de l’utilisateur s’affiche également.|
|/category|Affiche les noms des catégories compris par le système. Si utilisé avec le paramètre /v, l’identificateur global unique de catégorie (GUID) s’affiche également.|
|/ SubCategory|Affiche les noms des sous-catégories et leur GUID associé.|
|/v|Affiche le GUID avec la catégorie ou sous-catégorie ou lorsqu’il est utilisé avec/User, le SID de chaque utilisateur.|
|/r|Affiche la sortie sous la forme d’un rapport au format de valeurs séparées par des virgules (CSV).|
|/?|Affiche l'aide à l'invite de commandes.|
## <a name="remarks"></a>Notes
pour toutes les opérations de liste pour la stratégie par utilisateur, vous devez lire les autorisations sur ce jeu d’objets dans le descripteur de sécurité. Vous pouvez également effectuer des opérations de liste par possédant le **gérer le journal d’audit et de sécurité** droit d’utilisateur (SeSecurityPrivilege). Toutefois, ce droit permet un accès supplémentaire qui n’est pas nécessaire d’effectuer l’opération de liste.
## <a name="BKMK_examples"></a>Exemples
Pour répertorier tous les utilisateurs qui ont une stratégie d’audit définie, tapez :
```
auditpol /list /user
```
Pour répertorier tous les utilisateurs qui ont une stratégie d’audit définis et leur SID associé, tapez :
```
auditpol /list /user /v
```
Pour répertorier toutes les catégories et sous-catégories dans un format de rapport, tapez :
```
auditpol /list /subcategory:* /r
```
Pour répertorier les sous-catégories des catégories de suivi et des accès DS détaillées, tapez :
```
auditpol /list /subcategory:"detailed Tracking","DS Access"
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
