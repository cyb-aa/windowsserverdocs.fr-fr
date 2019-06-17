---
title: ipconfig
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 15071c2c-4815-4893-93b2-ab30232e312e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fff4e5088e3e08cf2e9e742d8fb6aa2187bb9e83
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438132"
---
# <a name="ipconfig"></a>ipconfig



Affiche toutes les valeurs de configuration de réseau TCP/IP actuelles et actualise les paramètres DHCP Dynamic Host Configuration Protocol () et le système DNS (Domain Name). Utilisée sans paramètres, **ipconfig** affiche Internet Protocol version 4 (IPv4) et IPv6 adresses, masque de sous-réseau et passerelle par défaut pour toutes les cartes.

## <a name="syntax"></a>Syntaxe

```
ipconfig [/allcompartments] [/all] [/renew [<Adapter>]] [/release [<Adapter>]] [/renew6[<Adapter>]] [/release6 [<Adapter>]] [/flushdns] [/displaydns] [/registerdns] [/showclassid <Adapter>] [/setclassid <Adapter> [<ClassID>]]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/all|Affiche la configuration TCP/IP intégrale pour tous les adaptateurs. Les cartes peuvent représenter des interfaces physiques, telles que des cartes réseau installées, ou des interfaces logiques, telles que des connexions d'accès à distance.|
|/allcompartments|Affiche la configuration TCP/IP intégrale pour tous les compartiments.|
|/displaydns|Affiche le contenu du cache de résolution DNS client, qui inclut les entrées préchargé à partir du fichier Hosts local et tout récemment obtenus des enregistrements de ressources pour les requêtes de nom résolus par l’ordinateur. Le service Client DNS utilise ces informations pour résoudre rapidement les noms fréquemment interrogés avant d’interroger ses serveurs DNS configurés.|
|/flushdns|Vide et réinitialise le contenu du cache de résolution client DNS. Au cours de la résolution des problèmes DNS, vous pouvez utiliser cette procédure pour ignorer les entrées de cache négatives à partir du cache, ainsi que toutes les entrées qui ont été ajoutées dynamiquement.|
|/registerdns|Lance l’inscription dynamique manuelle pour les noms DNS et les adresses IP qui sont configurés sur un ordinateur. Vous pouvez utiliser ce paramètre pour résoudre les problèmes d’échec d’inscription de nom DNS ou de résoudre un problème de mise à jour dynamique entre un client et le serveur DNS sans redémarrer l’ordinateur client. Les paramètres de DNS dans les propriétés avancées du protocole TCP/IP déterminent les noms sont inscrits dans DNS.|
|/ release [\<adaptateur >]|Envoie un message DHCPRELEASE au serveur DHCP pour libérer de la configuration DHCP actuelle et ignorer la configuration d’adresse IP pour toutes les cartes (si un adaptateur n’est pas spécifié) ou pour une carte spécifique si la *adaptateur* paramètre n’est inclus. Ce paramètre désactive TCP/IP pour les adaptateurs configurés pour obtenir une adresse IP automatiquement. Pour spécifier un nom de l’adaptateur, tapez le nom de l’adaptateur qui s’affiche lorsque vous utilisez **ipconfig** sans paramètres.|
|/release6[\<Adapter>]|Envoie un message DHCPRELEASE au serveur DHCPv6 pour libérer de la configuration DHCP actuelle et ignorer la configuration d’adresse IPv6 pour toutes les cartes (si un adaptateur n’est pas spécifié) ou pour une carte spécifique si la *adaptateur* paramètre n’est inclus. Ce paramètre désactive TCP/IP pour les adaptateurs configurés pour obtenir une adresse IP automatiquement. Pour spécifier un nom de l’adaptateur, tapez le nom de l’adaptateur qui s’affiche lorsque vous utilisez **ipconfig** sans paramètres.|
|/ renouveler [\<adaptateur >]|Renouvelle la configuration de DHCP pour toutes les cartes (si un adaptateur n’est pas spécifié) ou pour une carte spécifique si la *adaptateur* paramètre n’est inclus. Ce paramètre est disponible uniquement sur les ordinateurs avec les cartes qui sont configurés pour obtenir automatiquement une adresse IP. Pour spécifier un nom de l’adaptateur, tapez le nom de l’adaptateur qui s’affiche lorsque vous utilisez **ipconfig** sans paramètres.|
|/renew6 [\<adaptateur >]|Renouvelle la configuration de DHCPv6 pour toutes les cartes (si un adaptateur n’est pas spécifié) ou pour une carte spécifique si la *adaptateur* paramètre n’est inclus. Ce paramètre est disponible uniquement sur les ordinateurs avec les cartes qui sont configurés pour obtenir automatiquement une adresse IPv6. Pour spécifier un nom de l’adaptateur, tapez le nom de l’adaptateur qui s’affiche lorsque vous utilisez **ipconfig** sans paramètres.|
|/setclassid \<Adapter>[ <ClassID>]|Configure l’ID de classe DHCP d’une carte spécifique. Pour définir l’ID de classe DHCP pour toutes les cartes, utilisez l’astérisque ( **&#42;** ) caractère générique à la place de *adaptateur*. Ce paramètre est disponible uniquement sur les ordinateurs avec les cartes qui sont configurés pour obtenir automatiquement une adresse IP. Si un ID de classe DHCP n’est pas spécifié, l’ID de classe en cours est supprimé.|
|/showclassid \<adaptateur >|Affiche l’ID de classe DHCP d’une carte spécifique. Pour afficher l’ID de classe DHCP pour toutes les cartes, utilisez l’astérisque ( **&#42;** ) caractère générique à la place de *adaptateur*. Ce paramètre est disponible uniquement sur les ordinateurs avec les cartes qui sont configurés pour obtenir automatiquement une adresse IP.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

- Cette commande est particulièrement utile sur les ordinateurs qui sont configurés pour obtenir automatiquement une adresse IP. Cela permet aux utilisateurs de déterminer les valeurs de configuration TCP/IP ont été configurées par DHCP, APIPA Automatic Private IP Addressing () ou une configuration de remplacement.
- Si le nom que vous fournissez pour *adaptateur* contient des espaces, utilisez les guillemets autour du nom de l’adaptateur (exemple : **«** <em>Nom de l’adaptateur</em> **»** ).
- Pour les noms de l’adaptateur, **ipconfig** prend en charge l’utilisation de l’astérisque ( *) caractère générique pour spécifier des cartes avec des noms qui commencent par une chaîne spécifiée ou les cartes dont les noms contiennent une chaîne spécifiée. Par exemple, **Local\\** *    correspond à toutes les cartes qui commencent par la chaîne Local et  **\*Con\\** * correspond à toutes les cartes qui contiennent le chaîne Con.

## <a name="examples"></a>Exemples

Pour afficher la configuration TCP/IP de base pour toutes les cartes, tapez :
```
ipconfig
```
Pour afficher la configuration TCP/IP intégrale pour tous les adaptateurs, tapez :
```
ipconfig /all
```
Pour renouveler une configuration d’adresse IP attribuée par DHCP pour seulement l’adaptateur de la connexion au réseau Local, tapez :
```
ipconfig /renew "Local Area Connection"
```
Pour vider le cache de résolution DNS lors de la résolution des problèmes de résolution de nom DNS, tapez :
```
ipconfig /flushdns
```
Pour afficher l’ID de classe DHCP pour toutes les cartes avec des noms qui commencent par Local, tapez :
```
ipconfig /showclassid Local*
```
Pour définir l’ID de classe DHCP pour l’adaptateur de la connexion au réseau Local pour le TEST, tapez :
```
ipconfig /setclassid "Local Area Connection" TEST
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)