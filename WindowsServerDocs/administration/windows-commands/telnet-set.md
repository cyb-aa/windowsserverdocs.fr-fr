---
title: ensemble de Telnet
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 67316b5f-9c6f-43e3-86d5-dcff9ae2ac3e vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 18a49f2e4629b1410a1cec40fe77077c2988dce2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836120"
---
# <a name="telnet-set"></a>telnet: set

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Définit les options.   
## <a name="syntax"></a>Syntaxe  
```  
set [bsasdel] [crlf] [delasbs] [escape <Char>] [localecho] [logfile <FileName>] [logging] [mode {console | stream}] [ntlm] [term {ansi | vt100 | vt52 | vtnt}] [?]  
```  
### <a name="parameters"></a>Paramètres  
|Paramètre|Description|  
|-------|--------|  
|bsasdel|Envoie **retour arrière** comme un **supprimer**.|  
|crlf|Envoie CR et LF (0x0D, 0 x 0 a) lors de la **entrée** touche est enfoncée. Appelé mode nouvelle ligne.|  
|delasbs|Envoie **supprimer** comme un **retour arrière**.|  
|échappement <Character>|Définit le caractère d’échappement permet d’entrer l’invite du client telnet. Le caractère d’échappement peut être un caractère unique, ou il peut être une combinaison de la **CTRL** clé plus un caractère. Pour définir une combinaison de touches, maintenez la **CTRL** enfoncée pendant que vous tapez le caractère que vous souhaitez affecter.|  
|écho local|Active l’écho local.|  
|fichier journal <FileName>|Enregistre la session telnet active dans le fichier local. L’enregistrement démarre automatiquement lorsque vous paramétrez cette option.|  
|logging|Active la journalisation. Si aucun fichier journal n’est définie, un message d’erreur s’affiche.|  
|mode {console &#124; écran}|Définit le mode d’opération.|  
|ntlm|Active l’authentification NTLM.|  
|term {ansi &#124; vt100 &#124; vt52 &#124; vtnt}|Définit le type de terminal.|  
|?|Affiche l’aide de cette commande.|  
## <a name="remarks"></a>Notes  
1.  Vous pouvez utiliser la **unset** commande pour désactiver une option qui a été définie précédemment.  
2.  Sur les versions non anglaises de telnet, le **codeset** <option> est disponible. **CodeSet** <option> définit le code actuel défini avec une option qui peut être l’une des opérations suivantes : **shift JIS**, **EUC japonais**, **JIS Kanji**, **JIS Kanji (78)**, **DEC Kanji**, **NEC Kanji**. Vous devez définir le même code sur l’ordinateur distant.  
## <a name="BKMK_Examples"></a>Exemples  
Définir le fichier journal et commencer la journalisation pour le fichier local tnlog.txt  
```  
set logfile tnlog.txt  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)  
