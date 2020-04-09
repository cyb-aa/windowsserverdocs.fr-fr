---
title: lodctr
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5a849abd-6b31-4833-bc8a-306c05eca29a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 33c14970a669d24f1cc803003e8530712311c564
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80840952"
---
# <a name="lodctr"></a>lodctr

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vous permet d’enregistrer ou d’enregistrer les paramètres de nom et de Registre du compteur de performance dans un fichier et de désigner les services approuvés.
## <a name="syntax"></a>Syntaxe
```
lodctr <filename> [/s:<filename>] [/r:<filename>] [/t:<servicename>]
```
#### <a name="parameters"></a>Paramètres

|    Paramètre     |                                                                                                                                         Description                                                                                                                                          |
|------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <filename>    |                                                                                          Enregistre les paramètres de nom du compteur de performance et le texte d’explication fourni dans le nom de fichier d’initialisation.                                                                                          |
|  /s :<filename>   |                                                                                                       Enregistre les paramètres de registre des compteurs de performance et le texte d’explication dans le <filename>de fichiers.                                                                                                       |
|        /r        |                                Restaure les paramètres de registre des compteurs et le texte d’explication à partir des paramètres de registre actuels et des fichiers de performance mis en cache associés au registre.<p>Cette option est disponible uniquement dans le système d’exploitation Windows Server 2003.                                |
|  /r :<filename>   | Restaure les paramètres de Registre du compteur de performance et le texte d’explication à partir du <filename>de fichiers. **Avertissement :** si vous utilisez la commande **lodctr/r** , vous remplacerez tous les paramètres de Registre du compteur de performance et le texte d’explication, en les remplaçant par la configuration définie dans le fichier spécifié. |
| /t :<servicename> |                                                                                                                       Indique que le service <servicename> est approuvé.                                                                                                                       |
|        /?        |                                                                                                                             Affiche l'aide à l'invite de commandes.                                                                                                                             |

## <a name="remarks"></a>Notes
Si les informations que vous fournissez contiennent des espaces, utilisez des guillemets autour du texte (par exemple, <filename>).
## <a name="examples"></a><a name=BKMK_Examples></a>Illustre
Pour enregistrer les paramètres de registre de performance actuels et le texte d’explication du compteur dans le fichier **perf manuelle1. txt**:
```
lodctr /s:perf backup1.txt
```
## <a name="additional-references"></a>Références supplémentaires
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

