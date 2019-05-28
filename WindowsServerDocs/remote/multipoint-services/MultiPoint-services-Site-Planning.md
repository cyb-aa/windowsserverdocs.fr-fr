---
title: Planification de site MultiPoint Services
description: Informations de planification pour les déploiements de MultiPoint Services dans Windows Server 2016
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 063783cd-d748-489e-b175-46eadc993f7a
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: da27467efb842368167b7a315056506e99331e8d
ms.sourcegitcommit: 21165734a0f37c4cd702c275e85c9e7c42d6b3cb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2019
ms.locfileid: "65034597"
---
# <a name="multipoint-services-site-planning"></a>Planification de site MultiPoint Services
Vous devez envisager l’emplacement où un ou plusieurs ordinateurs exécutant MultiPoint Services et ses stations associées seront déployés.  
  
L’ordinateur qui exécute MultiPoint Services rôle doit avoir un accès pratique pour un bloc d’alimentation et les périphériques qui sont connectés directement, tel qu’une imprimante. En outre, l’ordinateur qui exécute MultiPoint Services doit avoir un accès pratique à une connexion réseau. Une connexion réseau est requise pour accéder à Internet et le cas échéant, un réseau local.  
  
Autres facteurs à prendre en considération sont les suivantes :  
  
-   Est le système MultiPoint Services être configuré dans une salle spécifique, ou s’il être configuré sur un panier propagée ou une table, afin qu’il peut être déplacé à partir d’un endroit à un autre ?  
  
    > [!NOTE]  
    > Si vous envisagez d’utiliser une configuration mobile, vous pouvez *associer* les stations avec MultiPoint Services chaque fois que vous vous reconnectez pour vous assurer que chaque ensemble clavier / souris sont associé à l’analyse appropriée.  
  
-   La station principale se trouve à côté d’autres stations, ou il sera distincte ? Par exemple, si le système MultiPoint Services est configuré dans une salle de classe, est la station principale sur le bureau de l’enseignant et les stations standards positionnée ailleurs dans la salle ? Lors du redémarrage de l’ordinateur qui exécute MultiPoint Services, la station principale ont accès aux écrans de démarrage. Si vous êtes inquiet à ce niveau d’accès de la classe, vous pouvez préférer à placer la station principale au support de l’enseignant.  
  
-   Combien de stations tiendra dans la salle ?  
  
-   Avez-vous besoin d’un réseau ? Une solution serveur unique qu’utilise vidéo direct connecté ou USB zéro client connecté stations ne nécessite pas d’un réseau.  
  
-   Y a-t-il suffisamment de connexions réseau dans la salle pour prendre en charge le nombre requis d’ordinateurs qui exécutent MultiPoint Services  
  
-   Où se trouvent prises de courant ?  
  
-   Avez-vous besoin d’un périphérique d’affichage supplémentaires, comme un projecteur ? Si vous envisagez d’utiliser un projecteur, se bloque au plafond, ou il positionné sur une table ?  
  
-   Le genre de câbles sera requis et combien va être nécessaire ?  
  
-   Prendre en compte la façon dont vous pouvez souhaiter développer à l’avenir. Vous ajouterez des stations plus ?  
  
## <a name="station-layout-and-configuration"></a>Configuration et mise en page de la station  
La disposition physique de votre site peut-être affecter votre choix du type de la station. Pour plus d’informations sur les types de l’autre station, reportez-vous à [Stations MultiPoint](MultiPoint-services-Stations.md) dans ce guide. Plusieurs types de station sont autorisées sur un seul MultiPoint Services. Cela vous offre une flexibilité supplémentaire pour répondre aux besoins de votre installation.  
  
### <a name="layout-for-direct-video-connected-stations"></a>Mise en page pour les stations connectées direct-vidéo  
  
-   Une station vidéo-connectés directement, la distance entre les analyses et l’ordinateur est limitée par la longueur du câble vidéo.  
  
-   Intermédiaires hubs ou concentrateurs de station enchaînés en série sont pris en charge pour la facilité de déploiement, mais le nombre maximal recommandé de hubs consécutifs est de trois. Cela signifie que la distance maximale à partir de l’ordinateur au concentrateur de station est 15 mètres, étant donné que chaque câble USB 2.0 a la longueur maximale de cinq mètres.  
  
> [!IMPORTANT]  
> Il doit toujours y avoir au moins une station de connectée vidéo directe par ordinateur d’agir en tant que la station principale.  
  
### <a name="layout-for-usb-zero-client-connected-stations"></a>Mise en page pour les stations USB zéro client connecté  
  
