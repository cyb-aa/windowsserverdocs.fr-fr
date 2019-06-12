---
title: winrs
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: f9c612a7c6f5d0935223b3c193c52fe970c970d7
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440055"
---
# <a name="winrs"></a>winrs

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Gestion à distance de Windows vous permet de gérer et exécuter des programmes à distance.   
## <a name="syntax"></a>Syntaxe  
```  
winrs [/<parameter>[:<value>]] <command>  
```  
### <a name="parameters"></a>Paramètres  

|           Paramètre            |                                                                                                                                                                                    Description                                                                                                                                                                                     |
|--------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      / distant :\<point de terminaison >       |                                                                                          Spécifie le point de terminaison cible à l’aide d’un nom NetBIOS ou la connexion standard :<br /><br />-   <url>: [\<transport>://]\<target>[:\<port>]<br /><br />Si non spécifié, **/r:localhost** est utilisé.                                                                                          |
|          / non chiffrés          | Spécifie que les messages à l’interpréteur de commandes à distance ne seront pas chiffrées. Cela est utile pour le dépannage ou lorsque le trafic réseau est déjà chiffré à l’aide de **ipsec**, ou lorsque la sécurité physique est appliquée.<br /><br />Par défaut, les messages sont chiffrés à l’aide de clés Kerberos ou NTLM.<br /><br />Cette option de ligne de commande est ignorée lorsque le transport HTTPS est sélectionné. |
|     / username :\<nom d’utilisateur >      |                                                                                Spécifie le nom d’utilisateur sur la ligne de commande.<br /><br />Si non spécifié, l’outil utilisera l’authentification par négociation ou l’invite de commandes pour le nom.<br /><br />Si **/username** est spécifié, **/password** doit également être spécifié.                                                                                 |
|     / Password :\<mot de passe >      |                                                                           Spécifie le mot de passe sur la ligne de commande.<br /><br />Si **/password** n’est pas spécifié, mais **/username** est, l’outil vous demande le mot de passe.<br /><br />Si **/password** est spécifié, **/username** doit également être spécifié.                                                                            |
|      /timeout :\<secondes >       |                                                                                                                                                                             Cette option est déconseillée.                                                                                                                                                                             |
|       /directory:\<path>       |                                                                                            Spécifie le répertoire de démarrage pour l’interpréteur de commandes distant.<br /><br />Si non spécifié, l’interpréteur de commandes à distance démarre dans le répertoire de base défini par la variable d’environnement **% USERPROFILE%** .                                                                                             |
| /environment:\<string>=<value> |                                                                          Spécifie une variable d’environnement unique défini lors de l’interpréteur démarre shell, ce qui permet de changer d’environnement par défaut pour.<br /><br />Plusieurs occurrences de ce commutateur doivent être utilisés pour spécifier plusieurs variables d’environnement.                                                                          |
|            /noecho             |                                                                                                    Spécifie qu’echo doit être désactivée. Cela peut être nécessaire pour garantir que les réponses de l’utilisateur aux invites à distance ne sont pas affichés localement.<br /><br />Par défaut echo est « activé ».                                                                                                    |
|           /noprofile           |                                              Spécifie que le profil utilisateur ne doit pas être chargé.<br /><br />Par défaut, le serveur tente de charger le profil utilisateur.<br /><br />Si l’utilisateur distant n’est pas un administrateur local sur le système cible, cette option est requise (la valeur par défaut entraîne erreur).                                               |
|         /allowdelegate         |                                                                                                                  Spécifie que les informations d’identification de l’utilisateur peuvent être utilisées pour accéder à un partage distant, par exemple, sur un ordinateur différent de celui du point de terminaison cible.                                                                                                                   |
|          /compression          |                                                                           Activer la compression.  Installations plus anciennes sur des ordinateurs distants ne peuvent pas prenant en charge la compression est désactivée par défaut.<br /><br />Paramètre par défaut est désactivée, étant donné que les anciennes installations sur des ordinateurs distants, n’acceptent pas la compression.                                                                           |
|            /usessl             |                                                                                                               Utiliser une connexion SSL lors de l’utilisation d’un point de terminaison distant.  Cette spécification au lieu du transport **https :** utilisera la valeur par défaut **WinRM** port par défaut.                                                                                                                |
|               /?               |                                                                                                                                                                        Affiche l'aide à l'invite de commandes.                                                                                                                                                                        |

## <a name="remarks"></a>Notes  
-   Toutes les options de ligne de commande acceptent soit formulaire court ou long. Par exemple **/r** et **/distant** sont valides.  
-   Pour mettre fin à la **/distant** commande, l’utilisateur peut taper **Ctrl-C** ou **Ctrl-Pause**, qui sera envoyée à l’interpréteur de commandes à distance. La seconde **Ctrl-C** forcer l’arrêt de **winrs.exe**.  
-   Pour gérer des interpréteurs de commandes à distance actives ou winrs configuration, utilisez l’outil de WinRM.  L’URI est l’alias pour gérer des interpréteurs de commandes actives **/cmd shell**.  Est l’URI alias pour la configuration de winrs **winrm/config/winrs**.  

## <a name="BKMK_Examples"></a>Exemples  
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

