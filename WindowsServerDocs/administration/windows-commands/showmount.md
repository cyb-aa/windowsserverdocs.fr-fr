---
title: showmount
description: Rubrique relative aux commandes Windows pour showmount, qui affiche les répertoires montés.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a6dd562e-e3bd-4ee6-be3b-6d29e29fd20e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6fa61d47bb14cf21d93beec0a6e9257b9f66737b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834222"
---
# <a name="showmount"></a>showmount

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vous pouvez utiliser **showmount** pour afficher les répertoires montés.  
  
## <a name="syntax"></a>Syntaxe  
```
showmount {-e|-a|-d} <Server>  
```

## <a name="description"></a>Description  
La commande **showmount**\-utilitaire de ligne affiche des informations sur les systèmes de fichiers montés, exportés par le serveur pour NFS sur l’ordinateur spécifié par le *serveur*. Si le *serveur* n’est pas fourni, **showmount** affiche des informations sur l’ordinateur sur lequel la commande **showmount** est exécutée.  
  
Vous devez fournir l’une des options suivantes :  
  
- **\-e** -affiche tous les systèmes de fichiers exportés sur le serveur.  
- **\-a** -affiche tout le système de fichiers réseau \(les clients NFS\) et les répertoires sur le serveur, chacun étant monté.  
- **\-d** -affiche tous les répertoires sur le serveur qui sont actuellement montés par les clients NFS.  
  
## <a name="see-also"></a>Voir aussi  
[Référence des commandes des services pour NFS](services-for-network-file-system-command-reference.md)  