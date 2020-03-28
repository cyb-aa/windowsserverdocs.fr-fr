---
title: Syntaxe des commandes Netsh, contextes et mise en forme
description: Vous pouvez utiliser cette rubrique pour apprendre à entrer des contextes et des sous-contextes netsh, comprendre la syntaxe et la mise en forme des commandes netsh et apprendre à exécuter des commandes netsh sur des ordinateurs locaux et distants qui exécutent Windows Server 2016 ou Windows 10.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 8cb9b59f-0255-4261-b49a-562c5ea50ee0
manager: brianlic
ms.author: lizross
author: eross-msft
ms.openlocfilehash: fe2318ad7feb732b86bcd69317bff20146923b64
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80312243"
---
# <a name="netsh-command-syntax-contexts-and-formatting"></a>Syntaxe des commandes Netsh, contextes et mise en forme

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour apprendre à entrer des contextes et des sous-contextes netsh, comprendre la syntaxe et la mise en forme des commandes netsh et apprendre à exécuter des commandes netsh sur des ordinateurs locaux et distants.

Netsh est un utilitaire de script en ligne de commande qui vous permet d’afficher ou de modifier la configuration réseau d’un ordinateur en cours d’exécution. Vous pouvez exécuter des commandes netsh en les tapant depuis l’invite netsh et en les intégrant à des fichiers de commandes ou des scripts. Les ordinateurs distants et l’ordinateur local peuvent être configurés à l’aide de commandes netsh.

Netsh fournit également une fonctionnalité de script vous permettant d’exécuter un ensemble de commandes en mode batch sur un ordinateur spécifié. Avec netsh, vous pouvez enregistrer un script de configuration dans un fichier texte pour le garder en cas de besoin ou pour vous aider à configurer d’autres ordinateurs.

## <a name="netsh-contexts"></a>Contextes netsh

Netsh interagit avec d’autres composants du système d’exploitation à l’aide de fichiers DLL \(bibliothèque de liens dynamiques\). 

Chaque DLL d’assistance netsh fournit un ensemble complet de fonctionnalités, appelé *contexte*, qui est un groupe de commandes propres à une fonctionnalité ou à un rôle serveur réseau. Ces contextes étendent les fonctionnalités de netsh en fournissant une prise en charge de la configuration et de la supervision pour un ou plusieurs services, utilitaires ou protocoles. Par exemple, Dhcpmon.dll fournit à netsh le contexte et l’ensemble de commandes nécessaires à la configuration et à la gestion des serveurs DHCP.

### <a name="obtain-a-list-of-contexts"></a>Obtenir la liste de contextes

Vous pouvez obtenir la liste des contextes netsh en ouvrant l’invite de commandes ou Windows PowerShell sur un ordinateur exécutant Windows Server 2016 ou Windows 10. Tapez la commande **netsh** et appuyez sur Entrée. Tapez **/?** , puis appuyez sur Entrée.

Voici un exemple de sortie de ces commandes sur un ordinateur exécutant Windows Server 2016 Datacenter.

