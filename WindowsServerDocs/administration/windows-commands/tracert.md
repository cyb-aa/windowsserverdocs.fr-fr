---
title: tracert
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9032a032-2e5e-49d4-9e86-f821600e4ba6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6a97c7656e646a22892eee5caa13d0163d05293d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837180"
---
# <a name="tracert"></a>tracert

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Détermine le chemin d’accès effectuée vers une destination en envoyant des messages de demande ou ICMPv6 echo de contrôle Message ICMP (Internet Protocol) vers la destination de façon incrémentielle plus en plus de temps aux valeurs de champ de vie (TTL). Le chemin d’accès affiché est la liste des interfaces de routeur de près/côté des routeurs dans le chemin d’accès entre un hôte source et une destination. L’interface de près/côté est l’interface du routeur qui est le plus proche de l’hôte d’envoi dans le chemin d’accès. Utilisé sans paramètres, tracert affiche l’aide.   

## <a name="syntax"></a>Syntaxe  
```  
tracert [/d] [/h <MaximumHops>] [/j <Hostlist>] [/w <timeout>] [/R] [/S <Srcaddr>] [/4][/6] <TargetName>  
```  
### <a name="parameters"></a>Paramètres  
|Paramètre|Description|  
|-------|--------|  
|/d|Empêche **tracert** de tenter de résoudre les adresses IP des routeurs intermédiaires à leurs noms. Cela peut accélérer l’affichage de **tracert** résultats.|  
|/h \<MaximumHops>|Spécifie le nombre maximal de sauts dans le chemin d’accès pour rechercher la cible (destination). La valeur par défaut est 30 tronçons.|  
|/j \<Hostlist>|Spécifie qui renvoient des messages de demande utilisent l’option itinéraire Source libre dans l’en-tête IP avec l’ensemble de destinations intermédiaires spécifiées dans *paramètre ListeHôtes*. Avec le routage de source libre, les destinations intermédiaires successives peuvent être séparées par un ou plusieurs routeurs. Le nombre maximal d’adresses ou des noms dans la liste d’hôtes est 9. Le *paramètre ListeHôtes* est une série d’adresses IP (en notation décimale) séparée par des espaces. Utilisez ce paramètre uniquement lors du suivi des adresses IPv4.|  
|/w \<délai d’attente >|Spécifie la quantité de temps en millisecondes à attendre l’heure ICMP a été dépassé ou message réponse à écho correspondant à un message de demande d’écho donné en réception. Si ne pas reçu dans le délai d’attente, un astérisque (*) s’affiche. Le délai d’expiration par défaut est 4000 (4 secondes).|  
|/R|Spécifie que l’en-tête d’extension du routage IPv6 est utilisé pour envoyer un message de demande d’écho à l’hôte local, à l’aide de la destination comme une destination intermédiaire et le test de l’itinéraire inverse.|  
|/S \<Srcaddr>|Spécifie l’adresse source pour l’utiliser dans l’écho de messages de demande. Utilisez ce paramètre uniquement lors du suivi des adresses IPv6.|  
|/4|Spécifie que tracert.exe peut utiliser uniquement IPv4 pour cette trace.|  
|/6|Spécifie que tracert.exe peut utiliser uniquement IPv6 pour cette trace.|  
|\<TargetName>|Spécifie la destination, identifiée par nom d’hôte ou adresse IP.|  
|/?|Affiche l'aide à l'invite de commandes.|  

## <a name="remarks"></a>Notes  
-   Cet outil de diagnostic détermine l’itinéraire menant vers une destination en envoyant des messages de demande avec divers temps ICMP echo aux valeurs de vie (TTL) vers la destination. Chaque routeur sur le chemin d’accès est nécessaire pour diminuer la durée de vie dans un paquet IP d’au moins 1 avant de le transférer. En effet, la durée de vie est un compteur de lien maximale. Lorsque la durée de vie sur un paquet atteint 0, le routeur est supposée retourner un message ICMP de dépassé le temps à l’ordinateur source. Tracert détermine le chemin d’accès en envoyant l’écho premier message de demande avec une durée de vie de 1 et l’incrémentation de la durée de vie de 1 à chaque transmission ultérieure jusqu'à ce que la cible réponde ou le nombre maximal de sauts est atteinte. Le nombre maximal de sauts est 30 par défaut et peut être spécifié à l’aide de la **/h** paramètre. Le chemin d’accès est déterminé en examinant les messages ICMP Temps dépassé renvoyés par les routeurs intermédiaires et l’écho de message de réponse retourné par la destination. Toutefois, certains routeurs ne retournent pas de messages temps dépassé pour les paquets avec des valeurs de durée de vie a expiré et sont invisile à la commande tracert. Dans ce cas, une ligne d’astérisques (*) s’affiche pour ce tronçon.  
-   Pour suivre un chemin d’accès et fournir une latence du réseau et perte de paquets pour chaque routeur et liaison dans le chemin d’accès, utilisez le **pathping** commande.  
-   Cette commande est disponible uniquement si le protocole TCP/IP (Internet Protocol) est installé en tant que composant dans les propriétés d’une carte réseau dans Connexions réseau.  

## <a name="BKMK_Examples"></a>Exemples  
Pour suivre le chemin d’accès à l’hôte nommé corp7.microsoft.com, tapez :  
```  
tracert corp7.microsoft.com  
```  
Pour suivre le chemin d’accès à l’hôte nommé corp7.microsoft.com et empêcher la résolution de toutes les adresses IP à son nom, tapez :  
```  
tracert /d corp7.microsoft.com  
```  
Pour suivre le chemin d’accès à l’hôte nommé corp7.microsoft.com et utiliser le 10.12.0.1/10.29.3.1/10.1.44.1 itinéraire source libre, tapez :  
```  
tracert /j 10.12.0.1 10.29.3.1 10.1.44.1 corp7.microsoft.com  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)  
