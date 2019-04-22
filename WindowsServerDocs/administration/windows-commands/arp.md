---
title: arp
description: Rubrique de commandes de Windows pour **arp** -affiche et modifie les entrées dans le cache de protocole de résolution (arp) adresse utilisée pour stocker les adresses IP et leurs adresses physiques résolus.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 827e96eb-1945-483f-980f-714703456f7c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5cd84269a5ac1a85d4b6cf359cc97f478a500c4f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825980"
---
# <a name="arp"></a>arp

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche et modifie les entrées dans le cache du protocole ARP (Address Resolution). Le cache ARP contient une ou plusieurs tables qui sont utilisés pour stocker les adresses IP et leurs adresses physiques Ethernet ou Token Ring résolues. Il existe une table distincte pour chaque carte réseau Ethernet ou Token Ring installée sur votre ordinateur. Utilisée sans paramètres, **arp** affiche l’aide d’informations.
## <a name="syntax"></a>Syntaxe
```
arp [/a [<Inetaddr>] [/n <ifaceaddr>]] [/g [<Inetaddr>] [-n <ifaceaddr>]] [/d <Inetaddr> [<ifaceaddr>]] [/s <Inetaddr> <Etheraddr> [<ifaceaddr>]]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|/a [<Inetaddr>] [/n <ifaceaddr>]|Affiche les tables de cache arp actif pour toutes les interfaces. Le paramètre /n respecte la casse.<br /><br />Pour afficher l’entrée de cache arp pour une adresse IP spécifique, utilisez **arp /a** avec la *AdrInet* paramètre, où *AdrInet* est une adresse IP. Si *AdrInet* n’est pas spécifié, la première interface applicable est utilisée.<br /><br />Pour afficher la table de cache arp pour une interface spécifique, utilisez le **/n *** AdrIface* paramètre conjointement avec la **/a** paramètre où *AdrIface* est l’adresse IP attribuée à l’interface.|
|/g [<Inetaddr>] [/n <ifaceaddr>]|Identique à **/a**.|
|[/d <Inetaddr> [<ifaceaddr>]|Supprime une entrée avec une adresse IP spécifique, où *AdrInet* est l’adresse IP.<br /><br />Pour supprimer une entrée dans une table pour une interface spécifique, utilisez le *AdrIface* paramètre où *AdrIface* est l’adresse IP attribuée à l’interface.<br /><br />Pour supprimer toutes les entrées, utilisez l’astérisque (\*) caractère générique à la place de *AdrInet*.|
|/s <Inetaddr> <Etheraddr> [<ifaceaddr>]|Ajoute une entrée statique à la mémoire cache arp qui résout l’adresse IP *AdrInet* à l’adresse physique *AdrEther*.<br /><br />Pour ajouter une entrée de cache arp statique à la table pour une interface spécifique, utilisez le *AdrIface* paramètre où *AdrIface* est une adresse IP attribuée à l’interface.|
|/?|Affiche l'aide à l'invite de commandes.|
## <a name="remarks"></a>Notes
-   Les adresses IP pour *AdrInet* et *AdrIface* sont exprimées en notation décimale.
-   L’adresse physique de *AdrEther* se compose de six octets exprimés en notation hexadécimale et séparés par des traits d’union (par exemple, 00-AA-00-4F-2A-9C).
-   Les entrées ajoutées avec la **/s** paramètre sont statique et de ne pas temps hors de la mémoire cache arp. Les entrées sont supprimées si le protocole TCP/IP est arrêté et démarré. Pour créer des entrées de cache arp statique permanente, placez le texte approprié **arp** commandes dans un lot de fichiers et utiliser des tâches planifiées pour exécuter le fichier de commandes au démarrage.
## <a name="BKMK_Examples"></a>Exemples
Pour afficher les tables de cache arp pour toutes les interfaces, tapez :
```
arp /a
```
Pour afficher la table de cache arp pour l’interface qui est affectée l’adresse IP 10.0.0.99, tapez :
```
arp /a /n 10.0.0.99
```
Pour ajouter une entrée de cache arp statique qui résout l’adresse IP 10.0.0.80 à l’adresse physique 00-AA-00-4F-2A-9C, tapez :
```
arp /s 10.0.0.80 00-AA-00-4F-2A-9C 
```
## <a name="additional-references"></a>Références supplémentaires
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
