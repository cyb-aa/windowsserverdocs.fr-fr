---
title: ensemble Telnet
description: La rubrique commandes Windows pour le jeu Telnet, qui définit des options.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 67316b5f-9c6f-43e3-86d5-dcff9ae2ac3e vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f8fefc18bde39713c3864361118ff4440ca12b0d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80833162"
---
# <a name="telnet-set"></a>Telnet : définir

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Définit des options.   

## <a name="syntax"></a>Syntaxe  
```  
set [bsasdel] [crlf] [delasbs] [escape <Char>] [localecho] [logfile <FileName>] [logging] [mode {console | stream}] [ntlm] [term {ansi | vt100 | vt52 | vtnt}] [?]  
```  
#### <a name="parameters"></a>Paramètres  

|                    Paramètre                     |                                                                                                                                              Description                                                                                                                                              |
|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                     bsasdel                      |                                                                                                                                 Envoie le **retour arrière** en tant que **suppression**.                                                                                                                                  |
|                       CRLF                       |                                                                                                        Envoie CR & LF (0x0D, 0x 0A) quand la touche **entrée** est enfoncée. Appelé mode nouvelle ligne.                                                                                                        |
|                     delasbs                      |                                                                                                                                 Envoie la **suppression** comme un **retour arrière**.                                                                                                                                  |
|                <Character> d’échappement                | Définit le caractère d’échappement utilisé pour accéder à l’invite du client Telnet. Le caractère d’échappement peut être un caractère unique, ou il peut s’agir d’une combinaison de la touche **CTRL** et d’un caractère. Pour définir une combinaison de touches de contrôle, maintenez la touche **CTRL** enfoncée pendant que vous tapez le caractère que vous souhaitez affecter. |
|                    localecho                     |                                                                                                                                         Active l’écho local.                                                                                                                                          |
|                fichier journal <FileName>                |                                                                                               Journalise la session Telnet en cours dans le fichier local. La journalisation démarre automatiquement lorsque vous définissez cette option.                                                                                               |
|                     enregistrer                      |                                                                                                                  Active la journalisation. Si aucun fichier journal n’est défini, un message d’erreur s’affiche.                                                                                                                   |
|           mode {écran &#124; de console}           |                                                                                                                                       Définit le mode d’opération.                                                                                                                                        |
|                       ntlm                       |                                                                                                                                     Active l’authentification NTLM.                                                                                                                                     |
| terme {ANSI &#124; VT100 &#124; VT52 &#124; VTNT} |                                                                                                                                        Définit le type de terminal.                                                                                                                                        |
|                        ?                         |                                                                                                                                    Affiche l’aide de cette commande.                                                                                                                                    |

## <a name="remarks"></a>Notes  
1. Vous pouvez utiliser la commande **unset** pour désactiver une option précédemment définie.  
2. Dans les versions non anglaises de Telnet, le **<option> de** codeset est disponible. Le jeu de **codes <option> définit** l’ensemble du code actuel sur une option, qui peut être l’une des suivantes : **Shift JIS**, **japonais EUC**, **JIS kanji**, **JIS kanji (78)** , **Dec**kanji, **NEC kanji**. Vous devez définir le même jeu de code sur l’ordinateur distant.  
   ## <a name="examples"></a><a name=BKMK_Examples></a>Illustre  
   Définir le fichier journal et commencer la journalisation dans le fichier local tnlog. txt  
   ```  
   set logfile tnlog.txt  
   ```  
   ## <a name="additional-references"></a>Références supplémentaires  
3. - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
