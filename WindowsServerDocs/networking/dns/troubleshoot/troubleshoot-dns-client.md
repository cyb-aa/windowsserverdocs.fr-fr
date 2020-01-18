---
title: Résolution des problèmes des clients DNS
description: Cet article explique comment résoudre les problèmes DNS à partir du côté client.
manager: dcscontentpm
ms.prod: ''
ms.technology: networking-dns
ms.topic: article
ms.author: delhan
ms.date: 8/8/2019
author: Deland-Han
ms.openlocfilehash: dd34fae73cdcb20a896750e20d4a28f8777a378a
ms.sourcegitcommit: c5709021aa98abd075d7a8f912d4fd2263db8803
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/18/2020
ms.locfileid: "76265771"
---
# <a name="troubleshooting-dns-clients"></a>Résolution des problèmes des clients DNS

Cet article explique comment résoudre les problèmes des clients DNS.

## <a name="check-ip-configuration"></a>Vérifier la configuration IP

1. Ouvrez une fenêtre d’invite de commandes en tant qu’administrateur sur l’ordinateur client.

2. Exécutez la commande suivante :

   ```cmd
   ipconfig /all
   ```

3. Vérifiez que le client dispose d’une adresse IP, d’un masque de sous-réseau et d’une passerelle par défaut valides pour le réseau auquel il est attaché et utilisé.

4. Vérifiez les serveurs DNS répertoriés dans la sortie et vérifiez que les adresses IP répertoriées sont correctes.

5. Vérifiez le suffixe DNS spécifique à la connexion dans la sortie et assurez-vous qu’il est correct.

Si le client n’a pas de configuration TCP/IP valide, utilisez l’une des méthodes suivantes :

* Pour les clients configurés de manière dynamique, utilisez la commande `ipconfig /renew` pour forcer manuellement le client à renouveler sa configuration d’adresse IP avec le serveur DHCP.

* Pour les clients configurés de manière statique, modifiez les propriétés TCP/IP du client pour utiliser des paramètres de configuration valides ou terminez sa configuration DNS pour le réseau.

## <a name="check-network-connection"></a>Vérifier la connexion réseau

### <a name="ping-test"></a>Test ping

Vérifiez que le client peut contacter un serveur DNS préféré (ou autre) en exécutant une commande ping sur le serveur DNS préféré par son adresse IP.

Par exemple, si le client utilise un serveur DNS préféré de 10.0.0.1, exécutez la commande suivante à l’invite de commandes :

```cmd
ping 10.0.0.1
```

Si aucun serveur DNS configuré ne répond à une commande ping directe de son adresse IP, cela indique que la source du problème est plus probable que la connectivité réseau entre le client et les serveurs DNS. Si c’est le cas, suivez les étapes de dépannage réseau TCP/IP de base pour résoudre le problème. Gardez à l’esprit que le trafic ICMP doit être autorisé via le pare-feu pour que la commande ping fonctionne.

### <a name="dns-query-tests"></a>Tests de requête DNS

Si le client DNS peut effectuer un test ping sur l’ordinateur serveur DNS, essayez d’utiliser les commandes de `nslookup` suivantes pour déterminer si le serveur peut répondre aux clients DNS. Étant donné que nslookup n’utilise pas le cache DNS du client, la résolution de noms utilisera le serveur DNS configuré du client.

#### <a name="test-a-client"></a>Tester un client

```cmd
nslookup <client>
```
  
Par exemple, si l’ordinateur client est appelé **CLIENT1**, exécutez la commande suivante :
  
```cmd
nslookup client1
```
  
Si aucune réponse correcte n’est retournée, essayez d’exécuter la commande suivante :
  
```cmd
nslookup <fqdn of client>
```
  
Par exemple, si le nom de domaine complet est **client1.Corp.contoso.com**, exécutez la commande suivante :

```cmd
nslookup client1.corp.contoso.com.
```

> [!NOTE]
> Vous devez inclure la période de fin lorsque vous exécutez ce test.

Si Windows trouve le nom de domaine complet, mais ne trouve pas le nom abrégé, vérifiez la configuration du suffixe DNS sous l’onglet DNS des paramètres TCP/IP avancés de la carte réseau. Pour plus d’informations, consultez Configuration de la [résolution DNS](https://docs.microsoft.com/previous-versions/tn-archive/dd163570(v=technet.10)#configuring-dns-resolution).

#### <a name="test-the-dns-server"></a>Tester le serveur DNS

```cmd
nslookup <DNS Server>
```

Par exemple, si le serveur DNS est nommé DC1, exécutez la commande suivante :

```cmd
nslookup dc1
```
Si les tests précédents ont réussi, ce test doit également réussir. Si ce test échoue, vérifiez la connectivité au serveur DNS.

#### <a name="test-the-failing-record"></a>Tester l’enregistrement défaillant

```cmd
nslookup <failed internal record>
```

Par exemple, si l’enregistrement défaillant était **App1.Corp.contoso.com**, exécutez la commande suivante :

```cmd
nslookup app1.corp.contoso.com
```

#### <a name="test-a-public-internet-address"></a>Tester une adresse Internet publique

```cmd
nslookup <external name>
```

Exemple : 
```cmd
nslookup bing.com
```

Si les quatre tests ont réussi, exécutez `ipconfig /displaydns` et vérifiez la sortie du nom qui a échoué. Si vous voyez « le nom n’existe pas » sous le nom d’échec, une réponse négative a été retournée à partir d’un serveur DNS et a été mise en cache sur le client. 

Pour résoudre le problème, effacez le cache en exécutant `ipconfig /flushdns`.

## <a name="next-step"></a>Étape suivante

Si la résolution de noms échoue toujours, accédez à la section [Dépannage des serveurs DNS](troubleshoot-dns-server.md) .
