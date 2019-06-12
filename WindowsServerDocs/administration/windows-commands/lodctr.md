---
title: lodctr
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: b67c11b24013adb911309ab2ea9bdbfdfc3408da
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437895"
---
# <a name="lodctr"></a>lodctr

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Permet de vous inscrire ou enregistrer des paramètres de nom et du Registre du compteur de performances dans un fichier et désigner des services de confiance.
## <a name="syntax"></a>Syntaxe
```
lodctr <filename> [/s:<filename>] [/r:<filename>] [/t:<servicename>]
```
### <a name="parameters"></a>Paramètres

|    Paramètre     |                                                                                                                                         Description                                                                                                                                          |
|------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <filename>    |                                                                                          Enregistre les paramètres de nom de compteur de Performance et le texte d’explication fournis dans le fichier d’initialisation FileName.                                                                                          |
|  /s:<filename>   |                                                                                                       Les performances enregistre les paramètres de Registre du compteur et texte d’explication au fichier <filename>.                                                                                                       |
|        /r        |                                Restaure les paramètres de Registre de compteur et le texte d’explication à partir des paramètres du Registre et des fichiers de performance liés au Registre.<br /><br />Cette option est disponible uniquement dans le système d’exploitation Windows Server 2003.                                |
|  /r:<filename>   | Performances des restaurations des paramètres de Registre du compteur et expliquer le texte à partir du fichier <filename>. **Avertissement :** si vous utilisez le **lodctr /r** commande, vous allez remplacer tous les paramètres de Registre de compteur de performances et le texte d’explication en les remplaçant par la configuration définie dans le fichier spécifié. |
| /t:<servicename> |                                                                                                                       Indique ce service <servicename> est approuvé.                                                                                                                       |
|        /?        |                                                                                                                             Affiche l'aide à l'invite de commandes.                                                                                                                             |

## <a name="remarks"></a>Notes
Si les informations que vous fournissez contient des espaces, utilisez des guillemets autour du texte (par exemple, «<filename>»).
## <a name="BKMK_Examples"></a>Exemples
Pour enregistrer les paramètres de Registre de performances actuels et au fichier texte d’explication du compteur **nommé perf backup1.txt**:
```
lodctr /s:"perf backup1.txt"
```
## <a name="additional-references"></a>Références supplémentaires
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

