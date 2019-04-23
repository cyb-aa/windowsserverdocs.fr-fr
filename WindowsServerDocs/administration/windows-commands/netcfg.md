---
title: netcfg
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e2daaab7-12db-4e36-b70c-db8906d084f7 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: aed535f843da6d735526ea97c07f94564dc00dc6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871320"
---
# <a name="netcfg"></a>netcfg

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Installe l’environnement de préinstallation Windows (WinPE), une version allégée de Windows permet de déployer des stations de travail.   
## <a name="syntax"></a>Syntaxe  
```  
netcfg [/v] [/e] [/winpe] [/l ] /c /i  
```  
### <a name="parameters"></a>Paramètres  
|Paramètre|Description|  
|-------|--------|  
|/v|Exécuter dans le mode détaillé (détaillé)|  
|/e|Utiliser des variables d’environnement maintenance lors de l’installation et désinstallation|  
|/winpe|Installe TCP/IP, NetBIOS et le Client Microsoft pour l’environnement de préinstallation Windows|  
|/l|Fournit l’emplacement de INF|  
|/c|Fournit la classe du composant à installer ; protocole, Service ou client|  
|/i|Fournit l’ID de composant|  
|/s|Fournit le type de composants à afficher<br /><br />\ta = adaptateurs, n = composants nets|  
|/?|Affiche l'aide à l'invite de commandes.|  
## <a name="BKMK_Examples"></a>Exemples  
Pour installer le protocole *exemple* à l’aide de c:\oemdir\example.inf :  
```  
netcfg /l c:\oemdir\example.inf /c p /i example  
```  
Pour installer le *MS_Server* service :  
```  
netcfg /c s /i MS_Server  
```  
Pour installer TCP/IP, NetBIOS et le Client Microsoft pour l’environnement de préinstallation Windows  
```  
netcfg /v /winpe  
```  
À afficher si le composant *MS_IPX* est installé :  
```  
netcfg /q MS_IPX  
```  
Pour désinstaller le composant *MS_IPX*:  
```  
netcfg /u MS_IPX  
```  
Pour afficher tous les installés des composants nets :  
```  
netcfg /s n  
```  
Afin d’afficher les chemins d’accès qui contient la liaison *MS_TCPIP*:  
```  
netcfg /b ms_tcpip  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)  
