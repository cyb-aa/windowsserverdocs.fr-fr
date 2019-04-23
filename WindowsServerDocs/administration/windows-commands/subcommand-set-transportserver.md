---
title: Set-TransportServer sous-commande
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: d144ed7d461cbebcd351aa4347fde20f35736c26
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886600"
---
# <a name="subcommand-set-transportserver"></a>Sous-commande : set-TransportServer

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Définit les paramètres de configuration pour un serveur de Transport.
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
|[/Server:<Server name>]|Spécifie le nom du serveur de Transport. Cela peut être le nom NetBIOS ou le nom de domaine complet (FQDN). Si aucun nom de serveur de Transport est spécifié, le serveur local est utilisé.|
|[/ObtainIpv4From:{Dhcp &#124; Range}]|Définit la source des adresses IPv4 comme suit :<br /><br />-[/ Démarrer : <IP address>] définit le début de la plage d’adresses IP. Cela est nécessaire et valide uniquement si cette option est définie sur **plage**.<br />-[Fin : <IP address>] définit la fin de la plage d’adresses IP. Cela est nécessaire et valide uniquement si cette option est définie sur **plage**.<br />-[/ startPort : <port>] définit le début de la plage de ports.<br />-[/ L’EndPort : <port>] définit la fin de la plage de ports.|
|[/ObtainIpv6From:Range]|Spécifie la source des adresses IPv6. Cette option s’applique uniquement à Windows Server 2008 R2 et la seule valeur possible est **plage**.<br /><br />-[/ Démarrer : <IP address>] définit le début de la plage d’adresses IP. Cela est nécessaire et valide uniquement si cette option est définie sur **plage**.<br />-[Fin : <IP address>] définit la fin de la plage d’adresses IP. Cela est nécessaire et valide uniquement si cette option est définie sur **plage**.<br />-[/ startPort : <port>] définit le début de la plage de ports.<br />-[/ L’EndPort : <port>] définit la fin de la plage de ports.|
|[/ Profile : {10 Mbits/s &#124; 100 Mbits/s &#124; 1 Gbit/s &#124; personnalisé}]|Spécifie le profil de réseau à utiliser. Cette option est uniquement disponible pour les serveurs exécutant Windows Server 2008 ou Windows Server 2003.|
|[/MulticastSessionPolicy]|Configure les paramètres de transfert pour les transmissions par multidiffusion. Cette commande est uniquement disponible pour Windows Server 2008 R2.<br /><br />-[/ Stratégie : {None &#124; déconnexion automatique &#124; flux multiples}] détermine comment gérer les clients lents. **Aucun** signifie conserver tous les clients dans une session à la même vitesse. **Déconnexion automatique** signifie que tous les clients qui en-dessous spécifié **/Threshold** sont déconnectés. **Flux multiples** signifie que les clients sont séparés en plusieurs sessions comme spécifié par **/StreamCount**.<br />-[/ Seuil :<Speed in KBps>] définit le taux de transfert en Kbits/s pour **/Policy:AutoDisconnect**. Les clients qui en-dessous de ce taux sont déconnectés de transmissions par multidiffusion.<br />-[/ StreamCount : {2 &#124; 3}] [/ de secours : {Oui &#124; No}] détermine le nombre de sessions pour **/Policy:Multistream**. **2** signifie que deux sessions (rapides et lentes) et **3** signifie que trois sessions (lente, moyen, fast).<br />-[/ De secours : {Oui &#124; No}] détermine si les clients qui sont déconnectés continue le transfert à l’aide d’une autre méthode (si pris en charge par le client). Si vous utilisez le client WDS, l’ordinateur se tourne vers monodiffusion. Wdsmcast.exe ne prend pas en charge un mécanisme de secours. Cette option s’applique également aux clients ne prenant pas en charge **flux multiples**. Dans ce cas, l’ordinateur revient à une autre méthode au lieu de passer à une session de transfert plus lente.|
## <a name="BKMK_examples"></a>Exemples
Pour définir la plage d’adresses IPv4 pour le serveur, tapez :
```
wdsutil /Set-TransportServer /ObtainIpv4From:Range /start:239.0.0.1 /End:239.0.0.100
```
Pour définir la plage d’adresses IPv4, plage de ports et le profil pour le serveur, tapez :
```
wdsutil /Set-TransportServer /Server:MyWDSServer /ObtainIpv4From:Range /start:239.0.0.1 /End:239.0.0.100 /startPort:12000 /EndPort:50000 /Profile:10mbps
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande disable-TransportServer](using-the-disable-transportserver-command.md)
[à l’aide de la commande enable-TransportServer](using-the-enable-transportserver-command.md) 
 [ À l’aide de la commande get-TransportServer](using-the-get-transportserver-command.md)
[sous-commande : start-TransportServer](subcommand-start-transportserver.md)
[sous-commande : stop-TransportServer](subcommand-stop-transportserver.md)
