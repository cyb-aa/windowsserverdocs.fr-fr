---
title: récupération d’Auditpol
description: 'La rubrique commandes Windows pour **Auditpol obtenir** : récupère les objets stratégie système, stratégie par utilisateur, options d’audit et objet descripteur de sécurité d’audit.'
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 296fc5afb540411d76b563faca42fc045b8df3b3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382487"
---
# <a name="auditpol-get"></a>récupération d’Auditpol

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Récupère la stratégie système, la stratégie par utilisateur, les options d’audit et l’objet descripteur de sécurité d’audit.

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
|    /User     | Affiche le principal de sécurité pour lequel la stratégie d’audit par utilisateur est interrogée. Le paramètre/Category ou/subcategory doit être spécifié. L’utilisateur peut être spécifié en tant qu’identificateur de sécurité (SID) ou nom. Si aucun compte d’utilisateur n’est spécifié, la stratégie d’audit du système est interrogée. |
|  /Category   |                                                          Une ou plusieurs catégories d’audit spécifiées par un identificateur global unique (GUID) ou un nom. Un astérisque (\*) peut être utilisé pour indiquer que toutes les catégories d’audit doivent être interrogées.                                                          |
| /Subcategory |                                                                                                                  Une ou plusieurs sous-catégories d’audit spécifiées par le GUID ou le nom.                                                                                                                  |
|     /SD      |                                                                                                        Récupère le descripteur de sécurité utilisé pour déléguer l’accès à la stratégie d’audit.                                                                                                        |
|   /option    |                                                                              Récupère la stratégie existante pour les options CrashOnAuditFail, FullprivilegeAuditing, AuditBaseObjects ou AuditBasedirectories.                                                                               |
|      /r      |                                                                                                              Affiche la sortie au format de rapport, valeur séparée par des virgules (CSV).                                                                                                              |
|      /?      |                                                                                                                             Affiche l'aide à l'invite de commandes.                                                                                                                             |

## <a name="remarks"></a>Notes
Toutes les catégories et sous-catégories peuvent être spécifiées par le GUID ou le nom entre guillemets. Les utilisateurs peuvent être spécifiés par le SID ou le nom.
pour toutes les opérations d’extraction pour la stratégie système et la stratégie par utilisateur, vous devez disposer de l’autorisation de lecture sur cet objet défini dans le descripteur de sécurité. Vous pouvez également effectuer des opérations de récupération en possédant le droit **d’utilisateur gérer le journal d’audit et de sécurité** (SeSecurityPrivilege). Toutefois, ce droit autorise un accès supplémentaire qui n’est pas nécessaire pour effectuer l’opération d’extraction.
## <a name="BKMK_examples"></a>Illustre
### <a name="examples-for-the-per-user-audit-policy"></a>Exemples pour la stratégie d’audit par utilisateur
Pour récupérer la stratégie d’audit par utilisateur pour le compte invité et afficher la sortie pour le système, le suivi détaillé et les catégories d’accès aux objets, tapez :
```
auditpol /get /user:{S-1-5-21-1443922412-3030960370-963420232-51} /category:"System","detailed Tracking","Object Access"
```
> [!NOTE]
> Cette commande est utile dans deux scénarios. Lorsque vous surveillez un compte d’utilisateur spécifique pour une activité suspecte, vous pouvez utiliser la commande/Get pour récupérer les résultats dans des catégories spécifiques à l’aide d’une stratégie d’inclusion pour activer des audits supplémentaires. Ou, si les paramètres d’audit d’un compte consignent de nombreux événements mais superflus, vous pouvez utiliser la commande/Get pour filtrer les événements superflus de ce compte avec une stratégie d’exclusion. Pour obtenir la liste de toutes les catégories, utilisez la commande Auditpol/List/Category.
> Pour récupérer la stratégie d’audit par utilisateur pour une catégorie et une sous-catégorie particulière, qui signalent les paramètres inclusifs et exclusifs de cette sous-catégorie sous la catégorie système du compte invité, tapez :
> ```
> auditpol /get /user:guest /category:"System" /subcategory:{0ccee921a-69ae-11d9-bed3-505054503030}
> ```
> Pour afficher la sortie au format de rapport et inclure le nom de l’ordinateur, la cible de la stratégie, la sous-catégorie, le GUID de la sous-catégorie, les paramètres d’inclusion et les paramètres d’exclusion, tapez :
> ```
> auditpol /get /user:guest /category:detailed Tracking" /r
> ```
> ### <a name="examples-for-the-system-audit-policy"></a>Exemples de stratégie d’audit du système
> Pour récupérer la stratégie pour la catégorie système et les sous-catégories qui signalent les paramètres de stratégie de catégorie et de sous-catégorie de la stratégie d’audit système, tapez :
> ```
> auditpol /get /category:"System" /subcategory:{0ccee921a-69ae-11d9-bed3-505054503030}
> ```
> Pour récupérer la stratégie pour la catégorie de suivi et les sous-catégories détaillées dans le format de rapport et inclure le nom de l’ordinateur, la cible de la stratégie, la sous-catégorie, le GUID de la sous-catégorie, les paramètres d’inclusion et les paramètres d’exclusion, tapez :
> ```
> auditpol /get /category:"detailed Tracking" /r
> ```
> Pour récupérer la stratégie de deux catégories avec les catégories spécifiées en tant que GUID, qui signalent tous les paramètres de stratégie d’audit de toutes les sous-catégories sous deux catégories, tapez :
> ```
> auditpol /get /category:{69979849-797a-11d9-bed3-505054503030},{69997984a-797a-11d9-bed3-505054503030} subcategory:{0ccee921a-69ae-11d9-bed3-505054503030}
> ```
> ### <a name="examples-for-auditing-options"></a>Exemples d’options d’audit
> Pour récupérer l’État, activé ou désactivé, de l’option AuditBaseObjects, tapez :
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
