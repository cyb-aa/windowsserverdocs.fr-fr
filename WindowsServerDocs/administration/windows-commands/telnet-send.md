---
title: envoi Telnet
description: Rubrique de référence pour telnet Send, qui envoie des commandes telnet au serveur Telnet.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7c217abc-1182-466e-914c-1ff16755021b vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 432401bbe2050a7954967a73b5ba8abeee5bb1d3
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721487"
---
# <a name="telnet-send"></a>Telnet : envoyer

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Envoie des commandes telnet au serveur Telnet.   

## <a name="syntax"></a>Syntaxe  
```  
sen[d] {ao | ayt | brk | esc | ip | synch | <string>} [?]  
```  
#### <a name="parameters"></a>Paramètres  

| Paramètre |                     Description                      |
|-----------|------------------------------------------------------|
|    AO     |       Envoie la sortie de l’annulation de la commande telnet.        |
|    ayt    |       Envoie la commande telnet ici.       |
|    brk    |            Envoie la commande telnet BRK.            |
|    esc    |      Envoie le caractère d’échappement Telnet actuel.      |
|    ip     |     Envoie le processus d’interruption de la commande telnet.     |
|   poche   |           Envoie la synchronisation de la commande telnet.           |
| <string>  | Envoie toute chaîne que vous tapez au serveur Telnet. |
|     ?     |     Affiche l’aide associée à cette commande.      |

## <a name="examples"></a>Exemples  
Envoyez-vous au serveur Telnet.  
```  
sen ayt  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
