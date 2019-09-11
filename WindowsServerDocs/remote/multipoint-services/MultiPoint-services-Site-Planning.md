---
title: Planification de site MultiPoint Services
description: Informations de planification pour les déploiements MultiPoint services dans Windows Server 2016
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
ms.openlocfilehash: 3d49b2861d81a938fb20544c3edeb0976ac6d327
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871643"
---
# <a name="multipoint-services-site-planning"></a>Planification de site MultiPoint Services
Vous devez prendre en compte l’emplacement où un ou plusieurs ordinateurs exécutant MultiPoint services et les stations qui leur sont associées seront déployés.  
  
L’ordinateur qui exécute le rôle MultiPoint services doit avoir un accès pratique à une alimentation et aux périphériques connectés directement à celui-ci, par exemple une imprimante. En outre, l’ordinateur qui exécute MultiPoint services doit avoir un accès pratique à une connexion réseau. Une connexion réseau est nécessaire pour accéder à Internet et, le cas échéant, un réseau local.  
  
Les facteurs supplémentaires à prendre en compte sont les suivants :  
  
-   Le système MultiPoint services sera-t-il configuré dans une salle spécifique ou sera-t-il configuré sur un panier ou une table, afin de pouvoir être déplacé de l’emplacement vers l’emplacement ?  
  
    > [!NOTE]  
    > Si vous envisagez d’utiliser une configuration mobile, vous pouvez *associer* les stations à multipoint services chaque fois que vous les reconnectez pour vous assurer que chaque clavier et chaque souris sont associés à l’analyse appropriée.  
  
-   La station principale sera-t-elle située à côté des autres stations ou sera-t-elle distincte ? Par exemple, si le système MultiPoint services est configuré dans une salle de classe, la station principale sera-t-elle sur le Bureau de l’enseignant et les stations standard seront placées ailleurs dans la salle ? Lorsque l’ordinateur exécutant MultiPoint services est redémarré, la station principale a accès aux écrans de démarrage. Si vous vous inquiétez de ce niveau d’accès dans un paramètre de classe, vous préférerez peut-être placer la station principale sur le Bureau de l’enseignant.  
  
-   Combien de stations peuvent être contenues dans la salle ?  
  
-   Avez-vous besoin d’un réseau ? Une solution de serveur unique qui utilise une vidéo directe connectée ou des stations USB connectées à zéro client n’a pas besoin d’un réseau.  
  
-   Y a-il suffisamment de connexions réseau dans la salle pour prendre en charge le nombre requis d’ordinateurs exécutant MultiPoint services  
  
-   Où se trouvent les prises d’alimentation ?  
  
-   Aurez-vous besoin d’un autre périphérique d’affichage, tel qu’un projecteur ? Si vous envisagez d’utiliser un projecteur, est-il bloqué à partir du plafond ou est-il positionné sur une table ?  
  
-   Quel type de câble sera requis et combien sera-t-il nécessaire ?  
  
-   Réfléchissez à la façon dont vous souhaiterez peut-être développer à l’avenir. Allez-vous ajouter d’autres stations ?  
  
## <a name="station-layout-and-configuration"></a>Disposition et configuration de la station  
La disposition physique de votre site peut avoir un impact sur le type de station choisi. Pour plus d’informations sur les différents types de station, reportez-vous à la rubrique sur les [stations multipoint](MultiPoint-services-Stations.md) dans ce guide. Plusieurs types de station sont autorisés sur un seul service MultiPoint. Vous bénéficiez ainsi d’une flexibilité accrue pour répondre à vos besoins d’installation.  
  
### <a name="layout-for-direct-video-connected-stations"></a>Disposition pour les stations connectées à la vidéo directe  
  
-   Pour une station connectée à la vidéo directe, la distance entre les moniteurs et l’ordinateur est limitée par la longueur du câble vidéo.  
  
-   L’utilisation de concentrateurs intermédiaires ou de hubs de station connectés en chaîne est prise en charge pour faciliter le déploiement, mais le nombre maximal recommandé de hubs consécutifs est de trois. Cela signifie que la distance maximale entre l’ordinateur et le concentrateur de station est de 15 mètres, car chaque câble USB 2,0 a une longueur maximale de cinq mètres.  
  
> [!IMPORTANT]  
> Il doit toujours y avoir au moins une station connectée à une vidéo directe par ordinateur pour agir en tant que station principale.  
  
### <a name="layout-for-usb-zero-client-connected-stations"></a>Disposition des stations connectées à un client USB zéro  
  
