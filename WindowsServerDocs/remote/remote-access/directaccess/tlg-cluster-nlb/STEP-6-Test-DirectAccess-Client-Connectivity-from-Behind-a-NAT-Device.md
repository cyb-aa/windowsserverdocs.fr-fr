---
title: ÉTAPE 6 test de la connectivité du client DirectAccess derrière un périphérique NAT
description: Cette rubrique fait partie du Guide de laboratoire de test-démonstration de DirectAccess dans un cluster avec Windows NLB pour Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: aded2881-99ed-4f18-868b-b765ab926597
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 472c1dc6c5531a7c8d41e40bc926bb3e25f73448
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367600"
---
# <a name="step-6-test-directaccess-client-connectivity-from-behind-a-nat-device"></a>ÉTAPE 6 test de la connectivité du client DirectAccess derrière un périphérique NAT

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Quand un client DirectAccess est connecté à Internet alors qu'il se situe derrière un périphérique NAT ou un serveur proxy web, il utilise Teredo ou IP-HTTPS pour établir la connexion au serveur d'accès à distance. 

Si le périphérique NAT active le port UDP sortant 3544 à l’adresse IP publique du serveur d’accès à distance, Teredo est utilisé. Si l'accès Teredo n'est pas disponible, le client DirectAccess revient à IP-HTTPS sur le port TCP sortant 443, qui permet un accès via des pare-feu ou des serveurs proxy web sur le port SSL classique. 

Si le proxy web nécessite une authentification, la connexion IP-HTTPS échoue. Les connexions IP-HTTPS échouent également si le proxy web effectue une inspection SSL sortante, car la session HTTPS se termine au niveau du proxy web et non du serveur d'accès à distance. Dans cette section, vous allez effectuer les mêmes tests que ceux exécutés lors de la connexion à l'aide d'une connexion 6to4 dans la section précédente.  
  
Les procédures suivantes sont effectuées sur les deux ordinateurs clients :  
  
1. Tester la connectivité Teredo. Le premier ensemble de tests est effectué quand le client DirectAccess est configuré pour utiliser Teredo. Il s'agit du paramètre automatique quand le périphérique NAT autorise l'accès sortant vers le port UDP 3544.  
  
2. Testez la connectivité IP-HTTPs. Le deuxième ensemble de tests est effectué quand le client DirectAccess est configuré pour utiliser IP-HTTPs. Pour illustrer la connectivité IP-HTTPS, Teredo est désactivé sur les ordinateurs clients.  
  
> [!TIP]  
> Il est recommandé d’effacer le cache d’Internet Explorer avant d’effectuer ces procédures pour vous assurer que vous testez la connexion et que vous ne récupérez pas les pages du site Web à partir du cache.  
  
## <a name="prerequisites"></a>Prérequis

Avant d'effectuer ces tests, débranchez CLIENT1 du commutateur Internet et connectez-le au commutateur du réseau domestique. Si le système vous demande le type de réseau que vous voulez définir comme réseau actuel, sélectionnez **Réseau domestique**.  
  
Démarrez EDGE1 et EDGE2 s'ils ne sont pas déjà en cours d'exécution.  
  
## <a name="test-teredo-connectivity"></a>Test de la connectivité Teredo  
  
1. Sur CLIENT1, ouvrez une fenêtre Windows PowerShell avec élévation de privilèges, tapez **ipconfig/all** et appuyez sur entrée.  
  
2. Examinez la sortie de la commande ipconfig.  
  
   CLIENT1 est maintenant connecté à Internet de derrière un périphérique NAT et une adresse IPv4 privée lui est affectée. Quand le client DirectAccess est derrière un périphérique NAT et qu'une adresse IPv4 privée lui est affectée, la technologie de transition IPv6 préférée est Teredo. Si vous examinez la sortie de la commande ipconfig, vous devriez voir une section de la pseudo-interface de tunneling Teredo de l’adaptateur de tunnel, puis une description de la carte de tunnel Teredo Microsoft, avec une adresse IP commençant par 2001 : cohérent avec Teredo -. Si vous ne voyez pas la section Teredo, activez Teredo avec la commande suivante : **netsh interface Teredo Set State enterpriseclient** , puis réexécutez la commande ipconfig. Aucune passerelle par défaut n'est répertoriée pour la carte tunnel Teredo.  
  
3. Dans la fenêtre Windows PowerShell, tapez **ipconfig/flushdns** et appuyez sur entrée.  
  
   Vous videz ainsi les entrées de résolution de noms susceptibles d'être restées dans le cache DNS client depuis la connexion de l'ordinateur client à Internet.  
  
