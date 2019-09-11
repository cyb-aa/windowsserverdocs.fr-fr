---
ms.assetid: ''
title: Version préliminaire d’Insider pour les fonctionnalités du service de temps Windows dans Windows Server 2019
description: Nouvelles fonctionnalités du service de temps Windows dans Windows Server 2019
author: shortpatti
ms.author: pashort
ms.date: 09/05/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: a0c4d01d095e8052a2192d6b6352732a6fe60919
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871790"
---
# <a name="insider-preview"></a>Insider preview 


## <a name="leap-second-support"></a>Prise en charge des secondes bissextiles


>S’applique à : Windows Server 2019 et Windows 10, version 1809

Une seconde bissextile est un ajustement occasionnel de 1 seconde à l’heure UTC. À mesure que la rotation de la terre ralentit, l’heure [UTC](https://en.wikipedia.org/wiki/Coordinated_Universal_Time) (une échelle de temps atomique) divergent du [temps solaire moyen](https://en.wikipedia.org/wiki/Solar_time#Mean_solar_time) ou du temps de l’astronomie.  Une fois que l’heure UTC s’est diverged d’au plus 0,9 secondes, une [seconde bissextile](https://en.wikipedia.org/wiki/Leap_second) est insérée pour conserver l’heure UTC synchronisée avec la durée solaire moyenne.

Les secondes bissextiles sont devenues importantes pour répondre aux exigences réglementaires de précision et de traçabilité à la fois dans le États-Unis et dans l’Union européenne.

Pour plus d'informations, voir :

-  Notre [blog d’annonce](https://blogs.technet.microsoft.com/networking/2018/07/18/top10-ws2019-hatime/)

-  Guide de validation pour les [développeurs](https://aka.ms/Dev-LeapSecond)

-  Guide de validation pour les professionnels de l' [informatique](https://aka.ms/ITPro-LeapSecond)


## <a name="precision-time-protocol"></a>Protocole de temps de précision

>S’applique à : Windows Server 2019 et Windows 10, version 1809

Un nouveau fournisseur de temps inclus dans Windows Server 2019 et Windows 10 (version 1809) vous permet de synchroniser l’heure à l’aide du protocole PTP (Precision Time Protocol). À mesure que le temps distribue sur un réseau, il rencontre un délai (latence) qui, s’il n’est pas pris en compte, ou s’il n’est pas symétrique, il devient de plus en plus difficile de comprendre l’horodatage envoyé à partir du serveur de temps. PTP permet aux périphériques réseau d’ajouter la latence introduite par chaque périphérique réseau dans les mesures de synchronisation, ce qui fournit un exemple d’heure beaucoup plus précis au client Windows.

Pour plus d'informations, voir :

-  Notre [blog d’annonce](https://blogs.technet.microsoft.com/networking/2018/07/18/top10-ws2019-hatime/)

-  Guide de validation pour les professionnels de l' [informatique](https://aka.ms/PTPValidation)


## <a name="software-timestamping"></a>Horodatage logiciel

>S’applique à : Windows Server 2019 et Windows 10, version 1809

Lors de la réception d’un paquet de synchronisation sur le réseau à partir d’un serveur de temps, il doit être traité par la pile de mise en réseau du système d’exploitation avant d’être consommé dans le service de temps. Chaque composant de la pile de mise en réseau introduit une quantité variable de latence qui affecte la précision de la mesure de synchronisation.

![horodatage logiciel](../media/Windows-Time-Service/software-timestamping.png)

Pour résoudre ce problème, l’horodatage logiciel nous permet d’horodater les paquets avant et après les « composants réseau Windows » indiqués ci-dessus pour tenir compte du délai dans le système d’exploitation.

Pour plus d'informations, voir :

-  Notre [blog d’annonce](https://blogs.technet.microsoft.com/networking/2018/07/18/top10-ws2019-hatime/)

-  Guide de validation pour les professionnels de l' [informatique](https://github.com/Microsoft/SDN/blob/master/FeatureGuide/Validation%20Guide%20-%20RS5%20-%20Software%20Timestamping.docx)



---