-   L’utilisation de concentrateurs intermédiaires ou de hubs de station connectés en chaîne est prise en charge pour faciliter le déploiement, mais le nombre maximal recommandé de hubs consécutifs est de trois. Cela signifie que la distance maximale entre l’ordinateur et le concentrateur de station est de 15 mètres, car chaque câble USB 2,0 a une longueur maximale de cinq mètres.  
  
-   Le nombre maximal recommandé de clients USB connectés à un concentrateur intermédiaire unique est de trois.  
  
    > [!NOTE]  
    > Certains ordinateurs sont fournis avec un concentrateur générique sur la carte mère, ce qui a pour effet d’ajouter un concentrateur supplémentaire entre le *concentrateur racine* de l’ordinateur et les concentrateurs de station.  
  
-   Si la vidéo est très utilisée, il est recommandé de ne pas connecter plus de deux clients USB à un port USB sur le serveur. Par exemple, si un hub intermédiaire est utilisé, seuls deux clients USB à zéro doivent être connectés à celui-ci. Ou si vous reliez des clients USB à zéro, seuls deux clients USB à zéro doivent être chaînés ensemble. L’ajout de chaque client USB zéro au port USB sur le serveur diminue la bande passante vidéo disponible.  
  
-   Si vous envisagez de connecter plus de trois clients USB à un seul port USB sur le serveur, il est recommandé d’utiliser une connexion USB 3,0 entre le serveur et le Hub intermédiaire.  
  
> [!NOTE]  
> Il est recommandé de vérifier les performances à l’aide de vos applications et de votre matériel afin de déterminer le nombre de clients à zéro USB que vous pouvez connecter à un port USB sur le serveur.  
  
![Concentrateurs en aval](./media/WMS_diagram6.gif)  
  
**Figure 5** Système MultiPoint services avec trois clients USB nuls connectés à un concentrateur intermédiaire unique  
  
### <a name="layout-for-rdp-over-lan-connected-stations"></a>Disposition des stations connectées à RDP-sur-LAN  
Il n’existe aucune limite de distance physique pour les clients LAN. À condition qu’ils se trouvent sur le réseau local, ils peuvent se connecter au système MultiPoint services.  
  
## <a name="using-additional-hubs"></a>Utilisation de hubs supplémentaires  
Vous pouvez utiliser des hubs supplémentaires pour faciliter l’installation. Il existe trois types de concentrateurs utilisés sur un système MultiPoint services :  
  
