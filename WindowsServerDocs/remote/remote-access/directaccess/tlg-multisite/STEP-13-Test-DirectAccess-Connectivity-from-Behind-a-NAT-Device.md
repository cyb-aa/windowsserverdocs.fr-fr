---
title: ÉTAPE 13 tester la connectivité DirectAccess derrière un périphérique NAT
description: 'Cette rubrique fait partie du Guide de laboratoire de Test : illustrer un déploiement Multisite DirectAccess pour Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 796825c3-5e3e-4745-a921-25ab90b95ede
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2d1661d43cd45614dfabc66fd9a737c55ab388ed
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446923"
---
# <a name="step-13-test-directaccess-connectivity-from-behind-a-nat-device"></a>ÉTAPE 13 tester la connectivité DirectAccess derrière un périphérique NAT

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Quand un client DirectAccess est connecté à Internet alors qu'il se situe derrière un périphérique NAT ou un serveur proxy web, il utilise Teredo ou IP-HTTPS pour établir la connexion au serveur d'accès à distance. Si le périphérique NAT autorise le port UDP sortant 3544 vers l’adresse IP du serveur d’accès à distance, Teredo est utilisé. Si l'accès Teredo n'est pas disponible, le client DirectAccess revient à IP-HTTPS sur le port TCP sortant 443, qui permet un accès via des pare-feu ou des serveurs proxy web sur le port SSL classique. Si le proxy web nécessite une authentification, la connexion IP-HTTPS échoue. Les connexions IP-HTTPS échouent également si le proxy web effectue une inspection SSL sortante, car la session HTTPS se termine au niveau du proxy web et non du serveur d'accès à distance.  
  
Les procédures suivantes sont effectuées sur les deux ordinateurs clients :  
  
1. Tester la connectivité Teredo. La première série de tests sont effectuées lorsque le client DirectAccess est configuré pour utiliser Teredo. Il s'agit du paramètre automatique quand le périphérique NAT autorise l'accès sortant vers le port UDP 3544. Tout d’abord exécuter les tests sur CLIENT1, puis exécutez les tests sur le CLIENT2.  
  
2. Tester la connectivité IP-HTTPS. La deuxième série de tests sont effectuées lorsque le client DirectAccess est configuré pour utiliser IP-HTTPS. Pour illustrer la connectivité IP-HTTPS, Teredo est désactivé sur les ordinateurs clients. Tout d’abord exécuter les tests sur CLIENT1, puis exécutez les tests sur le CLIENT2.  
  
## <a name="prerequisites"></a>Prérequis  
Démarrez EDGE1 et 2-EDGE1 si elles le ne sont pas déjà en cours d’exécution et vous assurer qu’ils sont connectés au sous-réseau Internet.  
  
Avant d’effectuer ces tests, débranchez CLIENT1 et CLIENT2 du commutateur Internet et les connecter au commutateur réseau domestique. Si vous demande quel type de réseau que vous voulez définir comme réseau actuel, sélectionnez **réseau domestique**.  
  
## <a name="TeredoCLIENT1"></a>Tester la connectivité Teredo  
  
1. Sur CLIENT1, ouvrez une fenêtre Windows PowerShell avec élévation de privilèges.  
  
2. Activer la carte de Teredo, type **teredo d’interface netsh définie état enterpriseclient**, puis appuyez sur ENTRÉE.  
  
3. Dans la fenêtre Windows PowerShell, tapez **ipconfig/all** et appuyez sur ENTRÉE.  
  
4. Examinez la sortie de la commande ipconfig.  
  
   Cet ordinateur est maintenant connecté à Internet de derrière un périphérique NAT et une adresse IPv4 privée lui est affectée. Quand le client DirectAccess est derrière un périphérique NAT et qu'une adresse IPv4 privée lui est affectée, la technologie de transition IPv6 préférée est Teredo. Si vous examinez la sortie de la commande ipconfig, vous devez voir une section pour la carte de Tunnel Teredo pseudo-interface de Tunneling, puis une description de carte de tunnel Microsoft Teredo, avec une adresse IP qui commence par 2001 : 0 cohérente avec la forme d’un Teredo adresse. Vous devez voir la passerelle par défaut répertoriée pour la carte de tunnel Teredo en tant que « :: ».  
  
5. Dans la fenêtre Windows PowerShell, tapez **ipconfig /flushdns** et appuyez sur ENTRÉE.  
  
   Vous videz ainsi les entrées de résolution de noms susceptibles d'être restées dans le cache DNS client depuis la connexion de l'ordinateur client à Internet.  
  
6. Dans la fenêtre Windows PowerShell, tapez **un test ping sur app1** et appuyez sur ENTRÉE. Vous devez voir des réponses à partir de l'adresse IPv6 d'APP1, 2001:db8:1::3.  
  
