---
title: Sélection du matériel pour votre système MultiPoint Services
description: Considérations matérielles pour MultiPoint services
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: e74961a2-bd38-48ae-b1c0-4b3eff761b4a
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 3dca1b68564c977394c1b71f72db0fde5727c861
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859072"
---
# <a name="selecting-hardware-for-your-multipoint-services-system"></a>Sélection du matériel pour votre système MultiPoint Services
Lorsque vous créez un système MultiPoint services, vous devez sélectionner un ordinateur qui répond à la configuration système requise pour Windows Server 2016. Si vous choisissez les composants à sélectionner, prenez en compte les éléments suivants :  
  
-   La gamme de prix cible de votre solution complète.  
  
-   Les types de scénarios d’utilisation que vous pouvez vous attendre pour le système MultiPoint services, par exemple si les utilisateurs exécutent des programmes multimédias, à l’aide de programmes de traitement de texte ou de productivité, ou en naviguant sur Internet.  
  
-   Si votre scénario a des exigences de traitement ou de mémoire importantes.  
  
-   Nombre d’utilisateurs qui peuvent utiliser le système en même temps. Si vous envisagez de disposer de nombreux utilisateurs sur votre système en même temps ou des utilisateurs qui utilisent des programmes utilisant beaucoup de ressources système, vous devez prévoir une puissance informatique plus importante pour votre système.  
  
-   Type de station. De combien de ports USB ou de ports vidéo avez-vous besoin ?  
  
-   Futurs plans d’extension. Prévoyez-vous d’ajouter des stations au système MultiPoint services à une date ultérieure ? Disposerez-vous de suffisamment d’emplacements de carte vidéo, de ports USB ou de prises de réseau ? Combien d’utilisateurs supplémentaires votre matériel devra-t-il prendre en charge ?  
  
-   Disposition physique. Pour plus d’informations, consultez [planification de site multipoint services](MultiPoint-services-Site-Planning.md).  
  
Un système MultiPoint services comprend généralement les composants suivants :  
  
-   Un ordinateur exécutant MultiPoint services, qui comprend un processeur, une RAM, des lecteurs de disque dur et des cartes vidéo.  
  
-   Un moniteur, un concentrateur de station, un clavier et une souris pour chaque station.  
  
-   Périphériques facultatifs pour les stations MultiPoint services, y compris les haut-parleurs, les casques, les microphones ou les dispositifs de stockage qui sont disponibles uniquement pour l’utilisateur de la station.  
  
-   Périphériques facultatifs qui sont disponibles pour tous les utilisateurs du système MultiPoint services, connectés directement à l’ordinateur hôte, tels que les imprimantes, les lecteurs de disques durs externes et les périphériques de stockage USB.  
  
Utilisez les informations suivantes pour prendre des décisions sur le matériel :  
  
