---
title: Sous-commande set-Server
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: da55c29d-a94a-4d73-877b-af480f906ca0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: abc3fe23558f077e0ba9ac69f2641e3b8c9cde4f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858380"
---
# <a name="subcommand-set-server"></a>Sous-commande : set-Server

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Configure les paramètres pour un serveur de Services de déploiement Windows.
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
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local doit être utilisé.|
|[/ Autoriser : {Oui &#124; No}]|Spécifie s’il faut autoriser ce serveur de contrôle de protocole DHCP (Dynamic Host).|
|[/ RogueDetection : {Oui &#124; No}]|Active ou désactive la détection DHCP non autorisés.|
|[/ AnswerClients : {toutes les &#124; connus &#124; None}]|Spécifie quels clients répondre à ce serveur. Si vous définissez cette valeur sur **connus**, un ordinateur doit être préinstallé dans les Services de domaine active directory (AD DS) avant qu’il y aura de réponse par le serveur Windows Deployment Services.|
|[/ Responsedelay :<time in seconds>]|La durée pendant laquelle le serveur attend avant de répondre à un client en cours de démarrage. Ce paramètre ne s’applique pas aux ordinateurs préinstallés.|
|[/ AllowN12forNewClients : {Oui &#124; No}]|pour Windows Server 2008, spécifie que les clients inconnus n’aura pas d’appuyer sur la touche F12 pour lancer un démarrage réseau. Connu les clients reçoivent le programme de démarrage spécifié pour l’ordinateur ou, le cas contraire, le programme d’amorçage spécifié pour l’architecture.<br /><br />pour Windows Server 2008 R2, cette option a été remplacée par la commande suivante : wdsutil /Set-Server /PxepromptPolicy /New:Noprompt|
|[/ ArchitectureDiscovery : {Oui &#124; No}]|Active ou désactive la détection de l’architecture. Cela facilite la découverte des clients x64 64 qui ne diffusez pas leur architecture correctement.|
|[/ resetBootProgram : {Oui &#124; No}]|Détermine si le chemin d’accès de démarrage est effacée pour un client qui a vient de démarrer sans nécessiter une touche F12.|
|[/DefaultX86X64Imagetype: {x86 &#124; x64 &#124; Both}]|Contrôle les images de démarrage s’affichera pour les clients x64 64.|
|[/UseDhcpPorts:{Yes &#124; No}]|Spécifie si le serveur PXE doit essayer de se lier au port DHCP, TCP port 67. Si DHCP et Services de déploiement Windows s’exécutent sur le même ordinateur, vous devez définir cette option sur **non** pour permettre au serveur DHCP utiliser le port et définir le **/DhcpOption60** paramètre **Oui**. Le paramètre par défaut pour cette valeur est **Oui**.|
|[/DhcpOption60:{Yes &#124; No}]|Spécifie si l’option DHCP 60 doit être configurée pour la prise en charge de PXE. Si DHCP et Services de déploiement Windows s’exécutent sur le même serveur, définissez cette option sur **Oui** et définir le **/UseDhcpPorts** option **non**. Le paramètre par défaut pour cette valeur est **non**.|
|[/RpcPort:<Port number>]|Spécifie le numéro de port TCP à utiliser pour traiter les demandes des clients.|
|[/PxepromptPolicy]|Configure comment connus (prédéfinis) et de nouveaux clients lancer un démarrage PXE. Cette option s’applique uniquement à Windows Server 2008 R2. Vous définissez les paramètres à l’aide des options suivantes :<br /><br />-[/ Connu : {OptIn&#124;désabonnement&#124;Noprompt}]-définit la stratégie pour les clients préconfigurés.<br />-[/ Nouveau : {OptIn&#124;désabonnement&#124;Noprompt}]-définit la stratégie pour les nouveaux clients.<br /><br />**OptIn** signifie que le client a besoin d’appuyer sur une touche dans l’ordre de démarrage PXE, sinon il se tourne vers le périphérique de démarrage suivant.<br /><br />**/Noprompt** signifie que le client sera toujours un démarrage PXE.<br /><br />**Désabonnement** signifie que le client sera un démarrage PXE, sauf si la touche ÉCHAP est enfoncée.|
|[/ BootProgram :<Relative path>] comme : {x86 &#124; ia64 &#124; x64}|Spécifie le chemin d’accès relatif du programme de démarrage dans le dossier remoteInstall (par exemple, **boot\x86\pxeboot.n12**) et spécifie l’architecture du programme de démarrage.|
|[/ N12BootProgram :<Relative path>] comme : {x86 &#124; ia64 &#124; x64}|Spécifie le chemin d’accès relatif du programme de démarrage ne nécessitant pas en appuyant sur la touche F12 (par exemple, **boot\x86\pxeboot.n12**) et spécifie l’architecture du programme de démarrage.|
|[/ Image d’initialisation :<Relative path>] comme : {x86 &#124; ia64 &#124; x64}|Spécifie le chemin d’accès relatif à l’image de démarrage doit recevoir les clients de démarrage et spécifie l’architecture de l’image de démarrage. Vous pouvez le spécifier pour chaque architecture.|
|[/PreferredDC:<DC Name>]|Spécifie le nom du contrôleur de domaine que les Services de déploiement Windows doit utiliser. Cela peut être le nom NetBIOS ou le nom de domaine complet.|
|[/PreferredGC:<GC Name>]|Spécifie le nom du serveur de catalogue global que les Services de déploiement Windows doit utiliser. Cela peut être le nom NetBIOS ou le nom de domaine complet.|
|[/ PrestageUsingMAC : {Oui &#124; No}]|Spécifie si les Services de déploiement Windows, lors de la création de comptes d’ordinateur dans AD DS, doit utiliser l’adresse MAC plutôt que le GUID/UUID pour identifier l’ordinateur.|
|[/NewMachineNamingPolicy:<Policy>]|Spécifie le format à utiliser lors de la génération des noms d’ordinateur pour les clients. Pour plus d’informations sur le format à utiliser pour <policy>, cliquez sur le serveur dans le composant logiciel enfichable mmc, cliquez sur **propriétés**et afficher le **directory Services** onglet. Par exemple, **/NewMachineNamingPolicy : % 61Username % #**.|
|[/NewMachineOU]|Permet de spécifier l’emplacement dans les services AD DS où seront créés les comptes d’ordinateur client. Vous spécifiez l’emplacement à l’aide des options suivantes.<br /><br />-[/ type : Serverdomain &#124; Userdomain &#124; UserOU &#124; personnalisé] Spécifie le type d’emplacement. **Serverdomain** crée des comptes dans le même domaine que le serveur Windows Deployment Services. **UserDomain** crée des comptes dans le même domaine que l’utilisateur qui effectue l’installation. **UserOU** crée des comptes dans l’unité d’organisation de l’utilisateur qui effectue l’installation. **Personnalisé** vous permet de spécifier un emplacement personnalisé (vous devez également spécifier une valeur pour **/ou** avec cette option).<br />-[/ Unité d’organisation :<Domain name of OU>] : Si vous spécifiez **personnalisé** pour le **/type** option, cette option spécifie l’unité d’organisation où les comptes d’ordinateur doivent être créés.|
|[/DomainSearchOrder:{GCOnly &#124; DCFirst}]|Spécifie la stratégie pour la recherche de comptes d’ordinateur dans AD DS (contrôleur de domaine ou de catalogue global).|
|[/ NewMachineDomainJoin : {Oui &#124; No}]|Spécifie si un ordinateur qui n’est pas déjà prédéfini dans AD DS doit être joint au domaine pendant l’installation. Le paramètre par défaut est **Oui**.|
|[/WdsClientLogging]|Spécifie le niveau de journalisation pour le serveur.<br /><br />-[/ Enabled : {Oui &#124; No}] - active ou désactive l’enregistrement des actions du client des Services de déploiement Windows.<br />-[/ LoggingLevel : {None &#124; erreurs &#124; avertissements &#124; Info}-définit le niveau de journalisation. **Aucun** équivaut à désactiver la journalisation. **Erreurs** est le plus bas niveau de journalisation et indique que seules les erreurs doivent être enregistrés. **Avertissements** inclut des avertissements et erreurs. **Informations** est le plus haut niveau de journalisation et inclut des erreurs, avertissements et événements d’information.|
|[/WdsUnattend]|Ces paramètres contrôlent le comportement d’installation sans assistance du client des Services de déploiement Windows. Vous définissez les paramètres à l’aide des options suivantes :<br /><br />-[/ Stratégie : {activé &#124; désactivé}]-Spécifie si l’installation sans assistance est utilisée.<br />-[/ CommandlinePrecedence : {Oui &#124; No}]-Spécifie si un fichier Autounattend.xml (s’il est présent sur le client) ou un fichier d’installation sans assistance qui a été passé directement au client des Services de déploiement Windows avec l’option /Unattend doit être utilisé à la place de fichier image d’installation sans assistance pendant une installation du client. Le paramètre par défaut est **non**.<br />-[/ Fichier :<Relative path> comme : {x86 &#124; ia64 &#124; x64}]-Spécifie le nom de fichier, le chemin d’accès et l’architecture du fichier d’installation sans assistance.|
|[/AutoaddPolicy]|Ces paramètres contrôlent la stratégie d’ajout automatique. Vous définissez les paramètres à l’aide des options suivantes :<br /><br />-[/ Stratégie : {AdminApproval &#124; désactivé}]- **AdminApprove** entraîne tous les ordinateurs inconnus à ajouter à une file d’attente, où l’administrateur peut puis passez en revue la liste des ordinateurs et approuver ou rejeter chaque demande, en tant que approprié. **Désactivé** indique qu’aucune action supplémentaire n’est effectuée lorsqu’un ordinateur inconnu tente de démarrage sur le serveur.<br />-[/ %5%n%n : {time en secondes}]-Spécifie l’intervalle (en secondes) pendant lequel le programme de démarrage réseau doit interroger le serveur Windows Deployment Services.<br />-[/ Valeur MaxRetry : <Number>]-Spécifie le nombre de fois où le programme de démarrage réseau doit interroger le serveur Windows Deployment Services. Cette valeur, ainsi que **/PollInterval**, détermine le délai d’attente pour un administrateur approuver ou rejeter l’ordinateur avant l’expiration du programme de démarrage réseau. Par exemple, un **valeur MaxRetry** valeur 10 et un **%5%n%n** vlue de 60 indiquerait que le client doit interroger le serveur 10 fois, en attente de 60 secondes entre les tentatives. Par conséquent, le client, expireraient après 10 minutes (10 x 60 secondes = 10 minutes).<br />-[/ Message : <Message>]-Spécifie le message qui est affiché pour le client dans la page de boîte de dialogue de programme de démarrage réseau.<br />-[/RetentionPeriod] - Spécifie le nombre de jours d’un ordinateur dans un état d’attente avant la purge automatiquement.<br />-[/ Approuvé : <time in days>]-Spécifie la période de rétention pour les ordinateurs approuvés. Vous devez utiliser ce paramètre avec le **/RetentionPeriod** option.<br />-[/ D’autres : <time in days>]-Spécifie la période de rétention pour les ordinateurs non approuvés (rejetés ou en attente). Vous devez utiliser ce paramètre avec le **/RetentionPeriod** option.|
|[/AutoaddSettings]|Spécifie les paramètres par défaut à appliquer à chaque ordinateur. Vous définissez les paramètres à l’aide des options suivantes :<br /><br />-Comme : {x86 &#124; ia64 &#124; x64}-Spécifie l’architecture.<br />-[/ BootProgram : <Relative path>]-Spécifie le programme d’amorçage envoyé à l’ordinateur approuvé. Si aucun programme de démarrage n’est spécifié, la valeur par défaut pour l’architecture de l’ordinateur (tel que spécifié sur le serveur) sera utilisé.<br />-[/ WdsClientUnattend : <Relative path>]-définit le chemin d’accès relatif au fichier d’installation sans assistance que le client approuvé doit recevoir.<br />-[/ ReferralServer : <Server name>]-Spécifie le serveur de Services de déploiement Windows que le client utilisera pour télécharger des images.<br />-[/ Image d’initialisation : <Relative path>]-Spécifie l’image de démarrage qui reçoit le client approuvé.<br />-[/ Utilisateur : < domaine\utilisateur &#124; User@Domain>]-définit les autorisations sur l’objet de compte d’ordinateur afin de donner des droits nécessaires pour joindre l’ordinateur au domaine de l’utilisateur spécifié.<br />-[JoinRights : {JoinOnly &#124; complète}]-Spécifie le type de droits à assigner à l’utilisateur. **JoinOnly** nécessite que l’administrateur réinitialiser le compte d’ordinateur avant que l’utilisateur puisse joindre l’ordinateur au domaine. **Complète** donne un accès complet à l’utilisateur, y compris le droit de joindre l’ordinateur au domaine.<br />-[/ JoinDomain : {Oui &#124; No}]-Spécifie si l’ordinateur doit être joint au domaine en tant que ce compte d’ordinateur lors d’une installation de Services de déploiement Windows. Le paramètre par défaut est **Oui**.|
|[/BindPolicy]|Configure les interfaces réseau pour le fournisseur PXE écouter. Vous définissez la stratégie à l’aide des options suivantes :<br /><br />-[/ Stratégie : {Include &#124; exclure}]-définit la stratégie de liaison d’interface à inclure ou exclure les adresses sur la liste d’interfaces.<br />-[/ Ajouter] - ajoute une interface à la liste. Vous devez également spécifier/addressType et/Address.<br />-[/remove] - supprime une interface à partir de la liste. Vous devez également spécifier/addressType et/Address.<br />- / d’adresse :<IP or MAC address> -Spécifie l’adresse IP ou MAC de l’interface pour ajouter ou supprimer.<br />-/ addressType : {IP &#124; MAC}-indique le type d’adresse spécifié dans le **/ d’adresse** option.|
|[/ RefreshPeriod : <seconds>]|Spécifie la fréquence (en secondes) le serveur sont actualise ses paramètres.|
|[/BannedGuidPolicy]|Gère la liste de GUID interdit à l’aide des options suivantes :<br /><br />-[/ Ajouter] / GUID :<GUID> -ajoute le GUID spécifié à la liste de GUID interdit. N’importe quel client avec ce GUID est identifié par son adresse MAC à la place.<br />-/ GUID [Remove] :<GUID> -supprime le GUID spécifié dans la liste des GUID bannis.|
|[/BcdRefreshPolicy]|Configure les paramètres de l’actualisation des fichiers de Bcd à l’aide des options suivantes :<br /><br />-[/ Enabled : {Oui &#124; No}]-Spécifie le Bcd l’actualisation de stratégie. Lorsque **/activé** a la valeur **Oui**, fichiers Bcd sont actualisées à l’intervalle de temps spécifié.<br />-[/ RefreshPeriod :<time in minutes>]-Spécifie l’intervalle de temps aux Bcd fichiers sont actualisés.|
|[/Transport]|Configure les options suivantes :<br /><br /><ul><li>[/ ObtainIpv4From : {Dhcp &#124; plage}]-Spécifie la source des adresses IPv4.<br /><br /><ul><li>[/ Démarrer : <starting Ipv4 address>]-Spécifie le début de la plage d’adresses IP. Cette option est obligatoire et valide uniquement si **/ObtainIpv4From** a la valeur **plage**</li><li>[De fin : <Ending Ipv4 address>]-Spécifie la fin de la plage d’adresses IP. Cette option est obligatoire et valide uniquement si **/ObtainIpv4From** a la valeur **plage**.</li></ul></li><li>[/ ObtainIpv6From:Range] [/ Démarrer :<start IP address>] [/ fin :<End IP address>] Spécifie la source des adresses IPv6. Cette option s’applique uniquement à Windows Server 2008 R2 et la seule valeur possible est la plage.</li><li>[/ startPort : <starting port>]-Spécifie le début de la plage de ports.</li><li>[/ L’EndPort : <Ending port>]-Spécifie la fin de la plage de ports.</li><li>[/ Profile : {10 Mbits/s &#124; 100 Mbits/s &#124; 1 Gbit/s &#124; personnalisé}]-Spécifie le profil de réseau à utiliser. Cette option est uniquement pris en charge forservers exécutant Windows Server 2008.</li><li>[/MulticastSessionPolicy]  Configure les paramètres de transfert pour les transmissions par multidiffusion. Cette commande est uniquement disponible pour Windows Server 2008 R2.<br /><br /><ul><li>[/ Stratégie : {None &#124; déconnexion automatique &#124; flux multiples}]-détermine comment gérer les clients lents. Aucun signifie conserver tous les clients dans une session à la même vitesse. Déconnexion automatique signifie que tous les clients qui en-dessous de la /Threshold spécifié seront déconnectés. Flux multiples signifie que les clients sont séparés en plusieurs sessions comme spécifié par /StreamCount.</li><li>[/ Seuil :<Speed in KBps>] : pour /Policy:AutoDisconnect, ce taux de transfert minimale en Kbits/s des groupes d’options. Les clients qui en-dessous de ce taux seront déconnectés de transmissions par multidiffusion.</li><li>[/ StreamCount : {2 &#124; 3}] [/ De secours : {Oui &#124; No}]-pour /Policy:Multistream, cette option détermine le nombre de sessions. 2 signifie que deux sessions (rapides et lents) 3 indique que trois sessions (lente, moyen, fast).</li><li>[/ De secours : {Oui&#124; No}]-détermine si les clients sont déconnectés continuent le transfert à l’aide d’une autre méthode (si pris en charge par le client). Si vous utilisez le client WDS, l’ordinateur utilisera monodiffusion. Wdsmcast.exe ne prend pas en charge un mécanisme de secours. Cette option s’applique également aux clients qui ne prennent pas en charge les flux multiples. Dans ce cas, l’ordinateur revient à une autre méthode au lieu de passer à une session de transfert plus lente.</li></ul></li></ul>|
## <a name="BKMK_examples"></a>Exemples
Pour configurer le serveur pour répondre uniquement aux clients connus, dans un délai de réponse de 4 minutes, tapez :
```
wdsutil /Set-Server /AnswerClients:Known /Responsedelay:4
```
Pour définir l’architecture et le programme de démarrage pour le serveur, tapez :
```
wdsutil /Set-Server /BootProgram:boot\x86\pxeboot.n12 /Architecture:x86
```
Pour activer la journalisation sur le serveur, tapez :
```
wdsutil /Set-Server /WdsClientLogging /Enabled:Yes /LoggingLevel:Warnings
```
Pour activer sur le serveur, ainsi que l’architecture et le fichier d’installation sans assistance de client, type d’installation sans assistance :
```
wdsutil /Set-Server /WdsUnattend /Policy:Enabled /File:WDSClientUnattend \unattend.xml /Architecture:x86
```
Pour définir celui-ci Pre-Boot execution Environment (PXE) pour tenter de lier aux ports TCP 67 et 60, tapez :
```
wdsutil /Set-server /UseDhcpPorts:No /DhcpOption60:Yes
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande disable-Server](using-the-disable-server-command.md)
[à l’aide de la commande enable-Server](using-the-enable-server-command.md)
[à l’aide du Get-Server commande](using-the-get-server-command.md)
[à l’aide de la commande Initialize-Server](using-the-initialize-server-command.md)
[sous-commande : start-Server](subcommand-start-server.md) 
 [ Sous-commande : stop-Server](subcommand-stop-server.md)
[l’Option de serveur uninitialize](the-uninitialize-server-option.md)