7. Dans la fenêtre Windows PowerShell, tapez **un test ping sur app2** et appuyez sur ENTRÉE. Vous devez voir des réponses à partir de l’adresse NAT64 affectée par EDGE1 à APP2, qui est dans ce cas fd**c9:9f4e:eb1b**: 7777::a00:4. Notez que les valeurs en gras peut varier en raison de la façon dont l’adresse est générée.  
  
8. Dans la fenêtre Windows PowerShell, tapez **ping 2-app1** et appuyez sur ENTRÉE. Vous devez voir des réponses à partir de l’adresse IPv6 de 2-APP1, 2001:db8:2::3.  
  
9. Ouvrez Internet Explorer, dans la barre d’adresses Internet Explorer, entrez **https://2-app1/** et appuyez sur ENTRÉE. Vous verrez le site Web IIS par défaut sur APP1-2.  
  
10. Dans la barre d’adresses Internet Explorer, entrez **https://app2/** et appuyez sur ENTRÉE. Vous allez voir le site web par défaut sur APP2.  
  
11. Sur le **Démarrer** , tapez<strong>\\\App2\Files</strong>, puis appuyez sur ENTRÉE. Double-cliquez sur le fichier Nouveau document texte. Cela démontre que vous avez pu vous connecter à un serveur IPv4 uniquement à l'aide de SMB pour obtenir une ressource sur un hôte IPv4 uniquement.  
  
12. Répétez cette procédure sur le CLIENT2.  
  
## <a name="IPHTTPS_CLIENT1"></a>Tester la connectivité IP-HTTPS  
  
1. Sur CLIENT1, ouvrez une avec élévation de privilèges fenêtre de Windows PowerShell, puis tapez **teredo d’interface netsh définie l’état désactivé** et appuyez sur ENTRÉE. Teredo est ainsi désactivé sur l'ordinateur client qui peut se configurer pour utiliser IP-HTTPS. La réponse **Ok** s'affiche quand la commande est terminée.  
  
2. Dans la fenêtre Windows PowerShell, tapez **ipconfig/all** et appuyez sur ENTRÉE.  
  
3. Examinez la sortie de la commande ipconfig. Cet ordinateur est maintenant connecté à Internet de derrière un périphérique NAT et une adresse IPv4 privée lui est affectée. Teredo est désactivé et le client DirectAccess revient à IP-HTTPS. Lorsque vous examinez la sortie de la commande ipconfig, vous consultez une section pour la carte Tunnel iphttpsinterface avec une adresse IP qui commence par 2001:db8:1:1000 ou 2001:db8:2:2000 cohérente avec une adresse IP-HTTPS basée sur les préfixes qui ont été configuré lors de la configuration DirectAccess. Vous ne verrez pas de passerelle par défaut répertoriée pour la carte tunnel IPHTTPSInterface.  
  
4. Dans la fenêtre Windows PowerShell, tapez **ipconfig /flushdns** et appuyez sur ENTRÉE. Vous videz ainsi les entrées de résolution de noms susceptibles d'être restées dans le cache DNS client depuis la connexion de l'ordinateur client au réseau d'entreprise.  
  
5. Dans la fenêtre Windows PowerShell, tapez **un test ping sur app1** et appuyez sur ENTRÉE. Vous devez voir des réponses à partir de l'adresse IPv6 d'APP1, 2001:db8:1::3.  
  
6. Dans la fenêtre Windows PowerShell, tapez **un test ping sur app2** et appuyez sur ENTRÉE. Vous devez voir des réponses à partir de l’adresse NAT64 affectée par EDGE1 à APP2, qui est dans ce cas fd**c9:9f4e:eb1b**: 7777::a00:4. Notez que les valeurs en gras peut varier en raison de la façon dont l’adresse est générée.  
  
7. Dans la fenêtre Windows PowerShell, tapez **ping 2-app1** et appuyez sur ENTRÉE. Vous devez voir des réponses à partir de l’adresse IPv6 de 2-APP1, 2001:db8:2::3.  
  
8. Ouvrez Internet Explorer, dans la barre d’adresses Internet Explorer, entrez **https://2-app1/** et appuyez sur ENTRÉE. Vous verrez le site Web IIS par défaut sur APP1-2.  
  
9. Dans la barre d’adresses Internet Explorer, entrez **https://app2/** et appuyez sur ENTRÉE. Vous allez voir le site web par défaut sur APP2.  
  
10. Sur le **Démarrer** , tapez<strong>\\\App2\Files</strong>, puis appuyez sur ENTRÉE. Double-cliquez sur le fichier Nouveau document texte. Cela démontre que vous avez pu vous connecter à un serveur IPv4 uniquement à l'aide de SMB pour obtenir une ressource sur un hôte IPv4 uniquement.  
  
11. Répétez cette procédure sur le CLIENT2.  
  


