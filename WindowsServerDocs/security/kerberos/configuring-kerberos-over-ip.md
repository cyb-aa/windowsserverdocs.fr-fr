---
title: Configuration de Kerberos pour l’adresse IP
description: Prise en charge Kerberos pour les noms principaux de service basés sur IP
author: daveba
ms.author: daveba
ms.openlocfilehash: 1061364528100fe005e80f64c6315f9fca69ad98
ms.sourcegitcommit: 2082335e1260826fcbc3dccc208870d2d9be9306
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69980300"
---
# <a name="kerberos-clients-allow-ipv4-and-ipv6-address-hostnames-in-service-principal-names-spns"></a>Les clients Kerberos autorisent les noms d’hôte des adresses IPv4 et IPv6 dans les noms de principal du service (SPN)

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2016

À compter de Windows 10 version 1507 et Windows Server 2016, les clients Kerberos peuvent être configurés pour prendre en charge les noms d’hôte IPv4 et IPv6 dans les SPN.

Par défaut, Windows ne tente pas l’authentification Kerberos pour un ordinateur hôte si le nom d’hôte est une adresse IP. Elle revient à d’autres protocoles d’authentification activés, comme NTLM. Toutefois, les applications sont parfois codées en dur pour utiliser des adresses IP, ce qui signifie que l’application revient à NTLM et n’utilise pas Kerberos. Cela peut entraîner des problèmes de compatibilité à mesure que les environnements se déplacent pour désactiver NTLM.

Pour réduire l’impact de la désactivation de NTLM, une nouvelle fonctionnalité qui permet aux administrateurs d’utiliser des adresses IP en tant que noms d’hôtes dans les noms principaux de service a été introduite. Cette fonctionnalité est activée sur le client par le biais d’une valeur de clé de registre.

```cmd
reg add "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System\Kerberos\Parameters" /v TryIPSPN /t REG_DWORD /d 1 /f
```

Pour configurer la prise en charge des noms d’hôtes d’adresse IP dans les noms de principal du service, créez une entrée TryIPSPN. Par défaut, cette entrée n’existe pas dans le Registre. Après avoir créé l’entrée, remplacez la valeur DWORD par 1. Cette valeur de registre doit être définie sur chaque ordinateur client qui doit accéder aux ressources protégées par Kerberos par adresse IP.

## <a name="configuring-a-service-principal-name-as-ip-address"></a>Configuration d’un nom de principal du service en tant qu’adresse IP

Un nom principal de service est un identificateur unique utilisé lors de l’authentification Kerberos pour identifier un service sur le réseau. Un SPN est composé d’un service, d’un nom d’hôte et éventuellement d’un port `service/hostname[:port]` sous forme `host/fs.contoso.com`de. Windows inscrira plusieurs noms de principal du service (SPN) sur un objet ordinateur lorsqu’un ordinateur sera joint à Active Directory.

Les adresses IP ne sont généralement pas utilisées à la place des noms d’hôtes, car les adresses IP sont souvent temporaires. Cela peut entraîner des conflits et des échecs d’authentification au fur et à mesure de l’expiration et du renouvellement des baux d’adresses. Par conséquent, l’inscription d’un SPN basé sur une adresse IP est un processus manuel qui ne doit être utilisé qu’en cas d’impossibilité de basculer vers un nom d’hôte basé sur DNS.

L’approche recommandée consiste à utiliser l’outil [Setspn. exe](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/cc731241(v=ws.11)) . Notez qu’un SPN ne peut être inscrit qu’à un seul compte dans Active Directory à la fois. il est donc recommandé que les adresses IP aient des baux statiques si le protocole DHCP est utilisé.

```
Setspn -s <service>/ip.address> <domain-user-account>  
```

Exemple :

```
Setspn -s host/192.168.1.1 server01
```
