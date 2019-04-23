---
ms.assetid: ''
title: Aperçu Insider pour les fonctionnalités de Service de temps Windows dans Windows Server 2019
description: Nouvelles fonctionnalités de Service de temps Windows dans Windows Server 2019
author: shortpatti
ms.author: pashort
ms.date: 09/05/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: ef0ff317f5957add5ecbe9f88ef83753b805ec41
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829020"
---
# <a name="insider-preview"></a>Insider preview 


## <a name="leap-second-support"></a>Prise en charge de saut de seconde


>S’applique à : Windows Server 2019 et Windows 10, version 1809

Un saut de seconde est un ajustement de 1 seconde occasionnel au format UTC. Comme le ralentissement de la rotation de la terre, [UTC](https://en.wikipedia.org/wiki/Coordinated_Universal_Time) (une échelle de temps atomique) diffère de [temps SOLAIRE](https://en.wikipedia.org/wiki/Solar_time#Mean_solar_time) ou heure astronomie.  Une fois que l’heure UTC est différente de.9 secondes au maximum, un [saut de seconde](https://en.wikipedia.org/wiki/Leap_second) est inséré pour que UTC restent synchronisés avec heure SOLAIRE moyenne.

Secondes intercalaires sont devenus importants répondre à la précision et la traçabilité réglementations dans les États-Unis et l’Union européenne.

Pour plus d'informations, consultez :

-  Notre [blog d’annonce](https://blogs.technet.microsoft.com/networking/2018/07/18/top10-ws2019-hatime/)

-  Guide de validation pour le [les développeurs](https://aka.ms/Dev-LeapSecond)

-  Guide de validation pour le [IT Pro](https://aka.ms/ITPro-LeapSecond)


## <a name="precision-time-protocol"></a>Protocole de temps de précision

>S’applique à : Windows Server 2019 et Windows 10, version 1809

Un nouveau fournisseur de temps inclus dans Windows Server 2019 et Windows 10 (version 1809) vous permet de synchroniser l’heure à l’aide de la précision temps PTP (Protocol). Comme temps distribue sur un réseau, il rencontre le retard (latence), qui si pas pris en compte, ou si elle n’est pas symétrique, il devient plus en plus difficile de comprendre l’horodatage envoyé à partir du serveur de temps. PTP permet aux périphériques réseau ajouter la latence introduite par chaque périphérique réseau dans les mesures de minutage offrant ainsi un échantillon de temps beaucoup plus précis pour le client windows.

Pour plus d'informations, consultez :

-  Notre [blog d’annonce](https://blogs.technet.microsoft.com/networking/2018/07/18/top10-ws2019-hatime/)

-  Guide de validation pour le [IT Pro](https://aka.ms/PTPValidation)


## <a name="software-timestamping"></a>Horodatage de logiciel

>S’applique à : Windows Server 2019 et Windows 10, version 1809

Lors de la réception d’un paquet de minutage sur le réseau à partir d’un serveur de temps, il doit être traité par la pile réseau du système d’exploitation avant d’être consommées dans le service de temps. Chaque composant dans la pile réseau présente une quantité variable de latence qui affecte la précision de la mesure de minutage.

![horodatage de logiciel](../media/Windows-Time-Service/software-timestamping.png)

Pour résoudre ce problème, horodatage du logiciel permet aux paquets d’horodatage avant et après le « Windows Networking composants » ci-dessus pour prendre en compte le délai dans le système d’exploitation.

Pour plus d'informations, consultez :

-  Notre [blog d’annonce](https://blogs.technet.microsoft.com/networking/2018/07/18/top10-ws2019-hatime/)

-  Guide de validation pour le [IT Pro](https://github.com/Microsoft/SDN/blob/master/FeatureGuide/Validation%20Guide%20-%20RS5%20-%20Software%20Timestamping.docx)



---