---
title: Syntaxe de commande Netsh, contextes et mise en forme
description: Vous pouvez utiliser cette rubrique pour savoir comment entrer sous-contextes et contextes netsh, comprendre netsh syntaxe et les commandes de mise en forme et comment exécuter des commandes netsh sur des ordinateurs locaux et distants qui exécutent Windows Server 2016 ou Windows 10.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 8cb9b59f-0255-4261-b49a-562c5ea50ee0
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c7c16674b831431ff5bb1d0172cc2b2a939008f6
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="netsh-command-syntax-contexts-and-formatting"></a>Syntaxe de commande Netsh, contextes et mise en forme

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette rubrique pour savoir comment entrer sous-contextes et contextes netsh, comprendre netsh syntaxe et les commandes de mise en forme et comment exécuter des commandes netsh sur les ordinateurs locaux et distants.

Netsh est un utilitaire de ligne de commande de script qui vous permet d’afficher ou modifier la configuration réseau d’un ordinateur qui est en cours d’exécution. Commandes Netsh peuvent être exécutées en tapant des commandes à l’invite de commandes netsh et ils peuvent être utilisés dans les fichiers de commandes ou des scripts. Ordinateurs distants et l’ordinateur local peuvent être configurés à l’aide des commandes netsh.

Netsh fournit également une fonctionnalité de script qui vous autorise à exécuter un groupe de commandes en mode batch sur un ordinateur spécifié. Avec netsh, vous pouvez enregistrer un script de configuration dans un fichier texte pour l’archivage ou pour vous aider à configurer d’autres ordinateurs.

## <a name="netsh-contexts"></a>Contextes Netsh

Netsh interagit avec d’autres composants du système d’exploitation à l’aide de fichiers \(DLL\) dynamic\-link library. 

Chaque DLL d’assistance netsh fournit un ensemble complet de fonctionnalités appelé un *contexte*, qui est un groupe de commandes spécifiques à un rôle de serveur de mise en réseau ou une fonctionnalité. Les contextes étendent les fonctionnalités de netsh grâce à une configuration et de surveillance de la prise en charge pour un ou plusieurs services, des utilitaires ou protocoles. Par exemple, Dhcpmon.dll fournit netsh avec le contexte et un ensemble de commandes nécessaires pour configurer et gérer les serveurs DHCP.

### <a name="obtain-a-list-of-contexts"></a>Obtenir une liste des contextes

Vous pouvez obtenir une liste des contextes netsh en ouvrant une invite de commandes ou Windows PowerShell sur un ordinateur exécutant Windows Server 2016 ou Windows 10. Tapez la commande **netsh** et appuyez sur ENTRÉE. Type **/? **, puis appuyez sur ENTRÉE.

Voici un exemple de sortie de ces commandes sur un ordinateur exécutant Windows Server 2016 Datacenter.

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

Contextes Netsh peuvent contenir des commandes et autres contextes, appelés *sous-contextes*. Par exemple, dans le contexte de routage, vous pouvez modifier le sous-contexte IP et IPv6.

Pour afficher une liste de commandes et sous-contextes que vous pouvez utiliser dans un contexte, à l’invite de commandes netsh, tapez le nom de contexte et tapez soit **/?** ou **aide**. Par exemple, pour afficher la liste des sous-contextes et commandes que vous pouvez utiliser dans le contexte de routage, à l’invite de commandes netsh \ (autrement dit, **netsh&gt;**\), tapez une des opérations suivantes:

**routage /?**

**aide de routage**

Pour effectuer des tâches dans le contexte d’un autre sans modifier votre contexte actuel, tapez le chemin d’accès du contexte de la commande que vous souhaitez utiliser à l’invite de commandes netsh. Par exemple, pour ajouter une interface nommée «Connexion au réseau Local» dans le contexte IGMP sans changer au préalable le contexte IGMP, à l’invite de commandes netsh, tapez:

**routage ip igmp ajouter interface «Connexion au réseau Local» au startupqueryinterval = 21**

## <a name="running-netsh-commands"></a>Exécution de commandes netsh

Pour exécuter une commande netsh, vous devez démarrer netsh à partir de l’invite de commandes en tapant **netsh** puis en appuyant sur ENTRÉE. Ensuite, vous pouvez modifier pour le contexte qui contient la commande que vous souhaitez utiliser. Les contextes sont disponibles varient selon les composants réseau que vous avez installés. Par exemple, si vous tapez **dhcp** l’invite de commandes netsh et appuyez sur entrée, netsh des modifications apportées au contexte de serveur DHCP. Si vous n’avez pas installé de DHCP, toutefois, le message suivant apparaît:

**La commande suivante n’a pas été trouvée: dhcp.**

## <a name="formatting-legend"></a>Légende de mise en forme

Vous pouvez utiliser la légende de mise en forme suivante pour interpréter et utilisez la syntaxe de commande netsh correct lorsque vous exécutez la commande à l’invite de commandes netsh ou dans un fichier de commandes ou un script.

