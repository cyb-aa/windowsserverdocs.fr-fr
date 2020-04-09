---
title: Configuration matérielle requise et recommandations relatives aux performances
description: Fournit les spécifications matérielles et de performances et les recommandations pour MultiPoint services
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 99a5c9c2-270f-4753-a28c-434882c03125
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: dcb139cddf6a7838511365c6a85dc12bd06a81eb
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80820342"
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
|**Améliorer**<p>Office, navigation Web, applications métier|PROCESSEUR : 2C<p>RAM : 2 GO|PROCESSEUR : 2C<p>RAM : 4 Go|UC : 4C<p>RAM : 6 GO|UC : 4C<p>RAM : 8 GO|UC : 4C + MT ou 6C<p>RAM : 10 GO| UC : 6C + MT<p>RAM : 12 GO|
|**Majuscule**<p>Office, navigation Web, applications métier et utilisation vidéo occasionnelle par certains utilisateurs|PROCESSEUR : 2C<p>RAM : 2 GO|PROCESSEUR : 2C<p>RAM : 4 Go|UC : 4C<p>RAM : 6 GO|UC : 4C + MT ou 6C<p>RAM : 8 GO|UC : 6C + MT<p>RAM : 10 GO| UC : 6C + MT<p>RAM : 12 GO| 
|**Utilisation intensive de la vidéo**<p>Office, navigation Web, applications métier et utilisation vidéo fréquente par tous les utilisateurs **Remarque :** le test vidéo a été effectué à l’aide de la vidéo 360p H. 264 à la résolution native.|UC : 4C + MT<p>RAM : 2 GO|UC : 6C + MT<p>RAM : 4 Go|PROCESSEUR : 8C + MT<p>RAM : 6 GO|UC : 12C + MT<p>RAM : 8 GO|UC : 16C + MT<p>RAM : 10 GO<p>-Client léger : RemoteFX<br />-Vidéo USB non recommandée| UC : 20-20 + MT<p>RAM : 12 GO<p>-Client léger : RemoteFX<br />-Vidéo USB non recommandée|   
  
## <a name="minimum-recommended-hardware-for-running-full-windows-10-virtual-desktops"></a>Configuration matérielle minimale recommandée pour l’exécution de bureaux virtuels Windows 10 complets  
L’exécution d’une instance de système d’exploitation virtuelle complète pour chaque station est plus gourmande en ressources de calcul que l’exécution des sessions MultiPoint Desktop par défaut. par conséquent, les exigences matérielles de l’ordinateur hôte par station sont plus élevées :  
  
1.  UC : 1 cœur ou thread par station  
  
2.  SSD (Solid State Drive)  
  
    1.  Capacité > = 20 Go par station + 40 Go pour le système d’exploitation hôte WMS  
  
    2.  E/s par seconde en lecture/écriture aléatoire > = 3K par station  
  
3.  RAM > = 2 Go par station + 2 Go pour le système d’exploitation hôte WMS  
  
Le paramètre de l’UC du BIOS a été configuré pour activer la virtualisation – traduction d’adresses de second niveau (SLAT)  
  
Pour plus d’informations sur le choix du meilleur matériel MultiPoint services pour vos besoins, contactez votre fournisseur de matériel.  