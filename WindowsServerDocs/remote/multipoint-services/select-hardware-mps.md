---
title: Sélection du matériel pour votre système MultiPoint Services
description: Considérations matérielles pour MultiPoint Services
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e74961a2-bd38-48ae-b1c0-4b3eff761b4a
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 969ab0e97b5456c71a43cc14bd82204481bdcc42
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835220"
---
# <a name="selecting-hardware-for-your-multipoint-services-system"></a>Sélection du matériel pour votre système MultiPoint Services
Lorsque vous générez un système MultiPoint Services, vous devez sélectionner un ordinateur qui répond à la configuration requise Windows Server 2016. Si vous décidez quels composants pour sélectionner, considérez les points suivants :  
  
-   La plage de prix de cible de votre solution complète.  
  
-   Les types de scénarios d’utilisation que vous pouvez vous attendre pour le système MultiPoint Services, telles que les utilisateurs sont en cours d’exécution des programmes multimédias, à l’aide du traitement de texte ou des programmes de productivité ou la navigation sur Internet.  
  
-   Si votre scénario a des demandes de traitement ou de la mémoire volumineux.  
  
-   Le nombre d’utilisateurs qui peut être à l’aide du système en même temps. Si vous prévoyez d’avoir plusieurs utilisateurs sur votre système en même temps, ou les utilisateurs qui utilisent des programmes utilisant beaucoup de système, vous devez prévoir plus de puissance de calcul pour votre système.  
  
-   Le type de stations. Le nombre de ports USB ou vidéo ports avez-vous besoin ?  
  
-   Projets d’expansion. Vous souhaitez ajouter des stations au système MultiPoint Services à une date ultérieure ? Vous aurez suffisamment les emplacements de carte vidéo, les ports USB, ou appuie sur le réseau ? Combien d’utilisateurs supplémentaires est votre matériel devront prendre en charge ?  
  
-   Disposition physique. Pour plus d’informations, consultez [planification de Site MultiPoint Services](MultiPoint-services-Site-Planning.md).  
  
En règle générale, un système MultiPoint Services inclut les composants suivants :  
  
-   Un ordinateur qui exécute MultiPoint Services, qui inclut une UC, RAM, disque dur et les cartes vidéo.  
  
-   Un moniteur de concentrateur de station, clavier et souris pour chaque station.  
  
-   Périphériques facultatifs pour les stations MultiPoint Services, y compris les intervenants, un casque, microphones ou dispositifs de stockage qui sont disponibles uniquement pour l’utilisateur de la station.  
  
-   Facultatifs périphériques qui sont disponibles pour tous les utilisateurs du système MultiPoint Services, directement connecté à l’ordinateur hôte, tels que les imprimantes, les disques durs externes et les périphériques de stockage USB.  
  
Utilisez les informations suivantes pour prendre des décisions de matériel :  
  
