---
title: showmount
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a6dd562e-e3bd-4ee6-be3b-6d29e29fd20e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 26acf24922e6ac53a5c902d65eb0f23bff0af93b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852350"
---
# <a name="showmount"></a>showmount

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vous pouvez utiliser **showmount** pour afficher les répertoires montés.  
  
## <a name="syntax"></a>Syntaxe  
```
showmount {-e|-a|-d} <Server>  
```

## <a name="description"></a>Description  
Le **showmount** commande\-utilitaire de ligne affiche des informations sur les systèmes de fichiers montés exporté par serveur pour NFS sur l’ordinateur spécifié par *Server*. Si *Server* n’est pas fourni, **showmount** affiche des informations sur l’ordinateur sur lequel le **showmount** commande est exécutée.  
  
Vous devez fournir une des options suivantes :  
  
- **\-e** -affiche tous les systèmes exportés sur le serveur de fichiers.  
- **\-un** -affiche tous les Network File System \(NFS\) les clients et les répertoires sur le serveur, chacun a monté.  
- **\-d** -affiche tous les répertoires sur le serveur qui sont actuellement monté par les clients NFS.  
  
## <a name="see-also"></a>Voir aussi  
[Services pour la référence de commande de système de fichiers réseau](services-for-network-file-system-command-reference.md)  