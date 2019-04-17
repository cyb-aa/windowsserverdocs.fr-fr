---
ms.assetid: e3ea1f67-60d4-4566-b24c-37faa95c3b2a
title: "Détermination du coût"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b32d1930fcef944fbd719338797f0f29b70fa29d
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="determining-the-cost"></a>Détermination du coût

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Vous affectez aux liens de sites de favoriser les connexions peu coûteuses sur des connexions coûteuses. Certaines applications et services, tels que le localisateur de contrôleur de domaine (DCLocator) et distribué fichier système espaces de noms (DFSN), également utilisent des informations sur les coûts de localiser les ressources le plus proche. Coût de lien de site peut servir à déterminer quel contrôleur de domaine est contacté par les clients dans un site si le contrôleur de domaine pour le domaine spécifié n’existe pas sur ce site. Le client contacte le contrôleur de domaine en utilisant le lien de site qui a le plus faible coût qui lui est assigné.  
  
Nous recommandons que la valeur de coût doit être définie en fonction de l’échelle du site. Coût est généralement basé sur la bande passante totale de la liaison mais également sur la disponibilité, la latence et le coût du lien.  
  
Pour déterminer les coûts de placer des liaisons de sites, documentez la vitesse de connexion pour chaque lien de site. Reportez-vous à la feuille de calcul (DSSTOPO_1.doc) «Géographique emplacements et des liaisons de Communication» dans [collecte des informations réseau](../../ad-ds/plan/Collecting-Network-Information.md) pour plus d’informations sur la vitesse de connexion que vous avez identifié.  
  
Le tableau suivant répertorie les vitesses pour différents types de réseaux.  
  
|Type de réseau|Vitesse|  
|----------------|---------|  
|Très lente|56kilobits par seconde (Kbits/s)|  
|Lent|64Kbits/s|  
|Réseau numérique d’intégration de Services (RNIS)|64 ou 128Kbits/s|  
|Relais de trame|Taux de variable, généralement entre 56Kbits/s et 1,5mégabits par seconde (Mbits/s)|  
|T1|1,5Mbits/s|  
|T3|45Mbits/s|  
|10BaseT|10Mbits/s|  
|Mode de transfert asynchrone (ATM)|Taux de variable, généralement entre 155Mbits/s et 622Mbits/s|  
|100BaseT|100Mbits/s|  
|Gigabit Ethernet|1gigabit par seconde (Gbits/s)|  
  
Utilisez le tableau suivant pour calculer le coût de chaque lien de site basé sur la vitesse du réseau étendu vitesse de liaison (WAN). Vitesse de liaison de réseau étendu n’est pas répertorié dans le tableau, vous pouvez calculer un facteur de coût relatif en divisant 1024 par le journal de la bande passante disponible, mesurée en Kbits/s.  
  
|Bande passante disponible (Kbits/s)|Coût|  
|--------------------------------|--------|  
|9.6|1,042|  
|19.2|798|  
|38.4|644|  
|56|586|  
|64|567|  
|128|486|  
|256|425|  
|512|378|  
|1,024|340|  
|2,048|309|  
|4,096|283|  
  
Ces frais ne reflètent pas de différences dans la fiabilité des liaisons de réseau. Définir des coûts plus élevés des liaisons réseau défaillants afin que vous n’êtes pas obligé de s’appuient sur ces liens pour la réplication. En plus coûts de lien de site paramètre, vous pouvez contrôler basculement de la réplication en cas d’échec d’un lien de sites.  
  


