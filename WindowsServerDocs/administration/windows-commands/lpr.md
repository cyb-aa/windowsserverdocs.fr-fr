---
title: lpr
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: afc8790b-8b52-45c4-acdf-be0ffa9da534 jpjofre
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f523c55f5974599c152f4fbae7d8143d5362af62
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724246"
---
# <a name="lpr"></a>lpr

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Envoie un fichier à un ordinateur ou un périphérique de partage d’imprimante exécutant le service LPD (Line Printer Daemon) en préparation de l’impression.  

## <a name="syntax"></a>Syntaxe  
```  
lpr [-S <ServerName>] -P <printerName> [-C <BannerContent>] [-J <JobName>] [-o | -o l] [-x] [-d] <filename>  
```  
### <a name="parameters"></a>Paramètres  

|     Paramètre      |                                                                                                           Description                                                                                                           |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  -S<ServerName>   |                                    Spécifie (par nom ou adresse IP) le périphérique de partage de l’ordinateur ou de l’imprimante qui héberge la file d’attente à l’impression LPD avec l’état que vous souhaitez afficher. Obligatoire.                                    |
|  -P<printerName>  |                                                              Spécifie (par nom) l’imprimante pour la file d’attente à l’impression avec l’état que vous souhaitez afficher. Obligatoire.                                                              |
| -C<BannerContent> |                Spécifie le contenu à imprimer sur la page de bannière du travail d’impression. Si vous n’incluez pas ce paramètre, le nom de l’ordinateur à partir duquel le travail d’impression a été envoyé s’affiche sur la page bannière.                 |
|    -J<JobName>    |                           Spécifie le nom du travail d’impression qui sera imprimé sur la page de bannière. Si vous n’incluez pas ce paramètre, le nom du fichier en cours d’impression apparaît sur la page de bannière.                            |
| [-o&#124;-o l]  | Spécifie le type de fichier que vous souhaitez imprimer. Le paramètre **-o** spécifie que vous souhaitez imprimer un fichier texte. Le paramètre **-o l** spécifie que vous souhaitez imprimer un fichier binaire (par exemple, un fichier PostScript). |
|         -d         |              Spécifie que le fichier de données doit être envoyé avant le fichier de contrôle. Utilisez ce paramètre si votre imprimante nécessite que le fichier de données soit d’abord envoyé. Pour plus d’informations, consultez la documentation de votre imprimante.               |
|         -X         |                               Spécifie que la commande **LPR** doit être compatible avec le système d’exploitation Sun Microsystems (appelé SunOS) pour les versions antérieures à 4.1.4_u1.                                |
|     <FileName>     |                                                                                      Spécifie (par nom) le fichier à imprimer. Obligatoire.                                                                                      |
|         /?         |                                                                                              Affiche l'aide à l'invite de commandes.                                                                                               |

## <a name="remarks"></a>Notes   
- Pour rechercher le nom de l’imprimante, ouvrez le dossier Imprimantes.  
- Les paramètres **-S**, **-P**, **-C**et **-J** respectent la casse et doivent être tapés en majuscules.  
  ## <a name="examples"></a>Exemples  
  Cet exemple montre comment imprimer le fichier texte document. txt dans la file d’attente d’impression Laserprinter1 sur un hôte LPD sur 10.0.0.45 :  
  ```  
  lpr -S 10.0.0.45 -P Laserprinter1 -o Document.txt  
  ```  
  Cet exemple montre comment imprimer le fichier Adobe PostScript PostScript_file. PS sur la file d’attente d’impression Laserprinter1 sur un hôte LPD sur 10.0.0.45 :  
  ```  
  lpr -S 10.0.0.45 -P Laserprinter1 -o l PostScript_file.ps  
  ```  

## <a name="additional-references"></a>Références supplémentaires  
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
-   [imprimer la référence des commandes](print-command-reference.md)  
