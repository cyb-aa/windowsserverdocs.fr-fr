---
title: rpcinfo
description: Découvrez comment répertorier les programmes sur un ordinateur distant.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7c342232-a8f0-42ff-8f11-d18c4981f5ca
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 4aba1e57d5a61103310fbe7abcac391e543be5aa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826370"
---
# <a name="rpcinfo"></a>rpcinfo

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Répertorie les programmes sur des ordinateurs distants. Le **rpcinfo** utilitaire de ligne de commande effectue une procédure distante appel (RPC) vers un serveur RPC et signale ce qu’il trouve. 

## <a name="syntax"></a>Syntaxe
```
rpcinfo [/p [<Node>]] [/b <Program version>] [/t <Node Program> [<version>]] [/u <Node Program> [<version>]]
```

### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|/p [\<Node>]|Répertorie tous les programmes sont inscrits auprès du mappeur de port sur l’hôte spécifié. Si vous ne spécifiez pas un nom de nœud (ordinateur), le programme interroge le mappeur de ports sur l’hôte local.|
|/b \<version du programme >|Demande une réponse de tous les nœuds de réseau qui ont le programme spécifié et la version enregistrée avec le mappeur de ports. Vous devez spécifier un nom de programme ou nombre et un numéro de version.|
|/t \<nœud programme > [\<version >]|Utilise le protocole de transport TCP pour appeler le programme spécifié. Vous devez spécifier un nom de nœud (ordinateur) et un nom de programme. Si vous ne spécifiez pas une version, le programme appelle toutes les versions.|
|/u \<nœud programme > [\<version >]|Utilise le protocole de transport UDP pour appeler le programme spécifié. Vous devez spécifier un nom de nœud (ordinateur) et un nom de programme. Si vous ne spécifiez pas une version, le programme appelle toutes les versions.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="BKMK_Examples"></a>Exemples
Pour répertorier tous les programmes sont inscrits auprès du mappeur de port, tapez :
```
rpcinfo /p [<Node>]
```
Pour demander une réponse à partir des nœuds réseau disposant d’un programme spécifié, tapez :
```
rpcinfo /b <Program version>
```
Pour utiliser le protocole TCP (Transmission Control) pour appeler un programme, tapez :
```
rpcinfo /t <Node Program> [<version>]
```
Utilisez le protocole UDP (User Datagram) pour appeler un programme :
```
rpcinfo /u <Node Program> [<version>]
```

## <a name="additional-references"></a>Références supplémentaires
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
