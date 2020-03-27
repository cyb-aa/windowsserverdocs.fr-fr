---
title: ÉTAPE 5 Testez la connectivité DirectAccess à partir d’Internet et du cluster
description: Cette rubrique fait partie du Guide de laboratoire de test-démonstration de DirectAccess dans un cluster avec Windows NLB pour Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8399bdfa-809a-45e4-9963-f9b6a631007f
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 92641ccf19f77becd9ed5476cd8c0178f4090f49
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80310837"
---
# <a name="step-5-test-directaccess-connectivity-from-the-internet-and-through-the-cluster"></a>ÉTAPE 5 Testez la connectivité DirectAccess à partir d’Internet et du cluster

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

CLIENT1 est maintenant prêt pour le test de DirectAccess.  
  
- Tester la connectivité DirectAccess à partir d’Internet. Connectez CLIENT1 à l’Internet simulé. Lorsqu’il est connecté à Internet simulé, les adresses IPv4 publiques sont attribuées au client. Lorsqu’un client DirectAccess se voit attribuer une adresse IPv4 publique, il tente d’établir une connexion au serveur d’accès à distance à l’aide d’une technologie de transition IPv6.  
  
- Tester la connectivité du client DirectAccess via le cluster. Tester la fonctionnalité de cluster. Avant de commencer le test, nous vous recommandons d’arrêter à la fois EDGE1 et EDGE2 pendant au moins cinq minutes. Il existe plusieurs raisons à cela, notamment les délais d’expiration du cache ARP et les modifications liées à l’équilibrage de la charge réseau. Lors de la validation de la configuration NLB dans un laboratoire de test, vous devez être patient, car les modifications apportées à la configuration ne seront pas immédiatement reflétées dans la connectivité jusqu’à ce qu’un laps de temps se soit écoulé. Il est important de tenir compte de ce point lorsque vous effectuez les tâches suivantes.  
  
    > [!TIP]  
    > Nous vous recommandons d’effacer le cache d’Internet Explorer avant d’effectuer cette procédure et chaque fois que vous testez la connexion via un autre serveur d’accès à distance pour vous assurer que vous testez la connexion et que vous ne récupérez pas les pages Web à partir du cache.  
  
## <a name="test-directaccess-connectivity-from-the-internet"></a>Tester la connectivité DirectAccess à partir d’Internet  
  
1. Débranchez CLIENT1 du commutateur corpnet et connectez-le au commutateur Internet. Attendez 30 secondes.  
  
2. Dans une fenêtre Windows PowerShell avec élévation de privilèges, tapez **ipconfig/flushdns** et appuyez sur entrée. Cette opération vide les entrées de résolution de noms qui peuvent encore exister dans le cache DNS du client à partir de lorsque l’ordinateur client a été connecté au Corpnet.  
  
3. Dans la fenêtre Windows PowerShell, tapez **« DnsClientNrptPolicy »** et appuyez sur entrée.  
  
   La sortie indique les paramètres actifs pour la table de stratégie de résolution de noms. Ces paramètres indiquent que toutes les connexions à. corp.contoso.com doivent être résolues par le serveur DNS d’accès à distance, avec l’adresse IPv6 2001 : DB8:1 :: 2. Notez également l'entrée de la table de stratégie de résolution de noms indiquant une exemption pour le nom nls.corp.contoso.com ; le serveur DNS d'accès à distance ne répond pas aux noms qui figurent sur la liste des exemptions. Vous pouvez exécuter une commande ping sur l’adresse IP du serveur DNS d’accès à distance pour confirmer la connectivité au serveur d’accès à distance ; par exemple, vous pouvez exécuter une commande ping sur 2001 : DB8:1 :: 2.  
  
4. Dans la fenêtre Windows PowerShell, tapez « **ping App1** » et appuyez sur entrée. Vous devez voir les réponses de l’adresse IPv6 pour APP1, qui dans ce cas est 2001 : DB8:1 :: 3.  
  
5. Dans la fenêtre Windows PowerShell, tapez **ping App2** et appuyez sur entrée. Vous devez voir des réponses à partir de l'adresse NAT64 affectée par EDGE1 à APP2, qui est ici fdc9:9f4e:eb1b:7777::a00:4.  
  
   La possibilité d’exécuter une commande ping APP2 est importante, car la réussite indique que vous avez pu établir une connexion à l’aide de NAT64/DNS64, car APP2 est une ressource IPv4 uniquement.  
  
6. Laissez la fenêtre Windows PowerShell ouverte pour la procédure suivante.  
  
