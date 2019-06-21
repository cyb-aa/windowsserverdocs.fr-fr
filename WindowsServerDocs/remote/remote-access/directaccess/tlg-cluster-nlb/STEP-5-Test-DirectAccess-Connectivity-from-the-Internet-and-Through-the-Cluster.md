---
title: ÉTAPE 5 tester la connectivité DirectAccess à partir d’Internet et par le biais du Cluster
description: Cette rubrique fait partie du Guide de laboratoire de Test - décrire de DirectAccess dans un Cluster avec équilibrage de charge réseau Windows pour Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8399bdfa-809a-45e4-9963-f9b6a631007f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 23aed915edb827fd0cd61e6778167108647269ea
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283341"
---
# <a name="step-5-test-directaccess-connectivity-from-the-internet-and-through-the-cluster"></a>ÉTAPE 5 tester la connectivité DirectAccess à partir d’Internet et par le biais du Cluster

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

CLIENT1 est maintenant prêt pour le test de DirectAccess.  
  
- Tester la connectivité DirectAccess à partir d’Internet. Connectez CLIENT1 à l’Internet simulé. Lorsque connecté à l’Internet simulé, le client est attribué des adresses IPv4 publiques. Lorsqu’un client DirectAccess est affecté à une adresse IPv4 publique, il essaie d’établir une connexion au serveur d’accès à distance à l’aide d’une technologie de transition IPv6.  
  
- Tester la connectivité des clients DirectAccess sur le cluster. Tester la fonctionnalité de cluster. Avant de commencer les tests, nous recommandons d’arrêter EDGE1 et EDGE2 pendant au moins cinq minutes. Il existe plusieurs raisons à cela, qui incluent des délais d’expiration du cache ARP et modifications liées à NLB. Lors de la validation de la configuration d’équilibrage de charge réseau dans un laboratoire de test, vous devrez être patient, comme les modifications de configuration seront immédiatement répercutées dans connectivité jusqu'à ce qu’après qu’un laps de temps écoulé. Ceci est important à prendre en compte lorsque vous exécutez les tâches suivantes.  
  
    > [!TIP]  
    > Nous recommandons que vous désactivez le cache Internet Explorer avant d’effectuer cette procédure chaque fois que vous testez la connexion via un autre serveur d’accès à distance pour vous assurer que vous êtes le test de la connexion et ne récupérant ne pas les pages Web à partir du cache.  
  
## <a name="test-directaccess-connectivity-from-the-internet"></a>Tester la connectivité DirectAccess à partir d’Internet  
  
1. Débranchez CLIENT1 du commutateur de réseau d’entreprise et connectez-le au commutateur Internet. Patientez 30 secondes.  
  
2. Dans une fenêtre Windows PowerShell avec élévation de privilèges, tapez **ipconfig /flushdns** et appuyez sur ENTRÉE. Vide les entrées de résolution de noms susceptibles d’être restées dans le cache DNS client depuis lorsque l’ordinateur client a été connecté au réseau d’entreprise.  
  
3. Dans la fenêtre Windows PowerShell, tapez **Get-DnsClientNrptPolicy** et appuyez sur ENTRÉE.  
  
   La sortie indique les paramètres actifs pour la table de stratégie de résolution de noms. Ces paramètres indiquent que toutes les connexions à. corp.contoso.com doivent être résolues par le serveur DNS d’accès à distance, avec le 2001:db8:1::2 d’adresse IPv6. Notez également l'entrée de la table de stratégie de résolution de noms indiquant une exemption pour le nom nls.corp.contoso.com ; le serveur DNS d'accès à distance ne répond pas aux noms qui figurent sur la liste des exemptions. Vous pouvez tester l’adresse d’IP de serveur DNS d’accès à distance pour vérifier la connectivité au serveur d’accès à distance ; Vous pouvez par exemple, ping 2001:db8:1::2.  
  
4. Dans la fenêtre Windows PowerShell, tapez **un test ping sur app1** et appuyez sur ENTRÉE. Vous devez voir des réponses à partir de l’adresse IPv6 pour APP1, qui est dans ce cas 2001:db8:1::3.  
  
5. Dans la fenêtre Windows PowerShell, tapez **un test ping sur app2** et appuyez sur ENTRÉE. Vous devez voir des réponses à partir de l'adresse NAT64 affectée par EDGE1 à APP2, qui est ici fdc9:9f4e:eb1b:7777::a00:4.  
  
   La possibilité d’un test ping sur APP2 est importante, car la réussite indique que vous étiez en mesure d’établir une connexion à l’aide de NAT64/DNS64, comme APP2 est une ressource uniquement IPv4.  
  
