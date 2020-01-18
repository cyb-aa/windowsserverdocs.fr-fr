---
title: Désactiver la mise en cache côté client DNS sur les clients DNS
description: Cet article explique comment désactiver la mise en cache côté client DNS sur les clients DNS.
manager: dcscontentpm
ms.prod: ''
ms.technology: networking-dns
ms.topic: article
ms.author: delhan
ms.date: 8/8/2019
author: Deland-Han
ms.openlocfilehash: 51a9dbfd05402a9d018aec3bfea8a5c89e9e5d5e
ms.sourcegitcommit: c5709021aa98abd075d7a8f912d4fd2263db8803
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/18/2020
ms.locfileid: "76265841"
---
# <a name="disable-dns-client-side-caching-on-dns-clients"></a>Désactiver la mise en cache côté client DNS sur les clients DNS

Windows contient un cache DNS côté client. La fonctionnalité de mise en cache DNS côté client peut générer une fausse impression que l’équilibrage de charge DNS « tourniquet » ne se produit pas du serveur DNS vers l’ordinateur client Windows. Lorsque vous utilisez la commande ping pour rechercher le même nom de domaine A-record, le client peut utiliser la même adresse IP.  

## <a name="how-to-disable-client-side-caching"></a>Comment désactiver la mise en cache côté client

Pour arrêter la mise en cache DNS, exécutez l’une des commandes suivantes :

```cmd
net stop dnscache
```

```cmd
sc servername stop dnscache
```


Pour désactiver le cache DNS de manière permanente dans Windows, utilisez l’outil contrôleur de services ou l’outil services pour définir le type de démarrage du service client DNS sur **désactivé**. Notez que le nom du service client DNS Windows peut également apparaître sous la forme « dnscache ». 

> [!NOTE]
> Si le cache de résolution DNS est désactivé, les performances globales de l’ordinateur client diminuent et le trafic réseau pour les requêtes DNS augmente. 

Le service client DNS optimise les performances de la résolution de noms DNS en stockant les noms précédemment résolus dans la mémoire. Si le service client DNS est désactivé, l’ordinateur peut toujours résoudre les noms DNS à l’aide des serveurs DNS du réseau. 

Lorsque le programme de résolution Windows reçoit une réponse, qu’il soit positif ou négatif, à une requête, il ajoute cette réponse à son cache et crée donc un enregistrement de ressource DNS. Le programme de résolution vérifie toujours le cache avant d’interroger un serveur DNS. Si un enregistrement de ressource DNS se trouve dans le cache, le programme de résolution utilise l’enregistrement à partir du cache au lieu d’interroger un serveur. Ce comportement accélère les requêtes et réduit le trafic réseau pour les requêtes DNS. 

Vous pouvez utiliser l’outil ipconfig pour afficher et vider le cache de résolution DNS. Pour afficher le cache de résolution DNS, exécutez la commande suivante à l’invite de commandes :

```cmd
ipconfig /displaydns 
```

Cette commande affiche le contenu du cache de résolution DNS, y compris les enregistrements de ressources DNS qui sont préchargés à partir du fichier hosts et tous les noms récemment interrogés qui ont été résolus par le système. Après un certain temps, le programme de résolution ignore l’enregistrement du cache. La période est spécifiée par la valeur **de durée de vie (TTL)** associée à l’enregistrement de ressource DNS. Vous pouvez également vider le cache manuellement. Une fois le cache vidé, l’ordinateur doit interroger à nouveau les serveurs DNS pour obtenir les enregistrements de ressources DNS qui ont été précédemment résolus par l’ordinateur. Pour supprimer les entrées dans le cache de résolution DNS, exécutez `ipconfig /flushdns` à partir d’une invite de commandes.

## <a name="using-the-registry-to-control-the-caching-time"></a>Utilisation du Registre pour contrôler la durée de mise en cache

> [!IMPORTANT]  
> Suivez attentivement les étapes décrites dans cette section. De graves problèmes peuvent se produire si vous modifiez le registre de façon incorrecte. Avant de le modifier, [sauvegardez le Registre afin de pouvoir le restaurer](https://support.microsoft.com/help/322756) en cas de problème.

La durée pendant laquelle une réponse positive ou négative est mise en cache dépend des valeurs des entrées de la clé de Registre suivante :

**HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Services\DNSCache\Parameters**

La durée de vie des réponses positives est la plus petite des valeurs suivantes : 

- Nombre de secondes spécifié dans la réponse à la requête reçue par le programme de résolution

- Valeur du paramètre de Registre **MaxCacheTtl** .

>[!Note]
>- La durée de vie par défaut pour les réponses positives est de 86 400 secondes (1 jour).
>- La durée de vie pour les réponses négatives est le nombre de secondes spécifié dans le paramètre de Registre MaxNegativeCacheTtl.
>- La durée de vie par défaut pour les réponses négatives est de 900 secondes (15 minutes).
Si vous ne souhaitez pas que les réponses négatives soient mises en cache, définissez le paramètre de Registre MaxNegativeCacheTtl sur 0.

Pour définir l’heure de mise en cache sur un ordinateur client :

1. Démarrez l’éditeur du Registre (regedit. exe).

2. Recherchez, puis cliquez sur la clé suivante dans le registre :

   **HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Services\Dnscache\Parameters**

3. Dans le menu Edition, pointez sur nouveau, cliquez sur valeur DWORD, puis ajoutez les valeurs de Registre suivantes :

   - Nom de la valeur : MaxCacheTtl

     Type de données : REG_DWORD

     Données de la valeur : valeur par défaut 86400 secondes. 
     
     Si vous réduisez la valeur de durée de vie maximale dans le cache DNS du client à 1 seconde, cela donne l’impression que le cache DNS côté client a été désactivé.    

   - Nom de la valeur : MaxNegativeCacheTtl

     Type de données : REG_DWORD

     Données de la valeur : valeur par défaut 900 secondes. 
     
     Définissez la valeur sur 0 si vous ne souhaitez pas que les réponses négatives soient mises en cache.

4. Tapez la valeur que vous souhaitez utiliser, puis cliquez sur OK.

5. Quittez l'Éditeur du Registre.