---
title: bitsadmin setproxysettings
description: Rubrique de commandes de Windows pour **bitsadmin setproxysettings** -définit les paramètres de proxy pour le travail spécifié.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: be8edb1b-614e-4d0b-a8f8-64b4bde3e64b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a3503aab55f5650cb9283ce8a9f1a17359bfd48b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825590"
---
# <a name="bitsadmin-setproxysettings"></a>bitsadmin setproxysettings



Définit les paramètres de proxy pour le travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /SetProxySettings <Job> <Usage> [List] [Bypass]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom d’affichage ou le GUID du travail|
|Utilisation|Une des valeurs suivantes :</br>-PRECONFIG : utiliser les valeurs par défaut de Internet Explorer du propriétaire.</br>-NO_PROXY : n’utilisez pas un serveur proxy.</br>-REMPLACER, utilisez une liste de proxy explicite et la liste de contournement. Un proxy et la liste de contournement proxy doivent suivre.</br>-Détection automatique, détecter automatiquement les paramètres de proxy.|
|List|Utilisé lorsque le *utilisation* paramètre est défini sur le remplacement, contient une liste délimitée par des virgules des serveurs proxy à utiliser.|
|Contournement|Utilisé lorsque le *utilisation* paramètre est défini sur le remplacement, contient une liste délimitée de noms d’hôtes ou adresses IP ou les deux, pour lequel les transferts ne doivent ne pas être acheminé via un proxy. Il peut s’agir  **\<local >** pour faire référence à tous les serveurs sur le même réseau local. Valeurs NULL ou « » peut être utilisé pour obtenir une liste de contournement proxy vide.|

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant définit les paramètres de proxy pour le travail nommé *myDownloadJob*.

```
C:\>bitsadmin /SetProxySettings myDownloadJob PRECONFIG
```

Voici quelques autres exemples.

```
bitsadmin /setproxysettings myDownloadJob NO_PROXY
bitsadmin /setproxysettings myDownloadJob OVERRIDE proxy1:80 ""
bitsadmin /setproxysettings myDownloadJob OVERRIDE proxy1,proxy2,proxy3 NULL
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)