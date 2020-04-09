---
title: ping
description: Utilisez la commande ping pour vérifier la connectivité réseau.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 49272671-2eec-4fa5-881f-65c24cfbef52
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: c9e03b45d889bcac87bd3e533ab69c7a07be74ee
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80837552"
---
# <a name="ping"></a>ping

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La commande **ping** vérifie la connectivité de niveau IP à un autre ordinateur TCP/IP en envoyant des messages de demande d’écho ICMP (Internet Control Message Protocol). La réception des messages de réponse à écho correspondants est affichée, ainsi que les durées d’aller-retour. ping est la commande TCP/IP principale utilisée pour résoudre les problèmes de connectivité, d’accessibilité et de résolution de noms. Utilisé sans paramètres, **ping** affiche l’aide.

## <a name="syntax"></a>Syntaxe

```
ping [/t] [/a] [/n <Count>] [/l <Size>] [/f] [/I <TTL>] [/v <TOS>] [/r <Count>] [/s <Count>] [{/j <Hostlist> | /k <Hostlist>}] [/w <timeout>] [/R] [/S <Srcaddr>] [/4] [/6] <TargetName>
```

#### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|-------|--------|
|/t|Spécifie que le ping continue à envoyer des messages de demande d’écho à la destination jusqu’à ce qu’il soit interrompu. Pour interrompre et afficher les statistiques, appuyez sur CTRL + ATTN. Pour interrompre et quitter **ping**, appuyez sur Ctrl + C.|
|/a|Spécifie que la résolution de noms inversée est effectuée sur l’adresse IP de destination. En cas de réussite, ping affiche le nom d’hôte correspondant.|
|/n \<nombre\>|Spécifie le nombre de messages de demande d’écho envoyés. La valeur par défaut est 4.|
|Taille de \<de/l\>|Spécifie la longueur, en octets, du champ de données dans les messages de demande d’écho envoyés. La valeur par défaut est 32. La taille maximale est de 65 527.|
|/f|Spécifie que les messages de demande d’écho sont envoyés avec l’indicateur do not fragment dans l’en-tête IP défini sur 1 (disponible sur IPv4 uniquement). Le message de demande d’écho ne peut pas être fragmenté par les routeurs dans le chemin d’accès à la destination. Ce paramètre est utile pour résoudre les problèmes de l’unité de transmission maximale (PMTU) Path.|
|/I \<TTL\>|Spécifie la valeur du champ TTL dans l’en-tête IP pour les messages de demande d’écho envoyés. La valeur par défaut est la valeur de durée de vie par défaut pour l’hôte. La *durée de vie* maximale est de 255.|
|\> des OT \<|Spécifie la valeur du champ type de service (TOS) dans l’en-tête IP pour les messages de demande d’écho envoyés (disponible sur IPv4 uniquement). La valeur par défaut est 0. *OT* est spécifié sous la forme d’une valeur décimale comprise entre 0 et 255.|
|/r nombre d' \<\>|Spécifie que l’option d’itinéraire d’enregistrement dans l’en-tête IP est utilisée pour enregistrer le chemin emprunté par le message de demande d’écho et le message de réponse d’écho correspondant (disponible sur IPv4 uniquement). Chaque tronçon du chemin d’accès utilise une entrée dans l’option Enregistrer l’itinéraire. Si possible, spécifiez un *nombre* supérieur ou égal au nombre de sauts entre la source et la destination. Le *nombre* doit être un minimum de 1 et un maximum de 9.|
|/s \<\>|Spécifie que l’option horodateur Internet dans l’en-tête IP est utilisée pour enregistrer l’heure d’arrivée du message de demande d’écho et le message de réponse à écho correspondant pour chaque tronçon. Le *nombre* doit être un minimum de 1 et un maximum de 4. Cela est requis pour les adresses de destination lien-local.|
|/j \<hostlist\>|Spécifie que les messages de demande d’écho utilisent l’option de route de source libre dans l’en-tête IP avec l’ensemble de destinations intermédiaires spécifié dans *hostlist* (disponible sur IPv4 uniquement). Avec un routage source libre, les destinations intermédiaires successives peuvent être séparées par un ou plusieurs routeurs. Le nombre maximal d’adresses ou de noms dans la liste d’ordinateurs hôtes est de 9. La liste hôte est une série d’adresses IP (en notation décimale séparée par des points), séparées par des espaces.|
|/k \<hostlist\>|Spécifie que les messages de demande d’écho utilisent l’option de routage source strict dans l’en-tête IP avec l’ensemble de destinations intermédiaires spécifié dans *hostlist* (disponible sur IPv4 uniquement). Avec un routage source strict, la destination intermédiaire suivante doit être directement accessible (il doit s’agir d’un voisin sur une interface du routeur). Le nombre maximal d’adresses ou de noms dans la liste d’ordinateurs hôtes est de 9. La liste hôte est une série d’adresses IP (en notation décimale séparée par des points), séparées par des espaces.|
|/w \<délai d’expiration\>|Spécifie la durée, en millisecondes, à attendre que le message de réponse à écho correspondant à un message de demande d’écho donné soit reçu. Si le message de réponse à écho n’est pas reçu dans le délai imparti, le message d’erreur « délai d’attente de la demande dépassé » s’affiche. Le délai d’attente par défaut est de 4000 (4 secondes).|
|/R|Spécifie que le chemin d’accès aller-retour est suivi (disponible sur IPv6 uniquement).|
|/S \<srcaddr\>|Spécifie l’adresse source à utiliser (disponible sur IPv6 uniquement).|
|/4|Spécifie que IPv4 est utilisé pour exécuter une commande ping. Ce paramètre n’est pas requis pour identifier l’hôte cible avec une adresse IPv4. Il est uniquement nécessaire d’identifier l’hôte cible par son nom.|
|/6|Spécifie que IPv6 est utilisé pour exécuter une commande ping. Ce paramètre n’est pas requis pour identifier l’hôte cible avec une adresse IPv6. Il est uniquement nécessaire d’identifier l’hôte cible par son nom.|
|\<TargetName\>|Spécifie le nom d’hôte ou l’adresse IP de la destination.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Vous pouvez utiliser la **commande ping** pour tester à la fois le nom de l’ordinateur et l’adresse IP de l’ordinateur. Si le test ping de l’adresse IP réussit, mais que le test ping du nom de l’ordinateur n’est pas possible, vous risquez de rencontrer un problème de résolution de noms. Dans ce cas, assurez-vous que le nom d’ordinateur que vous spécifiez peut être résolu par le biais du fichier d’hôtes local, en utilisant des requêtes DNS (Domain Name System) ou des techniques de résolution de noms NetBIOS.
-   Cette commande est disponible uniquement si le protocole TCP/IP (Internet Protocol) est installé en tant que composant dans les propriétés d’une carte réseau dans connexions réseau.

