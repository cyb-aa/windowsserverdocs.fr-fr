---
title: liste d’Auditpol
description: La rubrique commandes Windows pour la **liste** répertorie les catégories de stratégie d’audit et/ou les sous-catégories, ou répertorie les utilisateurs pour lesquels une stratégie d’audit par utilisateur est définie.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 27a89ae18838989b4f2df27d777c1c35249b8991
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382462"
---
# <a name="auditpol-list"></a>liste d’Auditpol

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

répertorie les catégories et/ou sous-catégories de stratégie d’audit, ou répertorie les utilisateurs pour lesquels une stratégie d’audit par utilisateur est définie.

## <a name="syntax"></a>Syntaxe
```
auditpol /list
[/user|/category|subcategory[:<categoryname>|<{guid}>|*]]
[/v] [/r]
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|/User|Récupère tous les utilisateurs pour lesquels la stratégie d’audit par utilisateur a été définie. S’il est utilisé avec le paramètre/v, l’identificateur de sécurité (SID) de l’utilisateur est également affiché.|
|/Category|Affiche les noms des catégories comprises par le système. S’il est utilisé avec le paramètre/v, l’identificateur global unique (GUID) de la catégorie est également affiché.|
|/Subcategory|Affiche les noms des sous-catégories et leur GUID associé.|
|/v|Affiche le GUID avec la catégorie ou la sous-catégorie, ou, s’il est utilisé avec/User, affiche le SID de chaque utilisateur.|
|/r|Affiche la sortie sous la forme d’un rapport au format de valeurs séparées par des virgules (CSV).|
|/?|Affiche l'aide à l'invite de commandes.|
## <a name="remarks"></a>Notes
pour toutes les opérations de liste pour la stratégie par utilisateur, vous devez disposer de l’autorisation de lecture sur cet objet défini dans le descripteur de sécurité. Vous pouvez également effectuer des opérations de liste en possédant le droit **d’utilisateur gérer le journal d’audit et de sécurité** (SeSecurityPrivilege). Toutefois, ce droit autorise un accès supplémentaire qui n’est pas nécessaire pour effectuer l’opération de liste.
## <a name="BKMK_examples"></a>Illustre
Pour répertorier tous les utilisateurs qui ont une stratégie d’audit définie, tapez :
```
auditpol /list /user
```
Pour répertorier tous les utilisateurs qui ont une stratégie d’audit définie et leur SID associé, tapez :
```
auditpol /list /user /v
```
Pour répertorier toutes les catégories et sous-catégories au format de rapport, tapez :
```
auditpol /list /subcategory:* /r
```
Pour répertorier les sous-catégories du suivi détaillé et des catégories d’accès DS, tapez :
```
auditpol /list /subcategory:"detailed Tracking","DS Access"
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