>    ```
>   PS C:\Windows\system32> netsh
>   netsh>/?
>    
>    The following commands are available:
>    
>    Commands in this context:
>    ..            - Goes up one context level.
>    ?             - Displays a list of commands.
>    abort         - Discards changes made while in offline mode.
>    add           - Adds a configuration entry to a list of entries.
>    advfirewall   - Changes to the `netsh advfirewall' context.
>    alias         - Adds an alias.
>    branchcache   - Changes to the `netsh branchcache' context.
>    bridge        - Changes to the `netsh bridge' context.
>    bye           - Exits the program.
>    commit        - Commits changes made while in offline mode.
>    delete        - Deletes a configuration entry from a list of entries.
>    dhcpclient    - Changes to the `netsh dhcpclient' context.
>    dnsclient     - Changes to the `netsh dnsclient' context.
>    dump          - Displays a configuration script.
>    exec          - Runs a script file.
>    exit          - Exits the program.
>    firewall      - Changes to the `netsh firewall' context.
>    help          - Displays a list of commands.
>    http          - Changes to the `netsh http' context.
>    interface     - Changes to the `netsh interface' context.
>    ipsec         - Changes to the `netsh ipsec' context.
>    ipsecdosprotection - Changes to the `netsh ipsecdosprotection' context.
>    lan           - Changes to the `netsh lan' context.
>    namespace     - Changes to the `netsh namespace' context.
>    netio         - Changes to the `netsh netio' context.
>    offline       - Sets the current mode to offline.
>    online        - Sets the current mode to online.
>    popd          - Pops a context from the stack.
>    pushd         - Pushes current context on stack.
>    quit          - Exits the program.
>    ras           - Changes to the `netsh ras' context.
>    rpc           - Changes to the `netsh rpc' context.
>    set           - Updates configuration settings.
>    show          - Displays information.
>    trace         - Changes to the `netsh trace' context.
>    unalias       - Deletes an alias.
>    wfp           - Changes to the `netsh wfp' context.
>    winhttp       - Changes to the `netsh winhttp' context.
>    winsock       - Changes to the `netsh winsock' context.
>    
>    The following sub-contexts are available:
>     advfirewall branchcache bridge dhcpclient dnsclient firewall http interface ipsec ipsecdosprotection lan namespace netio ras rpc trace wfp winhttp winsock
>    
>    To view help for a command, type the command, followed by a space, and then type ?.
>    ```

### <a name="subcontexts"></a>Sous-contextes

Les contextes netsh peuvent contenir des commandes et des contextes supplémentaires, appelés *sous-contextes*. Par exemple, dans le contexte Routing, vous pouvez passer aux sous-contextes IP et IPv6.

Pour afficher la liste des commandes et sous-contextes que vous pouvez utiliser dans un contexte, depuis l’invite netsh, tapez le nom du contexte, puis tapez **/?** ou **help**. Par exemple, pour afficher la liste des sous-contextes et commandes que vous pouvez utiliser dans le contexte Routing, depuis l’invite netsh \(c’est-à-dire, **netsh&gt;** \), tapez l’une des commandes suivantes :

**routing /?**

**routing help**

Pour effectuer des tâches dans un autre contexte sans changer de contexte actuel, tapez le chemin du contexte de la commande que vous souhaitez utiliser depuis l’invite de commandes netsh. Par exemple, pour ajouter une interface nommée « Connexion au réseau local » dans le contexte IGMP sans passer d’abord au contexte IGMP, depuis l’invite netsh, tapez :

**routing ip igmp add interface "Connexion au réseau local" startupqueryinterval=21**

## <a name="running-netsh-commands"></a>Exécution de commandes netsh

Pour exécuter une commande netsh, vous devez démarrer netsh depuis l’invite de commandes en tapant **netsh** puis en appuyant sur Entrée. Ensuite, vous pouvez passer au contexte qui contient la commande que vous souhaitez utiliser. Les contextes disponibles dépendent des composants réseau que vous avez installés. Par exemple, si vous tapez **dhcp** depuis l’invite netsh et que vous appuyez sur Entrée, netsh passe au contexte du serveur DHCP. Toutefois, si le protocole DHCP n’est pas installé, le message suivant s’affiche :

**La commande suivante n’a pas été trouvée : dhcp.**

## <a name="formatting-legend"></a>Légende de mise en forme

Vous pouvez utiliser la légende de mise en forme suivante pour interpréter la syntaxe de commande netsh et l’utiliser correctement quand vous exécutez une commande depuis l’invite netsh ou dans un fichier de commandes ou un script.

- Le texte en *italique* représente des informations que vous devez fournir quand vous entrez une commande. Par exemple, si une commande a un paramètre nommé -*UserName*, vous devez taper le nom d’utilisateur réel.
- Le texte en **gras** représente des informations que vous devez taper telles qu’elles sont indiquées quand vous entrez une commande.
- Le texte suivi d’un bouton de sélection \(...\) est un paramètre pouvant être répété plusieurs fois dans une ligne de commande.
- Le texte entre crochets [&nbsp;] est un élément facultatif.
- Le texte entre accolades {&nbsp;} avec des choix séparés par une barre verticale fournit un ensemble de choix dans lequel vous ne devez sélectionner qu’un seul élément, par exemple `{enable|disable}`.
- Le texte mis en forme avec la police Courier est du code ou une sortie du programme.

## <a name="running-netsh-commands-from-the-command-prompt-or-windows-powershell"></a>Exécution de commandes Netsh depuis l’invite de commandes ou Windows PowerShell

Pour démarrer Network Shell et entrer netsh depuis l’invite de commandes ou dans Windows PowerShell, vous pouvez utiliser la commande suivante.

### <a name="netsh"></a>netsh

Netsh est un utilitaire de script en ligne de commande qui vous permet, localement ou à distance, d’afficher ou de modifier la configuration réseau d’un ordinateur en cours d’exécution. Utilisé sans paramètres, **netsh** ouvre l’invite de commandes Netsh.exe \(c’est-à-dire, **netsh&gt;** \).

#### <a name="syntax"></a>Syntaxe

**netsh**\[ **-a**&nbsp;*AliasFile*\] \[ **-c**&nbsp;*Context* \] \[ **-r**&nbsp;*RemoteComputer*\] \[ **-u** \[ *DomainName\\* \] *UserName* \] \[ **-p**&nbsp;*Password* | \*\] \[{*NetshCommand* |  **-f**&nbsp;*ScriptFile*}\]

#### <a name="parameters"></a>Paramètres

**`-a`**

Facultatif. Spécifie que vous êtes redirigé vers l’invite **netsh** après l’exécution d’*AliasFile*.

**`AliasFile`**

Facultatif. Spécifie le nom du fichier texte qui contient une ou plusieurs commandes **netsh**.

**`-c`**

Facultatif. Spécifie que netsh entre dans le contexte **netsh** spécifié.

**`Context`**

Facultatif. Spécifie le contexte **netsh** dans lequel vous souhaitez entrer. 

**`-r`**

Facultatif. Spécifie que vous souhaitez que la commande s’exécute sur un ordinateur distant.

> [!IMPORTANT]
> Quand vous utilisez des commandes netsh à distance sur un autre ordinateur avec le paramètre **netsh –r**, le service Registre à distance doit être en cours d’exécution sur l’ordinateur distant. S’il n’est pas en cours d’exécution, Windows affiche le message d’erreur « Chemin réseau introuvable ».

***`RemoteComputer`***

Facultatif. Spécifie l’ordinateur distant que vous souhaitez configurer.

**`-u`**

Facultatif. Spécifie que vous souhaitez exécuter la commande netsh sous un compte d’utilisateur.

***`DomainName\\`***

Facultatif. Spécifie le domaine dans lequel se trouve le compte d’utilisateur. La valeur par défaut est le domaine local si *DomainName\\* n’est pas spécifié.

***`UserName`***

Facultatif. Spécifie le nom du compte d’utilisateur.

**`-p`**

Facultatif. Spécifie que vous souhaitez fournir un mot de passe pour le compte d’utilisateur.

***`Password`***

Facultatif. Spécifie le mot de passe du compte d’utilisateur que vous avez spécifié avec **-u** *UserName*.

***`NetshCommand`***

Facultatif. Spécifie la commande **netsh** que vous souhaitez exécuter.

**`-f`**

Facultatif. Quitte **netsh** après l’exécution du script que vous désignez avec *ScriptFile*.

***`ScriptFile`***

Facultatif. Spécifie le script que vous souhaitez exécuter.

**`/?`**

Facultatif. Affiche l’aide depuis l’invite de commandes netsh.

> [!NOTE]
> Si vous spécifiez **`-r`** suivi d’une autre commande, **netsh** exécute la commande sur l’ordinateur distant, puis revient à l’invite de commandes Cmd.exe. Si vous spécifiez **`-r`** sans aucune autre commande, **netsh** s’ouvre en mode distant. Le processus est similaire à l’utilisation de **set machine** depuis l’invite de commandes Netsh. Quand vous utilisez **`-r`** , vous définissez l’ordinateur cible pour l’instance actuelle de **netsh** uniquement. Une fois que vous avez quitté et regagné **netsh**, l’ordinateur cible est réinitialisé en tant qu’ordinateur local. Vous pouvez exécuter des commandes **netsh** sur un ordinateur distant en spécifiant un nom d’ordinateur stocké dans WINS, un nom UNC, un nom Internet résolu par le serveur DNS ou une adresse IP.

**Entrée de valeurs de chaîne de paramètre pour les commandes netsh**

Les informations de référence sur les commandes netsh sont jalonnées de commandes qui contiennent des paramètres pour lesquels une valeur de chaîne est requise.

Si une valeur de chaîne contient des espaces entre les caractères, à l’image des valeurs de chaîne qui se composent de plusieurs mots, il est nécessaire de la placer entre guillemets. Par exemple, dans le cas d’un paramètre nommé **interface** ayant la valeur de chaîne **Connexion réseau sans fil**, placez la valeur de chaîne entre guillemets :

**`interface="Wireless Network Connection"`**

