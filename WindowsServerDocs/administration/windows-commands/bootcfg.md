---
title: bootcfg
description: Rubrique de commandes de Windows pour **bootcfg** - configure, interroge ou modifie les paramètres du fichier Boot.ini.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3deb354c-5717-4066-bc79-b9323d559e44
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 79a1c0e22a3b162ba9492c80d114b2d5b943c744
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867020"
---
# <a name="bootcfg"></a>bootcfg

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Configure, interroge ou modifie les paramètres du fichier Boot.ini.  
## <a name="syntax"></a>Syntaxe  
```  
bootcfg <parameter> [arguments...]  
```  
## <a name="parameters"></a>Paramètres  
|Paramètre|Description|  
|-------|--------|  
|[bootcfg addsw](bootcfg-addsw.md)|Ajoute les options de chargement de système d’exploitation pour une entrée de système d’exploitation spécifié.|  
|[bootcfg copy](bootcfg-copy.md)|Effectue une copie d’une entrée de démarrage existante, auquel vous pouvez ajouter des options de ligne de commande.|  
|[bootcfg dbg1394](bootcfg-dbg1394.md)|Configure le débogage de port 1394 pour une entrée de système d’exploitation spécifié.|  
|[bootcfg debug](bootcfg-debug.md)|Ajoute ou modifie les paramètres de débogage pour une entrée de système d’exploitation spécifié.|  
|[bootcfg default](bootcfg-default.md)|Spécifie l’entrée de système d’exploitation pour indiquer que la valeur par défaut.|  
|[bootcfg delete](bootcfg-delete.md)|Supprime une entrée de système d’exploitation dans le **[systèmes d’exploitation]** section du fichier Boot.ini.|  
|[bootcfg ems](bootcfg-ems.md)|Permet à l’utilisateur Ajouter ou modifier les paramètres de redirection de la console de gestion des Services d’urgence à un ordinateur distant.|  
|[bootcfg query](bootcfg-query.md)|Interroge et affiche le [chargeur de démarrage] et **[systèmes d’exploitation]** section entrées à partir du fichier Boot.ini.|  
|[bootcfg raw](bootcfg-raw.md)|Ajoute les options de chargement du système d’exploitation spécifiées sous forme de chaîne à une entrée de système d’exploitation dans le **[systèmes d’exploitation]** section du fichier Boot.ini.|  
|[bootcfg rmsw](bootcfg-rmsw.md)|Supprime les options de chargement de système d’exploitation pour une entrée de système d’exploitation spécifié.|  
|[bootcfg timeout](bootcfg-timeout.md)|Modifie la valeur de délai d’attente de système d’exploitation.|  
