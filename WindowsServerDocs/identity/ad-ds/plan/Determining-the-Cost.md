---
ms.assetid: e3ea1f67-60d4-4566-b24c-37faa95c3b2a
title: Détermination du coût
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 1bbc75cab2e78d1001fa13d419072807f202fd61
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822582"
---
# <a name="determining-the-cost"></a>Détermination du coût

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vous attribuez des valeurs de coût aux liens de sites pour favoriser les connexions peu onéreuses sur des connexions coûteuses. Certains services et applications, tels que le localisateur de contrôleur de domaine (du localisateur) et les espaces de noms système de fichiers DFS (DFSN), utilisent également les informations de coût pour localiser les ressources les plus proches. Le coût d’un lien de site peut être utilisé pour déterminer quel contrôleur de domaine est contacté par les clients dans un site si le contrôleur de domaine pour le domaine spécifié n’existe pas sur ce site. Le client contacte le contrôleur de domaine à l’aide du lien de sites auquel est affecté le coût le plus bas.  
  
Nous vous recommandons de définir la valeur de coût sur l’ensemble du site. Le coût est généralement basé non seulement sur la bande passante totale du lien, mais également sur la disponibilité, la latence et le coût monétaire du lien.  
  
Pour déterminer les coûts à placer sur les liens de sites, documentez la vitesse de connexion de chaque lien de sites. Reportez-vous à la feuille de calcul « emplacements géographiques et liens de communication » (DSSTOPO_1. doc) dans [collecte des informations réseau](../../ad-ds/plan/Collecting-Network-Information.md) pour plus d’informations sur la vitesse de connexion que vous avez identifiée.  
  
Le tableau suivant répertorie les vitesses pour les différents types de réseaux.  
  
|Type de réseau|Vitesse|  
|----------------|---------|  
|Très lent|56 Ko par seconde (Ko/s)|  
|Lent|64 Kbits/s|  
|Réseau numérique à intégration de services (RNIS)|64 kbit/s ou 128 kbit/s|  
|Relais de trames|Vitesse variable, généralement comprise entre 56 kbit/s et 1,5 mégabits par seconde (Mbits/s)|  
|T1|1,5 Mbits/s|  
|T3|45 Mbits/s|  
|10Baset|10 Mbits/s|  
|Mode de transfert asynchrone (ATM)|Vitesse variable, généralement comprise entre 155 Mbits/s et 622 Mbits/s|  
|100Baset|100 Mbits/s|  
|Gigabit Ethernet|1 Go par seconde (Go/s)|  
  
Utilisez le tableau suivant pour calculer le coût de chaque lien de site en fonction de la vitesse de liaison du réseau étendu (WAN). Pour la vitesse de liaison de réseau étendu qui ne figure pas dans le tableau, vous pouvez calculer un facteur de coût relatif en divisant 1 024 par le journal de la bande passante disponible, mesuré en Kbits/s.  
  
|Bande passante disponible (kbps)|Coût|  
|--------------------------------|--------|  
|9,6|1 042|  
|19,2|798|  
|38,4|644|  
|56|586|  
|64|567|  
|128|486|  
|256|425|  
|512|378|  
|1 024|340|  
|2,048|309|  
|4 096|283|  
  
Ces coûts ne reflètent pas les différences de fiabilité entre les liaisons réseau. Définissez des coûts plus élevés sur les liaisons réseau sujettes aux défaillances, afin de ne pas avoir à vous appuyer sur ces liens pour la réplication. En définissant des coûts de liens de sites supérieurs, vous pouvez contrôler le basculement de réplication en cas d’échec d’une liaison de site.  
  