4. Dans la fenêtre Windows PowerShell, tapez **« DnsClientNrptPolicy »** et appuyez sur entrée.  
  
   La sortie indique les paramètres actifs pour la table de stratégie de résolution de noms. Ces paramètres indiquent que toutes les connexions à .corp.contoso.com doivent être résolues par le serveur DNS d'accès à distance, avec l'adresse IPv6 2001:db8:1::2. Notez également l'entrée de la table de stratégie de résolution de noms indiquant une exemption pour le nom nls.corp.contoso.com ; le serveur DNS d'accès à distance ne répond pas aux noms qui figurent sur la liste des exemptions. Vous pouvez effectuer un test ping sur l'adresse IP du serveur DNS d'accès à distance pour confirmer la connectivité au serveur d'accès à distance ; par exemple, vous pouvez ici effectuer un test ping sur 2001:db8:1::2.  
  
5. Dans la fenêtre Windows PowerShell, tapez « **ping App1** » et appuyez sur entrée. Vous devez voir des réponses à partir de l'adresse IPv6 d'APP1, 2001:db8:1::3.  
  
6. Dans la fenêtre Windows PowerShell, tapez **ping App2** et appuyez sur entrée. Vous devez voir des réponses à partir de l'adresse NAT64 affectée par EDGE1 à APP2, qui est ici fdc9:9f4e:eb1b:7777::a00:4.  
  
7. Laissez la fenêtre Windows PowerShell ouverte pour la procédure suivante.  
  
8. Ouvrez Internet Explorer, dans la barre d’adresses d’Internet Explorer, entrez **https://app1/** , puis appuyez sur entrée. Vous allez voir le site web IIS par défaut sur APP1.  
  
9. Dans la barre d’adresses d’Internet Explorer, entrez **https://app2/** et appuyez sur entrée. Vous allez voir le site web par défaut sur APP2.  
  
10. Dans l’écran d' **Accueil** , tapez<strong>\\ \ App2\Files</strong>, puis appuyez sur entrée. Double-cliquez sur le fichier Nouveau document texte. Cela démontre que vous avez pu vous connecter à un serveur IPv4 uniquement à l'aide de SMB pour obtenir une ressource sur un hôte IPv4 uniquement.  
  
## <a name="test-ip-https-connectivity"></a>Test de la connectivité IP-HTTPS  
  
1. Ouvrez une fenêtre Windows PowerShell avec élévation de privilèges, tapez **netsh interface Teredo Set State Disabled** et appuyez sur entrée. Teredo est ainsi désactivé sur l'ordinateur client qui peut se configurer pour utiliser IP-HTTPS. La réponse **Ok** s'affiche quand la commande est terminée.  
  
2. Dans la fenêtre Windows PowerShell, tapez **ipconfig/all** et appuyez sur entrée.  
  
3. Examinez la sortie de la commande ipconfig. Cet ordinateur est maintenant connecté à Internet de derrière un périphérique NAT et une adresse IPv4 privée lui est affectée. Teredo est désactivé et le client DirectAccess revient à IP-HTTPS. Lorsque vous examinez la sortie de la commande ipconfig, vous voyez une section pour l’adaptateur de tunnel iphttpsinterface avec une adresse IP commençant par 2001 : DB8:1 : 100 cohérent avec ce qui correspond à une adresse IP-HTTPs basée sur le préfixe qui a été configuré lors de la configuration DirectAccess. Aucune passerelle par défaut n'est répertoriée pour la carte tunnel IP-HTTPS.  
  
4. Dans la fenêtre Windows PowerShell, tapez **ipconfig/flushdns** et appuyez sur entrée. Vous videz ainsi les entrées de résolution de noms susceptibles d'être restées dans le cache DNS client depuis la connexion de l'ordinateur client au réseau d'entreprise.  
  
5. Dans la fenêtre Windows PowerShell, tapez « **ping App1** » et appuyez sur entrée. Vous devez voir des réponses à partir de l'adresse IPv6 d'APP1, 2001:db8:1::3.  
  
6. Dans la fenêtre Windows PowerShell, tapez **ping App2** et appuyez sur entrée. Vous devez voir des réponses à partir de l'adresse NAT64 affectée par EDGE1 à APP2, qui est ici fdc9:9f4e:eb1b:7777::a00:4.  
  
7. Ouvrez Internet Explorer, dans la barre d’adresses d’Internet Explorer, entrez **https://app1/** , puis appuyez sur entrée. Vous allez voir le site IIS par défaut sur APP1.  
  
8. Dans la barre d’adresses d’Internet Explorer, entrez **https://app2/** et appuyez sur entrée. Vous allez voir le site web par défaut sur APP2.  
  
9. Dans l’écran d' **Accueil** , tapez<strong>\\ \ App2\Files</strong>, puis appuyez sur entrée. Double-cliquez sur le fichier Nouveau document texte. Cela démontre que vous avez pu vous connecter à un serveur IPv4 uniquement à l'aide de SMB pour obtenir une ressource sur un hôte IPv4 uniquement.
