---
title: bitsadmin setaclflag
description: La rubrique commandes Windows pour **Bitsadmin setaclflag**, qui définit les indicateurs de propagation de la liste de contrôle d’accès (ACL).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6e3bcda0-827d-4dfd-8384-d1da018f3e10
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f0aae550e94d04db518edccafb1d6bcf46d0320b
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/11/2020
ms.locfileid: "81123065"
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
| le travail | Nom complet ou GUID du travail. |
| flags | Spécifiez une ou plusieurs des valeurs, y compris :<ul><li>**o** -copier les informations de propriétaire avec le fichier.</li><li>**g** -copier les informations de groupe avec le fichier.</li><li>**d** -copie les informations discrétionnaires sur la liste de contrôle d’accès (DACL) avec le fichier.</li><li>**s** -copier les informations de la liste de contrôle d’accès système (SACL) avec le fichier.</li></ul> |

## <a name="remarks"></a>Notes

Le commutateur/setaclflag est utilisé pour gérer les informations de propriétaire et de liste de contrôle d’accès lorsqu’un travail télécharge des données à partir d’un partage Windows (SMB).

## <a name="examples"></a>Exemples

L’exemple suivant définit les indicateurs de propagation de la liste de contrôle d’accès pour le travail nommé *myDownloadJob* afin de conserver les informations de propriétaire et de groupe avec les fichiers téléchargés.

```
C:\>bitsadmin /setaclflags myDownloadJob og
```

## <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)&reg;'    