-   [Sélection d’un processeur](#selecting-a-cpu)  
-   [Sélection des composants matériels](#selecting-hardware-components)  
  
## <a name="selecting-a-cpu"></a>Sélection d’un processeur  
Un système MultiPoint services est un environnement à plusieurs\-utilisateur, avec tous les utilisateurs connectés à un seul ordinateur hôte. Cela augmente l’utilisation du processeur, car tous les utilisateurs partagent le même ordinateur. Certaines tâches, telles que les programmes multimédias \(par exemple, les lecteurs multimédias ou la vidéo\-édition des logiciels\), ont des demandes de traitement plus importantes. Par conséquent, veillez à sélectionner un processeur qui peut gérer les exigences de traitement pour le nombre d’utilisateurs et de types de scénarios utilisateur qu’il devra prendre en charge.  
  
MultiPoint Services requiert un processeur x64\-et doit satisfaire à la configuration système requise pour l’ordinateur, comme décrit dans [Configuration matérielle requise et recommandations relatives aux performances](Hardware-Requirements-and-Performance-Recommendations.md).  
  
Les types de processeurs suivants ont été testés pour être utilisés sur un système MultiPoint services avec des programmes de traitement haute\-demande, tels que des programmes multimédias :  
  
-   **Processeur double\-Core :** Peut prendre en charge jusqu’à 8 stations.  
-   **Processeur Quad\-Core :** Peut prendre en charge jusqu’à 16 stations.
-   **Processeur Quad\-Core avec Multithreading :** Peut prendre en charge jusqu’à 20 stations.      
-   **Processeur Six\-cœurs avec Multithreading :** Peut prendre en charge jusqu’à 24 stations.  
  
Avec ces informations, sélectionnez un processeur qui répond aux exigences de traitement de votre système MultiPoint services. 
> [!NOTE] 
> Si vous exécutez des applications vidéo intensives, la recommandation est d’au moins un cœur par station. 
  
## <a name="selecting-hardware-components"></a>Sélection des composants matériels  
Lorsque vous créez un système MultiPoint services, tenez compte des composants matériels suivants dont vous pouvez avoir besoin :  
  
-   Matériel vidéo  
  
-   Matériel de la station MultiPoint services  
  
    -   *Concentrateurs USB*  
  
    -   Clients USB à zéro  
  
    -   Claviers et souris  
  
    -   Moniteurs  
  
-   Périphériques  
  
    -   Périphériques audio, tels que les haut-parleurs et les casques  
  
    -   Microphones  
  
    -   Périphériques de stockage de masse USB  
  
Une fois que vous avez sélectionné les composants matériels de votre système MultiPoint services, assurez-vous d’obtenir les pilotes actuels de 64\-bits pour les composants.  
  
Les rubriques suivantes fournissent des informations détaillées pour vous aider à sélectionner des composants pour votre système MultiPoint services :  
  
[Sélection du matériel vidéo](#selecting-video-hardware)  
[Sélection d’un appareil de station cliente\-directe\-connecté ou USB](#BKMK_Selectingdirect-video-connectedorUSBzeroclientstationdevices)   
[Sélection d’autres périphériques de station](#selecting-other-station-peripheral-devices)  
[Sélection de RDP\-sur\-réseau local\-matériel de station connectée](#BKMK_SelectingRDP-over-LAN-connectedstationhardware)  
[Sélection de périphériques audio](#selecting-audio-devices)  
  
## <a name="selecting-video-hardware"></a>Sélection du matériel vidéo
Le matériel vidéo que vous sélectionnez doit prendre en charge le nombre d’analyses dont vous aurez besoin pour le nombre d’utilisateurs que vous avez l’intention de travailler sur les stations MultiPoint services. En outre, différents types de matériel vidéo peuvent fournir une solution de performances plus élevée\-pour les applications graphiques\-des programmes intensifs, comme le contenu multimédia.  
  
Sélectionnez le matériel vidéo qui peut prendre en charge le nombre maximal d’analyses pour le type de performances requis par votre système MultiPoint services. Veillez à valider les performances du matériel vidéo que vous choisissez pour vous assurer qu’il répond à vos besoins en matière de performances.  
  
> [!NOTE]  
> Vous devez installer un pilote vidéo qui prend en charge l’extension de votre bureau sur plusieurs moniteurs.  
  
Les options matérielles vidéo sont les suivantes :  
  
-   Cartes vidéo internes utilisant une interface de bus PCI ou PCIe  
  
-   Contrôleurs vidéo externes connectés par USB  
  
Les sections suivantes décrivent les fonctionnalités de chacun de ces types de matériel vidéo. Vous pouvez combiner des cartes vidéo internes et des contrôleurs vidéo externes pour créer le système souhaité.  
  
### <a name="internal-video-cards"></a>Cartes vidéo internes  
Une carte vidéo interne est branchée\-dans la carte mère de l’ordinateur. La carte vidéo interne est une solution qui peut aider à améliorer les performances des graphiques\-des programmes multimédias intensifs. Toutefois, une carte vidéo interne requiert un emplacement PCI ou PCIe disponible pour brancher\-à la carte mère. De nombreuses cartes vidéo hautes performances\-requièrent un emplacement PCIe, mais il existe un nombre limité d’emplacements PCIe sur une carte mère. Vous devez savoir quels sont les types d’emplacements de carte vidéo disponibles sur votre ordinateur afin que vous puissiez acheter le type de carte vidéo approprié.  
  
Le nombre d’analyses qui peuvent se connecter à chaque carte vidéo dépend du GPU utilisé sur la carte et du nombre de ports pris en charge, ce qui est généralement de 2 à 6.  
  
Lorsque vous sélectionnez des cartes vidéo internes, sélectionnez des cartes vidéo qui prennent en charge le nombre de moniteurs requis pour créer le nombre souhaité de stations connectées à la vidéo directe. Le nombre maximal d’analyses pouvant être prises en charge est égal au nombre de cartes vidéo internes connectées\-à la carte mère, multiplié par le nombre de ports d’analyse sur chacune de ces cartes vidéo. Par exemple, si vous avez deux cartes vidéo internes et que chaque carte avait deux ports de moniteur, vous pouvez prendre en charge jusqu’à quatre moniteurs.    
  
### <a name="external-video-controllers"></a>Contrôleurs vidéo externes  
Les clients USB à zéro contiennent un contrôleur vidéo externe pour connecter un moniteur au client. Le client Zero USB peut également inclure des connexions pour les écouteurs, les haut-parleurs, un microphone ou tout autre périphérique.  
  
Sélectionnez un client USB zéro si vous souhaitez activer la prise en charge d’autres analyses sans ouvrir l’ordinateur, ou si vous souhaitez prendre en charge plus de stations que de sorties vidéo disponibles. Par exemple, si vous aviez précédemment quatre moniteurs branchés\-à des cartes vidéo internes et que vous souhaitez ajouter deux moniteurs supplémentaires, vous pouvez brancher\-deux contrôleurs vidéo externes sur l’ordinateur et disposer d’un espace pour deux autres moniteurs. De cette manière, vous pouvez combiner un client Zero USB avec le contrôleur vidéo et ne pas utiliser d’emplacements PCI ou PCIe supplémentaires sur la carte mère.  
  
## <a name="selecting-direct-video-connected-or-usb-zero-client-station-devices"></a><a name="BKMK_Selectingdirect-video-connectedorUSBzeroclientstationdevices"></a>Sélection de\-vidéo directe\-des appareils de station cliente connectés ou USB  
Une station MultiPoint services se compose d’un concentrateur de station ou d’un client USB zéro avec un clavier et une souris branchés\-, et d’un moniteur connecté\-à l’ordinateur hôte ou à un client USB Zero. D’autres périphériques peuvent être branchés\-dans le concentrateur de station ou le client USB zéro, mais ils ne sont pas requis pour créer une station MultiPoint. Ces autres périphériques sont décrits dans la rubrique [sélection d’autres périphériques de station](#selecting-other-station-peripheral-devices).  
  
Les appareils que vous sélectionnez pour créer une station MultiPoint services doivent répondre à la configuration minimale requise pour fonctionner avec MultiPoint services. Pour plus d’informations sur la configuration requise pour les périphériques de station MultiPoint services suivants, procédez comme suit :  
  
-   [Sélection de concentrateurs USB](#selecting-usb-hubs)  
-   [Sélection de clients USB à zéro](#selecting-usb-zero-clients)  
-   [Sélection des claviers et des périphériques de souris](#selecting-keyboards-and-mouse-devices)  
-   [Sélection des analyses](#selecting-monitors)  
  
### <a name="selecting-usb-hubs"></a>Sélection de concentrateurs USB  
Les concentrateurs USB utilisés dans un système MultiPoint services peuvent être un concentrateur USB générique. De tels hubs possèdent généralement quatre ports USB ou plus, et ils permettent de connecter plusieurs périphériques USB à un port USB unique sur l’ordinateur. Certains autres périphériques, tels que les claviers et les moniteurs vidéo, peuvent également intégrer un concentrateur USB à leur conception.  
  
Une considération supplémentaire est l’utilisation d’un concentrateur alimenté de manière *externe* , au lieu d’un concentrateur *bus\-alimenté* . Avec un concentrateur bus\-, la quantité de courant fournie par l’ordinateur hôte doit être suffisante pour fournir de la puissance à tous les périphériques connectés\-au Hub, sans dégrader les performances du système. Un concentrateur alimenté en externe vous permet de connecter un plus grand nombre de périphériques et de fournir une alimentation suffisante à tous. L’utilisation de hubs alimentés en externe permet d’éviter les problèmes de performances, les défaillances de port et d’autres problèmes intermittents.  
  
Lorsque vous sélectionnez un concentrateur USB pour votre système MultiPoint services, tenez compte de son utilisation. Le concentrateur peut être utilisé en tant que *concentrateur de station*, *Hub intermédiaire*ou *Hub en aval*. Reportez-vous au tableau suivant pour obtenir une description de chaque type de Hub. Nous vous recommandons d’utiliser tous les périphériques USB 2,0 ou une version ultérieure.
  
||Mue|  
|-|-----------|  
|Concentrateur de station|Peut être alimenté par le bus\-, à moins que des appareils\-alimentés soient branchés\-ou qu’un hub en aval soit connecté à celui-ci.|  
|Hub intermédiaire |Doit être alimenté en externe|  
|Hub en aval|Peut être alimenté en externe ou alimenté par bus en fonction des appareils branchés\-dans le Hub|  
|Câble d’extendeur USB actif|Les câbles USB actifs qui incluent un concentrateur USB sont généralement alimentés par bus ; par conséquent, ils ne sont pas recommandés pour connecter des hubs de station à l’ordinateur.|  
  
### <a name="selecting-usb-zero-clients"></a>Sélection de clients USB à zéro  
Un client USB à zéro est un concentrateur USB qui contient une sortie vidéo. Par conséquent, elle permet à une analyse d’être connectée à l’ordinateur via une connexion USB. Pour plus d’informations sur l’utilisation de clients USB nuls pour la vidéo, consultez [sélection de matériel vidéo](#selecting-video-hardware) dans ce document. Un client zéro USB peut également activer la connexion d’un ensemble de périphériques USB et non\-USB au concentrateur. Les clients USB nuls sont produits par des fabricants de matériel spécifiques et nécessitent l’installation d’un périphérique\-pilote spécifique.  
  
### <a name="selecting-keyboards-and-mouse-devices"></a>Sélection des claviers et des périphériques de souris  
Les périphériques clavier et souris que vous connectez\-à la station sont généralement des périphériques USB. Certains clients USB ne fournissent pas de ports PS\/2. dans ce cas, le clavier et la souris doivent utiliser PS\/2 pour se connecter au concentrateur de station. Vous pouvez également utiliser un clavier et une souris PS\/2 Si vous configurez une station connectée à un\-de la vidéo\-directe PS\/2.  
  
Un clavier avec un concentrateur interne peut être utilisé comme concentrateur de station. Toutefois, tous les autres périphériques de station doivent se connecter au concentrateur interne à l’aide de ports sur le clavier. Si un tel clavier est connecté à l’ordinateur via un autre concentrateur, ce concentrateur sera traité comme un concentrateur intermédiaire.  
  
Si vous utilisez des stations d’écran fractionnées\-, vous souhaiterez peut-être utiliser un mini-clavier qui n’a pas de pavé numérique pour que les deux claviers puissent tenir devant le moniteur.  
  
### <a name="selecting-monitors"></a>Sélection des analyses  
Une seule analyse doit être fournie pour chaque station MultiPoint services, à moins qu’un écran de\-fractionné soit planifié. Les moniteurs sont insérés dans la carte vidéo de l’ordinateur, le client USB Zero ou le client LAN\-. Tout type d’analyse pris en charge par la carte vidéo, le client USB Zero ou le client LAN\-peut être utilisé, y compris les moniteurs CRT.  
  
Certaines analyses spéciales incluent un client LAN\-interne ou un client USB Zero. Ces moniteurs incluent généralement des entrées audio\/jacks de sortie et des concentrateurs USB internes pour la connexion des claviers et des souris. Ils se connectent au serveur via une connexion USB ou LAN.  
  
#### <a name="display-resolution"></a>Résolution de l’affichage  
La résolution minimale prise en charge pour la zone d’affichage d’une station est de 512 x 768 pixels. Si le système MultiPoint services démarre et constate que la zone d’affichage d’une station est inférieure à la résolution minimale, un écran vide s’affiche sur cette station et la station n’est pas utilisable.  
  
Si un moniteur d’affichage doit être partagé par deux stations comme des stations d’écran fractionnées\-, la configuration minimale requise pour l’affichage est de 1024 x 768, afin que les zones de l’écran de la station individuelle résultante soient au moins 512 x 768. Pour une expérience utilisateur de fractionnement optimale\-écran, un écran étendu avec un minimum de résolution de 1600 x 900 est recommandé.  
  
## <a name="selecting-other-station-peripheral-devices"></a>Sélection d’autres périphériques de station  
MultiPoint Services prend en charge les périphériques connectés à un concentrateur de station, un client Zero USB ou directement à l’ordinateur. Les appareils connectés à un concentrateur de station sont associés à cette station spécifique. D’autres périphériques sont disponibles pour chaque station lorsqu’ils sont connectés directement à l’ordinateur. Les clients LAN peuvent également prendre en charge les périphériques.  
  
> [!IMPORTANT]  
> Un clavier ne peut pas être connecté à un concentrateur en aval \(par exemple, un concentrateur connecté à un concentrateur de station\). Si vous connectez un clavier à un hub en aval, tous les périphériques branchés\-dans le Hub en aval ne seront plus disponibles pour cette station. Ce comportement permet la prise en charge de connecteurs de station chaînée\-.  
  
**Disponible pour toutes les stations** Un périphérique USB qui est connecté à l’ordinateur \(par exemple, et non via un concentrateur de station\) est disponible pour toutes les stations. En fonction de l’appareil, il peut être utilisé par plusieurs utilisateurs à la fois, ou un seul utilisateur peut y accéder à la fois. Le tableau suivant explique comment accéder aux périphériques USB.  
  
> [!NOTE]  
> La colonne « connecté à l’ordinateur hôte » de la table fait référence au comportement lorsque l’ordinateur qui exécute MultiPoint services s’exécute en mode station avec les stations. Si vous exécutez en mode console, les périphériques connectés n’importe où se comportent de la même façon qu’un serveur standard dans une session de console.  
  
||Connecté à l’ordinateur hôte|Connecté à un concentrateur de station ou à un hub en aval|  
|-|------------------------------|----------------------------------------------|  
|Clavier|Non fonctionnel, sauf s’il fait partie d’une station PS/2. |Disponible pour une station individuelle<p>Ne peut pas être connecté à un hub en aval|  
|Souris|Non fonctionnel, sauf s’il fait partie d’une station PS/2. |Disponible pour une station individuelle|  
|Haut-parleur/casque|Non fonctionnel, sauf s’il fait partie d’une station PS/2.|Disponible pour une station individuelle|  
|Périphérique de stockage USB|Disponible pour toutes les stations|Disponible pour une station individuelle|  
|Contrôle consommateur HID|Non fonctionnel|Disponible pour une station individuelle|  
|Autres périphériques USB, tels que les appareils photo, les lecteurs de documents et les lecteurs de DVD|Disponible pour toutes les stations si elles sont prises en charge par Windows Server 2012|Disponible pour toutes les stations si elles sont prises en charge par Windows Server 2008 R2 Services Bureau à distance|  
  
## <a name="selecting-rdp-over-lan-connected-station-hardware"></a><a name="BKMK_SelectingRDP-over-LAN-connectedstationhardware"></a>Sélection de RDP\-sur\-réseau local\-matériel de station connectée  
Tout client LAN capable de se connecter à Services Bureau à distance à l’aide de protocole RDP (Remote Desktop Protocol) peut devenir une station MultiPoint services.  
  
Si vous souhaitez que le client LAN soit utilisé uniquement en tant que station MultiPoint, vous souhaiterez peut-être « verrouiller » votre client LAN. Par exemple, configurez votre client léger afin qu’il puisse se connecter uniquement à une session MultiPoint services, ou configurez vos ordinateurs de bureau de sorte que l’accès aux icônes de bureau et aux éléments de menu Démarrer tels qu’un navigateur Web soit supprimé pour empêcher l’accès direct à Internet. Vous pouvez effectuer ces configurations à l’aide des outils de configuration de votre client LAN ou de vos stratégies de groupe ou locales.  
  
## <a name="selecting-audio-devices"></a>Sélection de périphériques audio  
Il est important de s’assurer que lorsque vous sélectionnez des périphériques audio, ceux-ci peuvent être branchés dans le concentrateur de station, le client USB Zero ou le client LAN. Certains concentrateurs USB, les clients USB à zéro et les clients LAN disposent d’une prise jack audio analogique qui peut être utilisée avec les périphériques audio analogiques traditionnels \(tels que les\)casque ou earbuds. Les concentrateurs de station qui n’ont pas de prises analogiques peuvent utiliser des périphériques audio USB.  
  
Si vous avez configuré une vidéo\-directe de la carte\-connectée PS\/2 à l’aide de ports PS\/2 sur la carte mère de l’ordinateur pour le clavier et la souris, vous devez utiliser la valeur audio analogique sur la carte mère de l’ordinateur pour que le périphérique audio soit disponible pour cette station lorsque le système MultiPoint services est exécuté en mode station.  
  
Si vous ne disposez pas d’une vidéo PS\/2 direct\-\-connectée, le périphérique audio hôte sur la carte mère du système est disponible uniquement lorsque le système MultiPoint services s’exécute en mode console.  