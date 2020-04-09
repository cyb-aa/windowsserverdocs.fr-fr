---
title: Planification de la capacité DirectAccess
description: Vous pouvez utiliser cette rubrique pour obtenir un rapport sur les performances du serveur DirectAccess Windows Server 2012 afin de vous aider à planifier la capacité de DirectAccess dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: 456e5971-3aa7-4a24-bc5d-0c21fec7687e
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 413dc88ce9ec551a318b63f3df44ed256f289219
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80815792"
---
# <a name="directaccess-capacity-planning"></a>Planification de la capacité DirectAccess

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Ce document est un rapport sur les performances du serveur DirectAccess Windows Server 2012. Des tests ont été effectués pour déterminer la capacité de débit à l’aide de matériel informatique haut de gamme et bas de gamme. Les performances de l’UC sur le matériel haut de gamme et bas de gamme dépendaient du débit du trafic réseau et des types de clients utilisés. Un déploiement de DirectAccess classique (et la base de ces tests) porte sur 1/3 (30 %) de clients IPHTTPS et 2/3 (70 %) de clients Teredo. Les clients Teredo surpassent les clients IPHTTPS en partie parce que Windows Server 2012 utilise le partage du trafic entrant (RSS) qui permet l’utilisation de tous les cœurs d’UC. Dans ces tests, puisque le partage RSS est activé, l’hyperthreading est désactivé. En outre, TCP/IP dans Windows Server 2012 prend en charge le trafic UDP, ce qui permet aux clients Teredo d’équilibrer la charge entre les UC.  
  
Les données ont été recueillies à la fois à partir d’un serveur bas de gamme (4 cœurs, 4 Go) et à partir de matériel supposé plus classique sur un serveur haut de gamme (8 cœurs, 8 Go).  Voici une capture d’écran du nouveau gestionnaire des tâches de Windows 8 sur le matériel bas de gamme avec 750 clients (562 Teredo, 188 IPHTTPS), en cours d’exécution ~ 77 Mbits/s. Cela permet de simuler les utilisateurs qui ne présentent pas d’informations d’identification de carte à puce.  
  
Les résultats des tests indiquent que Teredo obtient de meilleures performances qu’IPHTTPS dans Windows 8, mais que l’utilisation de la bande passante de Teredo et d’IPHTTPS a été améliorée par rapport à Windows 7.  
  
![Résultats des tests](../../media/DirectAccess-Capacity-Planning/DACapacityPlanning1.gif)  
  
## <a name="high-end-hardware-test-environment"></a>Environnement de test sur un matériel haut de gamme  
Le diagramme suivant illustre les résultats de l’environnement de test des performances sur un matériel haut de gamme. Tous les résultats des tests et leur analyse sont détaillés dans ce document.  
  
||||  
|-|-|-|  
|Configuration-matériel|Matériel bas de gamme (4 Go de RAM, 4 cœurs)|Matériel haut de gamme (8 Go, 8 cœurs)|  
|Double tunnel<p>-PKI<p>-Y compris DNS64/NAT64|750 connexions simultanées en utilisant 50 % de l’UC, 50 % de mémoire avec un débit de carte réseau d’entreprise de 75 Mbits/s. L’objectif ambitieux est de 1 000 utilisateurs en utilisant 50 % de l’UC.|1500 connexions simultanées à 50% d’UC, 50% de mémoire avec un débit de carte réseau Corpnet de 150 Mbits/s.|  
## <a name="test-environment"></a>Environnement de test

**Topologie de banc de performances**  
  
![Environnement de test](../../media/DirectAccess-Capacity-Planning/DACapacityPlanning2.gif)  
  
L’environnement de test des performances est constitué d’un groupe de 5 ordinateurs. Pour le test sur du matériel bas de gamme, un serveur DirectAccess 4 cœurs, 4 Go a été utilisé et, pour le test sur du matériel haut de gamme, un serveur DirectAccess 8 cœurs, 16 Go a été utilisé. Pour les environnements de test sur ces deux types de matériel, les éléments suivants ont été utilisés : un serveur principal (l’émetteur) et deux ordinateurs clients (les destinataires).  Les destinataires sont répartis entre les deux ordinateurs clients. Sinon, les destinataires utiliseraient l’UC de manière intensive et limiteraient le nombre de clients et la bande passante. Côté réception, un simulateur pour simuler des centaines de clients (les clients HTTPS ou Teredo sont simulés). IPsec et DoSP sont tous deux configurés. Le partage RSS est activé sur le serveur DirectAccess. La taille de la file d’attente RSS a la valeur 8.  Si vous ne configurez pas RSS, un seul processeur est systématiquement utilisé de façon intensive alors que les autres cœurs sont sous-utilisés. À noter également que le serveur DirectAccess est un ordinateur 4 cœurs sur lequel l’hyperthreading est désactivé.  L’hyperthreading est désactivé car RSS fonctionne uniquement sur des cœurs physiques et l’utilisation de l’hyperthreading génère des résultats faussés. (Cela signifie que les cœurs ne sont pas tous chargés de manière uniforme).  
  
