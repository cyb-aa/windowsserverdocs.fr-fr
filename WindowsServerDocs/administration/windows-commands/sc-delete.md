---
title: Suppression de SC
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2fe94fb3-e4d1-47b5-b999-39995ecbb644
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b60127cf957a30d147c9992c74c01e37e5b8bf89
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871920"
---
# <a name="sc-delete"></a>Suppression de SC



Supprime une sous-clé de service à partir du Registre. Si le service est en cours d’exécution ou si un autre processus a un handle ouvert pour le service, le service est marqué pour suppression.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
sc [<ServerName>] delete [<ServiceName>]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<ServerName>|Spécifie le nom du serveur distant sur lequel se trouve le service. Le nom doit utiliser le format UNC Universal Naming Convention () (par exemple, \\ \\myserver). Pour exécuter SC.exe localement, omettez ce paramètre.|
|\<ServiceName>|Spécifie le nom de service retourné par la **getkeyname** opération.|
|?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

Utilisez **Ajout / Suppression de programmes** sur **le panneau de configuration** pour supprimer DHCP, DNS ou autres services de système d’exploitation intégré. Notez que **Ajout / Suppression de programmes** uniquement ne supprimera pas la sous-clé de Registre pour le service, mais il sera également désinstaller le service et supprimez tous les raccourcis à celui-ci.

## <a name="BKMK_examples"></a>Exemples

Pour supprimer la sous-clé service **NewServ** à partir du Registre sur l’ordinateur local, tapez :
```
sc delete newserv
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)