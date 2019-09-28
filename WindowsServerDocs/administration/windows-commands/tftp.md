---
title: tftp
description: Transférer des fichiers vers et à partir d’un ordinateur distant.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 66f729d090a78b74bc0334cd9b7276219a980e8c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370210"
---
# <a name="tftp"></a>tftp

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Transfère les fichiers vers et à partir d’un ordinateur distant, généralement un ordinateur exécutant UNIX, qui exécute le service ou démon trivial protocole FTP (TFTP). le protocole TFTP est généralement utilisé par les appareils ou systèmes intégrés qui récupèrent le microprogramme, les informations de configuration ou une image système pendant le processus de démarrage à partir d’un serveur TFTP.   

## <a name="syntax"></a>Syntaxe  
```  
tftp [-i] [<Host>] [{get | put}] <Source> [<Destination>]  
```  

### <a name="parameters"></a>Paramètres  
|Paramètre|Description|  
|-------|--------|  
|-i|Spécifie le mode de transfert d’images binaires (également appelé mode octet). En mode image binaire, le fichier est transféré en unités d’un octet. Utilisez ce mode lors du transfert de fichiers binaires. Si **-i** est omis, le fichier est transféré en mode ASCII. Il s’agit du mode de transfert par défaut. Ce mode convertit les caractères de fin de ligne (EOL) en un format approprié pour l’ordinateur spécifié. Utilisez ce mode pour transférer des fichiers texte. Si un transfert de fichier réussit, le taux de transfert de données est affiché.|  
|\<Host @ no__t-1|Spécifie l’ordinateur local ou distant.|  
|posé|Transfère la *source* du fichier sur l’ordinateur local vers la *destination* de fichier sur l’ordinateur distant. Étant donné que le protocole TFTP ne prend pas en charge l’authentification utilisateur, l’utilisateur doit être connecté à l’ordinateur distant et les fichiers doivent être accessibles en écriture sur l’ordinateur distant.|  
|get|Transfère la *destination* du fichier sur l’ordinateur distant vers la *source* du fichier sur l’ordinateur local.|  
|\<Source\>|Spécifie le fichier à transférer.|  
|\<Destination\>|Spécifie l’emplacement du transfert du fichier.|  

## <a name="remarks"></a>Notes  
-   Vous pouvez installer le client TFTP à l’aide de l’Assistant Ajout de fonctionnalités.  
-   Le protocole TFTP ne prend en charge aucun mécanisme d’authentification ou de chiffrement, et en tant que tel, il peut présenter un risque de sécurité lorsqu’il est présent. L’installation du client TFTP n’est pas recommandée pour les systèmes connectés à Internet.  
-   Le client TFTP est un logiciel facultatif qui est marqué comme étant déconseillé sur Windows Vista et les versions ultérieures du système d’exploitation Windows. Un service de serveur TFTP n’est plus fourni par Microsoft pour des raisons de sécurité.  

## <a name="BKMK_Examples"></a>Illustre  
Copiez le fichier **Boot. img** à partir de l’ordinateur distant **host1**.  
```  
tftp  -i Host1 get boot.img  
```  

## <a name="additional-references"></a>Références supplémentaires  
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
