---
title: Syntaxe des commandes Netsh, contextes et mise en forme
description: Vous pouvez utiliser cette rubrique pour savoir comment entrer les sous-contextes et contextes netsh, comprendre la syntaxe de netsh et de la commande mise en forme et comment exécuter les commandes netsh sur des ordinateurs locaux et distants qui exécutent Windows Server 2016 ou Windows 10.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 8cb9b59f-0255-4261-b49a-562c5ea50ee0
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: adb77d841ba4d69b0d36bc7f19d4707638530c97
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823690"
---
# <a name="netsh-command-syntax-contexts-and-formatting"></a>Syntaxe des commandes Netsh, contextes et mise en forme

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour savoir comment entrer les sous-contextes et contextes netsh, comprendre la syntaxe de netsh et de la commande mise en forme et comment exécuter les commandes netsh sur les ordinateurs locaux et distants.

Netsh est un utilitaire de ligne de commande de script qui vous permet d’afficher ou modifier la configuration du réseau d’un ordinateur qui est en cours d’exécution. Commandes Netsh peuvent être exécutées en tapant les commandes à l’invite netsh et ils peuvent être utilisés dans les fichiers de commandes ou des scripts. Ordinateurs distants et l’ordinateur local peuvent être configurés à l’aide des commandes netsh.

Netsh fournit également une fonctionnalité de script vous permettant d’exécuter un ensemble de commandes en mode batch sur un ordinateur spécifié. Avec netsh, vous pouvez enregistrer un script de configuration dans un fichier texte pour le garder en cas de besoin ou pour vous aider à configurer d’autres ordinateurs.

## <a name="netsh-contexts"></a>Contextes Netsh

Netsh interagit avec d’autres composants système d’exploitation à l’aide dynamiques\-bibliothèque de liens \(DLL\) fichiers. 

Chaque DLL d’assistance netsh fournit un ensemble complet de fonctionnalités appelé un *contexte*, qui est un groupe de commandes spécifiques à un rôle de serveur de mise en réseau ou une fonctionnalité. Ces contextes étendent les fonctionnalités de netsh en fournissant la configuration et la surveillance de la prise en charge pour un ou plusieurs services, utilitaires ou protocoles. Par exemple, Dhcpmon.dll fournit netsh avec le contexte et l’ensemble de commandes nécessaires pour configurer et gérer les serveurs DHCP.

### <a name="obtain-a-list-of-contexts"></a>Obtenir la liste des contextes

Vous pouvez obtenir une liste des contextes de netsh en ouvrant une invite de commandes ou Windows PowerShell sur un ordinateur exécutant Windows Server 2016 ou Windows 10. Tapez la commande **netsh** et appuyez sur ENTRÉE. Type **/ ?**, puis appuyez sur ENTRÉE.

