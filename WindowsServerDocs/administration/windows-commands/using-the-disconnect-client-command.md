---
title: Disconnect-Client
description: Rubrique relative aux commandes Windows pour Disconnect-Client, qui déconnecte un client d’un espace de noms ou d’une transmission par multidiffusion.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 876bbe6c-76ab-4de5-879b-d2066e700326
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2ba40a7e885cfa3e42065b939d3ddb21ead2f866
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831602"
---
# <a name="disconnect-client"></a>Disconnect-Client

Déconnecte un client d’un espace de noms ou d’une transmission par multidiffusion. À moins que vous ne spécifiiez **/force**, le client revient à une autre méthode de transfert (si elle est prise en charge par le client).

## <a name="syntax"></a>Syntaxe

```
WDSUTIL /Disconnect-Client /ClientId:<Client ID> [/Server:<Server name>] [/Force]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/ClientId : ID client\<>|Spécifie l’ID du client à déconnecter. Pour afficher l’ID d’un client, tapez **WDSUTIL/Get-MulticastTransmission/Show : clients**.|
|[/Server :\<nom du serveur >]|Spécifie le nom du serveur. Il peut s’agir du nom NetBIOS ou du nom de domaine complet (FQDN). Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
|/Force|Arrête complètement l’installation et n’utilise pas de méthode de secours. Notez que Wdsmcast. exe ne prend en charge aucun mécanisme de secours. Si vous n’utilisez pas cette option, le comportement par défaut est le suivant :</br>-Si vous utilisez le client des services de déploiement Windows, le client continue l’installation à l’aide de la monodiffusion.</br>-Si vous n’utilisez pas le client des services de déploiement Windows, l’installation échoue.</br>Important : vous devez utiliser cette option avec précaution, car l’installation échoue et l’ordinateur peut être laissé dans un état inutilisable.|

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

Pour déconnecter un client, tapez :
```
WDSUTIL /Disconnect-Client /ClientId:1
```
Pour déconnecter un client et forcer l’échec de l’installation, tapez :
```
WDSUTIL /Disconnect-Client /Server:MyWDSServer /ClientId:1 /Force
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)