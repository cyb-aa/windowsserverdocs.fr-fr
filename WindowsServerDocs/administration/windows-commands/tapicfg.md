---
title: tapicfg
description: Découvrez comment gérer une partition d’annuaire d’applications TAPI.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c0e642ce-5d98-4edb-9a65-1dff09aef4e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 694c32d9bbb8a873f34132e90c7201d3c9fe5132
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721562"
---
# <a name="tapicfg"></a>tapicfg

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crée, supprime ou affiche une partition d’annuaire d’applications TAPI, ou définit une partition d’annuaire d’applications TAPI par défaut. Les clients TAPI 3,1 peuvent utiliser les informations de cette partition d’annuaire d’applications avec le service Localisateur de service d’annuaire pour rechercher et communiquer avec les annuaires TAPI. Vous pouvez également utiliser **Tapicfg** pour créer ou supprimer des points de connexion de service, ce qui permet aux clients TAPI de localiser efficacement les partitions d’annuaire d’applications TAPI dans un domaine. Pour plus d’informations, consultez la section Notes. Pour afficher la syntaxe de la commande, cliquez sur une commande. 
-   [installation de tapicfg](#BKMK_install)
-   [tapicfg supprimer](#BKMK_remove)
-   [tapicfg publishscp](#BKMK_publishscp)
-   [tapicfg removescp](#BKMK_removescp)
-   [tapicfg afficher](#BKMK_show)
-   [tapicfg MakeDefault](#BKMK_makedefault)

## <a name="tapicfg-install"></a><a name="BKMK_install"></a>installation de tapicfg
Crée une partition d’annuaire d’applications TAPI.

### <a name="syntax"></a>Syntaxe
```
tapicfg install /directory:<PartitionName> [/server:<DCName>] [/forcedefault]
```
#### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|installer/répertoire :\<PartitionName>|Obligatoire. Spécifie le nom DNS de la partition d’annuaire d’applications TAPI à créer. Ce nom doit être un nom de domaine complet.|
|/Server : \<dcname>|Spécifie le nom DNS du contrôleur de domaine sur lequel la partition d’annuaire d’applications TAPI est créée. Si le nom du contrôleur de domaine n’est pas spécifié, le nom de l’ordinateur local est utilisé.|
|/forcedefault|Spécifie que ce répertoire est la partition d’annuaire d’applications TAPI par défaut pour le domaine. Il peut y avoir plusieurs partitions d’annuaire d’applications TAPI dans un domaine.<p>Si ce répertoire est la première partition de l’annuaire d’applications TAPI créée sur le domaine, elle est automatiquement définie par défaut, que vous utilisiez ou non l’option **/forcedefault** .|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="tapicfg-remove"></a><a name="BKMK_remove"></a>tapicfg supprimer
Supprime une partition d’annuaire d’applications TAPI.

### <a name="syntax"></a>Syntaxe
```
tapicfg remove /directory:<PartitionName>
```
#### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|supprimer/répertoire :\<PartitionName>|Obligatoire. Spécifie le nom DNS de la partition d’annuaire d’applications TAPI à supprimer. Notez que ce nom doit être un nom de domaine complet.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="tapicfg-publishscp"></a><a name="BKMK_publishscp"></a>tapicfg publishscp
Crée un point de connexion de service pour publier une partition d’annuaire d’applications TAPI.

### <a name="syntax"></a>Syntaxe
```
tapicfg publishscp /directory:<PartitionName> [/domain:<DomainName>] [/forcedefault]
```
#### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|publishscp/répertoire :\<PartitionName>|Obligatoire. Spécifie le nom DNS de la partition d’annuaire d’applications TAPI que le point de connexion de service doit publier.|
|/Domain :\<NomDomaine>|Spécifie le nom DNS du domaine dans lequel le point de connexion de service est créé. Si le nom de domaine n’est pas spécifié, le nom du domaine local est utilisé.|
|/forcedefault|Spécifie que ce répertoire est la partition d’annuaire d’applications TAPI par défaut pour le domaine. Il peut y avoir plusieurs partitions d’annuaire d’applications TAPI dans un domaine.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="tapicfg-removescp"></a><a name="BKMK_removescp"></a>tapicfg removescp
Supprime un point de connexion de service pour une partition d’annuaire d’applications TAPI.

### <a name="syntax"></a>Syntaxe
```
tapicfg removescp /directory:<PartitionName> [/domain:<DomainName>]
```
#### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|removescp/répertoire :\<PartitionName>|Obligatoire. Spécifie le nom DNS de la partition d’annuaire d’applications TAPI pour laquelle un point de connexion de service est supprimé.|
|/Domain : \<NomDomaine>|Spécifie le nom DNS du domaine à partir duquel le point de connexion de service est supprimé. Si le nom de domaine n’est pas spécifié, le nom du domaine local est utilisé.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="tapicfg-show"></a><a name="BKMK_show"></a>tapicfg afficher
Affiche les noms et les emplacements des partitions de l’annuaire d’applications TAPI dans le domaine.

### <a name="syntax"></a>Syntaxe
```
tapicfg show [/defaultonly][ /domain:<DomainName>]
```
#### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|/defaultonly|Affiche les noms et les emplacements de la partition d’annuaire d’applications TAPI par défaut dans le domaine.|
|/Domain : \<NomDomaine>|Spécifie le nom DNS du domaine pour lequel les partitions de l’annuaire d’applications TAPI sont affichées. Si le nom de domaine n’est pas spécifié, le nom du domaine local est utilisé.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="tapicfg-makedefault"></a><a name="BKMK_makedefault"></a>tapicfg MakeDefault
Définit la partition d’annuaire d’applications TAPI par défaut pour le domaine.

### <a name="syntax"></a>Syntaxe
```
tapicfg makedefault /directory:<PartitionName> [/domain:<DomainName>]  
```
#### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|MakeDefault/répertoire :\<PartitionName>|Obligatoire. Spécifie le nom DNS du jeu de partitions de l’annuaire d’applications TAPI comme partition par défaut pour le domaine. Notez que ce nom doit être un nom de domaine complet. Spécifie le nom DNS du domaine pour lequel la partition de l’annuaire d’applications TAPI est définie par défaut. Si le nom de domaine n’est pas spécifié, le nom du domaine local est utilisé.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes 
Vous devez être membre du groupe administrateurs de l’entreprise dans Active Directory pour exécuter **tapicfg install** (pour créer une partition d’annuaire d’applications TAPI) ou **Tapicfg Remove** (pour supprimer une partition d’annuaire d’applications TAPI).

Cet outil en ligne de commande peut être exécuté sur n’importe quel ordinateur membre du domaine.

Le texte fourni par l’utilisateur (par exemple, les noms de partitions d’annuaire d’applications TAPI, de serveurs et de domaines) avec des caractères internationaux ou Unicode ne s’affiche correctement que si les polices et la prise en charge linguistique appropriées sont installées.

Vous pouvez toujours utiliser les serveurs ILS (Internet Locator Service) de votre organisation, si ILS sont nécessaires pour prendre en charge certaines applications, car les clients TAPI exécutant Windows XP ou un système d’exploitation Windows Server 2003 peuvent interroger les serveurs ILS ou les partitions d’annuaire d’applications TAPI.

Vous pouvez utiliser **Tapicfg** pour créer ou supprimer des points de connexion de service. Si la partition de l’annuaire d’applications TAPI est renommée pour une raison quelconque (par exemple, si vous renommez le domaine dans lequel elle réside), vous devez supprimer le point de connexion de service existant et en créer un nouveau qui contient le nouveau nom DNS de la partition d’annuaire d’applications TAPI à publier. Sinon, les clients TAPI ne peuvent pas localiser la partition d’annuaire d’applications TAPI et y accéder. Vous pouvez également supprimer un point de connexion de service à des fins de maintenance ou de sécurité (par exemple, si vous ne souhaitez pas exposer des données TAPI sur une partition d’annuaire d’applications TAPI spécifique).

## <a name="examples"></a>Exemples
Pour créer une partition d’annuaire d’applications TAPI nommée tapifiction.testdom.microsoft.com sur un serveur nommé testdc.testdom.microsoft.com, puis la définir comme partition de l’annuaire d’applications TAPI par défaut pour le nouveau domaine, tapez :
```
tapicfg install /directory:tapifiction.testdom.microsoft.com /server:testdc.testdom.microsoft.com /forcedefault
```
Pour afficher le nom de la partition d’annuaire d’applications TAPI par défaut pour le nouveau domaine, tapez :
```
tapicfg show /defaultonly
```
## <a name="additional-references"></a>Références supplémentaires
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
