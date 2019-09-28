---
title: envoi Telnet
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7c217abc-1182-466e-914c-1ff16755021b vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 958e0e507e5a0ae836da98de8d677a116dbb38bd
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383630"
---
# <a name="telnet-send"></a>Telnet : envoyer

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Envoie des commandes telnet au serveur Telnet.   
## <a name="syntax"></a>Syntaxe  
```  
sen[d] {ao | ayt | brk | esc | ip | synch | <string>} [?]  
```  
### <a name="parameters"></a>Paramètres  

| Paramètre |                     Description                      |
|-----------|------------------------------------------------------|
|    ao     |       Envoie la sortie de l’annulation de la commande telnet.        |
|    ayt    |       Envoie la commande telnet ici.       |
|    brk    |            Envoie la commande telnet BRK.            |
|    Escudo    |      Envoie le caractère d’échappement Telnet actuel.      |
|    adressesIP     |     Envoie le processus d’interruption de la commande telnet.     |
|   poche   |           Envoie la synchronisation de la commande telnet.           |
| <string>  | Envoie toute chaîne que vous tapez au serveur Telnet. |
|     ?     |     Affiche l’aide associée à cette commande.      |

## <a name="BKMK_Examples"></a>Illustre  
Envoyez-vous au serveur Telnet.  
```  
sen ayt  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
