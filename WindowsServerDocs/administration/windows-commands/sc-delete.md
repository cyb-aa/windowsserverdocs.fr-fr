---
title: SC supprimer
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2fe94fb3-e4d1-47b5-b999-39995ecbb644
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dd40b5eb82def3b3c437cbdb5b60d279529d25a0
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722190"
---
# <a name="sc-delete"></a>SC supprimer



Supprime une sous-clé de service du Registre. Si le service est en cours d’exécution ou si un autre processus a un descripteur ouvert pour le service, le service est marqué pour suppression.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#examples).

## <a name="syntax"></a>Syntaxe

```
sc [<ServerName>] delete [<ServiceName>]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<Nom du serveur>|Spécifie le nom du serveur distant sur lequel se trouve le service. Le nom doit utiliser le format UNC (Universal Naming Convention) (par exemple, \\ \\monserveur). Pour exécuter SC. exe localement, omettez ce paramètre.|
|\<> ServiceName|Spécifie le nom du service retourné par l’opération **getkeyname** .|
|?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes 

Utilisez **Ajout/suppression de programmes** dans le **panneau de configuration** pour supprimer DHCP, DNS ou tout autre service de système d’exploitation intégré. Notez que l' **Ajout/suppression de programmes** supprimera non seulement la sous-clé du Registre pour le service, mais désinstallera également le service et supprimera les raccourcis qui y sont associés.

## <a name="examples"></a>Exemples

Pour supprimer la sous-clé de service **NewServ** du Registre sur l’ordinateur local, tapez :
```
sc delete newserv
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)