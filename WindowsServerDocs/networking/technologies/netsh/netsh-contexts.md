---
title: Syntaxe des commandes Netsh, contextes et mise en forme
description: Vous pouvez utiliser cette rubrique pour apprendre à entrer des contextes et des sous-contextes netsh, à comprendre la syntaxe et la mise en forme des commandes netsh, et comment exécuter des commandes netsh sur des ordinateurs locaux et distants qui exécutent Windows Server 2016 ou Windows 10.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 8cb9b59f-0255-4261-b49a-562c5ea50ee0
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 815e59ca00d6450b8ef09a034434c4209c928e78
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405579"
---
# <a name="netsh-command-syntax-contexts-and-formatting"></a>Syntaxe des commandes Netsh, contextes et mise en forme

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour apprendre à entrer des contextes et des sous-contextes netsh, à comprendre la syntaxe et la mise en forme des commandes netsh, et comment exécuter des commandes netsh sur des ordinateurs locaux et distants.

Netsh est un utilitaire de script de ligne de commande qui vous permet d’afficher ou de modifier la configuration réseau d’un ordinateur en cours d’exécution. Les commandes netsh peuvent être exécutées en tapant des commandes à l’invite netsh et peuvent être utilisées dans des fichiers de commandes ou des scripts. Les ordinateurs distants et l’ordinateur local peuvent être configurés à l’aide des commandes netsh.

Netsh fournit également une fonctionnalité de script vous permettant d’exécuter un ensemble de commandes en mode batch sur un ordinateur spécifié. Avec netsh, vous pouvez enregistrer un script de configuration dans un fichier texte pour le garder en cas de besoin ou pour vous aider à configurer d’autres ordinateurs.

## <a name="netsh-contexts"></a>Contextes netsh