-   [Hubs de station](#station-hubs)  
  
-   [Concentrateurs intermédiaires](#intermediate-hubs)  
  
-   [Hubs en aval](#downstream-hubs)  
  
### <a name="station-hubs"></a>Hubs de station  
Un concentrateur de station est un concentrateur externe qui a été associé à une station MultiPoint services. Au minimum, un clavier est branché sur le concentrateur de station. Des périphériques supplémentaires peuvent également être joints. Un concentrateur de station peut être un concentrateur USB générique conforme à la spécification USB 2,0 ou version ultérieure. Les concentrateurs de station doivent être alimentés de manière externe si les appareils à forte alimentation sont plug-ins.  
  
**Concentrateur racine** Un concentrateur USB intégré au contrôleur hôte sur la carte mère d’un ordinateur est appelé *concentrateur racine*. Les concentrateurs de station sont généralement connectés au hub racine sur l’ordinateur qui exécute MultiPoint services.  
  
> [!NOTE]  
> Les concentrateurs racine ne doivent pas être utilisés comme concentrateurs de station. Quand des ports USB sont intégrés à un ordinateur, il n’est souvent pas possible de déterminer à quel concentrateur racine USB ils sont connectés en interne. Par conséquent, si vous avez branché un clavier et une souris de station directement aux ports USB de l’ordinateur, vous êtes peut-être en train de brancher le clavier et la souris sur différents hubs racine USB. Pour vous assurer que le clavier et la souris se trouvent sur le même concentrateur, branchez un concentrateur de station sur le port USB de l’ordinateur, puis branchez le clavier et la souris sur ce concentrateur de station.  
  
**Gares** de connexion en chaîne Il peut être plus facile de connecter des hubs de station à un autre concentrateur de station plutôt que directement à l’ordinateur. Cela vous permet de connecter un concentrateur USB à un concentrateur de station qui est déjà connecté à l’ordinateur, afin de disposer d’un concentrateur de station connecté à un autre concentrateur de station.  
  
Il ne doit pas y avoir plus de trois clients USB ou hubs de station connectés en boucle consécutivement. Vous devez veiller à ce que la bande passante USB ne soit pas dépassée lors du chaînage des hubs de station.  
  
![Chaînage de stations](./media/WMS_diagram5.gif)  
  
**Figure 6** Système MultiPoint services avec des stations connectés en chaîne  
  
### <a name="intermediate-hubs"></a>Concentrateurs intermédiaires  
Un hub intermédiaire est un concentrateur situé entre le serveur et un concentrateur de station. Il est généralement utilisé pour augmenter le nombre de ports disponibles pour les concentrateurs de station ou pour étendre la distance entre les stations et l’ordinateur. Il est recommandé de ne pas utiliser plus de deux hubs intermédiaires entre un concentrateur de station et le serveur.  
  
Les concentrateurs intermédiaires doivent être de la norme USB 2,0 ou ultérieure, et ils doivent être alimentés en externe. USB 3,0 est recommandé entre le serveur et le Hub intermédiaire si vous connectez plus de trois clients USB à un concentrateur intermédiaire.  
  
### <a name="downstream-hubs"></a>Concentrateurs en aval  
Un hub en aval est connecté à un concentrateur de station pour ajouter d’autres ports disponibles pour les appareils de station. Un hub en aval peut être alimenté en externe ou alimenté par bus, en fonction des appareils connectés au concentrateur.  
  
![Connexions à zéro client USB multiples](./media/WMS_diagram4.gif "WMS_diagram4")  
  
**Figure 7** Système MultiPoint services avec un concentrateur intermédiaire, un concentrateur de station et un hub en aval  
  
## <a name="users-stations-and-computers"></a>Utilisateurs, stations et ordinateurs  
Le nombre de stations dont vous aurez besoin dépend du nombre de personnes qui auront besoin d’accéder en même temps aux ordinateurs exécutant MultiPoint services. De même, le nombre d’ordinateurs exécutant MultiPoint services dont vous aurez besoin dépend du nombre total de stations requis. Les stations connectées à la vidéo directe, les stations USB à zéro et les stations connectées au client, ainsi que les stations connectées via le réseau local, sont toutes considérées comme des stations. En outre, si la fonctionnalité de fractionnement de l’écran est utilisée, chaque moitié est considérée comme une station.  
  
## <a name="power-considerations"></a>Considérations relatives à l’alimentation  
Les composants suivants requièrent l’accès à une bande ou à une prise d’alimentation :  
  
-   Serveur  
-   Moniteurs
-   Concentrateurs \(intermédiaires, s’ils sont utilisés\) 
-   Certains clients USB à zéro  
-   Périphériques USB alimentés, tels que certains périphériques de stockage externes et lecteurs de DVD  
  
## <a name="sample-multipoint-services-system-layouts"></a>Exemples de dispositions système MultiPoint services  
Selon les meubles disponibles, la taille de la salle, le nombre d’ordinateurs qui exécutent MultiPoint services et les stations de la salle, il existe plusieurs façons d’organiser les stations physiques. Les diagrammes suivants illustrent cinq alternatives possibles.  
  
> [!NOTE]  
> Certains de ces diagrammes affichent un projecteur connecté au système MultiPoint services. Ce n’est qu’un exemple. l’inclusion d’un projecteur dans un système MultiPoint services est facultative.  
  
**Laboratoire informatique** Dans cette configuration, les stations sont disposées autour des murs de la salle, les étudiants étant confrontés aux murs.  
  
![Configuration de la salle de classe informatique](./media/WMS_ComputerLabLayout.gif)  
  
**Groupes** Dans cette configuration, il existe trois ordinateurs qui exécutent MultiPoint services, avec des stations en cluster sur chaque ordinateur.  
  
![Salle de classe configurée avec des unités d’accueil de serveur](./media/WMS_ClassroomPods.gif)  
  
**Salle de conférence** Dans cette configuration, les stations sont configurées dans des lignes. L’un des avantages de cette configuration est que tous les étudiants font face à l’instructeur.  
  
![Salle de classe configurée comme salle de conférence](./media/WMS_LectureRoom.gif)  
  
**Centre d’activités** Cette configuration est constituée d’une disposition de salle de cours traditionnelle pour les bureaux et elle dispose d’un domaine distinct avec un ordinateur unique exécutant MultiPoint services avec les stations qui lui sont associées.  
  
![Centre d’activités MultiPoint services](./media/WMSActivityCenter.gif)  
  
**Petite entreprise** Dans ce programme d’installation, l’ordinateur qui exécute multipoint services est placé dans un emplacement central et les utilisateurs au sein de l’entreprise se connectent à ce \(dernier\)à l’aide d’un réseau local LAN.  
  
![Station connectée USB zéro client](./media/Diagram1.gif)