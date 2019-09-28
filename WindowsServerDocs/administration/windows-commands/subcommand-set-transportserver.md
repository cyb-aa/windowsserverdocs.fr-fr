---
title: Sous-commande Set-TransportServer
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7863225c-f4b2-4cd0-b929-78a454bef249
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f91cca044d4c2922791ccb63b0e3cec4af685f17
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370745"
---
# <a name="subcommand-set-transportserver"></a>Sous-commande : Set-TransportServer

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Définit les paramètres de configuration d’un serveur de transport.
## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /Set-TransportServer [/Server:<Server name>]
     [/ObtainIpv4From:{Dhcp | Range}]
        [/start:<starting IP address>]
        [/End:<Ending IP address>]
     [/ObtainIpv6From:Range]\n\
        [/start:<start IP address>]\n\
        [/End:<End IP address>]      
     [/startPort:<starting port>
        [/EndPort:<starting port>
     [/Profile:{10Mbps | 100Mbps | 1Gbps | Custom}]    
     [/MulticastSessionPolicy]
             [/Policy:{None | AutoDisconnect | Multistream}]
                 [/Threshold:<Speed in KBps>]
                 [/StreamCount:{2 | 3}]
                 [/Fallback:{Yes | No}]
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Server:<Server name>]|Spécifie le nom du serveur de transport. Il peut s’agir du nom NetBIOS ou du nom de domaine complet (FQDN). Si aucun nom de serveur de transport n’est spécifié, le serveur local est utilisé.|
|[/ObtainIpv4From : {DHCP &#124; Range}]|Définit la source des adresses IPv4 comme suit :<br /><br />-[/Start : <IP address>] définit le début de la plage d’adresses IP. Cela est obligatoire et valide uniquement si cette option est définie sur **Range**.<br />-[/End : <IP address>] définit la fin de la plage d’adresses IP. Cela est obligatoire et valide uniquement si cette option est définie sur **Range**.<br />-[/startPort : <port>] définit le début de la plage de ports.<br />-[/EndPort : <port>] définit la fin de la plage de ports.|
|[/ObtainIpv6From : plage]|Spécifie la source des adresses IPv6. Cette option s’applique uniquement à Windows Server 2008 R2 et la seule valeur prise en charge est **plage**.<br /><br />-[/Start : <IP address>] définit le début de la plage d’adresses IP. Cela est obligatoire et valide uniquement si cette option est définie sur **Range**.<br />-[/End : <IP address>] définit la fin de la plage d’adresses IP. Cela est obligatoire et valide uniquement si cette option est définie sur **Range**.<br />-[/startPort : <port>] définit le début de la plage de ports.<br />-[/EndPort : <port>] définit la fin de la plage de ports.|
|[/Profile : {10 &#124; Mbits &#124; / &#124; s 100 Mbits/s 1Gbps personnalisé}]|Spécifie le profil réseau à utiliser. Cette option est disponible uniquement pour les serveurs exécutant Windows Server 2008 ou Windows Server 2003.|
|[/MulticastSessionPolicy]|Configure les paramètres de transfert pour les transmissions par multidiffusion. Cette commande est disponible uniquement pour Windows Server 2008 R2.<br /><br />-[/Policy : {None &#124; Disconnect &#124; MultiStream}] détermine comment gérer les clients lents. **None** signifie que tous les clients sont conservés dans une session à la même vitesse. La **déconnexion** automatique signifie que tous les clients qui chutent sous le **/Threshold** spécifié sont déconnectés. La **multidiffusion** signifie que les clients sont séparés en plusieurs sessions, comme spécifié par **/StreamCount**.<br />-[/Threshold : <Speed in KBps>] définit le taux de transfert minimal en Kbits/s pour **/Policy : Disconnect**. Les clients qui chutent au-dessous de ce taux sont déconnectés des transmissions par multidiffusion.<br />-[/StreamCount : {2 &#124; 3}] [/Fallback : {Yes &#124; no}] détermine le nombre de sessions pour **/Policy : MultiStream**. **2** signifie deux sessions (rapides et lentes) et **3** les trois sessions (lentes, moyennes, rapides).<br />-[/Fallback : {oui &#124; non}] détermine si les clients qui sont déconnectés poursuivront le transfert à l’aide d’une autre méthode (si elle est prise en charge par le client). Si vous utilisez le client WDS, l’ordinateur revient à la monodiffusion. Wdsmcast. exe ne prend pas en charge un mécanisme de secours. Cette option s’applique également aux clients qui ne prennent pas en charge la **multidiffusion**. Dans ce cas, l’ordinateur revient à une autre méthode au lieu de passer à une session de transfert plus lente.|
## <a name="BKMK_examples"></a>Illustre
Pour définir la plage d’adresses IPv4 pour le serveur, tapez :
```
wdsutil /Set-TransportServer /ObtainIpv4From:Range /start:239.0.0.1 /End:239.0.0.100
```
Pour définir la plage d’adresses IPv4, la plage de ports et le profil du serveur, tapez :
```
wdsutil /Set-TransportServer /Server:MyWDSServer /ObtainIpv4From:Range /start:239.0.0.1 /End:239.0.0.100 /startPort:12000 /EndPort:50000 /Profile:10mbps
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande disable-TransportServer](using-the-disable-transportserver-command.md)
[à l’aide de la commande Enable-TransportServer](using-the-enable-transportserver-command.md)
[à l’aide de la commande TransportServer](using-the-get-transportserver-command.md)
 sous-[commande : sous-commande Start-TransportServer](subcommand-start-transportserver.md)
[: Stop-TransportServer](subcommand-stop-transportserver.md)
