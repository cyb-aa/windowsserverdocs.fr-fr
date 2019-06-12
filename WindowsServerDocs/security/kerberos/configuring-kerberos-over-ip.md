---
title: Configuration de Kerberos pour l’adresse IP
description: Prise en charge de Kerberos pour les noms principaux de service basé sur IP
ms.openlocfilehash: 30741f7a0f1978fcaa6ac83c98a54c07e1ef25c5
ms.sourcegitcommit: c6acac3622e5d34714ca5c569805931681f98779
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66391520"
---
# <a name="kerberos-clients-allow-ipv4-and-ipv6-address-hostnames-in-service-principal-names-spns"></a>Les clients Kerberos autorisent des noms d’hôte des adresses IPv4 et IPv6 dans les noms de principal du Service (SPN)

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Depuis Windows 10 version 1507 et Windows Server 2016, les clients Kerberos peuvent être configurés pour prendre en charge des noms d’hôtes IPv4 et IPv6 dans les noms principaux de service.

Par défaut Windows ne tente pas de l’authentification Kerberos pour un ordinateur hôte si le nom d’hôte est une adresse IP. Il revient à d’autres protocoles d’authentification activées comme NTLM. Toutefois, les applications sont parfois codé en dur pour utiliser des adresses IP et ce qui signifie que l’application sera repasse en NTLM et n’utilisez pas Kerberos. Cela peut entraîner des problèmes de compatibilité que déplacement des environnements de désactiver NTLM.

Pour réduire l’impact de la désactivation de NTLM, une nouvelle fonctionnalité introduite qui permet aux administrateurs d’utiliser des adresses IP en tant que noms d’hôte dans les noms de principal du Service. Cette fonctionnalité est activée sur le client via une valeur de clé de Registre.

```cmd
reg add "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System\Kerberos\Parameters" /v TryIPSPN /t REG_DWORD /d 1 /f
```

Pour configurer la prise en charge des noms d’hôte des adresses IP dans les noms principaux de service, créez une entrée TryIPSPN. Par défaut, cette entrée n’existe pas dans le Registre. Une fois que vous avez créé l’entrée, remplacez la valeur DWORD par 1. Cette valeur de Registre sera doivent être définis sur chaque ordinateur client qui doit accéder aux ressources protégées par Kerberos par adresse IP.

## <a name="configuring-a-service-principal-name-as-ip-address"></a>Configuration d’un nom de principal du Service en tant qu’adresse IP

Un nom de principal du Service est un identificateur unique permettant d’identifier un service sur le réseau lors de l’authentification Kerberos. Un SPN est composé d’un service, nom d’hôte et, éventuellement, un port sous forme de `service/hostname[:port]` comme `host/fs.contoso.com`. Windows enregistre plusieurs noms principaux de service à un objet ordinateur lorsqu’un ordinateur est joint à Active Directory.

Adresses IP ne sont pas normalement utilisés à la place des noms d’hôte, car les adresses IP sont souvent temporaires. Cela peut entraîner les conflits et les échecs d’authentification comme des baux d’adresses expire et renouveler. Par conséquent, l’inscription d’un SPN de fondées sur l’adresse IP est un processus manuel et ne doit être utilisée que lorsqu’il est impossible de basculer vers un nom d’hôte basée sur DNS.

L’approche recommandée consiste à utiliser le [Setspn.exe](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/cc731241(v=ws.11)) outil. Notez qu’un SPN ne peut être inscrit à un seul compte dans Active Directory à la fois et il est donc recommandé que les adresses IP ont des baux statiques si DHCP est utilisé.

```
Setspn -s <service>/ip.address> <domain-user-account>  
```

Exemple :

```
Setspn -s host/192.168.1.1 server01
```
