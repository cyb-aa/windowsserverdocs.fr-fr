---
title: ftp mdir
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 90eec45b-558b-4b8d-bbe4-b56d98e1ca70 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0ac1e7cd50fe4d9325c272f74a7b81971c8bb12a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878210"
---
# <a name="ftp-mdir"></a>FTP : mdir

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche une liste de répertoires de fichiers et sous-répertoires dans un répertoire distant.   
## <a name="syntax"></a>Syntaxe  
```  
mdir <remoteFile>[ ] <LocalFile>  
```  
### <a name="parameters"></a>Paramètres  
|Paramètre|Description|  
|-------|--------|  
|<remoteFile>|Spécifie le répertoire ou le fichier pour lequel vous souhaitez afficher une liste.|  
|<LocalFile>|Spécifie un fichier local pour stocker la liste. Ce paramètre est obligatoire.|  
## <a name="remarks"></a>Notes  
-   Vous pouvez utiliser **mdir** pour spécifier plusieurs fichiers.  
-   Spécification *Fichier_distant*  
    Tapez un trait d’union (**-**) à utiliser le répertoire de travail actuel sur l’ordinateur distant.  
-   En spécifiant un *LocalFile*  
    Tapez un trait d’union (**-**) pour afficher la liste à l’écran.  
## <a name="BKMK_Examples"></a>Exemples  
Afficher une liste des répertoires **dir1** et **dir2** sur l’écran  
```  
mdir dir1 dir2 -  
```  
Enregistrer la liste des répertoires combiné **dir1** et **dir2** dans un fichier local appelé **ListRép.txt**  
```  
mdir dir1 dir2 dirlist.txt  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)  
