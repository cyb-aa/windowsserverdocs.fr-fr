---
title: ping
description: Utiliser la commande ping pour vérifier la connectivité de réseau.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 49272671-2eec-4fa5-881f-65c24cfbef52
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 1ac02a148061cd6eb8480c67f15e934f5fd57768
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816750"
---
# <a name="ping"></a>ping

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Le **ping** commande vérifie la connectivité de niveau IP vers un autre ordinateur TCP/IP en envoyant des messages de demande d’écho de contrôle Message ICMP (Internet Protocol). La réception de réponse messages sont affichés, ainsi que les durées de boucles à écho correspondant. ping est la commande TCP/IP principale utilisée pour résoudre les problèmes de connectivité, d’accessibilité et de résolution de noms. Utilisée sans paramètres, **ping** affiche l’aide.

## <a name="syntax"></a>Syntaxe

```
ping [/t] [/a] [/n <Count>] [/l <Size>] [/f] [/I <TTL>] [/v <TOS>] [/r <Count>] [/s <Count>] [{/j <Hostlist> | /k <Hostlist>}] [/w <timeout>] [/R] [/S <Srcaddr>] [/4] [/6] <TargetName>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|-------|--------|
|/t|Spécifie que la commande ping continuer à envoyer des messages de demande echo vers la destination jusqu'à interruption. Pour interrompre et afficher des statistiques, appuyez sur CTRL + Pause. Pour interrompre et quitter **ping**, appuyez sur CTRL + C.|
|/a|Spécifie que la résolution de noms inverse est effectuée sur l’adresse IP de destination. Si l’opération réussit, la commande ping affiche le nom d’hôte correspondant.|
|/n \<nombre\>|Spécifie le nombre d’écho envoyés des messages de demande. La valeur par défaut est 4.|
|/ l \<taille\>|Spécifie la longueur, en octets, du champ de données dans l’écho de messages de demande envoyées. La valeur par défaut est 32. La taille maximale est de 65 527.|
|/f|Spécifie qui renvoient des messages de demande sont envoyés avec les ne pas fragmenter indicateur dans l’en-tête IP défini sur 1 (disponible sur IPv4 uniquement). Le message de demande d’écho ne peuvent pas être fragmenté par les routeurs dans le chemin d’accès à la destination. Ce paramètre est utile pour résoudre les problèmes d’unité de Transmission maximale (PMTU) de chemin d’accès.|
|/I \<TTL\>|Spécifie la valeur du champ TTL dans l’en-tête IP d’écho envoyés des messages de demande. La valeur par défaut est la valeur de durée de vie par défaut pour l’hôte. La valeur maximale *TTL* est 255.|
|/v \<TOS\>|Spécifie la valeur du type de champ de Service (TOS) dans l’en-tête IP d’écho demander les messages envoyés (disponibles sur IPv4 uniquement). La valeur par défaut est 0. *Procédures* est spécifié comme une valeur décimale comprise entre 0 et 255.|
|/r \<nombre\>|Spécifie que l’option de Route d’enregistrement dans l’en-tête IP est utilisée pour enregistrer le chemin d’accès emprunté par le message de demande d’écho et correspondant message réponse à écho (disponible sur IPv4 uniquement). Chaque saut dans le chemin d’accès utilise une entrée dans l’option de Route d’enregistrement. Si possible, spécifiez un *nombre* qui est égal ou supérieur au nombre de tronçons entre la source et la destination. Le *nombre* doit être un minimum de 1 et un maximum de 9.|
|/s \<Count\>|Spécifie que l’option de timestamp Internet dans l’en-tête IP est utilisée pour enregistrer l’heure d’arrivée pour le message de demande d’écho et correspondant de renvoyer le message de réponse pour chaque tronçon. Le *nombre* doit être un minimum de 1 et un maximum de 4. Cela est nécessaire pour les adresses lien-local de destination.|
|/j \<Hostlist\>|Spécifie que l’utilisation de messages de demande de l’itinéraire Source libre de l’écho option dans l’en-tête IP avec l’ensemble de destinations intermédiaires spécifiées dans *paramètre ListeHôtes* (disponible sur IPv4 uniquement). Avec le routage de source libre, les destinations intermédiaires successives peuvent être séparées par un ou plusieurs routeurs. Le nombre maximal d’adresses ou des noms dans la liste d’hôtes est 9. La liste d’hôtes est une série d’adresses IP (en notation décimale) séparée par des espaces.|
|/k \<paramètre ListeHôtes\>|Spécifie que l’utilisation de messages de demande le routage Source Strict écho option dans l’en-tête IP avec l’ensemble de destinations intermédiaires spécifiées dans *paramètre ListeHôtes* (disponible sur IPv4 uniquement). Avec le routage source strict, la destination intermédiaire suivante doit être accessible directement (il doit être un voisin sur une interface du routeur). Le nombre maximal d’adresses ou des noms dans la liste d’hôtes est 9. La liste d’hôtes est une série d’adresses IP (en notation décimale) séparée par des espaces.|
|/w \<délai d’attente\>|Spécifie la quantité de temps, en millisecondes, à attendre l’écho de message de réponse qui correspond à un message de demande d’écho donné en réception. Si le message de réponse d’écho n’est pas reçue dans le délai d’attente, le message d’erreur « Requête a expiré » s’affiche. Le délai d’expiration par défaut est 4000 (4 secondes).|
|/R|Spécifie que le chemin d’accès aller-retour est suivi (disponible sur IPv6 uniquement).|
|/S \<Srcaddr\>|Spécifie l’adresse source à utiliser (disponible sur IPv6 uniquement).|
|/4|Spécifie que IPv4 est utilisée à la commande ping. Ce paramètre n’est pas obligatoire pour identifier l’ordinateur hôte cible avec une adresse IPv4. Il est uniquement nécessaire pour identifier l’ordinateur hôte cible par nom.|
|/6|Spécifie que IPv6 est utilisé à la commande ping. Ce paramètre n’est pas obligatoire pour identifier l’ordinateur hôte cible avec une adresse IPv6. Il est uniquement nécessaire pour identifier l’ordinateur hôte cible par nom.|
|\<TargetName\>|Spécifie le nom d’hôte ou adresse IP de la destination.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Vous pouvez utiliser **ping** pour tester le nom d’ordinateur et l’adresse IP de l’ordinateur. Si une commande ping l’adresse IP réussite, mais le nom de l’ordinateur exécutant la commande ping n’est pas, peut avoir un problème de résolution de nom. Dans ce cas, vérifiez que le nom d’ordinateur que vous spécifiez peut être résolu par le biais du fichier Hosts local, à l’aide de requêtes du système DNS (Domain Name), ou via NetBIOS nom techniques de résolution.
-   Cette commande est disponible uniquement si le protocole TCP/IP (Internet Protocol) est installé en tant que composant dans les propriétés d’une carte réseau dans Connexions réseau.

## <a name="BKMK_Examples"></a>Exemples

L’exemple suivant **ping** sortie de la commande :

```
C:\>ping example.microsoft.com       
         pinging example.microsoft.com [192.168.239.132] with 32 bytes of data:       
         Reply from 192.168.239.132: bytes=32 time=101ms TTL=124       
         Reply from 192.168.239.132: bytes=32 time=100ms TTL=124       
         Reply from 192.168.239.132: bytes=32 time=120ms TTL=124       
         Reply from 192.168.239.132: bytes=32 time=120ms TTL=124
```

Pour un test ping sur la destination 10.0.99.221 et résoudre 10.0.99.221 sur son nom d’hôte, tapez :

```
ping /a 10.0.99.221
```

Pour un test ping sur la destination 10.0.99.221 avec des messages de demande d’écho 10, chacun d’eux a un champ de données de 1 000 octets, tapez :

```
ping /n 10 /l 1000 10.0.99.221
```

Pour un test ping sur la destination 10.0.99.221 et enregistrer l’itinéraire pour 4 sauts, tapez :

```
ping /r 4 10.0.99.221
```

Pour un test ping sur la destination 10.0.99.221 et spécifier l’itinéraire source libre de 10.12.0.1-10.29.3.1-10.1.44.1, tapez :

```
ping /j 10.12.0.1 10.29.3.1 10.1.44.1 10.0.99.221
```

## <a name="additional-references"></a>Références supplémentaires
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
