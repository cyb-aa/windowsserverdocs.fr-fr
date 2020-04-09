---
title: bitsadmin setproxysettings
description: La rubrique commandes Windows pour Bitsadmin setproxysettings n', qui définit les paramètres de proxy pour le travail spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: be8edb1b-614e-4d0b-a8f8-64b4bde3e64b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4dea72d956d12070b2638f953a7a00dcb1ed7a9c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849202"
---
# <a name="bitsadmin-setproxysettings"></a>bitsadmin setproxysettings

Définit les paramètres de proxy pour le travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /SetProxySettings <Job> <Usage> [List] [Bypass]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|
|Utilisation|Une des valeurs suivantes :</br>-Preconfig : utilisez les paramètres par défaut d’Internet Explorer du propriétaire.</br>-NO_PROXY : n’utilisez pas de serveur proxy.</br>-OVERRIDE : utilisez une liste de proxy explicite et une liste de contournement. Une liste de contournement proxy et proxy doit suivre.</br>-DÉTECTION automatique : détecte automatiquement les paramètres du proxy.|
|Liste|Utilisé lorsque le paramètre d' *utilisation* est défini sur override : contient une liste délimitée par des virgules de serveurs proxy à utiliser.|
|Omettre|Utilisé lorsque le paramètre d' *utilisation* est défini sur override : contient une liste délimitée par des espaces de noms d’hôtes ou d’adresses IP, ou les deux, pour lesquels les transferts ne doivent pas être routés via un proxy. Cela peut être **\<> local** pour faire référence à tous les serveurs sur le même réseau local. Les valeurs NULL ou peuvent être utilisées pour une liste de contournement vide du proxy.|

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant définit les paramètres de proxy pour la tâche nommée *myDownloadJob*.

```
C:\>bitsadmin /SetProxySettings myDownloadJob PRECONFIG
```

Voici d’autres exemples.

```
bitsadmin /setproxysettings myDownloadJob NO_PROXY
bitsadmin /setproxysettings myDownloadJob OVERRIDE proxy1:80 
bitsadmin /setproxysettings myDownloadJob OVERRIDE proxy1,proxy2,proxy3 NULL
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)