Voici un exemple de sortie pour ces commandes sur un ordinateur exécutant Windows Server 2016 Datacenter.

    PS C:\Windows\system32> netsh
    netsh>/?
    
    The following commands are available:
    
    Commands in this context:
    ..            - Goes up one context level.
    ?             - Displays a list of commands.
    abort         - Discards changes made while in offline mode.
    add           - Adds a configuration entry to a list of entries.
    advfirewall   - Changes to the `netsh advfirewall' context.
    alias         - Adds an alias.
    branchcache   - Changes to the `netsh branchcache' context.
    bridge        - Changes to the `netsh bridge' context.
    bye           - Exits the program.
    commit        - Commits changes made while in offline mode.
    delete        - Deletes a configuration entry from a list of entries.
    dhcpclient    - Changes to the `netsh dhcpclient' context.
    dnsclient     - Changes to the `netsh dnsclient' context.
    dump          - Displays a configuration script.
    exec          - Runs a script file.
    exit          - Exits the program.
    firewall      - Changes to the `netsh firewall' context.
    help          - Displays a list of commands.
    http          - Changes to the `netsh http' context.
    interface     - Changes to the `netsh interface' context.
    ipsec         - Changes to the `netsh ipsec' context.
    ipsecdosprotection - Changes to the `netsh ipsecdosprotection' context.
    lan           - Changes to the `netsh lan' context.
    namespace     - Changes to the `netsh namespace' context.
    netio         - Changes to the `netsh netio' context.
    offline       - Sets the current mode to offline.
    online        - Sets the current mode to online.
    popd          - Pops a context from the stack.
    pushd         - Pushes current context on stack.
    quit          - Exits the program.
    ras           - Changes to the `netsh ras' context.
    rpc           - Changes to the `netsh rpc' context.
    set           - Updates configuration settings.
    show          - Displays information.
    trace         - Changes to the `netsh trace' context.
    unalias       - Deletes an alias.
    wfp           - Changes to the `netsh wfp' context.
    winhttp       - Changes to the `netsh winhttp' context.
    winsock       - Changes to the `netsh winsock' context.
    
    The following sub-contexts are available:
     advfirewall branchcache bridge dhcpclient dnsclient firewall http interface ipsec ipsecdosprotection lan namespace netio ras rpc trace wfp winhttp winsock
    
    To view help for a command, type the command, followed by a space, and then
     type ?.


### <a name="subcontexts"></a>Sous-contextes

Contextes Netsh peuvent contenir des commandes et les autres contextes, appelés *sous-contextes*. Par exemple, dans le contexte de routage, vous pouvez modifier le sous-contexte IP et IPv6.

Pour afficher une liste de commandes et de sous-contextes que vous pouvez utiliser dans un contexte, à l’invite netsh, tapez le nom de contexte, puis tapez **/ ?** ou **aide**. Par exemple, pour afficher une liste de sous-contextes et commandes que vous pouvez utiliser dans le contexte de routage, à l’invite netsh \(, autrement dit, **netsh&gt;**\), tapez une des opérations suivantes :

**routage / ?**

**routage d’aide**

Pour effectuer des tâches dans un autre contexte sans modifier votre contexte en cours, tapez le chemin d’accès du contexte de la commande que vous souhaitez utiliser à l’invite netsh. Par exemple, pour ajouter une interface nommée « Connexion au réseau Local » dans le contexte IGMP sans modifier au préalable le contexte IGMP, à l’invite netsh, tapez :

**routage ip igmp ajouter l’interface « Connexion au réseau Local » au startupqueryinterval = 21**

## <a name="running-netsh-commands"></a>Exécution de commandes netsh

Pour exécuter une commande netsh, vous devez démarrer netsh à partir de l’invite de commandes en tapant **netsh** puis appuyez sur ENTRÉE. Ensuite, vous pouvez modifier le contexte qui contient la commande que vous souhaitez utiliser. Les contextes disponibles varient selon les composants réseau que vous avez installée. Par exemple, si vous tapez **dhcp** à l’invite netsh appuyez sur entrée, netsh modifie le contexte de serveur DHCP. Si vous n’avez pas installé de DHCP, toutefois, le message suivant apparaît :

**La commande suivante est introuvable : dhcp.**

## <a name="formatting-legend"></a>Mise en forme de la légende

Vous pouvez utiliser la légende de la mise en forme suivante pour interpréter et d’utiliser la syntaxe de la commande netsh correct lorsque vous exécutez la commande à l’invite netsh ou dans un fichier de commandes ou un script.

- Texte dans *italique* informations que vous devez fournir pendant que vous tapez la commande. Par exemple, si une commande a un paramètre nommé -*nom d’utilisateur*, vous devez taper le nom d’utilisateur réel.
- Texte dans **gras** sont des informations qui vous devez taper exactement comme indiqué pendant que vous tapez la commande.
- Texte suivie de points de suspension \(... \) est un paramètre qui peut être répété plusieurs fois dans une ligne de commande.
- Le texte figurant entre crochets [&nbsp;] est un élément facultatif.
- Le texte figurant entre accolades {&nbsp;} avec choix séparés par un canal fournit un ensemble de choix à partir de laquelle vous devez sélectionner qu’un seul, tel que `{enable|disable}`.
- Texte qui est mis en forme avec la police courrier est sortie de code ou de programme.

## <a name="running-netsh-commands-from-the-command-prompt-or-windows-powershell"></a>Exécution de commandes Netsh à partir de l’invite de commandes ou Windows PowerShell

Pour démarrer l’environnement réseau et Entrez netsh à l’invite de commande ou dans Windows PowerShell, vous pouvez utiliser la commande suivante.

### <a name="netsh"></a>netsh

Netsh est un utilitaire de ligne de commande de script qui vous permet, localement ou à distance, afficher ou modifier la configuration du réseau d’un ordinateur en cours d’exécution. Utilisée sans paramètres, **netsh** ouvre l’invite de commandes Netsh.exe \(, autrement dit, **netsh&gt;**\).

#### <a name="syntax"></a>Syntaxe

**Netsh** \[ **-**&nbsp;*fichier_alias* \] \[ **- c** &nbsp;  *Contexte* \] \[ **- r**&nbsp;*Ordinateur_Distant* \] \[ **- u** \[ *DomainName\\*  \] *nom d’utilisateur* \] \[ **-p** &nbsp; *Mot de passe*  |  \* \] \[{*CommandeNetsh* | **-f** &nbsp; *ScriptFile*}\]

#### <a name="parameters"></a>Paramètres

**`-a`**

Facultatif. Spécifie que vous êtes redirigé vers la **netsh** invite après l’exécution *fichier_alias*.

**`AliasFile`**

Facultatif. Spécifie le nom du fichier texte qui contient un ou plusieurs **netsh** commandes.

**`-c`**

Facultatif. Spécifie que netsh saisit spécifié **netsh** contexte.

**`Context`**

Facultatif. Spécifie le **netsh** contexte que vous souhaitez entrer. 

**`-r`**

Facultatif. Indique que vous voulez que la commande à exécuter sur un ordinateur distant.

>[!IMPORTANT]
>Lorsque vous utilisez certaines commandes netsh à distance sur un autre ordinateur avec le **netsh – r** paramètre, le service Registre distant doit être en cours d’exécution sur l’ordinateur distant. Si elle n’est pas en cours d’exécution, Windows affiche un message d’erreur « Chemin d’accès réseau Not Found ».

***`RemoteComputer`***

Facultatif. Spécifie l’ordinateur distant que vous souhaitez configurer.

**`-u`**

Facultatif. Spécifie que vous souhaitez exécuter la commande netsh sous un compte d’utilisateur.

***`DomainName\\`***

Facultatif. Spécifie le domaine où se trouve le compte d’utilisateur. La valeur par défaut est le domaine local si *DomainName\\*  n’est pas spécifié.

***`UserName`***

Facultatif. Spécifie le nom de compte d’utilisateur.

**`-p`**

Facultatif. Spécifie que vous souhaitez fournir un mot de passe du compte d’utilisateur.

***`Password`***

Facultatif. Spécifie le mot de passe du compte d’utilisateur que vous avez spécifié avec **-u** *nom d’utilisateur*.

***`NetshCommand`***

Facultatif. Spécifie le **netsh** commande que vous souhaitez exécuter.

**`-f`**

Facultatif. Quitte **netsh** après avoir exécuté le script que vous désignez avec *ScriptFile*.

***`ScriptFile`***

Facultatif. Spécifie le script que vous souhaitez exécuter.

**`/?`**

Facultatif. Affiche l’aide à l’invite netsh.

>[!NOTE]
>Si vous spécifiez **`-r`** suivie d’une autre commande, **netsh** exécute la commande sur l’ordinateur distant, puis retourne à l’invite de commandes Cmd.exe. Si vous spécifiez **`-r`** sans une autre commande, **netsh** s’ouvre en mode distant. Le processus est similaire à l’utilisation de **ensemble machine** à l’invite de commande Netsh. Lorsque vous utilisez **`-r`**, vous définissez l’ordinateur cible pour l’instance actuelle de **netsh** uniquement. Une fois que vous quittez et entrez à nouveau **netsh**, l’ordinateur cible est réinitialisé en tant que l’ordinateur local. Vous pouvez exécuter **netsh** nom de commandes sur un ordinateur distant en spécifiant un ordinateur stockée dans WINS, un nom UNC, un nom Internet à résoudre par le serveur DNS ou une adresse IP.

**Taper des valeurs de chaîne de paramètre pour les commandes netsh**

Tout au long de la référence de la commande Netsh, il existe les commandes qui contiennent des paramètres pour lesquels une valeur de chaîne est requise.

Dans le cas où une valeur de chaîne contient des espaces entre les caractères, tels que les valeurs de chaîne qui se composent de plusieurs mots, il est nécessaire que vous placez la valeur de chaîne entre guillemets. Par exemple, pour un paramètre nommé **interface** avec une valeur de chaîne **connexion réseau sans fil**, utilisez des guillemets autour de la valeur de chaîne :

**`interface="Wireless Network Connection"`**

