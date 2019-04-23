---
title: telnet
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b70a6156-9413-4300-84ce-a34c467e2b4e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6439a91d82d6d199666629e333d8130bf65a384b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858560"
---
# <a name="telnet"></a>telnet

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Communique avec un ordinateur exécutant le service serveur telnet. 
## <a name="syntax"></a>Syntaxe
```
telnet [/a] [/e <EscapeChar>] [/f <FileName>] [/l <UserName>] [/t {vt100 | vt52 | ansi | vtnt}] [<Host> [<Port>]] [/?]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|/a|tentative d’ouverture de session automatique. Identique à/l option sauf utilise actuellement connecté sur le nom d’utilisateur s.|
|/e \<EscapeChar >|Permet d’entrer l’invite du client telnet de caractère d’échappement.|
|/f \<FileName>|Nom de fichier utilisé pour la journalisation côté client.|
|/ l \<nom d’utilisateur >|Spécifie le nom d’utilisateur pour vous connecter avec sur l’ordinateur distant.|
|/t {vt100 &#124; vt52 &#124; ansi &#124; vtnt}|Spécifie le type de terminal. Les types de terminal pris en charge sont vt100, vt52, ansi et vtnt.|
|\<Hôte > [\<Port >]|Spécifie le nom d’hôte ou l’adresse IP de l’ordinateur distant pour se connecter à et, éventuellement, le port TCP à utiliser (valeur par défaut est le port TCP 23).|
|/?|Affiche l'aide à l'invite de commandes. Vous pouvez également taper /h.|

## <a name="remarks"></a>Notes
-   Vous devez installer le logiciel client telnet avant de pouvoir exécuter cette commande. Pour plus d’informations, consultez [installation telnet](https://technet.microsoft.com/library/cc754293(v=ws.10).aspx).
-   Vous pouvez exécuter telnet sans paramètres à entrer dans le contexte telnet, indiqué par l’invite telnet (**Microsoft telnet >**). À partir de l’invite de telnet, vous pouvez utiliser des commandes telnet pour gérer l’ordinateur qui exécute le client telnet.

## <a name="BKMK_Examples"></a>Exemples
Utilisez telnet pour vous connecter à l’ordinateur qui exécute le Service serveur telnet à telnet.microsoft.com.
```
telnet telnet.microsoft.com
```
Utilisez telnet pour vous connecter à l’ordinateur exécutant le Service serveur telnet à telnet.microsoft.com sur le port TCP 44 et consigne l’activité de la session dans un fichier local appelé telnetlog.txt
```
telnet /f telnetlog.txt telnet.microsoft.com 44
```

## <a name="additional-references"></a>Références supplémentaires
-   [L’installation de telnet](https://technet.microsoft.com/library/cc754293(v=ws.10).aspx)
-   [informations techniques de référence de Telnet](https://technet.microsoft.com/library/cc754987(v=ws.10).aspx)
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
