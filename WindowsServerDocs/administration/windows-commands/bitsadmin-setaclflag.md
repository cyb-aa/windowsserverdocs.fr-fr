---
title: bitsadmin setaclflag
description: Rubrique de référence pour la commande Bitsadmin setaclflag, qui définit les indicateurs de propagation de la liste de contrôle d’accès (ACL).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6e3bcda0-827d-4dfd-8384-d1da018f3e10
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1852bd267fe22825d55f7522a81179e9290e2a00
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82716985"
---
# <a name="bitsadmin-setaclflag"></a>bitsadmin setaclflag

Définit les indicateurs de propagation de la liste de contrôle d’accès (ACL) pour le travail. Les indicateurs indiquent que vous souhaitez conserver les informations relatives au propriétaire et à la liste de contrôle d’accès avec le fichier en cours de téléchargement. Par exemple, pour conserver le propriétaire et le groupe avec le fichier, définissez le paramètre **Flags** sur `og`.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /setaclflag <job> <flags>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| travail | Nom complet ou GUID du travail. |
| flags | Spécifiez une ou plusieurs des valeurs, y compris :<ul><li>**o** -copier les informations de propriétaire avec le fichier.</li><li>**g** -copier les informations de groupe avec le fichier.</li><li>**d** -copie les informations discrétionnaires sur la liste de contrôle d’accès (DACL) avec le fichier.</li><li>**s** -copier les informations de la liste de contrôle d’accès système (SACL) avec le fichier.</li></ul> |

## <a name="examples"></a>Exemples

Pour définir les indicateurs de propagation de la liste de contrôle d’accès pour la tâche nommée *myDownloadJob*, elle conserve les informations de propriétaire et de groupe avec les fichiers téléchargés.

```
bitsadmin /setaclflags myDownloadJob og
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin](bitsadmin.md)
