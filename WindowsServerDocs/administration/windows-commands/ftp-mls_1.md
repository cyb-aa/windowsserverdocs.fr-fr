---
title: ftp mls_1
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4738fd49-0e80-4bdf-a773-0f973db3a710 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6cf81018fa590d38e55778d60b0cb0e849ab83de
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835700"
---
# <a name="ftp-mls1"></a>FTP : mls_1

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche une liste abrégée des fichiers et sous-répertoires dans un répertoire distant.   
## <a name="syntax"></a>Syntaxe  
```  
mls <remoteFile>[ ] <LocalFile>  
```  
### <a name="parameters"></a>Paramètres  
|Paramètre|Description|  
|-------|--------|  
|<remoteFile>|Spécifie le fichier pour lequel vous souhaitez afficher une liste.|  
|<LocalFile>|Spécifie un fichier local dans lequel stocker la liste.|  
## <a name="remarks"></a>Notes  
-   Spécification *Fichiers_distants*  
    Tapez un trait d’union (**-**) à utiliser le répertoire de travail actuel sur l’ordinateur distant.  
-   Spécification *LocalFile*  
    Tapez un trait d’union (**-**) pour afficher la liste à l’écran.  
## <a name="BKMK_Examples"></a>Exemples  
Afficher une liste abrégée des fichiers et sous-répertoires **dir1** et **dir2**.  
```  
mls dir1 dir2 -  
```  
Enregistrer une liste abrégée des fichiers et sous-répertoires **dir1** et **dir2** dans le fichier local **ListRép.txt**  
```  
mls dir1 dir2 dirlist.txt   
```  
## <a name="additional-references"></a>Références supplémentaires  
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)  
