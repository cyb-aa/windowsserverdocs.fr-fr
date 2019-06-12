---
title: lpr
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: afc8790b-8b52-45c4-acdf-be0ffa9da534 jpjofre
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ddc64f958dcb38129d81becfc8950059e055d8e5
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437478"
---
# <a name="lpr"></a>lpr

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Envoie un fichier sur un ordinateur ou un appareil exécutant le service démon LPD (Line printer) en préparation pour l’impression de partage d’imprimante.  

## <a name="syntax"></a>Syntaxe  
```  
lpr [-S <ServerName>] -P <printerName> [-C <BannerContent>] [-J <JobName>] [-o | "-o l"] [-x] [-d] <filename>  
```  
## <a name="parameters"></a>Paramètres  

|     Paramètre      |                                                                                                           Description                                                                                                           |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  -S <ServerName>   |                                    Spécifie (par nom ou adresse IP) de l’ordinateur ou un périphérique qui héberge la file d’attente d’impression LPD dont vous souhaitez afficher l’état de partage d’imprimante. Obligatoire.                                    |
|  -P <printerName>  |                                                              Spécifie (par nom) l’imprimante pour la file d’attente à l’impression dont vous souhaitez afficher l’état. Obligatoire.                                                              |
| -C <BannerContent> |                Spécifie le contenu à imprimer sur la page de la bannière du travail d’impression. Si vous n’incluez pas ce paramètre, le nom de l’ordinateur à partir de laquelle le travail d’impression a été envoyé apparaît sur la page d’accueil.                 |
|    -J <JobName>    |                           Spécifie le nom du travail d’impression qui est imprimé sur la page d’accueil. Si vous n’incluez pas ce paramètre, le nom du fichier en cours d’impression apparaît sur la page d’accueil.                            |
| [-o&#124; "-o l"]  | Spécifie le type de fichier que vous souhaitez imprimer. Le paramètre **-o** Spécifie que vous souhaitez imprimer un fichier texte. Le paramètre **»-o l »** Spécifie que vous souhaitez imprimer un fichier binaire (par exemple, un fichier PostScript). |
|         -d         |              Spécifie que le fichier de données doit être envoyé avant le fichier de contrôle. Utilisez ce paramètre si votre imprimante requiert le fichier de données pour être envoyés en premier. Pour plus d’informations, consultez la documentation de votre imprimante.               |
|         -x         |                               Spécifie que le **lpr** commande doit être compatible avec le Sun Microsystems système d’exploitation (appelé SunOS) pour les versions jusqu'à et y compris la version 4.1.4_u1.                                |
|     <FileName>     |                                                                                      Spécifie le fichier à imprimer (par nom). Obligatoire.                                                                                      |
|         /?         |                                                                                              Affiche l'aide à l'invite de commandes.                                                                                               |

## <a name="remarks"></a>Notes  
- Pour rechercher le nom de l’imprimante, ouvrez le dossier Imprimantes.  
- Le **-S**, **-P**, **- C**, et **-J** paramètres respectent la casse et doivent être tapées en majuscules.  
  ## <a name="BKMK_examples"></a>Exemples  
  Cet exemple montre comment imprimer un fichier texte « Document.txt » à la file d’attente de Laserprinter1 imprimante sur un ordinateur hôte à 10.0.0.45 LPD :  
  ```  
  lpr -S 10.0.0.45 -P Laserprinter1 -o Document.txt  
  ```  
  Cet exemple montre comment imprimer le fichier PostScript d’Adobe « PostScript_file.ps » à la file d’attente de Laserprinter1 imprimante sur un ordinateur hôte à 10.0.0.45 LPD :  
  ```  
  lpr -S 10.0.0.45 -P Laserprinter1 "-o l" PostScript_file.ps  
  ```  

#### <a name="additional-references"></a>Références supplémentaires  
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
-   [Référence des commandes d’impression](print-command-reference.md)  
