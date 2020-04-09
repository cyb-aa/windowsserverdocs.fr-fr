---
title: ipconfig
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 15071c2c-4815-4893-93b2-ab30232e312e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1c93b75d6518746df7ef936c7059bd03fcf96ab6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80842072"
---
# <a name="ipconfig"></a>ipconfig



Affiche toutes les valeurs de configuration réseau TCP/IP actuelles et actualise les paramètres DHCP (Dynamic Host Configuration Protocol) et DNS (Domain Name System). Utilisé sans paramètres, **ipconfig** affiche les adresses IPv4 et IPv6, le masque de sous-réseau et la passerelle par défaut pour tous les adaptateurs.

## <a name="syntax"></a>Syntaxe

```
ipconfig [/allcompartments] [/all] [/renew [<Adapter>]] [/release [<Adapter>]] [/renew6[<Adapter>]] [/release6 [<Adapter>]] [/flushdns] [/displaydns] [/registerdns] [/showclassid <Adapter>] [/setclassid <Adapter> [<ClassID>]]
```

#### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/all|Affiche la configuration TCP/IP complète pour toutes les cartes. Les cartes peuvent représenter des interfaces physiques, telles que des cartes réseau installées, ou des interfaces logiques, telles que des connexions d'accès à distance.|
|/allcompartments|Affiche la configuration TCP/IP complète pour tous les compartiments.|
|/displaydns|Affiche le contenu du cache de résolution client DNS, qui comprend les entrées préchargées du fichier hosts local et les enregistrements de ressources récemment obtenus pour les requêtes de nom résolues par l’ordinateur. Le service client DNS utilise ces informations pour résoudre rapidement les noms fréquemment interrogés, avant d’interroger ses serveurs DNS configurés.|
|/flushdns|Vide et réinitialise le contenu du cache de résolution client DNS. Lors de la résolution des problèmes DNS, vous pouvez utiliser cette procédure pour supprimer les entrées de cache négatives du cache, ainsi que toutes les autres entrées ajoutées de manière dynamique.|
|/registerdns|Lance l’inscription dynamique manuelle pour les noms DNS et les adresses IP qui sont configurés sur un ordinateur. Vous pouvez utiliser ce paramètre pour résoudre les problèmes liés à l’échec d’une inscription de nom DNS ou résoudre un problème de mise à jour dynamique entre un client et le serveur DNS sans redémarrer l’ordinateur client. Les paramètres DNS dans les propriétés avancées du protocole TCP/IP déterminent les noms qui sont inscrits dans DNS.|
|/Release [\<de l’adaptateur >]|Envoie un message DHCPRELEASE au serveur DHCP pour libérer la configuration DHCP actuelle et annuler la configuration de l’adresse IP pour tous les adaptateurs (si aucune carte n’est spécifiée) ou pour une carte spécifique si le paramètre de l' *adaptateur* est inclus. Ce paramètre désactive TCP/IP pour les adaptateurs configurés pour obtenir automatiquement une adresse IP. Pour spécifier un nom d’adaptateur, tapez le nom de la carte qui s’affiche lorsque vous utilisez **ipconfig** sans paramètres.|
|/Release6 [> de l’adaptateur\<]|Envoie un message DHCPRELEASE au serveur DHCPv6 pour libérer la configuration DHCP actuelle et ignorer la configuration de l’adresse IPv6 pour tous les adaptateurs (si aucune carte n’est spécifiée) ou pour une carte spécifique si le paramètre de la *carte* est inclus. Ce paramètre désactive TCP/IP pour les adaptateurs configurés pour obtenir automatiquement une adresse IP. Pour spécifier un nom d’adaptateur, tapez le nom de la carte qui s’affiche lorsque vous utilisez **ipconfig** sans paramètres.|
|/renew [\<> de l’adaptateur]|Renouvelle la configuration DHCP pour tous les adaptateurs (si aucune carte n’est spécifiée) ou pour une carte spécifique si le paramètre de la *carte* est inclus. Ce paramètre est disponible uniquement sur les ordinateurs dotés d’adaptateurs configurés pour obtenir automatiquement une adresse IP. Pour spécifier un nom d’adaptateur, tapez le nom de la carte qui s’affiche lorsque vous utilisez **ipconfig** sans paramètres.|
|/renew6 [> de l’adaptateur\<]|Renouvelle la configuration DHCPv6 pour tous les adaptateurs (si aucune carte n’est spécifiée) ou pour une carte spécifique si le paramètre de l' *adaptateur* est inclus. Ce paramètre est disponible uniquement sur les ordinateurs dotés d’adaptateurs configurés pour obtenir automatiquement une adresse IPv6. Pour spécifier un nom d’adaptateur, tapez le nom de la carte qui s’affiche lorsque vous utilisez **ipconfig** sans paramètres.|
|/setclassid \<de l’adaptateur > [<ClassID>]|Configure l’ID de classe DHCP pour un adaptateur spécifié. Pour définir l’ID de classe DHCP pour tous les adaptateurs, utilisez le **&#42;** caractère générique astérisque () à la place de l' *adaptateur*. Ce paramètre est disponible uniquement sur les ordinateurs dotés d’adaptateurs configurés pour obtenir automatiquement une adresse IP. Si aucun ID de classe DHCP n’est spécifié, l’ID de classe actuel est supprimé.|
|Adaptateur \</showclassid >|Affiche l’ID de classe DHCP pour un adaptateur spécifié. Pour afficher l’ID de classe DHCP pour tous les adaptateurs, utilisez le **&#42;** caractère générique astérisque () à la place de l' *adaptateur*. Ce paramètre est disponible uniquement sur les ordinateurs dotés d’adaptateurs configurés pour obtenir automatiquement une adresse IP.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

- Cette commande est particulièrement utile sur les ordinateurs qui sont configurés pour obtenir automatiquement une adresse IP. Cela permet aux utilisateurs de déterminer les valeurs de configuration TCP/IP qui ont été configurées par DHCP, l’adressage IP privé automatique (APIPA) ou une autre configuration.
- Si le nom que vous fournissez pour l' *adaptateur* contient des espaces, utilisez des guillemets autour du nom de l’adaptateur (par exemple : * * * * nom de l'<em>adaptateur</em>* * * *).
- Pour les noms d’adaptateurs, **ipconfig** prend en charge l’utilisation du caractère générique astérisque (\*) pour spécifier soit les adaptateurs dont le nom commence par une chaîne spécifiée, soit les adaptateurs dont le nom contient une chaîne spécifiée. Par exemple, la **\*locale** correspond à tous les adaptateurs qui commencent par la chaîne Local et **\*con\*** correspond à tous les adaptateurs contenant la chaîne con.

## <a name="examples"></a>Exemples

Pour afficher la configuration TCP/IP de base pour tous les adaptateurs, tapez :
```
ipconfig
```
Pour afficher la configuration TCP/IP complète pour tous les adaptateurs, tapez :
```
ipconfig /all
```
Pour renouveler une configuration d’adresse IP attribuée par DHCP uniquement pour l’adaptateur de connexion au réseau local, tapez :
```
ipconfig /renew Local Area Connection
```
Pour vider le cache de résolution DNS lors de la résolution des problèmes de résolution de noms DNS, tapez :
```
ipconfig /flushdns
```
Pour afficher l’ID de classe DHCP pour tous les adaptateurs dont le nom commence par local, tapez :
```
ipconfig /showclassid Local*
```
Pour définir l’ID de classe DHCP pour l’adaptateur de connexion au réseau local en TEST, tapez :
```
ipconfig /setclassid Local Area Connection TEST
```

## <a name="additional-references"></a>Références supplémentaires

-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
