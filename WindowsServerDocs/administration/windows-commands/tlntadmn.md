---
title: tlntadmn
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 78b61e8d-b953-44bb-8d57-f3b42da9e7a8 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c321c4ebcaf5fbbae30f6825fef18b478554f665
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884190"
---
# <a name="tlntadmn"></a>tlntadmn

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Administre un ordinateur local ou distant qui exécute le Service du serveur telnet.   
## <a name="syntax"></a>Syntaxe  
```  
tlntadmn [<computerName>] [-u <UserName>] [-p <Password>] [{start | stop | pause | continue}] [-s {<SessionID> | all}] [-k {<SessionID> | all}] [-m {<SessionID> | all}  <Message>] [config [dom = <Domain>] [ctrlakeymap = {yes | no}] [timeout = <hh>:<mm>:<ss>] [timeoutactive = {yes | no}] [maxfail = <attempts>] [maxconn = <Connections>] [port = <Number>] [sec {+ | -}NTLM {+ | -}passwd] [mode = {console | stream}]] [-?]  
```  
### <a name="parameters"></a>Paramètres  
|Paramètre|Description|  
|-------|--------|  
|\<computerName>|Spécifie le nom du serveur pour vous connecter à. La valeur par défaut est l'ordinateur local.|  
|-u \<nom d’utilisateur > -p \<mot de passe >|Spécifie les informations d’identification administratives pour un serveur distant que vous souhaitez administrer. Ce paramètre est obligatoire si vous souhaitez administrer un serveur distant auquel vous n'êtes pas connecté avec les informations d’identification d’administration.|  
|start|démarre le Service du serveur telnet.|  
|stop|Arrête le Service du serveur telnet|  
|pause|Suspend le Service du serveur telnet. Aucune nouvelle connexion sera être acceptée.|  
|Continuer|Reprend le Service du serveur telnet.|  
|-s {\<SessionID> &#124; all}|Affiche les sessions telnet active.|  
|-k {\<SessionID> &#124; all}|Termine la session telnet. Tapez l’ID de Session pour mettre fin à une session spécifique, ou tous pour mettre fin à toutes les sessions.|  
|-m {\<SessionID> &#124; all}  <Message>|Envoie un message à une ou plusieurs sessions. Tapez l’ID de session pour envoyer un message à une session spécifique, ou tous pour envoyer un message à toutes les sessions. Tapez le message que vous souhaitez envoyer entre guillemets.|  
|config dom = \<domaine >|Configure le domaine par défaut pour le serveur.|  
|config ctrlakeymap = {yes &#124; no}|Spécifie si vous souhaitez que le serveur telnet interpréter CTRL + A comme ALT. type **Oui** au mappage de la touche de raccourci ou type **aucun** pour empêcher le mappage.|  
|config timeout = \<hh>:\<mm>:\<ss>|Définit le délai d’attente en heures, minutes et secondes.|  
|config timeoutactive = {yes &#124; no|Permet le délai d’expiration de session inactive.|  
|config maxfail = \<attempts>|Définit le nombre maximal de tentatives de connexion avant la déconnexion.|  
|config maxconn = \<connexions >|Définit le nombre maximal de connexions.|  
|config port = < \Number >|Définit le port telnet. Vous devez spécifier le port avec un nombre entier inférieur à 1024.|  
|configuration s {+ &#124; -} NTLM {+ &#124; -} passwd|Spécifie si vous souhaitez utiliser NTLM, un mot de passe ou les deux pour authentifier les tentatives d’ouverture de session. Pour utiliser un type particulier d’authentification, tapez un signe plus (**+**) avant le type d’authentification. Pour éviter à l’aide d’un type particulier d’authentification, tapez un signe moins (**-**) avant le type d’authentification.|  
|config mode = {console &#124; stream}|Spécifie le mode de fonctionnement.|  
|-?|Affiche l'aide à l'invite de commandes.|  

## <a name="remarks"></a>Notes  
-   Pour afficher les paramètres du serveur, tapez **tlntadmn** sans aucun paramètre.  
-   Pour utiliser le **tlntadmn** commande, vous devez vous connecter à l’ordinateur local en tant qu’administrateur. Pour administrer un ordinateur distant, vous devez également fournir des informations d’identification administratives pour l’ordinateur distant. Vous pouvez le faire en vous connectant à l’ordinateur local avec un compte disposant des informations d’identification administratives pour l’ordinateur local et l’ordinateur distant. Si vous ne pouvez pas utiliser cette méthode, vous pouvez utiliser la **-u** et **-p** paramètres pour fournir des informations d’identification administratives pour l’ordinateur distant.  

## <a name="BKMK_Examples"></a>Exemples  
Configurer le délai d’expiration de session inactive à 30 minutes.  
```  
tlntadmn config timeout=0:30:0  
```  
Afficher les sessions telnet active.  
```  
tlntadmn -s  
```  

## <a name="additional-references"></a>Références supplémentaires  
-   [Guide des opérations de Telnet](https://technet.microsoft.com/library/cc753164(v=ws.10).aspx)  
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)  