-   Intermédiaires hubs ou concentrateurs de station enchaînés en série sont pris en charge pour la facilité de déploiement, mais le nombre maximal recommandé de hubs consécutifs est de trois. Cela signifie que la distance maximale à partir de l’ordinateur au concentrateur de station est 15 mètres, étant donné que chaque câble USB 2.0 a la longueur maximale de cinq mètres.  
  
-   Le nombre maximal recommandé de USB zéro clients connectés à un seul hub intermédiaire est de trois.  
  
    > [!NOTE]  
    > Certains ordinateurs sont fournis avec un concentrateur générique sur la carte mère, ce qui a pour effet de l’ajout d’un hub supplémentaire entre le *concentrateur racine* de l’ordinateur et les concentrateurs de station.  
  
-   Si la vidéo sera largement utilisée, il est recommandé de vous connecter pas plus de deux clients USB zéro à un port USB sur le serveur. Par exemple, si un hub intermédiaire est utilisé, seulement deux clients USB zéro doivent connectés à celui-ci. Ou, si vous êtes daisy chaînage clients USB zéro, seuls deux USB zéro clients doivent être chaînés ensemble. L’ajout de chaque USB zéro client vers le port USB sur le serveur réduit la bande passante disponible.  
  
-   Si vous envisagez de connecter plus de trois clients USB zéro à un seul port USB sur le serveur, l’utilisation de USB 3.0 entre le serveur et le concentrateur intermédiaire est recommandée.  
  
> [!NOTE]  
> Il est recommandé que vous vérifiez les performances à l’aide de vos applications et matériel pour décider combien USB zéro des clients, vous pouvez vous connecter à un port USB sur le serveur.  
  
![Concentrateurs en aval](./media/WMS_diagram6.gif)  
  
**Figure 5** système MultiPoint Services avec trois USB zéro clients connectés à un seul hub intermédiaire  
  
### <a name="layout-for-rdp-over-lan-connected-stations"></a>Mise en page pour les stations de RDP sur le réseau local connecté  
Il n’existe aucune limite de distance physique pour les clients du réseau local. Tant qu’ils se trouvent sur le réseau local, ils peuvent se connecter au système MultiPoint Services.  
  
## <a name="using-additional-hubs"></a>À l’aide des hubs supplémentaires  
Hubs supplémentaires peut être utilisé pour simplifier l’installation. Il existe trois types de concentrateurs qui sont utilisés sur un système MultiPoint Services :  
  
