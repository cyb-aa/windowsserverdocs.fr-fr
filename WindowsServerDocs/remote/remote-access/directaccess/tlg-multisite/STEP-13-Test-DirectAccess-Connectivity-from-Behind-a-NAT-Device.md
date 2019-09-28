---
title: ÉTAPE 13 test de la connectivité DirectAccess derrière un périphérique NAT
description: 'Cette rubrique fait partie du Guide de laboratoire de test : illustrer un déploiement multisite DirectAccess pour Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 796825c3-5e3e-4745-a921-25ab90b95ede
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 41701592c0d9b143c84ad3fbad3fd77491eff5a0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404720"
---
# <a name="step-13-test-directaccess-connectivity-from-behind-a-nat-device"></a>ÉTAPE 13 test de la connectivité DirectAccess derrière un périphérique NAT

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Quand un client DirectAccess est connecté à Internet alors qu'il se situe derrière un périphérique NAT ou un serveur proxy web, il utilise Teredo ou IP-HTTPS pour établir la connexion au serveur d'accès à distance. Si le périphérique NAT active le port UDP sortant 3544 à l’adresse IP publique du serveur d’accès à distance, Teredo est utilisé. Si l'accès Teredo n'est pas disponible, le client DirectAccess revient à IP-HTTPS sur le port TCP sortant 443, qui permet un accès via des pare-feu ou des serveurs proxy web sur le port SSL classique. Si le proxy web nécessite une authentification, la connexion IP-HTTPS échoue. Les connexions IP-HTTPS échouent également si le proxy web effectue une inspection SSL sortante, car la session HTTPS se termine au niveau du proxy web et non du serveur d'accès à distance.  
  
Les procédures suivantes sont effectuées sur les deux ordinateurs clients :  
  
1. Tester la connectivité Teredo. Le premier ensemble de tests est effectué quand le client DirectAccess est configuré pour utiliser Teredo. Il s'agit du paramètre automatique quand le périphérique NAT autorise l'accès sortant vers le port UDP 3544. Commencez par exécuter les tests sur CLIENT1, puis exécutez les tests sur CLIENT2.  
  
2. Testez la connectivité IP-HTTPs. Le deuxième ensemble de tests est effectué quand le client DirectAccess est configuré pour utiliser IP-HTTPs. Pour illustrer la connectivité IP-HTTPS, Teredo est désactivé sur les ordinateurs clients. Commencez par exécuter les tests sur CLIENT1, puis exécutez les tests sur CLIENT2.  
  
## <a name="prerequisites"></a>Prérequis  
Démarrez EDGE1 et 2-EDGE1 s’ils ne sont pas déjà en cours d’exécution, et assurez-vous qu’ils sont connectés au sous-réseau Internet.  
  
Avant d’effectuer ces tests, débranchez CLIENT1 et CLIENT2 du commutateur Internet et connectez-les au commutateur HomeNet Si le système vous demande le type de réseau pour lequel vous souhaitez définir le réseau actuel, sélectionnez **réseau privé**.  
  
## <a name="TeredoCLIENT1"></a>Tester la connectivité Teredo  
  
1. Sur CLIENT1, ouvrez une fenêtre Windows PowerShell avec élévation de privilèges.  
  
2. Activez l’adaptateur Teredo, tapez **netsh interface Teredo Set State enterpriseclient**, puis appuyez sur entrée.  
  
3. Dans la fenêtre Windows PowerShell, tapez **ipconfig/all** et appuyez sur entrée.  
  
4. Examinez la sortie de la commande ipconfig.  
  
   Cet ordinateur est maintenant connecté à Internet de derrière un périphérique NAT et une adresse IPv4 privée lui est affectée. Quand le client DirectAccess est derrière un périphérique NAT et qu'une adresse IPv4 privée lui est affectée, la technologie de transition IPv6 préférée est Teredo. Si vous examinez la sortie de la commande ipconfig, vous devriez voir une section de la pseudo-interface de tunneling Teredo de l’adaptateur de tunnel, puis une description de la carte de tunnel Teredo Microsoft, avec une adresse IP commençant par 2001:0 cohérente avec Teredo -. Vous devez voir la passerelle par défaut indiquée pour la carte de tunnel Teredo en tant que' :: '.  
  
5. Dans la fenêtre Windows PowerShell, tapez **ipconfig/flushdns** et appuyez sur entrée.  
  
   Vous videz ainsi les entrées de résolution de noms susceptibles d'être restées dans le cache DNS client depuis la connexion de l'ordinateur client à Internet.  
  
6. Dans la fenêtre Windows PowerShell, tapez « **ping App1** » et appuyez sur entrée. Vous devez voir des réponses à partir de l'adresse IPv6 d'APP1, 2001:db8:1::3.  
  