- Texte de *italique* les informations que vous devez fournir pendant que vous tapez la commande. Par exemple, si une commande a un paramètre nommé -*nom d’utilisateur*, vous devez taper le nom d’utilisateur.
- Texte de **gras** les informations que vous devez taper exactement comme affiché pendant que vous tapez la commande.
- Suivi par une \(...\) sélection de texte est un paramètre qui peut être répété plusieurs fois dans une ligne de commande.
- Texte entre crochets [&nbsp;] est un élément facultatif.
- Texte entre accolades {&nbsp;} avec choix séparés par un canal de communication fournit un ensemble de choix à partir de laquelle vous devez sélectionner qu’un seul, tel que `{enable|disable}`.
- Texte mis en forme avec la police Courier est code ou données de programme.

## <a name="running-netsh-commands-from-the-command-prompt-or-windows-powershell"></a>Exécution de commandes Netsh à partir de l’invite de commandes ou Windows PowerShell

Pour démarrer l’environnement réseau et Entrez netsh à l’invite de commandes ou dans Windows PowerShell, vous pouvez utiliser la commande suivante.

### <a name="netsh"></a>netsh

Netsh est un utilitaire de ligne de commande de script qui vous permet, localement ou à distance, afficher ou modifier la configuration réseau d’un ordinateur en cours d’exécution. Sans paramètres, **netsh** ouvre l’invite de commandes Netsh.exe \ (autrement dit, **netsh&gt;**\).

#### <a name="syntax"></a>Syntaxe

**netsh**\[ **-a**&nbsp;*AliasFile*\] \[ **-c**&nbsp;*Context* \] \[**-r**&nbsp;*RemoteComputer*\] \[ **-u** \[ *DomainName\\* \] *UserName* \] \[ **-p**&nbsp;*Password* | \*\] \[{*NetshCommand* | **-f**&nbsp;*ScriptFile*}\]

#### <a name="parameters"></a>Paramètres

**`-a`**

Facultatif. Spécifie que vous revenez à la **netsh** invite après l’exécution *fichier_alias*.

**`AliasFile`**

Facultatif. Spécifie le nom du fichier texte qui contient un ou plusieurs **netsh** commandes.

**`-c`**

Facultatif. Spécifie que netsh passe spécifié **netsh** contexte.

**`Context`**

Facultatif. Spécifie le **netsh** contexte que vous souhaitez entrer. 

**`-r`**

Facultatif. Indique que vous souhaitez que la commande à exécuter sur un ordinateur distant.

>[!IMPORTANT]
>Lorsque vous utilisez certaines commandes netsh à distance sur un autre ordinateur avec le **netsh – r** paramètre, le service de Registre distant doit être en cours d’exécution sur l’ordinateur distant. Si elle n’est pas en cours d’exécution, Windows affiche un message d’erreur «Réseau chemin introuvable».

***`RemoteComputer`***

Facultatif. Spécifie l’ordinateur distant que vous souhaitez configurer.

**`-u`**

Facultatif. Spécifie que vous souhaitez exécuter la commande netsh sous un compte d’utilisateur.

***`DomainName\\`***

Facultatif. Spécifie le domaine où se trouve le compte d’utilisateur. La valeur par défaut est le domaine local si *DomainName\\* n’est pas spécifié.

***`UserName`***

Facultatif. Spécifie le nom de compte d’utilisateur.

**`-p`**

Facultatif. Spécifie que vous souhaitez fournir un mot de passe pour le compte d’utilisateur.

***`Password`***

Facultatif. Spécifie le mot de passe du compte d’utilisateur que vous avez spécifié avec **-u** *nom d’utilisateur*.

***`NetshCommand`***

Facultatif. Spécifie le **netsh** commande que vous souhaitez exécuter.

**`-f`**

Facultatif. Quitte **netsh** après l’exécution du script que vous désignez avec *fichier script*.

***`ScriptFile`***

Facultatif. Spécifie le script que vous souhaitez exécuter.

**`/?`**

Facultatif. Affiche l’aide à l’invite de commandes netsh.

>[!NOTE]
>Si vous spécifiez ** `-r` ** suivie d’une autre commande, **netsh** exécute la commande sur l’ordinateur distant, puis retourne à l’invite de commandes Cmd.exe. Si vous spécifiez ** `-r` ** sans une autre commande, **netsh** s’ouvre dans un mode à distance. Le processus est similaire à l’utilisation de **set machine** à l’invite de commandes Netsh. Lorsque vous utilisez ** `-r` **, vous configurez l’ordinateur cible pour l’instance actuelle de **netsh** uniquement. Une fois que vous quittez et entrer de nouveau **netsh**, l’ordinateur cible est réinitialisé en tant que l’ordinateur local. Vous pouvez exécuter **netsh** nom de commandes sur un ordinateur distant en spécifiant un ordinateur stockée dans WINS, un nom UNC, un nom Internet à résoudre par le serveur DNS, ou une adresse IP.

**Tapez les valeurs de chaîne de paramètres pour les commandes netsh**

Les commandes qui contiennent des paramètres pour lesquels une valeur de chaîne est requise sont tout au long de la référence des commandes Netsh.

Dans le cas où une valeur de chaîne contient des espaces entre les caractères, tels que les valeurs de chaîne qui sont constituées de plusieurs mots, il est nécessaire que vous placez la valeur de chaîne entre guillemets. Par exemple, pour un paramètre nommé **interface** avec une valeur de chaîne **connexion réseau sans fil**, utilisez des guillemets autour de la valeur de chaîne:

**`interface="Wireless Network Connection"`**

