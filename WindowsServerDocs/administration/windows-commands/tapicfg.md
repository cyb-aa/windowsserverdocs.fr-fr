---
title: tapicfg
description: Découvrez comment gérer une partition d’annuaire TAPI application.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c0e642ce-5d98-4edb-9a65-1dff09aef4e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 6e89b1e3d7638fbcbe0140658d2d2a9af1ccd852
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886510"
---
# <a name="tapicfg"></a>tapicfg

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crée, supprime, ou affiche une partition d’annuaire application TAPI ou définit une partition d’annuaire par défaut TAPI application. Les clients de TAPI 3.1 peuvent utiliser des informations dans cette partition de répertoire d’application avec le service de recherche de service de répertoire et de communiquer avec les annuaires de l’interface TAPI. Vous pouvez également utiliser **tapicfg** pour créer ou supprimer des points de connexion de service, qui permettent aux clients TAPI identifier efficacement les partitions d’annuaire d’applications TAPI dans un domaine. Pour plus d’informations, consultez la section Notes. Pour afficher la syntaxe de commande, cliquez sur une commande. 
-   [installation de tapicfg](#BKMK_install)
-   [tapicfg remove](#BKMK_remove)
-   [tapicfg publishscp](#BKMK_publishscp)
-   [tapicfg removescp](#BKMK_removescp)
-   [tapicfg show](#BKMK_show)
-   [tapicfg makedefault](#BKMK_makedefault)

## <a name="BKMK_install"></a>installation de tapicfg
Crée une partition d’annuaire TAPI application.

### <a name="syntax"></a>Syntaxe
```
tapicfg install /directory:<PartitionName> [/server:<DCName>] [/forcedefault]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|installer/répertoire :\<PartitionName >|Obligatoire. Spécifie le nom DNS de la partition d’annuaire TAPI application doit être créé. Ce nom doit être un nom de domaine complet.|
|/ Server : \<DCName>|Spécifie le nom DNS du contrôleur de domaine sur lequel la partition d’annuaire application TAPI est créée. Si le nom de contrôleur de domaine n’est pas spécifié, le nom de l’ordinateur local est utilisé.|
|/forcedefault|Spécifie que ce répertoire est la partition d’annuaire application TAPI par défaut pour le domaine. Il peut y avoir plusieurs partitions d’annuaire applications TAPI dans un domaine.<br /><br />Si ce répertoire est la première partition d’annuaire d’applications TAPI créée sur le domaine, il est automatiquement défini comme la valeur par défaut, que vous utilisiez le **/forcedefault ou non** option.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="BKMK_remove"></a>tapicfg remove
Supprime une partition d’annuaire TAPI application.

### <a name="syntax"></a>Syntaxe
```
tapicfg remove /directory:<PartitionName>
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|remove /directory:\<PartitionName>|Obligatoire. Spécifie le nom DNS de la partition d’annuaire TAPI application à supprimer. Notez que ce nom doit être un nom de domaine complet.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="BKMK_publishscp"></a>tapicfg publishscp
Crée un point de connexion de service pour publier une partition d’annuaire TAPI application.

### <a name="syntax"></a>Syntaxe
```
tapicfg publishscp /directory:<PartitionName> [/domain:<DomainName>] [/forcedefault]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|/ répertoire publishscp :\<PartitionName >|Obligatoire. Spécifie le nom DNS de la partition d’annuaire application TAPI le service point de connexion publiera.|
|/ Domain :\<DomainName >|Spécifie le nom DNS du domaine dans lequel le service point de connexion est créé. Si le nom de domaine n’est pas spécifié, le nom du domaine local est utilisé.|
|/forcedefault|Spécifie que ce répertoire est la partition d’annuaire application TAPI par défaut pour le domaine. Il peut y avoir plusieurs partitions d’annuaire applications TAPI dans un domaine.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="BKMK_removescp"></a>tapicfg removescp
Supprime un point de connexion de service pour une partition d’annuaire TAPI application.

### <a name="syntax"></a>Syntaxe
```
tapicfg removescp /directory:<PartitionName> [/domain:<DomainName>]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|removescp /directory:\<PartitionName>|Obligatoire. Spécifie le nom DNS de la partition d’annuaire TAPI d’application pour laquelle un point de connexion de service est supprimé.|
|/Domain : \<DomainName>|Spécifie le nom DNS du domaine à partir duquel le point de connexion de service est supprimé. Si le nom de domaine n’est pas spécifié, le nom du domaine local est utilisé.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="BKMK_show"></a>tapicfg show
Affiche les noms et emplacements des partitions d’annuaire TAPI application dans le domaine.

### <a name="syntax"></a>Syntaxe
```
tapicfg show [/defaultonly][ /domain:<DomainName>]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|/defaultonly|Affiche les noms et emplacements d’uniquement la partition d’annuaire application par défaut TAPI dans le domaine.|
|/Domain : \<DomainName>|Spécifie le nom DNS du domaine pour lequel les partitions d’annuaire application TAPI sont affichées. Si le nom de domaine n’est pas spécifié, le nom du domaine local est utilisé.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="BKMK_makedefault"></a>tapicfg makedefault
Définit la partition d’annuaire application TAPI par défaut pour le domaine.

### <a name="syntax"></a>Syntaxe
```
tapicfg makedefault /directory:<PartitionName> [/domain:<DomainName>]  
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|/ répertoire makedefault :\<PartitionName >|Obligatoire. Spécifie le nom DNS de la partition d’annuaire application TAPI définie comme partition par défaut pour le domaine. Notez que ce nom doit être un nom de domaine complet. Spécifie le nom DNS du domaine pour lequel la partition d’annuaire application TAPI est définie comme la valeur par défaut. Si le nom de domaine n’est pas spécifié, le nom du domaine local est utilisé.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes
Vous devez être membre du groupe Administrateurs de l’entreprise dans active directory pour une exécution **tapicfg installer** (pour créer une partition d’annuaire application TAPI) ou **tapicfg supprimer** (pour supprimer une application TAPI partition d’annuaire).

Cet outil de ligne de commande peut être exécuté sur n’importe quel ordinateur qui est membre du domaine.

Texte de fournie par l’utilisateur (par exemple, les noms des partitions d’annuaire d’applications, les serveurs et les domaines TAPI) avec des caractères internationaux ou Unicode sont affichés uniquement correctement si les polices appropriées et prise en charge linguistique sont installés.

Vous pouvez toujours utiliser des serveurs de Service ILS (Internet Locator) de votre organisation, si ILS est nécessaire pour prendre en charge de certaines applications, car les clients TAPI exécutant Windows XP ou un système d’exploitation de Windows Server 2003 peuvent interroger les serveurs ILS ou application TAPI partitions d’annuaire.

Vous pouvez utiliser **tapicfg** pour créer ou supprimer des points de connexion de service. Si la partition d’annuaire application TAPI est renommée pour une raison quelconque (par exemple, si vous renommez le domaine dans lequel il réside), vous devez supprimer le point de connexion de service existant et créez-en un qui contient le nouveau nom DNS du répertoire TAPI partition à publier. Sinon, les clients TAPI ne peuvent pas rechercher et accéder à la partition d’annuaire TAPI application. Vous pouvez également supprimer un point de connexion de service pour des raisons de maintenance ou de sécurité (par exemple, si vous ne souhaitez pas exposer les données TAPI sur une partition de répertoire d’application TAPI spécifique).

## <a name="examples"></a>Exemples
Pour créer une partition d’annuaire TAPI application nommée tapifiction.testdom.microsoft.com sur un serveur nommé testdc.testdom.microsoft.com, puis définissez-la comme la partition d’annuaire application TAPI par défaut pour le nouveau domaine, tapez :
```
tapicfg install /directory:tapifiction.testdom.microsoft.com /server:testdc.testdom.microsoft.com /forcedefault
```
Pour afficher le nom de la partition d’annuaire par défaut TAPI application pour le nouveau domaine, tapez :
```
tapicfg show /defaultonly
```
## <a name="additional-references"></a>Références supplémentaires
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
