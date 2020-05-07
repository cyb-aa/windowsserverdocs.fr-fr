---
title: SC. exe supprimer
description: Découvrez comment annuler l’inscription des services à l’aide de l’utilitaire SC. exe
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2fe94fb3-e4d1-47b5-b999-39995ecbb644
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 284012cf6799df52832e62c3eea1b2f0fcd84805
ms.sourcegitcommit: 95b60384b0b070263465eaffb27b8e3bb052a4de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/06/2020
ms.locfileid: "82850110"
---
# <a name="scexe-delete"></a>SC. exe supprimer

Supprime une sous-clé de service du Registre. Si le service est en cours d’exécution ou si un autre processus a un descripteur ouvert pour le service, le service est marqué pour suppression.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#examples).

## <a name="syntax"></a>Syntaxe

```
sc.exe [<ServerName>] delete [<ServiceName>]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<Nom du serveur>|Spécifie le nom du serveur distant sur lequel se trouve le service. Le nom doit utiliser le format UNC (Universal Naming Convention) (par exemple, \\ \\monserveur). Pour exécuter SC. exe localement, omettez ce paramètre.|
|\<> ServiceName|Spécifie le nom du service retourné par l’opération **getkeyname** .|
|?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes 

Il n’est pas recommandé d’utiliser SC. exe pour supprimer des services de système d’exploitation intégrés tels que DHCP, DNS ou Internet Information Services. Pour installer, supprimer ou reconfigurer des rôles, des services et des composants de système d’exploitation, consultez [installer ou désinstaller des rôles, des services de rôle ou des fonctionnalités](/WindowsServerDocs/administration/server-manager/install-or-uninstall-roles-role-services-or-features.md)

## <a name="examples"></a>Exemples

Pour supprimer la sous-clé de service **NewServ** du Registre sur l’ordinateur local, tapez :
```
sc.exe delete newserv
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
