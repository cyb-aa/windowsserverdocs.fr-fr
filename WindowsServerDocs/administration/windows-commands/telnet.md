---
title: telnet
description: La rubrique commandes Windows pour Telnet, qui communique avec un ordinateur exécutant le service du serveur Telnet.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b70a6156-9413-4300-84ce-a34c467e2b4e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b59a3891dd276c6ab0b8e7a8a0a2d11a6b6b55c0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80833132"
---
# <a name="telnet"></a>telnet

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Communique avec un ordinateur exécutant le service du serveur Telnet.
 
## <a name="syntax"></a>Syntaxe
```
telnet [/a] [/e <EscapeChar>] [/f <FileName>] [/l <UserName>] [/t {vt100 | vt52 | ansi | vtnt}] [<Host> [<Port>]] [/?]
```
#### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|/a|tentez une ouverture de session automatique. Identique à l’option/l, sauf que utilise le nom de l’utilisateur actuellement connecté.|
|/e \<EscapeChar >|Caractère d’échappement utilisé pour accéder à l’invite du client Telnet.|
|/f \<nom du fichier >|Nom de fichier utilisé pour la journalisation côté client.|
|/l \<nom d’utilisateur >|Spécifie le nom d’utilisateur à utiliser pour ouvrir une session sur l’ordinateur distant.|
|/t {VT100 &#124; VT52 &#124; ANSI &#124; VTNT}|Spécifie le type de terminal. Les types de terminaux pris en charge sont VT100, VT52, ANSI et VTNT.|
|\<> hôte [\<port >]|Spécifie le nom d’hôte ou l’adresse IP de l’ordinateur distant auquel se connecter, et éventuellement le port TCP à utiliser (le port par défaut est le port TCP 23).|
|/?|Affiche l'aide à l'invite de commandes. Vous pouvez également taper/h.|

## <a name="remarks"></a>Notes
-   Vous devez installer le logiciel client Telnet avant de pouvoir exécuter cette commande. Pour plus d’informations, consultez [installation de Telnet](https://technet.microsoft.com/library/cc754293(v=ws.10).aspx).
-   Vous pouvez exécuter Telnet sans paramètres pour entrer dans le contexte Telnet, indiqué par l’invite de commandes telnet (**Microsoft telnet >** ). À partir de l’invite Telnet, vous pouvez utiliser les commandes telnet pour gérer l’ordinateur exécutant le client Telnet.

## <a name="examples"></a><a name=BKMK_Examples></a>Illustre
Utilisez Telnet pour vous connecter à l’ordinateur exécutant le service de serveur Telnet sur telnet.microsoft.com.
```
telnet telnet.microsoft.com
```
Utiliser Telnet pour se connecter à l’ordinateur exécutant le service de serveur Telnet sur telnet.microsoft.com sur le port TCP 44 et enregistrer l’activité de session dans un fichier local nommé telnetlog. txt
```
telnet /f telnetlog.txt telnet.microsoft.com 44
```

## <a name="additional-references"></a>Références supplémentaires
-   [Installation de Telnet](https://technet.microsoft.com/library/cc754293(v=ws.10).aspx)
-   [informations techniques de référence sur Telnet](https://technet.microsoft.com/library/cc754987(v=ws.10).aspx)
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
