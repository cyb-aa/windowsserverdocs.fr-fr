---
title: Résolution des problèmes liés au DNS (Domain Name System)
description: Cet article explique comment collecter des données lorsque des problèmes DNS se produisent.
manager: dcscontentpm
ms.technology: networking-dns
ms.topic: article
ms.author: delhan
ms.date: 8/8/2019
author: Deland-Han
ms.openlocfilehash: 04bfbfbd2957aec21da966a48feca8d7f160a29c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860042"
---
# <a name="troubleshooting-domain-name-system-dns-issues"></a>Résolution des problèmes liés au DNS (Domain Name System)
 
Les problèmes de résolution de noms de domaine peuvent être décomposés en problèmes côté client et côté serveur. En général, vous devez commencer par la résolution des problèmes côté client, sauf si vous déterminez pendant la phase d’étendue que le problème se produit sur le côté serveur.

- [Résolution des problèmes des clients DNS](troubleshoot-dns-client.md)

- [Dépannage des serveurs DNS](troubleshoot-dns-server.md)
 
## <a name="data-collection"></a>Collecte de données
 
Nous vous recommandons de collecter simultanément des données sur les côtés client et serveur lorsque le problème se produit. Toutefois, selon le problème réel, vous pouvez démarrer votre collection sur un seul jeu de données sur le client DNS ou le serveur DNS.
 
Pour collecter un diagnostic réseau Windows à partir d’un client affecté et de son serveur DNS configuré, procédez comme suit :

1. Démarrer des captures réseau sur le client et le serveur :

   ```cmd
   netsh trace start capture=yes tracefile=c:\%computername%_nettrace.etl
   ```

2. Effacez le cache DNS sur le client DNS en exécutant la commande suivante :

   ```cmd
   ipconfig /flushdns
   ```

3. Reproduisez le problème.

4. Arrêter et enregistrer des traces :

   ```cmd
   netsh trace stop
   ```

5. Enregistrez les fichiers NetTrace. cab à partir de chaque ordinateur. Ces informations sont utiles lorsque vous contactez Support Microsoft.