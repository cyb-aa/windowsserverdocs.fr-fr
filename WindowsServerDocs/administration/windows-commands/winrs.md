---
title: winrs
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c370de31-5651-400a-872d-ef229aae2309
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d6ee44cb530614485f0dbd58a9ec13a4788370fe
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361939"
---
# <a name="winrs"></a>winrs

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La gestion à distance de Windows vous permet de gérer et d’exécuter des programmes à distance.   
## <a name="syntax"></a>Syntaxe  
```  
winrs [/<parameter>[:<value>]] <command>  
```  
### <a name="parameters"></a>Paramètres  

|           Paramètre            |                                                                                                                                                                                    Description                                                                                                                                                                                     |
|--------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      /Remote : point de terminaison de\<>       |                                                                                          Spécifie le point de terminaison cible à l’aide d’un nom NetBIOS ou de la connexion standard :<br /><br />-   <url>: [\<transport >://]\<cible > [ :\<port >]<br /><br />s’il n’est pas spécifié, **/r : localhost** est utilisé.                                                                                          |
|          /unencrypted          | Spécifie que les messages de l’interpréteur de commandes distant ne seront pas chiffrés. Cela est utile pour la résolution des problèmes ou lorsque le trafic réseau est déjà chiffré à l’aide d' **IPSec**, ou lorsque la sécurité physique est appliquée.<br /><br />Par défaut, les messages sont chiffrés à l’aide de clés Kerberos ou NTLM.<br /><br />Cette option de ligne de commande est ignorée lorsque le transport HTTPs est sélectionné. |
|     /username :\<le nom d’utilisateur >      |                                                                                Spécifie le nom d’utilisateur sur la ligne de commande.<br /><br />s’il n’est pas spécifié, l’outil utilise l’authentification par négociation ou invite à entrer le nom.<br /><br />Si **/username** est spécifié, **/Password** doit également être spécifié.                                                                                 |
|     /Password :\<mot de passe >      |                                                                           Spécifie le mot de passe sur la ligne de commande.<br /><br />Si **/Password** n’est pas spécifié mais que **/username** est, l’outil vous invite à entrer le mot de passe.<br /><br />Si **/Password** est spécifié, **/username** doit également être spécifié.                                                                            |
|      /Timeout :\<secondes >       |                                                                                                                                                                             Cette option est déconseillée.                                                                                                                                                                             |
|       /Répertoire : chemin d’accès\<>       |                                                                                            Spécifie le répertoire de démarrage de l’interpréteur de commandes distant.<br /><br />s’il n’est pas spécifié, le shell distant démarre dans le répertoire de démarrage de l’utilisateur défini par la variable d’environnement **% UserProfile%** .                                                                                             |
| /Environment :\<> de chaîne =<value> |                                                                          Spécifie une variable d’environnement unique à définir au démarrage de l’interpréteur de commandes, ce qui permet de modifier l’environnement par défaut pour l’interpréteur de commandes.<br /><br />Plusieurs occurrences de ce commutateur doivent être utilisées pour spécifier plusieurs variables d’environnement.                                                                          |
|            /noecho             |                                                                                                    Spécifie que l’écho doit être désactivé. Cela peut être nécessaire pour s’assurer que les réponses de l’utilisateur aux invites distantes ne s’affichent pas localement.<br /><br />Par défaut, l’écho est « on ».                                                                                                    |
|           /noprofile           |                                              Spécifie que le profil de l’utilisateur ne doit pas être chargé.<br /><br />Par défaut, le serveur tente de charger le profil utilisateur.<br /><br />Si l’utilisateur distant n’est pas un administrateur local sur le système cible, cette option est requise (la valeur par défaut génère une erreur).                                               |
|         /allowdelegate         |                                                                                                                  Spécifie que les informations d’identification de l’utilisateur peuvent être utilisées pour accéder à un partage distant, par exemple, sur un ordinateur différent du point de terminaison cible.                                                                                                                   |
|          /compression          |                                                                           Activez la compression.  Les installations plus anciennes sur des ordinateurs distants peuvent ne pas prendre en charge la compression. par défaut, elle est désactivée.<br /><br />La valeur par défaut est OFF, car les installations plus anciennes sur des ordinateurs distants ne prennent peut-être pas en charge la compression.                                                                           |
|            /usessl             |                                                                                                               Utilisez une connexion SSL lors de l’utilisation d’un point de terminaison distant.  Si vous spécifiez cette valeur au lieu du transport **https** , le port par défaut **WinRM** est utilisé.                                                                                                                |
|               /?               |                                                                                                                                                                        Affiche l'aide à l'invite de commandes.                                                                                                                                                                        |

## <a name="remarks"></a>Notes  
-   Toutes les options de ligne de commande acceptent une forme abrégée ou une forme longue. Par exemple, **/r** et **/Remote** sont valides.  
-   Pour terminer la commande **/Remote** , l’utilisateur peut taper **Ctrl-C** ou **CTRL-Break**, qui sera envoyé à l’interpréteur de commandes distant. La deuxième **Ctrl-C** force l’arrêt de **Winrs. exe**.  
-   Pour gérer les interpréteurs de commande à distance ou la configuration Winrs, utilisez l’outil WinRM.  L’alias d’URI pour gérer les interpréteurs de commandes actifs est **Shell/cmd**.  L’alias d’URI pour la configuration de Winrs est **winrm/config/Winrs**.  

## <a name="BKMK_Examples"></a>Illustre  
```  
winrs /r:https://contoso.com command  
```  
```  
winrs /r:contoso.com /usessl command  
```  
```  
winrs /r:myserver command  
```  
```  
winrs /r:http://127.0.0.1 command  
```  
```  
winrs /r:http://169.51.2.101:80 /unencrypted command  
```  
```  
winrs /r:https://[::FFFF:129.144.52.38] command  
```  
```  
winrs /r:http://[1080:0:0:0:8:800:200C:417A]:80 command  
```  
```  
winrs /r:https://contoso.com /t:600 /u:administrator /p:$%fgh7 ipconfig  
```  
```  
winrs /r:myserver /env:path=^%path^%;c:\tools /env:TEMP=d:\temp config.cmd  
```  
```  
winrs /r:myserver netdom join myserver /domain:testdomain /userd:johns /passwordd:$%fgh789  
```  
```  
winrs /r:myserver /ad /u:administrator /p:$%fgh7 dir \\anotherserver\share  
```  

## <a name="additional-references"></a>Références supplémentaires  
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  

