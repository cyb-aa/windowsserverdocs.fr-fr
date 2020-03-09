---
title: Configuration matérielle requise et recommandations relatives aux performances
description: Fournit les spécifications matérielles et de performances et les recommandations pour MultiPoint services
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 99a5c9c2-270f-4753-a28c-434882c03125
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 284131028b308ee86389f25102d934390ba2f16d
ms.sourcegitcommit: 06ae7c34c648538e15c4d9fe330668e7df32fbba
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78371869"
---
# <a name="hardware-requirements-and-performance-recommendations"></a>Configuration matérielle requise et recommandations relatives aux performances
Cette rubrique décrit le matériel requis pour exécuter un système MultiPoint services et prendre en charge les scénarios d’application utilisateur. Le scénario utilisateur affecte directement les besoins en bande passante du processeur, de la RAM et du réseau.  

## <a name="optimize-multipoint-services-system-performance"></a>Optimiser les performances du système MultiPoint services  
Les performances de votre système MultiPoint services sont directement affectées par la capacité du processeur, du GPU et de la quantité de RAM disponible sur l’ordinateur qui exécute MultiPoint services.  
  
### <a name="applications-and-internet-content"></a>Applications et contenu Internet  
MultiPoint services étant une solution de calcul de ressources partagées, le type et le nombre d’applications qui s’exécutent sur les stations peuvent avoir un impact sur les performances de votre système MultiPoint services. Il est important de prendre en compte les types de programmes utilisés régulièrement lorsque vous planifiez votre système. Par exemple, une application gourmande en ressources graphiques requiert un ordinateur plus puissant qu’une application telle qu’un traitement de texte. La surcharge de l’ordinateur avec des applications gourmandes en graphiques entraînera probablement des problèmes de décalage dans l’ensemble du système.  
  
Le type de contenu accessible par les applications affecte également les performances du système. Si plusieurs stations utilisent des navigateurs Web pour accéder à du contenu multimédia, par exemple une vidéo en mode plein écran, vous pouvez connecter moins de stations avant de nuire aux performances du système. À l’inverse, si les différentes stations utilisent des navigateurs Web pour accéder au contenu Web statique, davantage de stations peuvent être connectées sans incidence significative sur les performances.  
  
### <a name="hardware-recommendations"></a>Recommandations matérielles  
Pour obtenir de bonnes performances avec votre système MultiPoint services sous différentes charges, suivez les instructions du tableau suivant lorsque vous planifiez et testez votre système. Il s’agit des services forMultiPoint requis de base. Le dimensionnement réel de la configuration dépend de la configuration de votre système, de la charge de travail que vous exécutez et de la fonctionnalité matérielle. Vous devez toujours valider en testant vos applications et votre matériel.  
  
> [!NOTE]  
> 2C = 2 cœurs, 4C = 4 cœurs, 6C = 6 cœurs, MT = multithread. La vitesse du processeur doit être d’au moins 2,0 gigahertz (GHz).  
  
### <a name="minimum-recommended-hardware-for-running-default-multipoint-server-stations"></a>Configuration matérielle minimale recommandée pour l’exécution de stations de serveurs MultiPoint par défaut  
  
|Scénario d’application|Jusqu’à 5 stations|Stations 6-8|Stations 9-12|Stations 13-16|Stations 17-20|Stations 21-24|  
|------------------------|----------------------|-------------------|------------------|-------------------|-------------------|-----------------|  
|**Améliorer**<br /><br />Office, navigation Web, applications métier|PROCESSEUR : 2C<br /><br />RAM : 2 GO|PROCESSEUR : 2C<br /><br />RAM : 4 Go|UC : 4C<br /><br />RAM : 6 GO|UC : 4C<br /><br />RAM : 8 GO|UC : 4C + MT ou 6C<br /><br />RAM : 10 GO| UC : 6C + MT<br /><br />RAM : 12 GO|
|**Majuscule**<br /><br />Office, navigation Web, applications métier et utilisation vidéo occasionnelle par certains utilisateurs|PROCESSEUR : 2C<br /><br />RAM : 2 GO|PROCESSEUR : 2C<br /><br />RAM : 4 Go|UC : 4C<br /><br />RAM : 6 GO|UC : 4C + MT ou 6C<br /><br />RAM : 8 GO|UC : 6C + MT<br /><br />RAM : 10 GO| UC : 6C + MT<br /><br />RAM : 12 GO| 
|**Utilisation intensive de la vidéo**<br /><br />Office, navigation Web, applications métier et utilisation vidéo fréquente par tous les utilisateurs **Remarque :** le test vidéo a été effectué à l’aide de la vidéo 360p H. 264 à la résolution native.|UC : 4C + MT<br /><br />RAM : 2 GO|UC : 6C + MT<br /><br />RAM : 4 Go|PROCESSEUR : 8C + MT<br /><br />RAM : 6 GO|UC : 12C + MT<br /><br />RAM : 8 GO|UC : 16C + MT<br /><br />RAM : 10 GO<br /><br />-Client léger : RemoteFX<br />-Vidéo USB non recommandée| UC : 20-20 + MT<br /><br />RAM : 12 GO<br /><br />-Client léger : RemoteFX<br />-Vidéo USB non recommandée|   
  
## <a name="minimum-recommended-hardware-for-running-full-windows-10-virtual-desktops"></a>Configuration matérielle minimale recommandée pour l’exécution de bureaux virtuels Windows 10 complets  
L’exécution d’une instance de système d’exploitation virtuelle complète pour chaque station est plus gourmande en ressources de calcul que l’exécution des sessions MultiPoint Desktop par défaut. par conséquent, les exigences matérielles de l’ordinateur hôte par station sont plus élevées :  
  
1.  UC : 1 cœur ou thread par station  
  
2.  SSD (Solid State Drive)  
  
    1.  Capacité > = 20 Go par station + 40 Go pour le système d’exploitation hôte WMS  
  
    2.  E/s par seconde en lecture/écriture aléatoire > = 3K par station  
  
3.  RAM > = 2 Go par station + 2 Go pour le système d’exploitation hôte WMS  
  
Le paramètre de l’UC du BIOS a été configuré pour activer la virtualisation – traduction d’adresses de second niveau (SLAT)  
  
Pour plus d’informations sur le choix du meilleur matériel MultiPoint services pour vos besoins, contactez votre fournisseur de matériel.  