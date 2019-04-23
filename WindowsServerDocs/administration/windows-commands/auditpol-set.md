---
title: ensemble de l’outil Auditpol
description: Rubrique de commandes de Windows pour **auditpol set** - définit la stratégie d’audit par utilisateur, la stratégie d’audit système ou options d’audit.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f4947486-87bd-48cb-ba81-7230c8e70895
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c9c0fb17620147d2de5b991c1a9a0fb95e782677
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826990"
---
# <a name="auditpol-set"></a>ensemble de l’outil Auditpol

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Stratégie d’audit de jeux par utilisateur, stratégie d’audit système ou options d’audit.

## <a name="syntax"></a>Syntaxe
```
auditpol /set
[/user[:<username>|<{sid}>][/include][/exclude]]
[/category:<name>|<{guid}>[,:<name|<{guid}> ]]
[/success:<enable>|<disable>][/failure:<enable>|<disable>]
[/subcategory:<name>|<{guid}>[,:<name|<{guid}> ]]
[/success:<enable>|<disable>][/failure:<enable>|<disable>]
[/option:<option name> /value: <enable>|<disable>]
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|/User|Le principal de sécurité pour lequel l’utilisateur par la stratégie d’audit spécifié par la catégorie ou sous-catégorie est défini. La catégorie ou sous-catégorie option doit être spécifiée, comme un identificateur de sécurité (SID) ou un nom.|
|/include|Spécifié avec /user ; Indique que la stratégie l’utilisateur par utilisateur provoque un audit doit être généré même s’il n’est pas spécifié par la stratégie d’audit système. Ce paramètre est la valeur par défaut et est automatiquement appliqué si ni le / inclure ni /exclude paramètres sont explicitement spécifiés.|
|/exclude|Spécifié avec /user ; Indique que la stratégie l’utilisateur par utilisateur provoque un audit à supprimer, quel que soit la stratégie d’audit système. Ce paramètre est ignoré pour les utilisateurs qui sont membres du groupe Administrateurs local.|
|/category|Une ou plusieurs catégories d’audit spécifiés par nom ou identificateur global unique (GUID). Si aucun utilisateur n’est spécifié, la stratégie système est définie.|
|/ SubCategory|Un ou plusieurs des sous-catégories d’audit spécifiés par GUID ou nom. Si aucun utilisateur n’est spécifié, la stratégie système est définie.|
|/Success|Spécifie l’audit des réussites. Ce paramètre est la valeur par défaut et est automatiquement appliqué si /failure ni /success paramètres sont explicitement spécifiés. Ce paramètre doit être utilisé avec un paramètre qui indique s’il faut activer ou désactiver le paramètre.|
|/Failure|Spécifie l’audit des échecs. Ce paramètre doit être utilisé avec un paramètre qui indique s’il faut activer ou désactiver le paramètre.|
|/option|Définit la stratégie d’audit pour les options CrashOnAuditFail, FullprivilegeAuditing, AuditBaseObjects ou AuditBasedirectories.|
|/sd|Définit le descripteur de sécurité utilisé pour déléguer l’accès à la stratégie d’audit. Le descripteur de sécurité doit être spécifié à l’aide de la définition de langage SDDL (Security Descriptor). Le descripteur de sécurité doit avoir une liste de contrôle d’accès discrétionnaire (DACL).|
|/?|Affiche l'aide à l'invite de commandes.|
## <a name="remarks"></a>Notes
pour toutes les opérations de jeu pour les stratégies d’utilisateur et système, vous devez avoir écrire ou jeu d’autorisations de contrôle total sur cet objet dans le descripteur de sécurité. Vous pouvez également effectuer des opérations de jeu en utilisant le **gérer le journal d’audit et de sécurité** droit d’utilisateur (SeSecurityPrivilege). Toutefois, ce droit permet un accès supplémentaire qui n’est pas nécessaire d’effectuer l’opération de définition.
## <a name="BKMK_examples"></a>Exemples
### <a name="examples-for-the-per-user-audit-policy"></a>Exemples pour la stratégie d’audit par utilisateur
Pour définir l’utilisateur par la stratégie d’audit pour toutes les sous-catégories sous la catégorie de suivi détaillée pour l’utilisateur MarcelRug afin que toutes les tentatives réussies de l’utilisateur doit être auditées, tapez :
```
auditpol /set /user:mikedan /category:"detailed Tracking" /include /success:enable
```
Pour définir la stratégie d’audit par utilisateur pour les catégories spécifiées par nom et les GUID et les sous-catégories spécifiés par GUID pour supprimer l’audit pour toutes les tentatives réussies ou ayant échoués, tapez :
```
auditpol /set /user:mikedan /exclude /category:"Object Access","System",{6997984b-797a-11d9-bed3-505054503030} 
/subcategory:{0ccee9210-69ae-11d9-bed3-505054503030},:{0ccee9211-69ae-11d9-bed3-505054503030}, /success:enable /failure:enable
```
Pour définir la stratégie d’audit par utilisateur pour l’utilisateur spécifié pour toutes les catégories de la suppression de l’audit de toutes les valeurs sauf tentatives réussies, tapez :
```
auditpol /set /user:mikedan /exclude /category:* /success:enable
```
### <a name="examples-for-the-system-audit-policy"></a>Exemples pour la stratégie d’audit système
Pour définir la stratégie d’audit système pour toutes les sous-catégories sous la catégorie de suivi détaillée pour inclure l’audit des tentatives réussies uniquement, tapez :
```
auditpol /set /category:"detailed Tracking" /success:enable
```
> [!NOTE]
> Le paramètre d’échec n’est pas modifié.
Pour définir la stratégie d’audit système pour les catégories de l’accès aux objets et système (ce qui est implicite, car les sous-catégories sont répertoriés) et les sous-catégories spécifiés par le GUID pour la suppression de tentatives ayant échoué et l’audit des tentatives réussies, tapez :
```
auditpol /set /subcategory:{0ccee9210-69ae-11d9-bed3-505054503030},{0ccee9211-69ae-11d9-bed3-505054503030}, /failure:disable /success:enable
```
### <a name="example-for-auditing-options"></a>Exemple pour les options d’audit
Pour définir les options d’audit à l’état activé pour l’option CrashOnAuditFail, tapez :
```
auditpol /set /option:CrashOnAuditFail /value:enable
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
