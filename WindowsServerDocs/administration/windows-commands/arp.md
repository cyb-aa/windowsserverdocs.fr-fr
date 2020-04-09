---
title: arp
description: La rubrique commandes Windows pour **ARP**, qui affiche et modifie les entrées dans le cache ARP (Address Resolution Protocol) utilisé pour stocker les adresses IP et leurs adresses physiques résolues.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 827e96eb-1945-483f-980f-714703456f7c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 289ab87c99512c7c34154d4b85d541db82ab296b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851312"
---
# <a name="arp"></a>arp

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche et modifie les entrées dans le cache ARP (Address Resolution Protocol). Le cache ARP contient une ou plusieurs tables qui sont utilisées pour stocker les adresses IP et leurs adresses physiques Ethernet ou Token Ring résolues. Il existe une table distincte pour chaque carte réseau Ethernet ou Token Ring installée sur votre ordinateur. Utilisé sans paramètres, **ARP** affiche des informations d’aide.

## <a name="syntax"></a>Syntaxe
```
arp [/a [<Inetaddr>] [/n <ifaceaddr>]] [/g [<Inetaddr>] [-n <ifaceaddr>]] [/d <Inetaddr> [<ifaceaddr>]] [/s <Inetaddr> <Etheraddr> [<ifaceaddr>]]
```
#### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| `/a [<Inetaddr>] [/n <ifaceaddr>]` | Affiche les tables de cache ARP actuelles pour toutes les interfaces. Le paramètre `/n` respecte la casse. Pour afficher l’entrée de cache ARP pour une adresse IP spécifique, utilisez **ARP/a** avec le paramètre *InetAddr* , où *InetAddr* est une adresse IP. Si *InetAddr* n’est pas spécifié, la première interface applicable est utilisée. Pour afficher la table de cache ARP pour une interface spécifique, utilisez le paramètre * */n * * * ifaceaddr* conjointement avec le paramètre **/a** où *ifaceaddr* est l’adresse IP assignée à l’interface. |
| `/g [<Inetaddr>] [/n <ifaceaddr> `] | Identique à **/a**. |
|`[/d <Inetaddr> [<ifaceaddr>]` | Supprime une entrée avec une adresse IP spécifique, où *InetAddr* est l’adresse IP. Pour supprimer une entrée dans une table pour une interface spécifique, utilisez le paramètre *ifaceaddr* où *ifaceaddr* est l’adresse IP assignée à l’interface. Pour supprimer toutes les entrées, utilisez le caractère générique astérisque (\*) à la place de *InetAddr*. |
|`/s <Inetaddr> <Etheraddr> [<ifaceaddr>]` | Ajoute une entrée statique au cache ARP qui résout l’adresse IP *InetAddr* en adresse physique *Etheraddr*. Pour ajouter une entrée de cache ARP statique à la table pour une interface spécifique, utilisez le paramètre *ifaceaddr* où *ifaceaddr* est une adresse IP affectée à l’interface. |
|`/?`| Affiche l'aide à l'invite de commandes. |

## <a name="remarks"></a>Notes
- Les adresses IP pour *InetAddr* et *ifaceaddr* sont exprimées en notation décimale séparée par des points.
- L’adresse physique de *Etheraddr* se compose de six octets exprimés en notation hexadécimale et séparés par des traits d’Union (par exemple, 00-AA-00-4F-2A-9c).

- Les entrées ajoutées avec le paramètre **/s** sont statiques et n’expirent pas dans le cache ARP. Les entrées sont supprimées si le protocole TCP/IP est arrêté et démarré. Pour créer des entrées de cache ARP statiques permanentes, placez les commandes **ARP** appropriées dans un fichier de commandes et utilisez des tâches planifiées pour exécuter le fichier de commandes au démarrage.

## <a name="examples"></a><a name=BKMK_Examples></a>Illustre

Pour afficher les tables de cache ARP pour toutes les interfaces, tapez :

```
arp /a
```

Pour afficher la table de cache ARP de l’interface à laquelle est affectée l’adresse IP 10.0.0.99, tapez :

```
arp /a /n 10.0.0.99
```

Pour ajouter une entrée de cache ARP statique qui résout l’adresse IP 10.0.0.80 à l’adresse physique 00-AA-00-4F-2A-9C, tapez :

```
arp /s 10.0.0.80 00-AA-00-4F-2A-9C 
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
