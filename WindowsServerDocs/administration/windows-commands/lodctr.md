---
title: lodctr
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5a849abd-6b31-4833-bc8a-306c05eca29a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e20e085e5e2cadcb0684ef57f80137a6043b1e64
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724439"
---
# <a name="lodctr"></a>lodctr

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vous permet d’enregistrer ou d’enregistrer les paramètres de nom et de Registre du compteur de performance dans un fichier et de désigner les services approuvés.
## <a name="syntax"></a>Syntaxe
```
lodctr <filename> [/s:<filename>] [/r:<filename>] [/t:<servicename>]
```
#### <a name="parameters"></a>Paramètres

|    Paramètre     |                                                                                                                                         Description                                                                                                                                          |
|------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <filename>    |                                                                                          Enregistre les paramètres de nom du compteur de performance et le texte d’explication fourni dans le nom de fichier d’initialisation.                                                                                          |
|  commutateur<filename>   |                                                                                                       Enregistre les paramètres de registre des compteurs de performances et <filename>le texte d’explication dans un fichier.                                                                                                       |
|        /r        |                                Restaure les paramètres de registre des compteurs et le texte d’explication à partir des paramètres de registre actuels et des fichiers de performance mis en cache associés au registre.<p>Cette option est disponible uniquement dans le système d’exploitation Windows Server 2003.                                |
|  /r<filename>   | Restaure les paramètres du registre des compteurs de performances et le <filename>texte d’explication à partir du fichier. **Avertissement :** si vous utilisez la commande **lodctr/r** , vous remplacerez tous les paramètres de Registre du compteur de performance et le texte d’explication, en les remplaçant par la configuration définie dans le fichier spécifié. |
| /t:<servicename> |                                                                                                                       Indique que le <servicename> service est approuvé.                                                                                                                       |
|        /?        |                                                                                                                             Affiche l'aide à l'invite de commandes.                                                                                                                             |

## <a name="remarks"></a>Notes 
Si les informations que vous fournissez contiennent des espaces, utilisez des guillemets autour du texte ( <filename>par exemple,).
## <a name="examples"></a>Exemples
Pour enregistrer les paramètres de registre de performance actuels et le texte d’explication du compteur dans le fichier **perf manuelle1. txt**:
```
lodctr /s:perf backup1.txt
```
## <a name="additional-references"></a>Références supplémentaires
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

