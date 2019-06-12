---
title: AuditPol get
description: Rubrique de commandes de Windows pour **auditpol obtenir** -récupère la stratégie système, la stratégie par utilisateur, l’audit des options et objet de descripteur de sécurité d’audit.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fe13de4e-836c-4207-b47c-64b6272d6c41
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 03ba59b19af42ab2d3fdd1dd52d976d381779640
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435147"
---
# <a name="auditpol-get"></a>AuditPol get

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Récupère la stratégie système, la stratégie par utilisateur, l’audit des options et objet de descripteur de sécurité d’audit.

## <a name="syntax"></a>Syntaxe
```
auditpol /get 
[/user[:<username>|<{sid}>]]
[/category:*|<name>|<{guid}>[,:<name|<{guid}> ]]
[/subcategory:*|<name>|<{guid}>[,:<name|<{guid}> ]]
[/option:<option name>]
[/sd]
[/r]
```
## <a name="parameters"></a>Paramètres

|  Paramètre   |                                                                                                                                         Description                                                                                                                                          |
|--------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /User     | Affiche l’entité de sécurité pour lesquels la stratégie d’audit par utilisateur est interrogée. Paramètre/SubCategory ou /category doit être spécifié. L’utilisateur peut être spécifié comme un identificateur de sécurité (SID) ou un nom. Si aucun compte d’utilisateur n’est spécifié, la stratégie d’audit système est interrogée. |
|  /category   |                                                          Une ou plusieurs catégories d’audit spécifiés par nom ou identificateur global unique (GUID). Un astérisque (\*) peut être utilisé pour indiquer que toutes les catégories d’audit doivent être interrogés.                                                          |
| / SubCategory |                                                                                                                  Un ou plusieurs des sous-catégories d’audit spécifiés par GUID ou nom.                                                                                                                  |
|     /sd      |                                                                                                        Récupère le descripteur de sécurité utilisé pour déléguer l’accès à la stratégie d’audit.                                                                                                        |
|   /option    |                                                                              Récupère la stratégie existante pour les options CrashOnAuditFail, FullprivilegeAuditing, AuditBaseObjects ou AuditBasedirectories.                                                                               |
|      /r      |                                                                                                              Affiche la sortie au format de rapport, valeurs séparées par des virgules (CSV).                                                                                                              |
|      /?      |                                                                                                                             Affiche l'aide à l'invite de commandes.                                                                                                                             |

## <a name="remarks"></a>Notes
Toutes les catégories et sous-catégories peuvent être spécifiés par le GUID ou le nom entouré guillemets. Utilisateurs peuvent être spécifiés par SID ou par nom.
pour toutes les opérations get pour les stratégies d’utilisateur et système, vous devez être autorisés à lire ce jeu d’objets dans le descripteur de sécurité. Vous pouvez également effectuer des opérations get par possédant le **gérer le journal d’audit et de sécurité** droit d’utilisateur (SeSecurityPrivilege). Toutefois, ce droit permet un accès supplémentaire qui n’est pas nécessaire pour effectuer l’opération get.
## <a name="BKMK_examples"></a>Exemples
### <a name="examples-for-the-per-user-audit-policy"></a>Exemples pour la stratégie d’audit par utilisateur
Pour récupérer la stratégie d’audit par utilisateur pour le compte invité et afficher la sortie pour le système, les suivi détaillé et catégories de l’accès aux objets, tapez :
```
auditpol /get /user:{S-1-5-21-1443922412-3030960370-963420232-51} /category:"System","detailed Tracking","Object Access"
```
> [!NOTE]
> Cette commande est utile dans deux scénarios. Lorsque vous analysez un compte d’utilisateur spécifique pour une activité suspecte, vous pouvez utiliser la commande /Get / pour récupérer les résultats dans des catégories spécifiques à l’aide d’une stratégie d’inscription pour activer l’audit supplémentaires. Ou, si les paramètres d’audit sur un compte connectez nombreuses mais événements superflues, vous pouvez utiliser la commande /Get / pour filtrer des événements superflus pour ce compte avec une stratégie d’exclusion. Pour obtenir la liste de toutes les catégories, utilisez la commande de /category auditpol /list.
> Pour récupérer la stratégie d’audit par utilisateur pour une catégorie et une sous-catégorie particulière, qui signale les paramètres inclusives et exclusives de cette sous-catégorie sous la catégorie système pour le compte invité, tapez :
> ```
> auditpol /get /user:guest /category:"System" /subcategory:{0ccee921a-69ae-11d9-bed3-505054503030}
> ```
> Pour afficher la sortie au format de rapport et d’inclure le nom de l’ordinateur cible de la stratégie, subcategory, sous-catégorie GUID, paramètres d’inclusion et paramètres d’exclusion, tapez :
> ```
> auditpol /get /user:guest /category:detailed Tracking" /r
> ```
> ### <a name="examples-for-the-system-audit-policy"></a>Exemples pour la stratégie d’audit système
> Pour récupérer la stratégie pour le système de catégories et sous-catégories qui signale les paramètres de stratégie de catégorie et sous-catégorie pour la stratégie d’audit système, tapez :
> ```
> auditpol /get /category:"System" /subcategory:{0ccee921a-69ae-11d9-bed3-505054503030}
> ```
> Pour récupérer la stratégie pour la catégorie de suivi détaillée et les sous-catégories dans un format de rapport et inclure le nom de l’ordinateur, la cible de la stratégie, la sous-catégorie, la sous-catégorie GUID, paramètres d’inclusion et paramètres d’exclusion, le type de :
> ```
> auditpol /get /category:"detailed Tracking" /r
> ```
> Pour récupérer la stratégie pour les deux catégories avec les catégories spécifiées sous forme de GUID, qui signale tous les paramètres de stratégie d’audit de toutes les sous-catégories sous deux catégories, type :
> ```
> auditpol /get /category:{69979849-797a-11d9-bed3-505054503030},{69997984a-797a-11d9-bed3-505054503030} subcategory:{0ccee921a-69ae-11d9-bed3-505054503030}
> ```
> ### <a name="examples-for-auditing-options"></a>Exemples d’options d’audit
> Pour récupérer l’état activé ou désactivé de l’option AuditBaseObjects, tapez :
> ```
> auditpol /get /option:AuditBaseObjects
> ```
> [!NOTE]
> Les options disponibles sont AuditBaseObjects, AuditBaseOperations et FullprivilegeAuditing.
> Pour récupérer l’état activé, désactivé ou 2 de l’option CrashOnAuditFail, tapez :
> ```
> auditpol /get /option:CrashOnAuditFail /r
> ```
> #### <a name="additional-references"></a>Références supplémentaires
> [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
