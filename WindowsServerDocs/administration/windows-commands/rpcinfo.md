---
title: rpcinfo
description: Découvrez comment répertorier les programmes sur un ordinateur distant.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7c342232-a8f0-42ff-8f11-d18c4981f5ca
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 03450a370c84eb4659b9ebfde0729fee52e6c1f6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80835552"
---
# <a name="rpcinfo"></a>rpcinfo

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Répertorie les programmes installés sur des ordinateurs distants. L’utilitaire de ligne de commande **rpcinfo** effectue un appel de procédure distante (RPC) sur un serveur RPC et signale ce qu’il trouve. 

## <a name="syntax"></a>Syntaxe
```
rpcinfo [/p [<Node>]] [/b <Program version>] [/t <Node Program> [<version>]] [/u <Node Program> [<version>]]
```

#### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|/p [\<> de nœud]|répertorie tous les programmes inscrits avec le mappeur de port sur l’ordinateur hôte spécifié. Si vous ne spécifiez pas de nom de nœud (ordinateur), le programme interroge le mappeur de port sur l’hôte local.|
|/b \<version du programme >|Demande une réponse de tous les nœuds réseau qui ont le programme et la version spécifiés inscrits auprès du mappeur de port. Vous devez spécifier un nom de programme ou un numéro de version, ainsi qu’un numéro de version.|
|/t \<du nœud programme > [\<version >]|Utilise le protocole de transport TCP pour appeler le programme spécifié. Vous devez spécifier à la fois un nom de nœud (ordinateur) et un nom de programme. Si vous ne spécifiez pas de version, le programme appelle toutes les versions.|
|/u \<nœud programme > [\<version >]|Utilise le protocole de transport UDP pour appeler le programme spécifié. Vous devez spécifier à la fois un nom de nœud (ordinateur) et un nom de programme. Si vous ne spécifiez pas de version, le programme appelle toutes les versions.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="examples"></a><a name="BKMK_Examples"></a>Illustre
Pour répertorier tous les programmes inscrits avec le mappeur de port, tapez :
```
rpcinfo /p [<Node>]
```
Pour demander une réponse des nœuds réseau qui ont un programme spécifié, tapez :
```
rpcinfo /b <Program version>
```
Pour utiliser le protocole TCP (Transmission Control Protocol) pour appeler un programme, tapez :
```
rpcinfo /t <Node Program> [<version>]
```
Utilisez le protocole UDP (User Datagram Protocol) pour appeler un programme :
```
rpcinfo /u <Node Program> [<version>]
```

## <a name="additional-references"></a>Références supplémentaires
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
