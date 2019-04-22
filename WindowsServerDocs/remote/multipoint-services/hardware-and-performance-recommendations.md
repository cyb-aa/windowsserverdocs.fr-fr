---
title: Configuration matérielle requise et recommandations relatives aux performances
description: Fournit des recommandations pour MultiPoint Services et les exigences matérielles et de performances
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 99a5c9c2-270f-4753-a28c-434882c03125
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: b47f4c224d4a4f6f4aa104b6d5ee5d93590ac0c4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823390"
---
# <a name="hardware-requirements-and-performance-recommendations"></a>Configuration matérielle requise et recommandations relatives aux performances
Cette rubrique décrit le matériel est requis pour exécuter un système MultiPoint Services et de prendre en charge les scénarios d’application utilisateur. Le scénario utilisateur affecte directement le processeur, mémoire RAM et besoins en bande passante réseau.  

## <a name="optimize-multipoint-services-system-performance"></a>Optimiser les performances du système MultiPoint Services  
Les performances de votre système MultiPoint Services seront directement affectées par la capacité de l’UC, le GPU et la quantité de mémoire vive disponible sur l’ordinateur qui exécute MultiPoint Services.  
  
### <a name="applications-and-internet-content"></a>Applications et contenu Internet  
MultiPoint Services étant une solution informatique de ressource partagée, le type et le nombre d’applications qui s’exécutent sur les stations peuvent avoir un impact sur les performances de votre système MultiPoint Services. Il est important de prendre en compte les types de programmes qui sont utilisés régulièrement lorsque vous planifiez votre système. Par exemple, une application de graphiques requiert un ordinateur plus puissant qu’une application telle qu’un traitement de texte. La surcharge de l’ordinateur avec les applications graphiques est susceptible de provoquer des problèmes de décalage tout au long de l’ensemble du système.  
  
Le type de contenu qui est accessible par les applications affecte également les performances du système. Si plusieurs stations sont à l’aide des navigateurs web pour accéder au contenu multimédia, telles que la vidéo, moins stations peuvent être connectées avant d’affecter les performances du système. À l’inverse, si plusieurs stations sont à l’aide de navigateurs web pour accéder au contenu web statique, plusieurs stations peuvent être connectées sans un effet significatif sur les performances.  
  
### <a name="hardware-recommendations"></a>Recommandations matérielles  
Pour obtenir de bonnes performances avec votre système MultiPoint Services sous différentes charges, utilisez les instructions dans le tableau suivant quand vous planifiez et test du système. Ceux-ci sont les exigences de base forMultiPoint Services. Le dimensionnement de la configuration réelle varie selon votre configuration système, la charge de travail que vous exécutez et les capacités matérielles. Vous devez toujours valider par tester vos applications et le matériel.  
  
> [!NOTE]  
> 2 C = 2 cœurs, 4, C = 4 cœurs, 6, C = 6 cœurs, MT = le multithreading. Vitesse du processeur doit être au moins 2.0 gigahertz (GHz).  
  
### <a name="minimum-recommended-hardware-for-running-default-multipoint-server-stations"></a>Minimum matériel recommandé pour les stations MultiPoint Server par défaut en cours d’exécution  
  
|Scénario d’application|Stations jusqu'à 5|6-8 stations|9-12 stations|Stations de 13 à 16|17-20 stations|Du 21 au 24 stations|  
|------------------------|----------------------|-------------------|------------------|-------------------|-------------------|-----------------|  
|**Productivité**<br /><br />Office, les applications de navigation, line-of-business web|UC : 2C<br /><br />RAM : 2 GO|UC : 2C<br /><br />RAM : 4 Go|UC : 4C<br /><br />RAM : 6 GO|UC : 4C<br /><br />RAM : 8 GO|UC : C + MT 4 ou 6C<br /><br />RAM : 10 Go| UC : 6C + MT<br /><br />RAM : 12 GO|
|**Mixed**<br /><br />Office, la navigation web, line of business applications et l’utilisation occasionnelle vidéo par certains utilisateurs|UC : 2C<br /><br />RAM : 2 GO|UC : 2C<br /><br />RAM : 4 Go|UC : 4C<br /><br />RAM : 6 GO|UC : C + MT 4 ou 6C<br /><br />RAM : 8 GO|UC : 6C + MT<br /><br />RAM : 10 Go| UC : 6C + MT<br /><br />RAM : 12 GO| 
|**Vidéo intensif**<br /><br />Office, navigation web, line of business applications et une utilisation fréquente vidéo par tous les utilisateurs **Remarque :** Vidéo de test a été effectuée à l’aide de la vidéo H.264 de 360p à résolution native.|UC : 4C + MT<br /><br />RAM : 2 GO|UC : 6C + MT<br /><br />RAM : 4 Go|UC : 8C + MT<br /><br />RAM : 6 GO|UC : 12C + MT<br /><br />RAM : 8 GO|UC : 16C + MT<br /><br />RAM : 10 Go<br /><br />-Un Client léger : RemoteFX<br />-Vidéo USB déconseillé| UC : 20C + MT<br /><br />RAM : 12 GO<br /><br />-Un Client léger : RemoteFX<br />-Vidéo USB déconseillé|   
  
## <a name="minimum-recommended-hardware-for-running-full-windows-10-virtual-desktops"></a>Minimum matériel recommandé pour des bureaux virtuels Windows 10 complètes en cours d’exécution  
Exécution d’une instance de système d’exploitation virtuel complet pour chaque station est que plus gourmandes en ressources de calcul que l’exécution les sessions de bureau par défaut MultiPoint, par conséquent, les exigences de matériel hôte par station sont plus élevés :  
  
1.  UC : 1 core ou thread par station  
  
2.  Solid State Drive (SSD)  
  
    1.  Capacité > = 20 Go par station + 40 Go pour le système d’exploitation hôte de WMS  
  
    2.  E/s de lecture/écriture aléatoires > = 3K par station  
  
3.  RAM > = 2 Go par station + 2 Go pour le système d’exploitation hôte de WMS  
  
Paramètres de l’UC de BIOS a été configuré pour activer la virtualisation – adresse traduction (second niveau)  
  
Pour plus d’informations sur le choix le meilleur matériel MultiPoint Services à vos besoins, contactez votre fournisseur de matériel.  