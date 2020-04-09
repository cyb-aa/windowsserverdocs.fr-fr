---
title: bootcfg
description: La rubrique commandes Windows pour bootcfg, qui configure, interroge ou modifie les paramètres du fichier Boot. ini.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3deb354c-5717-4066-bc79-b9323d559e44
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a977b857242c030515a09a67eb0d284ade7a0beb
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848382"
---
# <a name="bootcfg"></a>bootcfg

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Configure, interroge ou modifie les paramètres du fichier Boot.ini.

## <a name="syntax"></a>Syntaxe

```  
bootcfg <parameter> [arguments...]  
```

### <a name="parameters"></a>Paramètres

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
|[bootcfg rmsw](bootcfg-rmsw.md)|Supprime les options de chargement du système d’exploitation pour une entrée de système d’exploitation spécifiée.|  
|[bootcfg timeout](bootcfg-timeout.md)|modifie la valeur du délai d’attente du système d’exploitation.|  
