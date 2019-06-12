---
title: envoi de Telnet
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 36dc7f861e88cf991af57dda2f150107c6870f0f
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441041"
---
# <a name="telnet-send"></a>telnet: send

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Envoie des commandes telnet sur le serveur telnet.   
## <a name="syntax"></a>Syntaxe  
```  
sen[d] {ao | ayt | brk | esc | ip | synch | <string>} [?]  
```  
### <a name="parameters"></a>Paramètres  

| Paramètre |                     Description                      |
|-----------|------------------------------------------------------|
|    ao     |       Envoie la commande telnet abandonner la sortie.        |
|    ayt    |       Envoie la commande telnet sont vous il.       |
|    brk    |            Envoie le brk commande telnet.            |
|    esc    |      Envoie le caractère d’échappement telnet actuel.      |
|    ip     |     Envoie la commande telnet interrompre le processus.     |
|   synchronisation   |           Envoie la synchronisation de commande telnet.           |
| <string>  | Envoie la chaîne que vous tapez pour le serveur telnet. |
|     ?     |     Affiche l’aide associée à cette commande.      |

## <a name="BKMK_Examples"></a>Exemples  
Envoi êtes-vous il sur le serveur telnet.  
```  
sen ayt  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