6. Laissez la fenêtre Windows PowerShell ouverte pour la procédure suivante.  
  
7. Ouvrez Internet Explorer, dans la barre d’adresses Internet Explorer, entrez **https://app1/** et appuyez sur ENTRÉE. Vous allez voir le site web IIS par défaut sur APP1.  
  
8. Dans la barre d’adresses Internet Explorer, entrez **https://app2/** et appuyez sur ENTRÉE. Vous allez voir le site web par défaut sur APP2.  
  
9. Sur le **Démarrer** , tapez<strong>\\\App2\Files</strong>, puis appuyez sur ENTRÉE. Double-cliquez sur le fichier Nouveau document texte.  
  
    Cet exemple montre que vous avez pu se connecter à un serveur IPv4 uniquement à l’aide de SMB pour obtenir une ressource dans le domaine de ressources.  
  
10. Sur le **Démarrer** , tapez**wf.msc**, puis appuyez sur ENTRÉE.  
  
11. Dans le **pare-feu Windows avec fonctions avancées de sécurité** de la console, notez que seule la **privé** ou **profil Public** est actif. Le pare-feu Windows doit être activé pour DirectAccess fonctionne correctement. Si le pare-feu Windows est désactivé, la connectivité DirectAccess ne fonctionne pas.  
  
12. Dans le volet gauche de la console, développez le **surveillance** nœud, puis cliquez sur le **les règles de sécurité de connexion** nœud. Vous devez voir les règles de sécurité de connexion active : **Stratégie DirectAccess-ClientToCorp**, **stratégie DirectAccess-ClientToDNS64NAT64PrefixExemption**, **stratégie DirectAccess-ClientToInfra**, et **DirectAccess Stratégie-ClientToNlaExempt**. Faites défiler le volet du milieu vers la droite pour afficher le **des méthodes d’authentification 1er** et **2nd méthodes d’authentification** colonnes. Notez que la première règle (ClientToCorp) utilise Kerberos V5 pour établir le tunnel intranet et la troisième règle (ClientToInfra) utilise NTLMv2 pour établir le tunnel d’infrastructure.  
  
13. Dans le volet gauche de la console, développez le **Associations de sécurité** nœud, puis cliquez sur le **Mode principal** nœud. Notez que les associations de sécurité de tunnel infrastructure à l’aide de NTLMv2 et l’association de sécurité du tunnel intranet à l’aide de Kerberos V5. Cliquez sur l’entrée qui affiche **utilisateur (Kerberos V5)** en tant que le **2e méthode d’authentification** et cliquez sur **propriétés**. Sur le **général** onglet, notez le **seconde authentification ID Local** est **CORP\User1**, indiquant que User1 a réussi à s’authentifier auprès du domaine CORP à l’aide Kerberos.  
  
## <a name="test-directaccess-client-connectivity-through-the-cluster"></a>Tester la connectivité du client DirectAccess sur le cluster  
  
1. Effectuer un arrêt approprié sur EDGE2.  
  
   Vous pouvez utiliser le Gestionnaire d’équilibrage de charge réseau pour afficher l’état des serveurs lors de l’exécution de ces tests.  
  
2. Sur CLIENT1, dans la fenêtre Windows PowerShell, tapez **ipconfig /flushdns** et appuyez sur ENTRÉE. Vide les entrées de résolution de noms susceptibles d’être restées dans le cache DNS du client.  
  
3. Dans la fenêtre Windows PowerShell, un test ping sur APP1 et APP2. Vous devez recevoir des réponses à partir de ces ressources.  
  
4. Sur le **Démarrer** , tapez<strong>\\\app2\files</strong>. Vous devez voir le dossier partagé sur l’ordinateur APP2. La possibilité d’ouvrir le partage de fichiers sur APP2 indique que le deuxième tunnel, ce qui nécessite l’authentification Kerberos pour l’utilisateur, fonctionne correctement.  
  
5. Ouvrez Internet Explorer, puis les sites Web https://app1/ et https://app2/. La possibilité d’ouvrir les deux sites Web confirme que les première et deuxième tunnels ne soient opérationnels et de fonctionnement. Fermez Internet Explorer.  
  
6. Démarrez l’ordinateur EDGE2.  
  
7. Sur EDGE1 effectuer un arrêt approprié.  
  
8. Attendez 5 minutes, puis revenez à CLIENT1. Effectuez les étapes 2 à 5. Cela confirme que CLIENT1 a été en mesure de basculer en toute transparence à EDGE2 après que EDGE1 est devenue indisponible.
