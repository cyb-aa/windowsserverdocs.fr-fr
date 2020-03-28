---
ms.assetid: ''
title: Insider Preview pour les fonctionnalités de service de temps Windows dans Windows Server 2019
description: Nouvelles fonctionnalités de service de temps Windows dans Windows Server 2019
author: eross-msft
ms.author: lizross
ms.date: 09/05/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: 3c61b1ca7a11e01d2d8234cb505998946a774691
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80315013"
---
# <a name="insider-preview"></a>Insider Preview 


## <a name="leap-second-support"></a>Prise en charge des secondes intercalaires


>S'applique à : Windows Server 2019 et Windows 10, version 1809

Une seconde intercalaire est un ajustement occasionnel de 1 seconde du temps UTC. À mesure que la rotation de la Terre ralentit, le temps [UTC](https://en.wikipedia.org/wiki/Coordinated_Universal_Time) (échelle de temps atomique) s’écarte du [temps solaire moyen](https://en.wikipedia.org/wiki/Solar_time#Mean_solar_time) ou du temps astronomique.  Une fois que l’heure UTC s’est écartée d’au plus 0,9 seconde, une [seconde intercalaire](https://en.wikipedia.org/wiki/Leap_second) est insérée pour maintenir l’heure UTC synchronisée avec le temps solaire moyen.

Les secondes intercalaires sont devenues importantes pour répondre aux exigences réglementaires de précision et de traçabilité à la fois aux États-Unis et dans l’Union européenne.

Pour plus d’informations, voir :

-  Notre [blog d’annonces](https://blogs.technet.microsoft.com/networking/2018/07/18/top10-ws2019-hatime/)

-  Guide de validation pour les [développeurs](https://aka.ms/Dev-LeapSecond)

-  Guide de validation pour les [professionnels de l’informatique](https://aka.ms/ITPro-LeapSecond)


## <a name="precision-time-protocol"></a>Protocole PTP (Precision Time Protocol)

>S'applique à : Windows Server 2019 et Windows 10, version 1809

Un nouveau fournisseur de temps inclus dans Windows Server 2019 et Windows 10 (version 1809) vous permet de synchroniser l’heure à l’aide du protocole PTP (Precision Time Protocol). Quand l’heure est distribuée sur un réseau, elle rencontre un délai (latence) qui, s’il n’est pas pris en compte ou n’est pas symétrique, complique la compréhension de l’horodatage envoyé du serveur de temps. Le protocole PTP permet aux appareils réseau d’ajouter la latence introduite par chacun d’eux dans les mesures de synchronisation, fournissant ainsi au client Windows un échantillon de temps beaucoup plus précis.

Pour plus d’informations, voir :

-  Notre [blog d’annonces](https://blogs.technet.microsoft.com/networking/2018/07/18/top10-ws2019-hatime/)

-  Guide de validation pour les [professionnels de l’informatique](https://aka.ms/PTPValidation)


## <a name="software-timestamping"></a>Horodatage logiciel

>S'applique à : Windows Server 2019 et Windows 10, version 1809

Quand un paquet de synchronisation est reçu sur le réseau à partir d’un serveur de temps, il doit être traité par la pile réseau du système d’exploitation avant d’être consommé dans le service de temps. Chaque composant de la pile réseau introduit une quantité variable de latence qui affecte la précision de la mesure de synchronisation.

![Horodatage logiciel](../media/Windows-Time-Service/software-timestamping.png)

Pour résoudre ce problème, l’horodatage logiciel nous permet d’horodater les paquets avant et après les « composants réseau Windows » indiqués ci-dessus pour tenir compte du délai dans le système d’exploitation.

Pour plus d’informations, voir :

-  Notre [blog d’annonces](https://blogs.technet.microsoft.com/networking/2018/07/18/top10-ws2019-hatime/)

-  Guide de validation pour les [professionnels de l’informatique](https://github.com/Microsoft/SDN/blob/master/FeatureGuide/Validation%20Guide%20-%20RS5%20-%20Software%20Timestamping.docx)



---