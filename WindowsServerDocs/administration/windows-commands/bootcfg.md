---
title: bootcfg
description: La rubrique commandes Windows pour **bootcfg** -configure, interroge ou modifie les paramètres du fichier Boot. ini.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 2d66296327a2221093e5434f69e15e7c55df1f6b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379852"
---
# <a name="bootcfg"></a>bootcfg

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Configure, interroge ou modifie les paramètres du fichier Boot.ini.  
## <a name="syntax"></a>Syntaxe  
```  
bootcfg <parameter> [arguments...]  
```  
## <a name="parameters"></a>Paramètres  
|Paramètre|Description|  
|-------|--------|  
|[bootcfg addsw](bootcfg-addsw.md)|Ajoute des options de chargement du système d’exploitation pour une entrée de système d’exploitation spécifiée.|  
|[bootcfg copy](bootcfg-copy.md)|Effectue une copie d’une entrée de démarrage existante, à laquelle vous pouvez ajouter des options de ligne de commande.|  
|[bootcfg dbg1394](bootcfg-dbg1394.md)|Configure le débogage de port 1394 pour une entrée de système d’exploitation spécifiée.|  
|[bootcfg debug](bootcfg-debug.md)|Ajoute ou modifie les paramètres de débogage pour une entrée de système d’exploitation spécifiée.|  
|[bootcfg default](bootcfg-default.md)|Spécifie l’entrée du système d’exploitation à désigner comme valeur par défaut.|  
|[bootcfg delete](bootcfg-delete.md)|supprime une entrée du système d’exploitation dans la section **[Operating Systems]** du fichier Boot. ini.|  
|[bootcfg ems](bootcfg-ems.md)|Permet à l’utilisateur d’ajouter ou de modifier les paramètres de redirection de la console des services de gestion d’urgence vers un ordinateur distant.|  
|[bootcfg query](bootcfg-query.md)|Interroge et affiche les entrées de la section [Boot Loader] et **[Operating Systems]** à partir de boot. ini.|  
|[bootcfg raw](bootcfg-raw.md)|Ajoute des options de chargement du système d’exploitation spécifiées sous forme de chaîne à une entrée du système d’exploitation dans la section **[Operating Systems]** du fichier Boot. ini.|  
|[bootcfg rmsw](bootcfg-rmsw.md)|supprime les options de chargement du système d’exploitation pour une entrée de système d’exploitation spécifiée.|  
|[bootcfg timeout](bootcfg-timeout.md)|modifie la valeur du délai d’attente du système d’exploitation.|  
