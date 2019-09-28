---
title: lodctr
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5a849abd-6b31-4833-bc8a-306c05eca29a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 96a2818110b766071eb83822abd34d8c0d00132d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374572"
---
# <a name="lodctr"></a>lodctr

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Vous permet d’enregistrer ou d’enregistrer les paramètres de nom et de Registre du compteur de performance dans un fichier et de désigner les services approuvés.
## <a name="syntax"></a>Syntaxe
```
lodctr <filename> [/s:<filename>] [/r:<filename>] [/t:<servicename>]
```
### <a name="parameters"></a>Paramètres

|    Paramètre     |                                                                                                                                         Description                                                                                                                                          |
|------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <filename>    |                                                                                          Enregistre les paramètres de nom du compteur de performance et le texte d’explication fourni dans le nom de fichier d’initialisation.                                                                                          |
|  /s : <filename>   |                                                                                                       Enregistre les paramètres de Registre du compteur de performance et le texte d’explication dans le fichier <filename>.                                                                                                       |
|        /r        |                                Restaure les paramètres de registre des compteurs et le texte d’explication à partir des paramètres de registre actuels et des fichiers de performance mis en cache associés au registre.<br /><br />Cette option est disponible uniquement dans le système d’exploitation Windows Server 2003.                                |
|  /r : <filename>   | Restaure les paramètres de Registre du compteur de performance et le texte d’explication à partir du fichier <filename>. **Avertissement :** si vous utilisez la commande **lodctr/r** , vous remplacerez tous les paramètres de Registre du compteur de performance et le texte d’explication, en les remplaçant par la configuration définie dans le fichier spécifié. |
| /t : <servicename> |                                                                                                                       Indique que le service <servicename> est approuvé.                                                                                                                       |
|        /?        |                                                                                                                             Affiche l'aide à l'invite de commandes.                                                                                                                             |

## <a name="remarks"></a>Notes
Si les informations que vous fournissez contiennent des espaces, utilisez des guillemets autour du texte (par exemple, « <filename> »).
## <a name="BKMK_Examples"></a>Illustre
Pour enregistrer les paramètres de registre de performance actuels et le texte d’explication du compteur dans le fichier **perf manuelle1. txt**:
```
lodctr /s:"perf backup1.txt"
```
## <a name="additional-references"></a>Références supplémentaires
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

