---
title: Sous-commande Set-Server
description: La rubrique commandes Windows pour la sous-commande Set-Server, qui a configuré les paramètres d’un serveur des services de déploiement Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: da55c29d-a94a-4d73-877b-af480f906ca0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0a2f8cd64030951841751c5d2f5c0bd3fc5ce0e3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80833882"
---
# <a name="subcommand-set-server"></a>Sous-commande : Set-Server

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Configure les paramètres d’un serveur des services de déploiement Windows.

## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /Set-Server [/Server:<Server name>]
    [/Authorize:{Yes | No}]
    [/RogueDetection:{Yes | No}]
    [/AnswerClients:{All | Known | None}]
    [/Responsedelay:<time in seconds>]
    [/AllowN12forNewClients:{Yes | No}]
    [/ArchitectureDiscovery:{Yes | No}]
    [/resetBootProgram:{Yes | No}]
    [/DefaultX86X64Imagetype:{x86 | x64 | Both}]
    [/UseDhcpPorts:{Yes | No}]
    [/DhcpOption60:{Yes | No}]
    [/RpcPort:<Port number>]
    [/PxepromptPolicy 
        [/Known:{OptIn | Noprompt | OptOut}]
        [/New:{OptIn | Noprompt | OptOut}] 
    [/BootProgram:<Relative path>]
         /Architecture:{x86 | ia64 | x64}
    [/N12BootProgram:<Relative path>]
         /Architecture:{x86 | ia64 | x64}
    [/BootImage:<Relative path>]
         /Architecture:{x86 | ia64 | x64}
    [/PreferredDC:<DC Name>]
    [/PreferredGC:<GC Name>]
    [/PrestageUsingMAC:{Yes | No}]
    [/NewMachineNamingPolicy:<Policy>]
    [/NewMachineOU]
         [/type:{Serverdomain | Userdomain | UserOU | Custom}]
         [/OU:<Domain name of OU>]
    [/DomainSearchOrder:{GCOnly | DCFirst}]
    [/NewMachineDomainJoin:{Yes | No}]
    [/OSCMenuName:<Name>]
    [/WdsClientLogging]
         [/Enabled:{Yes | No}]
         [/LoggingLevel:{None | Errors | Warnings | Info}]
    [/WdsUnattend]
         [/Policy:{Enabled | Disabled}]
         [/CommandlinePrecedence:{Yes | No}]
         [/File:<path>]
             /Architecture:{x86 | ia64 | x64}
    [/AutoaddPolicy]
         [/Policy:{AdminApproval | Disabled}]
         [/PollInterval:{time in seconds}]
         [/MaxRetry:{Retries}]
         [/Message:<Message>]
         [/RetentionPeriod]
             [/Approved:<time in days>]
             [/Others:<time in days>]
    [/AutoaddSettings]
         /Architecture:{x86 | ia64 | x64}
         [/BootProgram:<Relative path>]
         [/ReferralServer:<Server name>
         [/WdsClientUnattend:<Relative path>]
         [/BootImage:<Relative path>]
         [/User:<Owner>]
         [/JoinRights:{JoinOnly | Full}]
         [/JoinDomain:{Yes | No}]
    [/BindPolicy]
         [/Policy:{Include | Exclude}]
         [/add]
              /address:<IP or MAC address>
              /addresstype:{IP | MAC}
         [/remove]
              /address:<IP or MAC address>
              /addresstype:{IP | MAC}
    [/RefreshPeriod:<time in seconds>]
    [/BannedGuidPolicy]
         [/add]
              /Guid:<GUID>
         [/remove]
             /Guid:<GUID>
    [/BcdRefreshPolicy]
         [/Enabled:{Yes | No}]
         [/RefreshPeriod:<time in minutes>]
    [/Transport]
         [/ObtainIpv4From:{Dhcp | Range}]
             [/start:<start IP address>]
             [/End:<End IP address>]
         [/ObtainIpv6From:Range]
             [/start:<start IP address>]
             [/End:<End IP address>]
         [/startPort:<start Port>
             [/EndPort:<start Port>
        [/Profile:{10Mbps | 100Mbps | 1Gbps | Custom}]
        [/MulticastSessionPolicy]
             [/Policy:{None | AutoDisconnect | Multistream}]
                 [/Threshold:<Speed in KBps>]
                 [/StreamCount:{2 | 3}]
                 [/Fallback:{Yes | No}]    
        [/forceNative]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
|[/Authorize : {oui &#124; non}]|Spécifie s’il faut autoriser ce serveur dans le protocole DHCP (Dynamic Host Control Protocol).|
|[/RogueDetection : {oui &#124; non}]|Active ou désactive la détection de serveurs DHCP non autorisés.|
|[/AnswerClients : {All &#124; connu &#124; None}]|Spécifie les clients auxquels ce serveur répond. Si vous définissez cette valeur sur **connu**, un ordinateur doit être prédéfini dans les services de domaine Active directory (AD DS) avant qu’il ne soit traité par le serveur des services de déploiement Windows.|
|[/ResponseDelay :<time in seconds>]|Durée d’attente du serveur avant la réponse à un client de démarrage. Ce paramètre ne s’applique pas aux ordinateurs prédéfinis.|
|[/AllowN12forNewClients : {oui &#124; non}]|pour Windows Server 2008, spécifie que les clients inconnus n’auront pas besoin d’appuyer sur la touche F12 pour lancer un démarrage réseau. Les clients connus recevront le programme de démarrage spécifié pour l’ordinateur ou, s’ils ne sont pas spécifiés, le programme de démarrage spécifié pour l’architecture.<p>pour Windows Server 2008 R2, cette option a été remplacée par la commande suivante : WDSUTIL/Set-Server/PxepromptPolicy/New : noprompt|
|[/ArchitectureDiscovery : {oui &#124; non}]|Active ou désactive la découverte d’architecture. Cela facilite la découverte des clients x64 qui ne diffusent pas leur architecture correctement.|
|[/resetBootProgram : {oui &#124; non}]|Détermine si le chemin d’accès de démarrage sera effacé pour un client qui vient de démarrer sans nécessiter une pression sur la touche F12.|
|[/DefaultX86X64Imagetype : {x86 &#124; x64 &#124; }]|Détermine les images de démarrage qui seront affichées aux clients x64.|
|[/UseDhcpPorts : {oui &#124; non}]|Spécifie si le serveur PXE doit tenter d’établir une liaison avec le port DHCP, à savoir le port TCP 67. Si les services de déploiement DHCP et Windows s’exécutent sur le même ordinateur, vous devez définir cette option sur **non** pour permettre au serveur DHCP d’utiliser le port et définir le paramètre **/DhcpOption60** sur **Oui**. Le paramètre par défaut pour cette valeur est **Oui**.|
|[/DhcpOption60 : {oui &#124; non}]|Spécifie si l’option DHCP 60 doit être configurée pour la prise en charge PXE. Si les services de déploiement DHCP et Windows s’exécutent sur le même serveur, attribuez la valeur **Oui** à cette option et attribuez la valeur **non**à l’option **/UseDhcpPorts** Le paramètre par défaut pour cette valeur est **non**.|
|[/RpcPort :<Port number>]|Spécifie le numéro de port TCP à utiliser pour traiter les demandes des clients.|
|[/PxepromptPolicy]|Configure la façon dont les nouveaux clients connus (prédéfinis) et nouveaux lancent un démarrage PXE. Cette option s’applique uniquement à Windows Server 2008 R2. Vous définissez les paramètres à l’aide des options suivantes :<p>-[/Known : {OptIn&#124;désabonnement&#124;noprompt}]-définit la stratégie pour les clients prédéfinis.<br />-[/New : {OptIn&#124;désabonnement&#124;noprompt}]-définit la stratégie pour les nouveaux clients.<p>**Optin** signifie que le client doit appuyer sur une touche pour démarrer PXE. sinon, il revient au périphérique de démarrage suivant.<p>**Noprompt** signifie que le client effectuera toujours un démarrage PXE.<p>**Désabonnement** signifie que le client démarrera PXE à moins que la touche Échap soit enfoncée.|
|[/BootProgram :<Relative path>]/architecture : {x86 &#124; ia64 &#124; x64}|Spécifie le chemin d’accès relatif du programme de démarrage dans le dossier remoteInstall (par exemple, **boot\x86\pxeboot.n12**) et spécifie l’architecture du programme de démarrage.|
|[/N12BootProgram :<Relative path>]/architecture : {x86 &#124; ia64 &#124; x64}|Spécifie le chemin d’accès relatif au programme de démarrage qui ne requiert pas l’appui sur la touche F12 (par exemple, **boot\x86\pxeboot.n12**) et spécifie l’architecture du programme de démarrage.|
|[/BootImage :<Relative path>]/architecture : {x86 &#124; ia64 &#124; x64}|Spécifie le chemin d’accès relatif à l’image de démarrage que les clients qui démarrent doivent recevoir, et spécifie l’architecture de l’image de démarrage. Vous pouvez spécifier cela pour chaque architecture.|
|[/PreferredDC :<DC Name>]|Spécifie le nom du contrôleur de domaine que les services de déploiement Windows doivent utiliser. Il peut s’agir du nom NetBIOS ou du nom de domaine complet (FQDN).|
|[/PreferredGC :<GC Name>]|Spécifie le nom du serveur de catalogue global que les services de déploiement Windows doivent utiliser. Il peut s’agir du nom NetBIOS ou du nom de domaine complet (FQDN).|
|[/PrestageUsingMAC : {oui &#124; non}]|Spécifie si les services de déploiement Windows, lors de la création de comptes d’ordinateur dans AD DS, doivent utiliser l’adresse MAC plutôt que le GUID/UUID pour identifier l’ordinateur.|
|[/NewMachineNamingPolicy :<Policy>]|Spécifie le format à utiliser lors de la génération de noms d’ordinateur pour les clients. Pour plus d’informations sur le format à utiliser pour <policy>, cliquez avec le bouton droit sur le serveur dans le composant logiciel enfichable MMC, cliquez sur **Propriétés**, puis affichez l’onglet **services d’annuaire** . Par exemple, **/NewMachineNamingPolicy :% 61Username% #** .|
|[/NewMachineOU]|Utilisé pour spécifier l’emplacement dans AD DS où les comptes d’ordinateur client seront créés. Vous spécifiez l’emplacement à l’aide des options suivantes.<p>-[/type : ServerDomain &#124; UserDomain &#124; UserOU &#124; personnalisé] spécifie le type d’emplacement. **ServerDomain** crée des comptes dans le même domaine que le serveur des services de déploiement Windows. **UserDomain** crée des comptes dans le même domaine que l’utilisateur qui effectue l’installation. **UserOU** crée des comptes dans l’unité d’organisation de l’utilisateur qui effectue l’installation. **Personnalisé** vous permet de spécifier un emplacement personnalisé (vous devez également spécifier une valeur pour **/ou** avec cette option).<br />-[/OU :<Domain name of OU>]-Si vous spécifiez **Custom** pour l’option **/type** , cette option spécifie l’unité d’organisation dans laquelle les comptes d’ordinateur doivent être créés.|
|[/DomainSearchOrder : {GCOnly &#124; DCFirst}]|Spécifie la stratégie de recherche de comptes d’ordinateur dans AD DS (catalogue global ou contrôleur de domaine).|
|[/NewMachineDomainJoin : {oui &#124; non}]|Spécifie si un ordinateur qui n’est pas déjà préinstallé dans AD DS doit être joint au domaine lors de l’installation. La valeur par défaut est **Oui**.|
|/WdsClientLogging|Spécifie le niveau de journalisation pour le serveur.<p>-[/Enabled : {oui &#124; non}]-active ou désactive la journalisation des actions du client des services de déploiement Windows.<br />-[/LoggingLevel : {None &#124; &#124; Errors &#124; Warnings info}-définit le niveau de journalisation. **None** est équivalent à la désactivation de la journalisation. Le niveau de journalisation des **Erreurs** est le plus bas et indique que seules les erreurs sont consignées. Les **avertissements** incluent les avertissements et les erreurs. **Info** est le plus haut niveau de journalisation et comprend des erreurs, des avertissements et des événements d’information.|
|/WdsUnattend|Ces paramètres contrôlent le comportement d’installation sans assistance du client des services de déploiement Windows. Vous définissez les paramètres à l’aide des options suivantes :<p>-[/Policy : {Enabled &#124; disabled}]-spécifie si l’installation sans assistance est utilisée ou non.<br />-[/CommandlinePrecedence : {oui &#124; non}]-spécifie si un fichier Autounattend. XML (s’il est présent sur le client) ou un fichier d’installation sans assistance transmis directement au client des services de déploiement Windows avec l’option/unattend sera utilisé à la place d’un fichier d’installation sans assistance d’image lors de l’installation d’un client. Le paramètre par défaut est **non**.<br />-[/File :<Relative path>/architecture : {x86 &#124; ia64 &#124; x64}]-spécifie le nom de fichier, le chemin d’accès et l’architecture du fichier d’installation sans assistance.|
|[/AutoaddPolicy]|Ces paramètres contrôlent la stratégie d’ajout automatique. Vous définissez les paramètres à l’aide des options suivantes :<p>-[/Policy : {AdminApproval &#124; disabled}]- **AdminApprove** provoque l’ajout de tous les ordinateurs inconnus à une file d’attente en attente, où l’administrateur peut ensuite passer en revue la liste des ordinateurs et approuver ou refuser chaque demande, le cas échéant. **Désactivé** indique qu’aucune action supplémentaire n’est effectuée lorsqu’un ordinateur inconnu tente de démarrer sur le serveur.<br />-[/PollInterval : {durée en secondes}] : spécifie l’intervalle (en secondes) auquel le programme de démarrage réseau doit interroger le serveur des services de déploiement Windows.<br />-[/MaxRetry : <Number>]-spécifie le nombre de fois que le programme de démarrage réseau doit interroger le serveur des services de déploiement Windows. Cette valeur, ainsi que **/PollInterval**, détermine la durée pendant laquelle le programme de démarrage réseau attend qu’un administrateur approuve ou rejette l’ordinateur avant le dépassement du délai d’attente. Par exemple, une valeur **MaxRetry** de 10 et un vlue **intervalle** de 60 indiquent que le client doit interroger le serveur 10 fois, en attendant 60 secondes entre les tentatives. Par conséquent, le délai d’expiration du client est de 10 minutes (10 x 60 secondes = 10 minutes).<br />-[/Message : <Message>] : spécifie le message qui s’affiche sur le client dans la page de boîte de dialogue programme de démarrage réseau.<br />-[/RetentionPeriod] : spécifie le nombre de jours pendant lesquels un ordinateur peut être dans un état d’attente avant d’être vidé automatiquement.<br />-[/Approved : <time in days>]-spécifie la période de rétention des ordinateurs approuvés. Vous devez utiliser ce paramètre avec l’option **/RetentionPeriod** .<br />-[/Others : <time in days>]-spécifie la période de rétention pour les ordinateurs non approuvés (rejetés ou en attente). Vous devez utiliser ce paramètre avec l’option **/RetentionPeriod** .|
|[/AutoaddSettings]|Spécifie les paramètres par défaut à appliquer à chaque ordinateur. Vous définissez les paramètres à l’aide des options suivantes :<p>-/Architecture : {x86 &#124; ia64 &#124; x64}-spécifie l’architecture.<br />-[/BootProgram : <Relative path>]-spécifie le programme de démarrage envoyé à l’ordinateur approuvé. Si aucun programme de démarrage n’est spécifié, la valeur par défaut de l’architecture de l’ordinateur (telle que spécifiée sur le serveur) sera utilisée.<br />-[/WdsClientUnattend : <Relative path>]-définit le chemin d’accès relatif au fichier d’installation sans assistance que le client approuvé doit recevoir.<br />-[/ReferralServer : <Server name>]-spécifie le serveur des services de déploiement Windows que le client utilisera pour télécharger des images.<br />-[/BootImage : <Relative path>] : spécifie l’image de démarrage que le client approuvé recevra.<br />-[/User : < domaine\utilisateur &#124; User@Domain>]-définit les autorisations sur l’objet compte d’ordinateur pour accorder à l’utilisateur spécifié les droits nécessaires pour joindre l’ordinateur au domaine.<br />-[JoinRights : {JoinOnly &#124; Full}]-spécifie le type de droits à assigner à l’utilisateur. **JoinOnly** requiert que l’administrateur réinitialise le compte d’ordinateur pour que l’utilisateur puisse joindre l’ordinateur au domaine. **Full** donne un accès complet à l’utilisateur, y compris le droit de joindre l’ordinateur au domaine.<br />-[/JoinDomain : {oui &#124; non}]-spécifie si l’ordinateur doit être joint au domaine en tant que ce compte d’ordinateur au cours d’une installation des services de déploiement Windows. La valeur par défaut est **Oui**.|
|/BindPolicy|Configure les interfaces réseau pour que le fournisseur PXE écoute. Vous définissez la stratégie à l’aide des options suivantes :<p>-[/Policy : {include &#124; Exclude}]-définit la stratégie de liaison d’interface pour inclure ou exclure les adresses dans la liste d’interfaces.<br />-[/Add]-ajoute une interface à la liste. Vous devez également spécifier/AddressType et/Address.<br />-[/Remove]-supprime une interface de la liste. Vous devez également spécifier/AddressType et/Address.<br />-/Address :<IP or MAC address> : spécifie l’adresse IP ou MAC de l’interface à ajouter ou supprimer.<br />-/AddressType : {IP &#124; Mac} : indique le type d’adresse spécifié dans l’option **/Address** .|
|[/RefreshPeriod : <seconds>]|Spécifie la fréquence (en secondes) à laquelle le serveur actualise ses paramètres.|
|[/BannedGuidPolicy]|Gère la liste des GUID bannis à l’aide des options suivantes :<p>-[/Add]/GUID :<GUID>-ajoute le GUID spécifié à la liste des GUID interdits. Tout client avec ce GUID est identifié par son adresse MAC à la place.<br />-[/Remove]/GUID :<GUID>-supprime le GUID spécifié de la liste des GUID interdits.|
|[/BcdRefreshPolicy]|Configure les paramètres d’actualisation des fichiers BCD à l’aide des options suivantes :<p>-[/Enabled : {oui &#124; non}]-spécifie la stratégie d’actualisation BCD. Quand **/Enabled** est défini sur **Yes**, les fichiers BCD sont actualisés à l’intervalle de temps spécifié.<br />-[/RefreshPeriod :<time in minutes>] : spécifie l’intervalle de temps auquel les fichiers BCD sont actualisés.|
|/|Configure les options suivantes :<p><ul><li>[/ObtainIpv4From : {DHCP &#124; Range}]-spécifie la source des adresses IPv4.<p><ul><li>[/Start : <starting Ipv4 address>]-spécifie le début de la plage d’adresses IP. Cette option est obligatoire et valide uniquement si **/ObtainIpv4From** est défini sur **Range**</li><li>[/End : <Ending Ipv4 address>]-spécifie la fin de la plage d’adresses IP. Cette option est obligatoire et valide uniquement si **/ObtainIpv4From** est défini sur **Range**.</li></ul></li><li>[/ObtainIpv6From : plage] [/Start :<start IP address>] [/End :<End IP address>]  Spécifie la source des adresses IPv6. Cette option s’applique uniquement à Windows Server 2008 R2 et la seule valeur prise en charge est plage.</li><li>[/startPort : <starting port>]-spécifie le début de la plage de ports.</li><li>[/EndPort : <Ending port>]-spécifie la fin de la plage de ports.</li><li>[/Profile : {10 &#124; Mbits &#124; / &#124; s 100 Mbits/s 1Gbps Custom}]-spécifie le profil réseau à utiliser. Cette option est uniquement prise en charge forservers exécutant Windows Server 2008.</li><li>[/MulticastSessionPolicy]  Configure les paramètres de transfert pour les transmissions par multidiffusion. Cette commande est disponible uniquement pour Windows Server 2008 R2.<p><ul><li>[/Policy : {None &#124; Disconnect &#124; MultiStream}]-détermine comment gérer les clients lents. None signifie que tous les clients sont conservés dans une session à la même vitesse. La déconnexion automatique signifie que tous les clients qui chutent sous le/Threshold spécifié seront déconnectés. La multidiffusion signifie que les clients seront séparés en plusieurs sessions, comme spécifié par/StreamCount.</li><li>[/Threshold :<Speed in KBps>]-pour/Policy : AutoConnect, cette option définit le taux de transfert minimal en Kbits/s. Les clients qui chutent au-dessous de ce taux seront déconnectés des transmissions par multidiffusion.</li><li>[/StreamCount : {2 &#124; 3}] [/Fallback : {oui &#124; non}]-pour/Policy : MultiStream, cette option détermine le nombre de sessions. 2 signifie deux sessions (rapides et lentes) 3 signifie trois sessions (lentes, moyennes, rapides).</li><li>[/Fallback : {oui&#124; non}]-détermine si les clients déconnectés poursuivront le transfert à l’aide d’une autre méthode (si elle est prise en charge par le client). Si vous utilisez le client WDS, l’ordinateur va revenir à la monodiffusion. Wdsmcast. exe ne prend pas en charge un mécanisme de secours. Cette option s’applique également aux clients qui ne prennent pas en charge la multidiffusion. Dans ce cas, l’ordinateur revient à une autre méthode au lieu de passer à une session de transfert plus lente.</li></ul></li></ul>|
## <a name="examples"></a><a name=BKMK_examples></a>Illustre
Pour définir le serveur pour qu’il réponde uniquement aux clients connus, avec un délai de réponse de 4 minutes, tapez :
```
wdsutil /Set-Server /AnswerClients:Known /Responsedelay:4
```
Pour définir le programme de démarrage et l’architecture du serveur, tapez :
```
wdsutil /Set-Server /BootProgram:boot\x86\pxeboot.n12 /Architecture:x86
```
Pour activer la journalisation sur le serveur, tapez :
```
wdsutil /Set-Server /WdsClientLogging /Enabled:Yes /LoggingLevel:Warnings
```
Pour activer l’installation sans assistance sur le serveur, ainsi que sur l’architecture et le fichier d’installation sans assistance client, tapez :
```
wdsutil /Set-Server /WdsUnattend /Policy:Enabled /File:WDSClientUnattend \unattend.xml /Architecture:x86
```
Pour configurer le serveur PXE (Pre-boot Execution Environment) pour qu’il tente d’établir une liaison avec les ports TCP 67 et 60, tapez :
```
wdsutil /Set-server /UseDhcpPorts:No /DhcpOption60:Yes
```
## <a name="additional-references"></a>Références supplémentaires
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande disable-Server](using-the-disable-server-command.md)
[à l’aide de la commande Enable-Server](using-the-enable-server-command.md)
[à l’aide de](using-the-get-server-command.md) la commande de serveur [d’initialisation
à l’aide de la commande Initialize-server](using-the-initialize-server-command.md)
sous-commande [: Start-Server](subcommand-start-server.md)
[subcommand : Stop-Server](subcommand-stop-server.md)
[l’option Uninitialize-Server](the-uninitialize-server-option.md)
