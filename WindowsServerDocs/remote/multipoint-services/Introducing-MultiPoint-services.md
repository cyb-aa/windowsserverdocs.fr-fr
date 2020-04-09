---
title: Présentation de MultiPoint Services
description: Fournit une vue d’ensemble de MultiPoint services, un moyen de permettre à plusieurs utilisateurs de partager un système
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 1cbef744-4661-4ba9-9e2b-0bbd8854fd5c
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: e547156bb46d7195baa64a0094f1d3ca432eb016
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859172"
---
# <a name="introducing-multipoint-services"></a>Présentation de MultiPoint Services
Le rôle MultiPoint services dans Windows Server 2016 permet à plusieurs utilisateurs, chacun avec leur propre expérience Windows indépendante et familière, de partager simultanément un ordinateur. Plusieurs méthodes permettent aux utilisateurs d’accéder à leurs sessions. L’une des méthodes consiste à communiquer à distance sur le serveur à l’aide des [applications de bureau à distance](../remote-desktop-services/clients/remote-desktop-clients.md) avec n’importe quel appareil. Une autre méthode consiste à utiliser les stations physiques connectées au serveur MultiPoint :  
  
-   Directement sur les ports vidéo de l’ordinateur  
  
-   Par le biais de clients sans fil USB spécialisés (également appelés Hubs USB multifonctions), ainsi que par le biais de périphériques USB-sur-Ethernet similaires.  
  
-   Sur le réseau local (LAN)  
  
Chacune de ces méthodes est décrite plus en détail dans les [stations multipoint services](MultiPoint-services-Stations.md) , plus loin dans ce document.  
  
Ce document aborde les facteurs suivants à prendre en compte lorsque vous envisagez de déployer MultiPoint services :  
  
-   Quels sont les types de postes de travail à utiliser avec votre système MultiPoint services : vous aurez besoin de sessions, de machines virtuelles ou de PC Windows ?  
  
-   [Sélection du matériel pour votre système multipoint services](Selecting-Hardware-for-Your-MultiPoint-services-System.md): quelles décisions matérielles devez-vous prendre ?  
  
-   [Configuration matérielle requise et recommandations relatives aux performances](Hardware-Requirements-and-Performance-Recommendations.md): quel matériel est requis pour multipoint services ?  
  
-   [Planification de site multipoint services](MultiPoint-services-Site-Planning.md): où sont situés les ordinateurs qui exécutent multipoint services et leurs stations, et comment seront-ils configurés ?  
  
-   [Considérations relatives au réseau et comptes d’utilisateur](Network-Considerations-and-User-Accounts.md): l’environnement de mise en réseau dans lequel le système multipoint services est déployé peut affecter la manière dont les comptes d’utilisateur sont gérés. Qu’est-ce que votre environnement réseau ? Comment les comptes d’utilisateur seront-ils gérés ?  
  
-   [Stockage de fichiers avec multipoint services](Storing-Files-with-MultiPoint-services.md): où seront stockés les fichiers utilisateur et comment seront-ils accessibles ?  
  
-   [Liste de vérification de prédéploiement](Predeployment-Checklist.md)  