## <a name="testing-results-for-low-end-hardware"></a>Résultats des tests pour le matériel bas de gamme :

Les tests ont été effectués à la fois avec 1 000 et 750 clients.  Dans tous les cas, la répartition du trafic était de 70 % pour Teredo et 30 % pour IPHTTPS.  Tous les tests impliquaient le trafic TCP sur Nat64 avec 2 tunnels IPsec par client.  Dans tous les tests, l’utilisation de la mémoire était légère et celle de l’UC acceptable.  
  
**Résultats des tests individuel :**  
  
Les sections suivantes décrivent les tests individuels. Chaque titre de section met en évidence les éléments clés de chaque test, puis une description résumée des résultats et un diagramme illustrant les données de résultat détaillées.  
  
**Performances de bas de gamme : 750 clients, 70/30 fractionnement, 84,17 Mbits/s débit :**  
  
Les trois tests suivants représentent le matériel bas de gamme.  Les tests exécutés ci-dessous portaient sur 750 clients avec un débit de 84,17 Mbits/s et un trafic réparti sur 562 Teredo et 188 IPHTTPS. L’unité MTU Teredo a été définie sur 1472 et la dérivation Teredo a été activée. L’utilisation de l’UC était en moyenne de 46,42 % entre les trois tests et l’utilisation moyenne de la mémoire, exprimée sous forme de pourcentage des octets validés de la mémoire totale disponible de 4 Go, était de 25,95 %.  
  
||||||||  
|-|-|-|-|-|-|-|  
|**Scénario**|**CPUAvg (à partir du compteur)**|**Mbits/s (côté entreprise)**|**Mbits/s (côté Internet)**|**QMSA active**|**MMSA active**|**Utilisation des mém. (système 4 Go)**|  
|**MATÉRIEL bas de gamme.  562 clients Teredo.  188 clients IPHTTPS.**|47,7472542|84,3|119,13|1502,05|1502,1|26,27%|  
|**MATÉRIEL bas de gamme.  562 clients Teredo.  188 clients IPHTTPS.**|46,3889778|84,146|118,73|1501,25|1501,2|25,90%|  
|**MATÉRIEL bas de gamme.  562 clients Teredo.  188 clients IPHTTPS.**|45,113082|84,0494|118,43|1546,14|1546,1|25,68%|  
  
**1000 clients, 70/30 fractionnement, 78 Mbits/s débit :**  
  
Les trois tests suivants représentent les performances sur le matériel bas de gamme. Les tests exécutés ci-dessous portaient sur 1000 clients avec un débit moyen de 78,64 Mbits/s et un trafic réparti sur 700 Teredo et 300 IPHTTPS.  L’unité MTU Teredo a été définie sur 1472 et la dérivation Teredo a été activée.  L’utilisation de l’UC était en moyenne de 50,7% et l’utilisation moyenne de la mémoire, exprimée sous forme de pourcentage des octets validés de la mémoire totale disponible de 4 Go, était de 28,7%.  
  
||||||||  
|-|-|-|-|-|-|-|  
|**Scénario**|**CPUAvg (à partir du compteur)**|**Mbits/s (côté entreprise)**|**Mbits/s (côté Internet)**|**QMSA active**|**MMSA active**|**Utilisation des mém. (système 4 Go)**|  
|**MATÉRIEL bas de gamme.  700 clients Teredo.  300 clients IPHTTPS.**|51,28406247|78,6432|113,19|2002,42|1502,1|25,59%|  
|**MATÉRIEL bas de gamme.  700 clients Teredo.  300 clients IPHTTPS.**|51,06993128|78,6402|113,22|2001,4|1501,2|30,87%|  
|**MATÉRIEL bas de gamme.  700 clients Teredo.  300 clients IPHTTPS.**|49,75235617|78,6387|113,2|2002,6|1546,1|30,66%|  
  