-   [Concentrateurs de station](#station-hubs)  
  
-   [Hubs intermédiaires](#intermediate-hubs)  
  
-   [Concentrateurs en aval](#downstream-hubs)  
  
### <a name="station-hubs"></a>Concentrateurs de station  
Un concentrateur de station est un concentrateur externe qui a été associé à une station MultiPoint Services. Au minimum, le concentrateur de station aura un clavier branché à celui-ci. Elle peut également avoir des périphériques supplémentaires. Un concentrateur de station peut être un concentrateur USB générique qui est conforme à la norme USB 2.0 ou spécification ultérieure. Concentrateurs de station doivent être alimentés de façon externe si les appareils hautes performances vont des plug-in pour les.  
  
**Concentrateur racine** concentrateur USB de A qui est intégré au contrôleur sur la carte mère d’un ordinateur hôte est appelé un *concentrateur racine*. Concentrateurs de station sont généralement branché au concentrateur racine sur l’ordinateur qui exécute MultiPoint Services.  
  
> [!NOTE]  
> Hubs de racine ne doivent pas servir de concentrateurs de station. Lorsque les ports USB sont intégrés à un ordinateur, souvent il n’est pas possible de déterminer quel concentrateur USB racine, ils sont connectés en interne à. Par conséquent, si vous branché un clavier de la station et une souris directement aux ports USB de l’ordinateur, vous pourrez réellement être branchant-dans le clavier et la souris pour différents concentrateurs USB racine. Pour garantir que le clavier et la souris se trouvent sur le même hub, plug-in un concentrateur de station au port USB de l’ordinateur et puis plug-in le clavier et la souris pour que la station hub.  
  
**Stations en cascade** il peut être plus facile de connecter des concentrateurs de station vers un autre concentrateur de station plutôt que directement à l’ordinateur. Cela vous permet de vous connecter un concentrateur USB à un concentrateur de station qui est déjà branché à l’ordinateur, afin que vous ayez un concentrateur de station attaché à un autre concentrateur de station.  
  
Il doit être pas plus de trois clients USB zéro ou station hubs connectés en boucle consécutivement. Il convient que la bande passante USB n’est pas dépassée en cascade quand des concentrateurs de station.  
  
![Chaînage de stations](./media/WMS_diagram5.gif)  
  
**Figure 6** système MultiPoint Services avec les stations enchaînés en série  
  
### <a name="intermediate-hubs"></a>Hubs intermédiaires  
Un hub intermédiaire est un concentrateur est comprise entre le serveur et un concentrateur de station. Il est généralement utilisé pour augmenter le nombre de ports qui sont disponibles pour les concentrateurs de station ou pour étendre la distance des stations à partir de l’ordinateur. Il est recommandé que pas plus de deux hubs intermédiaires sont utilisés entre un concentrateur de station et le serveur.  
  
Hubs intermédiaires doivent être USB 2.0 ou version ultérieure, et ils doivent être alimentés de façon externe. USB 3.0 est recommandée entre le serveur et le concentrateur intermédiaire si vous vous connectez plus de trois clients USB zéro à un hub intermédiaire.  
  
### <a name="downstream-hubs"></a>Concentrateurs en aval  
Un concentrateur en aval est connecté à un concentrateur de station pour ajouter des ports supplémentaires disponibles pour les appareils de la station. Un concentrateur en aval peut être mise hors tension ou en externe alimenté par bus, selon les périphériques qui sont branché au hub.  
  
![Plusieurs connexions USB zéro client](./media/WMS_diagram4.gif "WMS_diagram4")  
  
**Figure 7** MultiPoint Services système avec un concentrateur intermédiaire, un concentrateur de station et un concentrateur en aval  
  
## <a name="users-stations-and-computers"></a>Les utilisateurs, les stations et les ordinateurs  
Le nombre de stations, que vous devez varie selon le nombre de personnes qui devront accéder aux ordinateurs exécutant MultiPoint Services en même temps. De même, le nombre d’ordinateurs qui exécutent MultiPoint Services, vous devez varie selon le nombre total de stations requis. Vidéo-connectés directement les stations, les stations de zéro-client-connectés par USB et les stations RDP-over-connectés au réseau local sont toutes les stations pris en compte. En outre, si la fonctionnalité fractionné est utilisée, chaque partie est considérée comme une station.  
  
## <a name="power-considerations"></a>Considérations relatives à la puissance  
Les composants suivants requièrent l’accès à une bande d’alimentation ou un outlet :  
  
-   Server  
-   Moniteurs
-   Les intermédiaires hubs \(si utilisé\) 
-   Certains clients USB zéro  
-   Périphériques USB, tels que certains périphériques de stockage externes et les lecteurs de DVD  
  
## <a name="sample-multipoint-services-system-layouts"></a>Dispositions de système MultiPoint Services exemple  
En fonction de mobilier disponible, la taille de la salle, le nombre d’ordinateurs qui exécutent MultiPoint Services et les stations dans la salle, il existe de diverses manières que les stations physiques peuvent être organisées. Les diagrammes suivants illustrent des cinq alternatives possibles.  
  
> [!NOTE]  
> Certains de ces diagrammes montrent un projecteur connecté au système MultiPoint Services. Il s’agit uniquement d’un exemple ; y compris un projecteur dans un système MultiPoint Services est facultatif.  
  
**Laboratoire informatique** dans cette configuration, les stations sont organisées autour des murs de la salle, avec les étudiants accessible sur les murs.  
  
![Configuration de la salle de classe informatique](./media/WMS_ComputerLabLayout.gif)  
  
**Groupes** dans cette configuration, il existe trois ordinateurs qui exécutent MultiPoint Services, avec les stations de cluster autour de chaque ordinateur.  
  
![Salle de classe configurée avec des unités d’accueil de serveur](./media/WMS_ClassroomPods.gif)  
  
**Salle de conférence** dans cette configuration, les stations sont définies dans les lignes. L’avantage de cette configuration est que tous les étudiants doivent faire face le formateur.  
  
![Salle de classe configurée comme salle de conférence](./media/WMS_LectureRoom.gif)  
  
**Centre d’activités** cette configuration se compose d’une mise en page de salle de conférence traditionnels pour les services de support technique, et il a une zone distincte avec un seul ordinateur qui exécute MultiPoint Services avec ses stations associées.  
  
![Centre d’activités multiPoint Services](./media/WMSActivityCenter.gif)  
  
**Petite entreprise** dans cette configuration, l’ordinateur qui exécute MultiPoint Services est placé dans un emplacement central et les utilisateurs tout au long de l’office vous y connecter à l’aide d’un réseau local \(LAN\).  
  
![Station connectée USB zéro client](./media/Diagram1.gif)