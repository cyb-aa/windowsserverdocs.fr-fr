---
title: tracert
description: La rubrique commandes Windows pour tracert, qui détermine le chemin d’accès à une destination, en envoyant des demandes d’écho ICMP (Internet Control Message Protocol) ou des messages ICMPv6 à la destination avec des valeurs de champ Durée de vie (TTL) de manière incrémentielle.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9032a032-2e5e-49d4-9e86-f821600e4ba6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a4485763aecf46aa91664c6a6a42c437be518f02
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80832642"
---
# <a name="tracert"></a>tracert

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Détermine le chemin d’accès à une destination en envoyant la demande d’écho ICMP (Internet Control Message Protocol) ou des messages ICMPv6 à la destination avec des valeurs de champ TTL (Time to Live) de manière incrémentielle. Le chemin d’accès affiché est la liste des interfaces de routeur near/Side des routeurs dans le chemin d’accès entre un hôte source et une destination. L’interface near/Side est l’interface du routeur qui est le plus proche de l’hôte d’envoi dans le chemin d’accès. Utilisé sans paramètres, tracert affiche l’aide.   

## <a name="syntax"></a>Syntaxe  
```  
tracert [/d] [/h <MaximumHops>] [/j <Hostlist>] [/w <timeout>] [/R] [/S <Srcaddr>] [/4][/6] <TargetName>  
```  
#### <a name="parameters"></a>Paramètres  
|Paramètre|Description|  
|-------|--------|  
|/d|Empêche le **traceurt** de tenter de résoudre les adresses IP des routeurs intermédiaires en leurs noms. Cela peut accélérer l’affichage des résultats de **tracert** .|  
|/h \<MaximumHops >|Spécifie le nombre maximal de sauts dans le chemin d’accès pour rechercher la cible (destination). La valeur par défaut est 30 tronçons.|  
|/j \<hostlist >|Spécifie que les messages de demande d’écho utilisent l’option de route de source libre dans l’en-tête IP avec l’ensemble de destinations intermédiaires spécifié dans *hostlist*. Avec un routage source libre, les destinations intermédiaires successives peuvent être séparées par un ou plusieurs routeurs. Le nombre maximal d’adresses ou de noms dans la liste d’ordinateurs hôtes est de 9. Le *hostlist* est une série d’adresses IP (en notation décimale séparée par des points), séparées par des espaces. Utilisez ce paramètre uniquement lors du suivi des adresses IPv4.|  
|/w \<délai d’expiration >|Spécifie la durée, en millisecondes, d’attente pour la réception du message ICMP de dépassement de délai ou de réponse à écho correspondant à un message de demande d’écho donné. S’il n’est pas reçu dans le délai d’attente, un astérisque (*) est affiché. Le délai d’attente par défaut est de 4000 (4 secondes).|  
|/R|Spécifie que l’en-tête d’extension de routage IPv6 doit être utilisé pour envoyer un message de demande d’écho à l’hôte local, en utilisant la destination comme destination intermédiaire et en testant l’itinéraire inverse.|  
|/S \<srcaddr >|Spécifie l’adresse source à utiliser dans les messages de demande d’écho. Utilisez ce paramètre uniquement lors du suivi des adresses IPv6.|  
|/4|Spécifie que tracert. exe peut uniquement utiliser IPv4 pour cette trace.|  
|/6|Spécifie que tracert. exe peut utiliser uniquement IPv6 pour ce suivi.|  
|\<TargetName >|Spécifie la destination, identifiée par une adresse IP ou un nom d’hôte.|  
|/?|Affiche l'aide à l'invite de commandes.|  

## <a name="remarks"></a>Notes  
-   Cet outil de diagnostic détermine le chemin emprunté à une destination en envoyant des messages de demande d’écho ICMP avec des valeurs de durée de vie (TTL) différentes à la destination. Chaque routeur sur le chemin d’accès est nécessaire pour décrémenter la durée de vie d’un paquet IP d’au moins 1 avant de le transférer. En effet, la durée de vie est un compteur de liens maximal. Lorsque la durée de vie d’un paquet atteint 0, le routeur est supposé renvoyer un message ICMP temps dépassé à l’ordinateur source. tracert détermine le chemin d’accès en envoyant le premier message de demande d’écho avec une durée de vie de 1 et en incrémentant la durée de vie de 1 sur chaque transmission suivante jusqu’à ce que la cible réponde ou que le nombre maximal de tronçons soit atteint. Le nombre maximal de tronçons est 30 par défaut et peut être spécifié à l’aide du paramètre **/h** . Le chemin d’accès est déterminé en examinant les messages ICMP temps dépassé renvoyés par les routeurs intermédiaires et le message de réponse à écho renvoyé par la destination. Toutefois, certains routeurs ne retournent pas de messages de temps dépassé pour les paquets dont les valeurs TTL ont expiré et sont invisile à la commande tracert. Dans ce cas, une ligne d’astérisques (*) s’affiche pour ce tronçon.  
-   Pour suivre un chemin d’accès et fournir une latence réseau et une perte de paquets pour chaque routeur et chaque lien dans le chemin d’accès, utilisez la commande **pathping** .  
-   Cette commande est disponible uniquement si le protocole TCP/IP (Internet Protocol) est installé en tant que composant dans les propriétés d’une carte réseau dans connexions réseau.  

## <a name="examples"></a><a name=BKMK_Examples></a>Illustre  
Pour suivre le chemin d’accès à l’ordinateur hôte nommé corp7.microsoft.com, tapez :  
```  
tracert corp7.microsoft.com  
```  
Pour suivre le chemin d’accès à l’ordinateur hôte nommé corp7.microsoft.com et empêcher la résolution de chaque adresse IP en son nom, tapez :  
```  
tracert /d corp7.microsoft.com  
```  
Pour suivre le chemin d’accès à l’ordinateur hôte nommé corp7.microsoft.com et utiliser l’Itinéraire source libre 10.12.0.1/10.29.3.1/10.1.44.1, tapez :  
```  
tracert /j 10.12.0.1 10.29.3.1 10.1.44.1 corp7.microsoft.com  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
