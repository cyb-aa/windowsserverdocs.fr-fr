---
title: Désactiver la mise en cache côté client DNS sur les clients DNS
description: Cet article explique comment désactiver la mise en cache côté client DNS sur les clients DNS.
manager: willchen
ms.prod: ''
ms.technology: networking-dns
ms.topic: article
ms.author: delhan
ms.date: 8/8/2019
author: Deland-Han
ms.openlocfilehash: 0b20400029b462798587c2291431b5a7c3d61775
ms.sourcegitcommit: 0e3c2473a54f915d35687d30d1b4b1ac2bae4068
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68917806"
---
# <a name="disable-dns-client-side-caching-on-dns-clients"></a>Désactiver la mise en cache côté client DNS sur les clients DNS

Windows contient un cache DNS côté client. La fonctionnalité de mise en cache DNS côté client peut générer une fausse impression que l’équilibrage de charge DNS «tourniquet» ne se produit pas du serveur DNS vers l’ordinateur client Windows. Lorsque vous utilisez la commande ping pour rechercher le même nom de domaine A-record, le client peut utiliser la même adresse IP.  

## <a name="how-to-disable-client-side-caching"></a>Comment désactiver la mise en cache côté client

Pour arrêter la mise en cache DNS, exécutez l’une des commandes suivantes:

```cmd
net stop dnscache
```

```cmd
sc servername stop dnscache
```


Pour désactiver le cache DNS de manière permanente dans Windows, utilisez l’outil contrôleur de services ou l’outil services pour définir le type de démarrage du service client DNS sur **désactivé**. Notez que le nom du service client DNS Windows peut également apparaître sous la forme «dnscache». 

> [!NOTE]
> Si le cache de résolution DNS est désactivé, les performances globales de l’ordinateur client diminuent et le trafic réseau pour les requêtes DNS augmente. 

Le service client DNS optimise les performances de la résolution de noms DNS en stockant les noms précédemment résolus dans la mémoire. Si le service client DNS est désactivé, l’ordinateur peut toujours résoudre les noms DNS à l’aide des serveurs DNS du réseau. 

Lorsque le programme de résolution Windows reçoit une réponse, qu’il soit positif ou négatif, à une requête, il ajoute cette réponse à son cache et crée donc un enregistrement de ressource DNS. Le programme de résolution vérifie toujours le cache avant d’interroger un serveur DNS. Si un enregistrement de ressource DNS se trouve dans le cache, le programme de résolution utilise l’enregistrement à partir du cache au lieu d’interroger un serveur. Ce comportement accélère les requêtes et réduit le trafic réseau pour les requêtes DNS. 

Vous pouvez utiliser l’outil ipconfig pour afficher et vider le cache de résolution DNS. Pour afficher le cache de résolution DNS, exécutez la commande suivante à l’invite de commandes:

```cmd
ipconfig /displaydns 
```

Cette commande affiche le contenu du cache de résolution DNS, y compris les enregistrements de ressources DNS qui sont préchargés à partir du fichier hosts et tous les noms récemment interrogés qui ont été résolus par le système. Après un certain temps, le programme de résolution ignore l’enregistrement du cache. La période est spécifiée par la valeur **de durée de vie (TTL)** associée à l’enregistrement de ressource DNS. Vous pouvez également vider le cache manuellement. Une fois le cache vidé, l’ordinateur doit interroger à nouveau les serveurs DNS pour obtenir les enregistrements de ressources DNS qui ont été précédemment résolus par l’ordinateur. Pour supprimer les entrées dans le cache de résolution DNS, `ipconfig /flushdns` exécutez à partir d’une invite de commandes.

## <a name="next-step"></a>Étape suivante

Pour plus d’informations, consultez Désactivation de [la mise en cache DNS côté client dans Windows](https://support.microsoft.com/kb/318803) .
