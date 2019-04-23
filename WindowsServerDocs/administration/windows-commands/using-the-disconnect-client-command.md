---
title: À l’aide de la commande Client de se déconnecter
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 876bbe6c-76ab-4de5-879b-d2066e700326
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b89f6c2ff6d41230afd0a2b251ad6982dfa235b1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841750"
---
# <a name="using-the-disconnect-client-command"></a>À l’aide de la commande Client de se déconnecter



Déconnecte un client d’une transmission par multidiffusion ou d’un espace de noms. Sauf si vous spécifiez **/Force**, le client se tourne vers une autre méthode de transfert (si elle est prise en charge par le client).

## <a name="syntax"></a>Syntaxe

```
WDSUTIL /Disconnect-Client /ClientId:<Client ID> [/Server:<Server name>] [/Force]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/ ClientId :\<ID Client >|Spécifie l’ID du client d’être déconnectés. Pour afficher l’ID d’un client, tapez **WDSUTIL /get-multicasttransmission /show:clients**.|
|[/ Server :\<nom du serveur >]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom de domaine complet (FQDN). Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
|[/Force]|Arrête complètement de l’installation et n’utilise pas une méthode de secours. Notez que Wdsmcast.exe ne prend pas en charge n’importe quel mécanisme de secours. Si vous n’utilisez pas cette option, le comportement par défaut est comme suit :</br>-Si vous utilisez le client des Services de déploiement Windows, le client continue l’installation à l’aide de monodiffusion.</br>-Si vous n’utilisez pas le client des Services de déploiement Windows, l’installation échoue.</br>Important : Vous devez utiliser cette option avec précaution, car l’installation échoue et l’ordinateur peut être laissé dans un état inutilisable.|

## <a name="BKMK_examples"></a>Exemples

Pour déconnecter un client, tapez :
```
WDSUTIL /Disconnect-Client /ClientId:1
```
Pour vous déconnecter d’un client et forcer l’échec de l’installation, tapez :
```
WDSUTIL /Disconnect-Client /Server:MyWDSServer /ClientId:1 /Force
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)