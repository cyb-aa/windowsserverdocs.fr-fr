---
title: netcfg
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 8f8368aaff16592a55cc9def84d593cf323f28ee
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373290"
---
# <a name="netcfg"></a>netcfg

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Installe le environnement de préinstallation Windows (WinPE) (WinPE), une version allégée de Windows utilisée pour déployer des stations de travail.   
## <a name="syntax"></a>Syntaxe  
```  
netcfg [/v] [/e] [/winpe] [/l ] /c /i  
```  
### <a name="parameters"></a>Paramètres  
|Paramètre|Description|  
|-------|--------|  
|/v|Exécuter en mode détaillé (détaillé)|  
|/e|Utiliser les variables d’environnement de maintenance lors de l’installation et de la désinstallation|  
|/winpe|Installe TCP/IP, NetBIOS et client Microsoft pour la préinstallation de Windows motivant|  
|/l|Fournit l’emplacement du fichier INF|  
|/c|Fournit la classe du composant à installer ; Protocole, service ou client|  
|/i|Fournit l’ID du composant.|  
|/s|Fournit le type de composants à afficher.<br /><br />\ta = adaptateurs, n = composants réseau|  
|/?|Affiche l'aide à l'invite de commandes.|  
## <a name="BKMK_Examples"></a>Illustre  
Pour installer l' *exemple* de protocole à l’aide de c:\oemdir\example.inf :  
```  
netcfg /l c:\oemdir\example.inf /c p /i example  
```  
Pour installer le service *MS_Server* :  
```  
netcfg /c s /i MS_Server  
```  
Pour installer le protocole TCP/IP, NetBIOS et client Microsoft pour l’environnement de préinstallation Windows  
```  
netcfg /v /winpe  
```  
Pour afficher si le composant *MS_IPX* est installé :  
```  
netcfg /q MS_IPX  
```  
Pour désinstaller le composant *MS_IPX*:  
```  
netcfg /u MS_IPX  
```  
Pour afficher tous les composants réseau installés :  
```  
netcfg /s n  
```  
Pour afficher les chemins de liaison contenant *MS_TCPIP*:  
```  
netcfg /b ms_tcpip  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
