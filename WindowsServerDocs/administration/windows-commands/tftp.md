---
title: tftp
description: Transférer des fichiers vers et depuis un ordinateur distant.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 772f19a8-dafe-45cd-878a-f5691f6568ef vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ad195409076840fda0e8d6bf5cd0c295a62cdede
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845900"
---
# <a name="tftp"></a>tftp

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Transfère les fichiers vers et depuis un ordinateur distant, généralement un ordinateur exécutant UNIX, qui exécute le service de protocole Trivial FTP (tftp) ou démon. TFTP est généralement utilisée par les périphériques intégrés ou des systèmes qui récupèrent des microprogrammes, informations de configuration ou une image du système pendant le processus de démarrage à partir d’un serveur tftp.   

## <a name="syntax"></a>Syntaxe  
```  
tftp [-i] [<Host>] [{get | put}] <Source> [<Destination>]  
```  

### <a name="parameters"></a>Paramètres  
|Paramètre|Description|  
|-------|--------|  
|-i|Spécifie le mode de transfert d’image binaire (également appelé mode octet). En mode image binaire, le fichier est transféré en unités de 1 octet. Utilisez ce mode lors du transfert de fichiers binaires. Si **-i** est omis, le fichier est transféré en mode ASCII. Il s’agit du mode de transfert par défaut. Ce mode convertit les caractères (EOL) de fin de ligne en un format approprié pour l’ordinateur spécifié. Utilisez ce mode lors du transfert de fichiers texte. Si un transfert de fichiers est réussi, le taux de transfert de données s’affiche.|  
|\<Hôte\>|Spécifie l’ordinateur local ou distant.|  
|Put|Transfère le fichier *Source* sur l’ordinateur local au fichier *Destination* sur l’ordinateur distant. Étant donné que le protocole tftp ne prend pas en charge l’authentification utilisateur, l’utilisateur doit être connecté à l’ordinateur distant, et les fichiers doivent être accessibles en écriture sur l’ordinateur distant.|  
|get|Transfère le fichier *Destination* sur l’ordinateur distant dans le fichier *Source* sur l’ordinateur local.|  
|\<Source\>|Spécifie le fichier à transférer.|  
|\<Destination\>|Spécifie l’emplacement de transfert de fichiers.|  

## <a name="remarks"></a>Notes  
-   Vous pouvez installer le client tftp à l’aide de l’Assistant de fonctionnalités ajouter.  
-   Le protocole tftp ne prend pas en charge n’importe quel mécanisme d’authentification ou de chiffrement et par conséquent peut introduire un risque de sécurité lorsqu’il est présent. Installer le client tftp n’est pas recommandée pour les systèmes connectés à Internet.  
-   Le client tftp est un logiciel facultatif et marquée comme étant obsolète dans Windows Vista et versions ultérieures du système d’exploitation Windows. Un service de serveur tftp n’est ne sont plus fourni par Microsoft pour des raisons de sécurité.  

## <a name="BKMK_Examples"></a>Exemples  
Copiez le fichier **boot.img** à partir de l’ordinateur distant **Host1**.  
```  
tftp  -i Host1 get boot.img  
```  

## <a name="additional-references"></a>Références supplémentaires  
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)  