## <a name="examples"></a><a name="BKMK_Examples"></a>Illustre

L’exemple suivant montre le résultat de la commande **ping** :

```
C:\>ping example.microsoft.com       
         pinging example.microsoft.com [192.168.239.132] with 32 bytes of data:       
         Reply from 192.168.239.132: bytes=32 time=101ms TTL=124       
         Reply from 192.168.239.132: bytes=32 time=100ms TTL=124       
         Reply from 192.168.239.132: bytes=32 time=120ms TTL=124       
         Reply from 192.168.239.132: bytes=32 time=120ms TTL=124
```

Pour exécuter une commande ping sur le 10.0.99.221 de destination et résoudre 10.0.99.221 en son nom d’hôte, tapez :

```
ping /a 10.0.99.221
```

Pour exécuter une commande ping sur le 10.0.99.221 de destination avec 10 messages de demande d’écho, chacun d’entre eux ayant un champ de données de 1000 octets, tapez :

```
ping /n 10 /l 1000 10.0.99.221
```

Pour exécuter une commande ping sur le 10.0.99.221 de destination et enregistrer l’itinéraire pour 4 tronçons, tapez :

```
ping /r 4 10.0.99.221
```

Pour exécuter une commande ping sur le 10.0.99.221 de destination et spécifier l’Itinéraire source libre de 10.12.0.1-10.29.3.1-10.1.44.1, tapez :

```
ping /j 10.12.0.1 10.29.3.1 10.1.44.1 10.0.99.221
```

## <a name="additional-references"></a>Références supplémentaires
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