-   [Choix d’un processeur](#selecting-a-cpu)  
-   [Sélection des composants matériels](#selecting-hardware-components)  
  
## <a name="selecting-a-cpu"></a>Choix d’un processeur  
Un système MultiPoint Services est un multiple\-l’environnement utilisateur, avec tous les utilisateurs connectés à un seul ordinateur hôte. Cela augmente l’utilisation du processeur, car tous les utilisateurs partagent le même ordinateur. Certaines tâches, tels que des programmes multimédias \(, par exemple, les lecteurs multimédias ou vidéo\-logiciel de retouche\), ont des exigences de traitement supérieure. Par conséquent, veillez à sélectionner un processeur qui peut gérer les exigences de traitement pour le nombre d’utilisateurs et les types de scénarios utilisateur qui seront nécessaires pour prendre en charge.  
  
MultiPoint Services nécessite une x64\-en fonction du processeur et doit respecter la configuration système requise pour l’ordinateur comme décrit dans [configuration matérielle requise et les recommandations relatives aux performances](Hardware-Requirements-and-Performance-Recommendations.md).  
  
Les types de processeurs suivants ont été testés pour être utilisés sur un système MultiPoint Services avec haute\-exigent des programmes de traitement, tels que des programmes multimédias :  
  
-   **Double\-processeur Intel core :** Peut prendre en charge jusqu'à 8 stations.  
-   **Quadruple\-processeur Intel core :** Peut prendre en charge jusqu'à 16 stations.
-   **Quadruple\-cœur de processeur avec le multithreading :** Peut prendre en charge jusqu'à 20 stations.      
-   **Six\-cœur de processeur avec le multithreading :** Peut prendre en charge jusqu'à 24 stations.  
  
Avec ces informations, sélectionnez un processeur qui respecte les exigences de traitement pour votre système MultiPoint Services. 
> [!NOTE] 
> Si vous exécutez des applications gourmandes en vidéo la recommandation est au moins un cœur par station. 
  
## <a name="selecting-hardware-components"></a>Sélection des composants matériels  
Lorsque vous générez un système MultiPoint Services, prenez en compte les composants matériels suivants que vous devrez peut-être :  
  
-   Matériel vidéo  
  
-   Matériel de la station multiPoint Services  
  
    -   *Concentrateurs USB*  
  
    -   Clients USB zéro  
  
    -   Claviers et souris  
  
    -   Moniteurs  
  
-   Tous les périphériques  
  
    -   Périphériques audio, tels que les haut-parleurs et les casques  
  
    -   Microphones  
  
    -   Périphériques de stockage de masse USB  
  
Lorsque vous avez sélectionné les composants matériels de votre système MultiPoint Services, assurez-vous que vous obtenez actuel, 64\-bits des pilotes pour les composants.  
  
Les rubriques suivantes fournissent des informations détaillées pour vous aider à sélectionner les composants de votre système MultiPoint Services :  
  
[Sélection du matériel vidéo](#selecting-video-hardware)  
[En sélectionnant direct\-vidéo\-connecté ou USB zéro les appareils clients de station](#BKMK_Selectingdirect-video-connectedorUSBzeroclientstationdevices)   
[Sélection d’autres périphériques de la station](#selecting-other-station-peripheral-devices)  
[En sélectionnant RDP\-sur\-LAN\-connecté le matériel de la station](#BKMK_SelectingRDP-over-LAN-connectedstationhardware)  
[Sélection de périphériques audio](#selecting-audio-devices)  
  
## <a name="selecting-video-hardware"></a>Sélection du matériel vidéo
Le matériel vidéo que vous sélectionnez doit prendre en charge le nombre d’écrans dont vous aurez besoin pour le nombre d’utilisateurs que vous prévoyez d’utiliser le travail sur les stations MultiPoint Services. En outre, les différents types de matériel vidéo peuvent fournir un degré plus élevé\-solution de performances pour les graphiques\-programmes intensifs, tel que le contenu multimédia.  
  
Sélectionnez le matériel vidéo qui peut prendre en charge le nombre maximal d’écrans pour le type de performances requis par votre système MultiPoint Services. Assurez-vous que vous validez les performances du matériel vidéo que vous choisissez pour vous assurer qu’il répond à vos besoins de performances.  
  
> [!NOTE]  
> Vous devez installer un pilote vidéo qui prend en charge l’extension de votre bureau sur plusieurs moniteurs.  
  
Options de matériel vidéo sont les suivantes :  
  
-   Cartes vidéo internes qui utilisent un PCI ou une interface de bus de PCIe  
  
-   Contrôleurs vidéo externes connectés par USB  
  
Les sections suivantes décrivent les fonctionnalités de chacun de ces types de matériel vidéo. Vous pouvez combiner des cartes vidéo internes et externes contrôleurs vidéo pour créer le système que vous souhaitez.  
  
### <a name="internal-video-cards"></a>Cartes vidéo internes  
Une carte vidéo interne est branchée\-dans à la carte mère sur l’ordinateur. La carte vidéo interne est une solution qui peut améliorer les performances des graphiques\-programmes multimédias intensifs. Toutefois, une carte vidéo interne nécessite un emplacement disponible de la norme PCI ou PCIe brancher\-dans à la carte mère. Nombreux haute\-cartes vidéo performances nécessitent un emplacement PCIe, mais il existe un nombre limité d’emplacements PCIe sur une carte mère. Vous devez savoir quelles sont les emplacements de carte vidéo sont disponibles sur votre ordinateur afin que vous pouvez acheter le type de cartes vidéo approprié.  
  
Le nombre d’analyses qui peuvent se connecter à chaque carte vidéo varie selon le GPU est utilisé sur la carte et le nombre de ports que pris en charge, ce qui est généralement comprise entre 2 et 6.  
  
Lorsque vous sélectionnez les cartes vidéo internes, sélectionnez les cartes vidéo qui prennent en charge le nombre d’écrans nécessaires pour créer le nombre souhaité de stations connectées de vidéo directs. Le nombre maximal de moniteurs pouvant être pris en charge est égal au nombre de cartes vidéo internes qui sont branchés\-dans à la carte mère multipliée par le nombre de ports d’analyse sur chacun de ces cartes vidéo. Par exemple, si vous aviez deux cartes vidéo internes et chaque carte avait deux ports d’analyse, vous pourrez prendre en charge jusqu'à quatre analyses.    
  
### <a name="external-video-controllers"></a>Contrôleurs vidéo externes  
Les clients USB zéro contiennent un contrôleur vidéo externe pour connecter un moniteur au client. Le client USB zéro peut-être également inclure des connexions pour un casque, haut-parleurs, un microphone ou autres périphériques.  
  
Sélectionnez une clé USB zéro client si vous souhaitez activer la prise en charge de moniteurs supplémentaires sans ouvrir l’ordinateur, ou si vous souhaitez prendre en charge des stations de plus que les sorties vidéo disponibles. Par exemple, si vous aviez précédemment connectés de quatre analyses\-dans des cartes vidéo internes, vous à ajouter deux moniteurs de plus, vous pouvez incorporer\-dans les deux contrôleurs vidéo externes à l’ordinateur et prendre en charge deux moniteurs plus. De cette manière, vous pouvez combiner un USB zéro client avec le contrôleur vidéo et pas utiliser les emplacements de la norme PCI ou PCIe supplémentaires sur la carte mère.  
  
## <a name="BKMK_Selectingdirect-video-connectedorUSBzeroclientstationdevices"></a>En sélectionnant direct\-vidéo\-connecté ou USB zéro les appareils clients de station  
Une station MultiPoint Services compose d’un concentrateur de station ou d’USB zéro client avec un clavier et une souris connectés\-in et un moniteur qui est connecté\-dans à l’ordinateur hôte ou dans un client USB zéro. Autres périphériques peuvent être connectés\-dans à la station de concentrateur ou USB zéro client, mais ils n’êtes pas obligés de créer une station MultiPoint. Ces périphériques sont décrites dans [en sélectionnant d’autres périphériques station](#selecting-other-station-peripheral-devices).  
  
Les appareils que vous sélectionnez cette option pour créer une station MultiPoint Services doivent respecter la configuration minimale requise pour travailler avec MultiPoint Services. Pour plus d’informations sur la configuration requise pour les appareils de station MultiPoint Services suivants sont fournis dans cette rubrique :  
  
-   [En sélectionnant les concentrateurs USB](#selecting-usb-hubs)  
-   [En sélectionnant USB zéro clients](#selecting-usb-zero-clients)  
-   [En sélectionnant les claviers et souris](#selecting-keyboards-and-mouse-devices)  
-   [Sélection des moniteurs](#selecting-monitors)  
  
### <a name="selecting-usb-hubs"></a>En sélectionnant les concentrateurs USB  
Les concentrateurs USB utilisés dans un système MultiPoint Services peuvent être un concentrateur USB générique. Ces concentrateurs disposent généralement d’au moins quatre ports USB, et ils permettent de connecter plusieurs périphériques USB d’être connecté à un seul port USB sur l’ordinateur. D’autres périphériques, tels que les claviers et moniteurs vidéo, peuvent également inclure un concentrateur USB dans leur conception.  
  
Une considération supplémentaire est l’utilisation d’un *alimentés de façon externe* hub, au lieu d’un *bus\-powered* hub. Avec un bus\-concentrateur alimenté, la quantité d’actuel qui est fournie par l’hôte de l’ordinateur doit être suffisante pour fournir la puissance pour tous les périphériques sont connectés\-dans le concentrateur, sans dégrader les performances du système. Un concentrateur alimenté de façon externe vous permet de vous permettent de connecter des périphériques plus et fournissent une puissance suffisante à tous les éléments. L’utilisation de hubs alimentés de façon externe peut aider à empêcher les problèmes de performances, les échecs de port et les autres problèmes intermittents.  
  
Lorsque vous sélectionnez un concentrateur USB pour votre système MultiPoint Services, envisagez de son utilisation. Le concentrateur peut être utilisé comme un *concentrateur de station*, un *concentrateur intermédiaire*, ou un *hub en aval*. Consultez le tableau suivant pour obtenir une description sur chaque type de concentrateur. Nous vous recommandons de tous les périphériques USB à être USB 2.0 ou version ultérieure.
  
||Sous tension|  
|-|-----------|  
|Concentrateur de station|Peut être bus\-sous tension, sauf si élevé\-périphériques seront être branchés\-dans ou un concentrateur en aval est connecté à celui-ci|  
|Concentrateur intermédiaire |Doit être alimentés de façon externe|  
|Hub en aval|Peut être alimentés de façon externe ou selon les appareils sont branchés alimenté par le bus\-dans le concentrateur|  
|Câble d’extendeur USB Active|Câbles USB actives qui incluent un concentrateur USB sont généralement des bus sous tension ; Par conséquent, ils ne sont pas recommandées pour la connexion des concentrateurs de station sur l’ordinateur.|  
  
### <a name="selecting-usb-zero-clients"></a>En sélectionnant USB zéro clients  
Un client USB zéro est un concentrateur USB qui contient une sortie vidéo. Par conséquent, il permet une analyse d’être connecté à l’ordinateur via une connexion USB. Pour plus d’informations sur l’utilisation de clients USB zéro pour la vidéo, consultez [sélection du matériel vidéo](#selecting-video-hardware) dans ce document. Un client USB zéro permettre également activer la connexion d’une variété de USB et non\-périphériques USB au concentrateur. Les clients USB zéro sont produites par des fabricants de matériel spécifique, et ils nécessitent l’installation d’un appareil\-pilote spécifique.  
  
### <a name="selecting-keyboards-and-mouse-devices"></a>En sélectionnant les claviers et souris  
Les appareils de clavier et souris vous branchez\-dans à la station sera généralement les périphériques USB. Certains pilotes USB zéro clients fournissent PS\/2 ports, auquel cas, le clavier et souris doit utiliser PS\/2 pour se connecter au concentrateur de station. Vous pouvez également utiliser un PS\/2 du clavier et souris et si vous configurez un PS\/2 direct\-vidéo\-station connectée.  
  
Un clavier avec un concentrateur interne peut être utilisé comme un concentrateur de station. Toutefois, tous les autres appareils de station doivent se connecter au concentrateur interne à l’aide de ports sur le clavier. Si un tel clavier est connecté à l’ordinateur via un autre concentrateur, ce hub est considéré comme concentrateur intermédiaire.  
  
Si vous utilisez le fractionnement\-stations à écran, vous souhaiterez à l’aide d’un clavier mini qui n’a pas un pavé numérique pour que les deux claviers tiennent devant l’analyse.  
  
### <a name="selecting-monitors"></a>Sélection des moniteurs  
Il doit y avoir un moniteur fourni pour chaque station MultiPoint Services, sauf si un fractionnement\-écran est prévu. Les moniteurs sont branchés sur la carte vidéo sur l’ordinateur, le client USB zéro ou le réseau local\-client basé sur. N’importe quel type de moniteur est pris en charge par la carte vidéo, USB zéro client ou LAN\-client en fonction peut être utilisé, y compris les moniteurs CRT.  
  
Certaines analyses spéciaux incluent un réseau local interne\-en fonction de client ou USB zéro client. Ces moniteurs incluent généralement l’entrée audio\/prises et les concentrateurs USB internes pour la connexion des claviers et des souris de sortie. Ils se connectent au serveur via une connexion USB ou une réseau local.  
  
#### <a name="display-resolution"></a>Résolution d’affichage  
La résolution minimale prise en charge pour la zone d’affichage d’une station est 512 x 768 pixels. Si le système MultiPoint Services démarre et constate que zone d’affichage d’une station est inférieure à la résolution minimale, un écran vide s’affichera sur cette station et la station ne sera pas utilisable.  
  
Si un écran d’affichage doit être partagé par deux stations fractionné\-écran stations, la configuration minimale requise pour l’affichage est de 1024 x 768, afin que les zones d’écran station individuels qui en résulte sont au moins 512 x 768. Pour une meilleure fractionner\-expérience utilisateur d’écran, un écran large avec un minimum de résolution de 1600 x 900 est recommandé.  
  
## <a name="selecting-other-station-peripheral-devices"></a>Sélection d’autres périphériques de la station  
MultiPoint Services prend en charge les périphériques qui sont connectés à un concentrateur de station, un client USB zéro, ou directement à l’ordinateur. Périphériques connectés à un concentrateur de station sera associés à cette station spécifique. Autres périphériques sont disponibles sur chaque station lorsqu’il est connecté directement à l’ordinateur. Les clients du réseau local peuvent également en charge les périphériques.  
  
> [!IMPORTANT]  
> Un clavier ne peut pas être connecté à un concentrateur en aval \(, par exemple, un concentrateur qui est connecté à un concentrateur de station\). Si vous branchez un clavier à un concentrateur en aval, tous les périphériques connectés\-dans le concentrateur en aval ne sera plus disponible à cette station. Ce comportement permet la prise en charge de connexion en cascade\-chaînées concentrateurs de station.  
  
**Disponible sur toutes les stations** APPAREIL A USB connecté à l’ordinateur \(par exemple, non par le biais d’un concentrateur de station\) est disponible pour toutes les stations. En fonction de l’appareil, il peut être utilisé par plusieurs utilisateurs en même temps, ou qu’un seul utilisateur peut y accéder à la fois. Le tableau suivant explique comment les périphériques USB sont accessibles.  
  
> [!NOTE]  
> La colonne « Connecté à ordinateur hôte » dans la table fait référence au comportement lorsque l’ordinateur qui exécute MultiPoint Services est en cours d’exécution en mode station avec stations. Si vous êtes en mode console, les périphériques connectés n’importe où se comportent de la même façon que d’un serveur standard dans une session de console.  
  
||Connecté à l’ordinateur hôte|Connecté à un concentrateur de Station ou Hub en aval|  
|-|------------------------------|----------------------------------------------|  
|Clavier|Ne fonctionne pas, sauf si elle fait partie d’une station PS/2. |Disponible à la station individuelle<br /><br />Ne peut pas être connecté à un concentrateur en aval|  
|Souris|Ne fonctionne pas, sauf si elle fait partie d’une station PS/2. |Disponible à la station individuelle|  
|Haut-parleur/casque|Ne fonctionne pas, sauf si elle fait partie d’une station PS/2.|Disponible à la station individuelle|  
|Périphérique de stockage USB|Disponible sur toutes les stations|Disponible à la station individuelle|  
|Contrôle consommateur HID|Ne fonctionne pas|Disponible à la station individuelle|  
|Autres périphériques USB, tels que les appareils photo, lecteurs de documents et les lecteurs de DVD|Disponible sur toutes les stations si pris en charge par Windows Server 2012|Disponible sur toutes les stations si pris en charge Windows Server 2008 R2 Services Bureau à distance|  
  
## <a name="BKMK_SelectingRDP-over-LAN-connectedstationhardware"></a>En sélectionnant RDP\-sur\-LAN\-connecté le matériel de la station  
N’importe quel client de réseau local qui peut se connecter à des Services Bureau à distance, à l’aide de Remote Desktop Protocol, peut devenir une station MultiPoint Services.  
  
Si vous souhaitez que le client de réseau local pour être utilisé uniquement comme une station MultiPoint, vous souhaiterez « verrouiller » votre client de réseau local. Par exemple, configurer votre client léger afin qu’il peut uniquement se connecter à une session MultiPoint Services, ou configurer vos ordinateurs de bureau afin que l’accès aux icônes du bureau et du Menu Démarrer des éléments comme un navigateur web est supprimé pour empêcher tout accès Internet direct. Vous pouvez appliquer ces configurations à l’aide de vos outils de configuration de client de réseau local ou de groupe ou de stratégies locales.  
  
## <a name="selecting-audio-devices"></a>Sélection de périphériques audio  
Il est important pour vous assurer que lorsque vous sélectionnez les périphériques audio, ils peuvent être branchés sur le concentrateur de station, USB zéro client ou client de réseau local. Certains concentrateurs USB, les clients USB zéro et clients du réseau local ont un jack audio analogique qui peut être utilisé avec les périphériques audio analogiques classiques \(comme un casque ou écouteurs par\). Concentrateurs de station qui n’ont pas les osselets analogiques peuvent utiliser les périphériques audio USB.  
  
Si vous avez configuré un PS\/2 direct\-vidéo\-station connectée à l’aide de PS\/2 ports sur la carte mère pour le clavier et souris, vous devez utiliser l’audio analogique sur la carte mère dans afin que le périphérique audio soit disponible pour cette station lorsque le système MultiPoint Services s’exécute en mode station.  
  
Si vous n’avez pas un PS\/2 direct\-vidéo\-station connectée, le périphérique audio hôte sur la carte mère du système seront disponible uniquement lorsque le système MultiPoint Services est en cours d’exécution en mode console.  