7. Dans la fenêtre Windows PowerShell, tapez **ping App2** et appuyez sur entrée. Vous devez voir les réponses de l’adresse NAT64 assignée par EDGE1 à APP2, qui dans ce cas est FD**C9:9f4e : eb1b**: porte :: A00:4. Notez que les valeurs en gras varient en fonction de la façon dont l’adresse est générée.  
  
8. Dans la fenêtre Windows PowerShell, tapez **ping 2-App1** , puis appuyez sur entrée. Vous devez voir les réponses de l’adresse IPv6 2-APP1, 2001 : DB8:2 :: 3.  
  
9. Ouvrez Internet Explorer, dans la barre d’adresses d’Internet Explorer, entrez **https://2-app1/** , puis appuyez sur entrée. Vous verrez le site Web IIS par défaut sur 2-APP1.  
  
10. Dans la barre d’adresses d’Internet Explorer, entrez **https://app2/** et appuyez sur entrée. Vous allez voir le site web par défaut sur APP2.  
  
11. Dans l’écran d' **Accueil** , tapez<strong>\\ \ App2\Files</strong>, puis appuyez sur entrée. Double-cliquez sur le fichier Nouveau document texte. Cela démontre que vous avez pu vous connecter à un serveur IPv4 uniquement à l'aide de SMB pour obtenir une ressource sur un hôte IPv4 uniquement.  
  
12. Répétez cette procédure sur CLIENT2.  
  
## <a name="IPHTTPS_CLIENT1"></a>Tester la connectivité IP-HTTPs  
  
1. Sur CLIENT1, ouvrez une fenêtre Windows PowerShell avec élévation de privilèges, puis tapez **netsh interface Teredo Set State Disabled** et appuyez sur entrée. Teredo est ainsi désactivé sur l'ordinateur client qui peut se configurer pour utiliser IP-HTTPS. La réponse **Ok** s'affiche quand la commande est terminée.  
  
2. Dans la fenêtre Windows PowerShell, tapez **ipconfig/all** et appuyez sur entrée.  
  
3. Examinez la sortie de la commande ipconfig. Cet ordinateur est maintenant connecté à Internet de derrière un périphérique NAT et une adresse IPv4 privée lui est affectée. Teredo est désactivé et le client DirectAccess revient à IP-HTTPS. Lorsque vous examinez la sortie de la commande ipconfig, vous voyez une section pour la carte tunnel iphttpsinterface avec une adresse IP commençant par 2001 : DB8:1 : 1000 ou 2001 : DB8:2 : 2000 cohérent avec cette adresse IP-HTTPs en fonction des préfixes configuré lors de la configuration de DirectAccess. Vous ne verrez pas de passerelle par défaut indiquée pour la carte tunnel IPHTTPSInterface.  
  
4. Dans la fenêtre Windows PowerShell, tapez **ipconfig/flushdns** et appuyez sur entrée. Vous videz ainsi les entrées de résolution de noms susceptibles d'être restées dans le cache DNS client depuis la connexion de l'ordinateur client au réseau d'entreprise.  
  
5. Dans la fenêtre Windows PowerShell, tapez « **ping App1** » et appuyez sur entrée. Vous devez voir des réponses à partir de l'adresse IPv6 d'APP1, 2001:db8:1::3.  
  
6. Dans la fenêtre Windows PowerShell, tapez **ping App2** et appuyez sur entrée. Vous devez voir les réponses de l’adresse NAT64 assignée par EDGE1 à APP2, qui dans ce cas est FD**C9:9f4e : eb1b**: porte :: A00:4. Notez que les valeurs en gras varient en fonction de la façon dont l’adresse est générée.  
  
7. Dans la fenêtre Windows PowerShell, tapez **ping 2-App1** , puis appuyez sur entrée. Vous devez voir les réponses de l’adresse IPv6 2-APP1, 2001 : DB8:2 :: 3.  
  
8. Ouvrez Internet Explorer, dans la barre d’adresses d’Internet Explorer, entrez **https://2-app1/** , puis appuyez sur entrée. Vous verrez le site Web IIS par défaut sur 2-APP1.  
  
9. Dans la barre d’adresses d’Internet Explorer, entrez **https://app2/** et appuyez sur entrée. Vous allez voir le site web par défaut sur APP2.  
  
10. Dans l’écran d' **Accueil** , tapez<strong>\\ \ App2\Files</strong>, puis appuyez sur entrée. Double-cliquez sur le fichier Nouveau document texte. Cela démontre que vous avez pu vous connecter à un serveur IPv4 uniquement à l'aide de SMB pour obtenir une ressource sur un hôte IPv4 uniquement.  
  
11. Répétez cette procédure sur CLIENT2.  
  


