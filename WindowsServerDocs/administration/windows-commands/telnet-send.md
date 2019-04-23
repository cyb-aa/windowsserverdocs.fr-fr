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
ms.openlocfilehash: 32345b22395107f4a2c3d88894126d4e5e0875a1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842360"
---
# <a name="telnet-send"></a>telnet: send

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Envoie des commandes telnet sur le serveur telnet.   
## <a name="syntax"></a>Syntaxe  
```  
sen[d] {ao | ayt | brk | esc | ip | synch | <string>} [?]  
```  
### <a name="parameters"></a>Paramètres  
|Paramètre|Description|  
|-------|--------|  
|ao|Envoie la commande telnet abandonner la sortie.|  
|ayt|Envoie la commande telnet sont vous il.|  
|brk|Envoie le brk commande telnet.|  
|esc|Envoie le caractère d’échappement telnet actuel.|  
|ip|Envoie la commande telnet interrompre le processus.|  
|synchronisation|Envoie la synchronisation de commande telnet.|  
|<string>|Envoie la chaîne que vous tapez pour le serveur telnet.|  
|?|Affiche l’aide associée à cette commande.|  
## <a name="BKMK_Examples"></a>Exemples  
Envoi êtes-vous il sur le serveur telnet.  
```  
sen ayt  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)  