7. Ouvrez Internet Explorer, dans la barre d’adresses d’Internet Explorer, entrez **https://app1/** , puis appuyez sur entrée. Vous allez voir le site web IIS par défaut sur APP1.  
  
8. Dans la barre d’adresses d’Internet Explorer, entrez **https://app2/** , puis appuyez sur entrée. Vous allez voir le site web par défaut sur APP2.  
  
9. Dans l’écran d' **Accueil** , tapez<strong>\\\App2\Files</strong>, puis appuyez sur entrée. Double-cliquez sur le fichier Nouveau document texte.  
  
    Cela démontre que vous avez pu vous connecter à un serveur IPv4 uniquement à l’aide de SMB pour obtenir une ressource dans le domaine de ressources.  
  
10. Dans l’écran d' **Accueil** , tapez**WF. msc**, puis appuyez sur entrée.  
  
11. Dans la console **pare-feu Windows avec fonctions avancées de sécurité** , Notez que seul le profil **privé** ou **public** est actif. Le pare-feu Windows doit être activé pour que DirectAccess fonctionne correctement. Si le pare-feu Windows est désactivé, la connectivité DirectAccess ne fonctionne pas.  
  
12. Dans le volet gauche de la console, développez le nœud **analyse** , puis cliquez sur le nœud **règles de sécurité de connexion** . Vous devez voir les règles de sécurité de connexion actives : **Stratégie DirectAccess-ClientToCorp**, **Stratégie DirectAccess-ClientToDNS64NAT64PrefixExemption**, **Stratégie DirectAccess-ClientToInfra**et **Stratégie DirectAccess-ClientToNlaExempt**. Faites défiler le volet central vers la droite pour afficher les **premières méthodes d’authentification** et les colonnes de la **2e méthode d’authentification** . Notez que la première règle (ClientToCorp) utilise Kerberos V5 pour établir le tunnel intranet et que la troisième règle (ClientToInfra) utilise NTLMv2 pour établir le tunnel d’infrastructure.  
  
13. Dans le volet gauche de la console, développez le nœud **associations de sécurité** , puis cliquez sur le nœud **mode principal** . Notez les associations de sécurité de tunnel d’infrastructure utilisant NTLMv2 et l’Association de sécurité de tunnel intranet à l’aide de Kerberos V5. Cliquez avec le bouton droit sur l’entrée qui affiche **utilisateur (Kerberos V5)** comme **deuxième méthode d’authentification** , puis cliquez sur **Propriétés**. Sous l’onglet **général** , Notez que le **deuxième ID local d’authentification** est **corp\user1.** , ce qui indique que User1 a pu s’authentifier auprès du domaine Corp à l’aide de Kerberos.  
  
## <a name="test-directaccess-client-connectivity-through-the-cluster"></a>Tester la connectivité du client DirectAccess par le biais du cluster  
  
1. Effectuez un arrêt approprié sur EDGE2.  
  
   Vous pouvez utiliser le gestionnaire d’équilibrage de la charge réseau pour afficher l’état des serveurs lors de l’exécution de ces tests.  
  
2. Sur CLIENT1, dans la fenêtre Windows PowerShell, tapez **ipconfig/flushdns** et appuyez sur entrée. Cette opération vide les entrées de résolution de noms qui peuvent encore exister dans le cache DNS du client.  
  
3. Dans la fenêtre Windows PowerShell, exécutez la commande ping APP1 et APP2. Vous devez recevoir des réponses de ces deux ressources.  
  
4. Dans l’écran d' **Accueil** , tapez<strong>\\\app2\files</strong>. Vous devez voir le dossier partagé sur l’ordinateur APP2. La possibilité d’ouvrir le partage de fichiers sur APP2 indique que le deuxième tunnel, qui requiert l’authentification Kerberos pour l’utilisateur, fonctionne correctement.  
  
5. Ouvrez Internet Explorer, puis ouvrez le site Web https://app1/ et https://app2/. La possibilité d’ouvrir les deux sites Web confirme que le premier et le deuxième tunnel sont opérationnels. Fermez Internet Explorer.  
  
6. Démarrez l’ordinateur EDGE2.  
  
7. Sur EDGE1, effectuez un arrêt approprié.  
  
8. Attendez 5 minutes, puis revenez à CLIENT1. Effectuez les étapes 2-5. Cela confirme que CLIENT1 a pu basculer en toute transparence vers EDGE2 une fois que EDGE1 n’est plus disponible.
