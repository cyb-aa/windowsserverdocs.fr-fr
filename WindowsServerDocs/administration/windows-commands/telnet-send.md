---
title: envoi Telnet
description: Rubrique relative aux commandes Windows pour telnet Send, qui envoie des commandes telnet au serveur Telnet.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7c217abc-1182-466e-914c-1ff16755021b vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6fef48ca04a3817f58d063bc8b23f5c11c4ea197
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80833282"
---
# <a name="telnet-send"></a>Telnet : envoyer

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Envoie des commandes telnet au serveur Telnet.   

## <a name="syntax"></a>Syntaxe  
```  
sen[d] {ao | ayt | brk | esc | ip | synch | <string>} [?]  
```  
#### <a name="parameters"></a>Paramètres  

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

## <a name="examples"></a><a name=BKMK_Examples></a>Illustre  
Envoyez-vous au serveur Telnet.  
```  
sen ayt  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