**1000 clients, 70/30 fractionnement, 109 Mbits/s débit :**  
  
Les trois tests suivants exécutés portaient sur 1000 clients avec un débit moyen de 109,2 Mbits/s et un trafic réparti sur 700 Teredo et 300 IPHTTPS. L’unité MTU Teredo a été définie sur 1472 et la dérivation Teredo a été activée. L’utilisation de l’UC était en moyenne de 59,06% entre les trois tests et l’utilisation moyenne de la mémoire, exprimée sous forme de pourcentage des octets validés de la mémoire totale disponible de 4 Go, était de 27,34%.  
  
||||||||  
|-|-|-|-|-|-|-|  
|**Scénario**|**CPUAvg (à partir du compteur)**|**Mbits/s (côté entreprise)**|**Mbits/s (côté Internet)**|**QMSA active**|**MMSA active**|**Utilisation des mém. (système 4 Go)**|  
|**MATÉRIEL bas de gamme.  700 clients Teredo.  300 clients IPHTTPS.**|59,81640675|108,305|153,14|2001,64|2001,6|24,38%|  
|**MATÉRIEL bas de gamme.  700 clients Teredo.  300 clients IPHTTPS.**|59,46473798|110,969|157,53|2005,22|2005,2|28,72%|  
|**MATÉRIEL bas de gamme.  700 clients Teredo.  300 clients IPHTTPS.**|57,89089768|108,305|153,14|1999,53|2018,3|24,38%|  
  
## <a name="testing-results-for-high-end-hardware"></a>Résultats des tests pour le matériel haut de gamme :  
Les tests ont été effectués avec 1500 clients. La répartition du trafic était de 70 % pour Teredo et 30 % pour IPHTTPS. Tous les tests impliquaient le trafic TCP sur Nat64 avec 2 tunnels IPsec par client. Dans tous les tests, l’utilisation de la mémoire était légère et celle de l’UC acceptable.  
  
**Résultats des tests individuel :**  
  
Les sections suivantes décrivent les tests individuels. Chaque titre de section met en évidence les éléments clés de chaque test, puis une description résumée des résultats et un diagramme contenant les données de résultat détaillées.  
  
**1500 clients, 70/30 fractionnement, 153,2 Mbits/s débit**  
  
Les cinq tests suivants représentent le matériel haut de gamme. Les tests exécutés ci-dessous portaient sur 1500 clients avec un débit moyen de 153,2 Mbits/s et un trafic réparti sur 1050 Teredo et 450 IPHTTPS. L’utilisation de l’UC était en moyenne de 50,68% entre les cinq tests et l’utilisation moyenne de la mémoire, exprimée sous forme de pourcentage des octets validés de la mémoire totale disponible de 8 Go, était de 22,25%.  
  
||||||||  
|-|-|-|-|-|-|-|  
|**Scénario**|**CPUAvg (à partir du compteur)**|**Mbits/s (côté entreprise)**|**Mbits/s (côté Internet)**|**QMSA active**|**MMSA active**|**Utilisation des mém. (système 4 Go)**|  
|**MATÉRIEL haut de gamme.  1050 clients Teredo.  450 clients IPHTTPS.**|51,712437|157,029|216,29|3000,31|3046|21,58%|  
|**MATÉRIEL haut de gamme.  1050 clients Teredo.  450 clients IPHTTPS.**|48,86020205|151,012|206,53|3002,86|3045,3|21,15%|  
|**MATÉRIEL haut de gamme.  1050 clients Teredo.  450 clients IPHTTPS.**|52,23979519|155,511|213,45|3001,15|3002,9|22,90%|  
|**MATÉRIEL haut de gamme.  1050 clients Teredo.  450 clients IPHTTPS.**|51,26269767|155,09|212,92|3000,74|3002,4|22,91%|  
|**MATÉRIEL haut de gamme.  1050 clients Teredo.  450 clients IPHTTPS.**|50,15751307|154,772|211,92|3000,9|3002,1|22,93%|  
|**MATÉRIEL haut de gamme.  1050 clients Teredo.  450 clients IPHTTPS.**|49,83665607|145,994|201,92|3000,51|3006|22,03%|  
  
![Résultats du test de matériel haut de gamme](../../media/DirectAccess-Capacity-Planning/DACapacityPlanning3.gif)  
  