Netsh interagit avec d’autres composants du système d’exploitation à l’aide de la bibliothèque dynamique @ no__t-0link \(DLL @ no__t-2 fichiers. 

Chaque DLL d’assistance netsh fournit un ensemble complet de fonctionnalités, appelé *contexte*, qui est un groupe de commandes spécifiques à un rôle ou une fonctionnalité de serveur de mise en réseau. Ces contextes étendent les fonctionnalités de netsh en fournissant une prise en charge de la configuration et de la surveillance pour un ou plusieurs services, utilitaires ou protocoles. Par exemple, dhcpmon. dll fournit netsh avec le contexte et l’ensemble de commandes nécessaires à la configuration et à la gestion des serveurs DHCP.

### <a name="obtain-a-list-of-contexts"></a>Obtenir une liste de contextes

Vous pouvez obtenir la liste des contextes netsh en ouvrant l’invite de commandes ou Windows PowerShell sur un ordinateur exécutant Windows Server 2016 ou Windows 10. Tapez la commande **netsh** et appuyez sur entrée. Tapez **/ ?** , puis appuyez sur entrée.

Voici un exemple de sortie pour ces commandes sur un ordinateur exécutant Windows Server 2016 Datacenter.

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

Les contextes netsh peuvent contenir des commandes et des contextes supplémentaires *, appelés sous-contextes*. Par exemple, dans le contexte de routage, vous pouvez passer aux sous-contextes IP et IPv6.

Pour afficher une liste de commandes et de sous-contextes que vous pouvez utiliser dans un contexte, à l’invite netsh, tapez le nom du contexte, puis tapez **/ ?** ou de **l’aide**. Par exemple, pour afficher une liste de sous-contextes et de commandes que vous pouvez utiliser dans le contexte de routage, à l’invite netsh \(that est, **netsh @ no__t-2**\), tapez l’une des valeurs suivantes :

**routage/ ?**

**aide sur le routage**

Pour effectuer des tâches dans un autre contexte sans modifier le contexte actuel, tapez le chemin d’accès du contexte de la commande que vous souhaitez utiliser à l’invite de commandes netsh. Par exemple, pour ajouter une interface nommée « connexion au réseau local » dans le contexte IGMP sans passer d’abord au contexte IGMP, à l’invite netsh, tapez :

**routage IP IGMP Add interface "connexion au réseau local" startupqueryinterval = 21**

## <a name="running-netsh-commands"></a>Exécution de commandes netsh

Pour exécuter une commande netsh, vous devez démarrer netsh à partir de l’invite de commandes en tapant **netsh** , puis en appuyant sur entrée. Ensuite, vous pouvez passer au contexte qui contient la commande que vous souhaitez utiliser. Les contextes disponibles dépendent des composants réseau que vous avez installés. Par exemple, si vous tapez **DHCP** à l’invite netsh et appuyez sur entrée, netsh passe au contexte du serveur DHCP. Toutefois, si le protocole DHCP n’est pas installé, le message suivant s’affiche :

**La commande suivante est introuvable : DHCP.**

## <a name="formatting-legend"></a>Mise en forme de la légende

Vous pouvez utiliser la légende de mise en forme suivante pour interpréter et utiliser la syntaxe de commande netsh correcte lorsque vous exécutez la commande à l’invite netsh ou dans un fichier de commandes ou un script.

- Le texte en *italique* correspond à des informations que vous devez fournir lors de la saisie de la commande. Par exemple, si une commande a un paramètre nommé-*username*, vous devez taper le nom d’utilisateur réel.
- Le texte en **gras** est une information que vous devez taper exactement comme indiqué lors de la saisie de la commande.
- Le texte suivi d’un bouton de sélection \(... \) est un paramètre qui peut être répété plusieurs fois dans une ligne de commande.
- Le texte entre crochets [&nbsp;] est un élément facultatif.
- Le texte entre accolades {&nbsp;} avec des choix séparés par un canal fournit un ensemble de choix à partir duquel vous ne devez en sélectionner qu’un, tel que `{enable|disable}`.
- Le texte mis en forme avec la police courier est du code ou de la sortie du programme.

## <a name="running-netsh-commands-from-the-command-prompt-or-windows-powershell"></a>Exécution de commandes netsh à partir de l’invite de commandes ou de Windows PowerShell

Pour démarrer Network Shell et entrer netsh à l’invite de commandes ou dans Windows PowerShell, vous pouvez utiliser la commande suivante.

### <a name="netsh"></a>netsh

Netsh est un utilitaire de script de ligne de commande qui vous permet, soit localement, soit à distance, d’afficher ou de modifier la configuration réseau d’un ordinateur en cours d’exécution. Utilisé sans paramètres, **netsh** ouvre l’invite de commandes netsh. exe \( que est, **netsh @ no__t-3**\).

#### <a name="syntax"></a>Syntaxe

**netsh**\[ **-a**&nbsp;*AliasFile*\] \[ **-c**&nbsp;*Context* 0 1 **-r**3*ordinateur_distant*5 6 **-u** 8  *Nom_domaine @ no__t-20* 1 *nom_utilisateur* 3 4 **-p**6 8 @ no__t-29 @ no__t-*30 @no__t-* 31 {*NetshCommand*3 **-f**5*scriptfile*} 7

#### <a name="parameters"></a>Paramètres

**`-a`**

Facultatif. Spécifie que vous revenez à l’invite **netsh** après l’exécution de *AliasFile*.

**`AliasFile`**

Facultatif. Spécifie le nom du fichier texte qui contient une ou plusieurs commandes **netsh** .

**`-c`**

Facultatif. Spécifie que netsh entre dans le contexte **netsh** spécifié.

**`Context`**

Facultatif. Spécifie le contexte **netsh** que vous souhaitez entrer. 

**`-r`**

Facultatif. Spécifie que vous souhaitez que la commande s’exécute sur un ordinateur distant.

> [!IMPORTANT]
> Lorsque vous utilisez des commandes netsh à distance sur un autre ordinateur avec le paramètre **netsh – r** , le service d’accès à distance au registre doit être en cours d’exécution sur l’ordinateur distant. S’il n’est pas en cours d’exécution, Windows affiche le message d’erreur « chemin d’accès réseau introuvable ».

***`RemoteComputer`***

Facultatif. Spécifie l’ordinateur distant que vous souhaitez configurer.

**`-u`**

Facultatif. Spécifie que vous souhaitez exécuter la commande netsh sous un compte d’utilisateur.

***`DomainName\\`***

Facultatif. Spécifie le domaine dans lequel se trouve le compte d’utilisateur. La valeur par défaut est le domaine local si *DomainName @ no__t-1* n’est pas spécifié.

***`UserName`***

Facultatif. Spécifie le nom du compte d’utilisateur.

**`-p`**

Facultatif. Spécifie que vous souhaitez fournir un mot de passe pour le compte d’utilisateur.

***`Password`***

Facultatif. Spécifie le mot de passe du compte d’utilisateur que vous avez spécifié avec le *nom d’utilisateur* **-u** .

***`NetshCommand`***

Facultatif. Spécifie la commande **netsh** que vous souhaitez exécuter.

**`-f`**

Facultatif. Quitte **netsh** après l’exécution du script que vous désignez avec *scriptfile*.

***`ScriptFile`***

Facultatif. Spécifie le script que vous souhaitez exécuter.

**`/?`**

Facultatif. Affiche l’aide à l’invite de commandes netsh.

> [!NOTE]
> Si vous spécifiez **`-r`** suivie d’une autre commande, **netsh** exécute la commande sur l’ordinateur distant, puis revient à l’invite de commandes cmd. exe. Si vous spécifiez **`-r`** sans autre commande, **netsh** s’ouvre en mode distant. Le processus est similaire à l’utilisation de l' **option set machine** à l’invite de commandes netsh. Lorsque vous utilisez **`-r`** , vous ne définissez l’ordinateur cible que pour l’instance actuelle de **netsh** . Une fois que vous avez quitté et saisi de nouveau **netsh**, l’ordinateur cible est réinitialisé en tant qu’ordinateur local. Vous pouvez exécuter des commandes **netsh** sur un ordinateur distant en spécifiant un nom d’ordinateur stocké dans WINS, un nom UNC, un nom Internet à résoudre par le serveur DNS ou une adresse IP.

**Saisie de valeurs de chaîne de paramètre pour les commandes netsh**

Tout au long de la référence de commande netsh, il existe des commandes qui contiennent des paramètres pour lesquels une valeur de chaîne est requise.

Dans le cas où une valeur de chaîne contient des espaces entre les caractères, tels que des valeurs de chaîne qui se composent de plusieurs mots, il est nécessaire de placer la valeur de chaîne entre guillemets. Par exemple, pour un paramètre nommé **interface** avec une valeur de chaîne de **connexion réseau sans fil**, utilisez des guillemets autour de la valeur de chaîne :

**`interface="Wireless Network Connection"`**

