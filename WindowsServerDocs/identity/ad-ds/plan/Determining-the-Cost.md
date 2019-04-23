---
ms.assetid: e3ea1f67-60d4-4566-b24c-37faa95c3b2a
title: Détermination du coût
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0b34f1672311768d644c467fda10dc2fc643282d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847080"
---
# <a name="determining-the-cost"></a>Détermination du coût

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vous affectez aux liens de sites à favoriser les connexions peu coûteuses sur les connexions coûteuses. Certaines applications et services, tels que le localisateur de contrôleur de domaine (DCLocator) et distribuée fichier système espaces de noms (DFSN), vous devez également utilisent les informations de coût pour localiser les ressources le plus proche. Coût du lien de site peut être utilisé pour déterminer quel contrôleur de domaine est contacté par les clients dans un site si le contrôleur de domaine pour le domaine spécifié n’existe pas sur ce site. Le client contacte le contrôleur de domaine en utilisant le lien de site qui est le plus faible qui lui est assignée.  
  
Nous recommandons que la valeur de coût être définie sur une base de l’échelle du site. Coût repose généralement non seulement sur la bande passante totale de la liaison, mais également sur la disponibilité, la latence et le coût monétaire de la liaison.  
  
Pour déterminer les coûts pour placer des liaisons de sites, documentez la vitesse de connexion pour chaque lien de site. Reportez-vous à la feuille de calcul « Géographique emplacements et liaisons de Communication » (DSSTOPO_1.doc) [collecte des informations réseau](../../ad-ds/plan/Collecting-Network-Information.md) pour plus d’informations sur la vitesse de connexion que vous avez identifié.  
  
Le tableau suivant répertorie les vitesses pour différents types de réseaux.  
  
|Type de réseau|Vitesse|  
|----------------|---------|  
|Très lent|56 Ko par seconde (Kbits/s)|  
|Lent|64 Kbits/s|  
|Réseau numérique à intégration de Services (RNIS)|64 Kbits/s ou 128 Kbits/s|  
|Relais de trames|Taux variable, généralement entre 56 Kbits/s et 1,5 mégabits par seconde (Mbits/s)|  
|T1|1,5 Mbits/s|  
|T3|45 Mbits/s|  
|10BaseT|10 Mbits/s|  
|Mode de transfert asynchrone (ATM)|Taux variable, généralement entre 155 et 622 Mbits/s|  
|100BaseT|100 Mbits/s|  
|Gigabit Ethernet|1 gigabit par seconde (Gbits/s)|  
  
Utilisez le tableau suivant pour calculer le coût de chaque liaison de site en fonction de la vitesse de liaison de vitesse (étendu WAN) de réseau étendu. Pour la vitesse de liaison réseau étendu n’est pas répertoriée dans la table, vous pouvez calculer un facteur de coût relatif en divisant les 1 024 par le journal de la bande passante disponible, tel que mesuré en Kbits/s.  
  
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
  
Ces coûts ne reflètent pas les différences en matière de fiabilité entre les liens de réseau. Définir des coûts plus élevés sur des liens enclin aux défaillances réseau afin que vous n’êtes pas obligé de s’appuient sur ces liens pour la réplication. Par paramètre supérieur coûts, vous pouvez contrôler le basculement de la réplication en cas d’échec d’un lien de sites.  
  


