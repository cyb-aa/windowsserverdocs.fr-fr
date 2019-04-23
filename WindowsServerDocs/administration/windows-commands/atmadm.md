---
title: atmadm
description: Rubrique de commandes de Windows pour **atmadm** -surveille les connexions et les adresses qui sont inscrits par l’atM appellent le gestionnaire sur un réseau transfert asynchrone (atM).
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 37156c2e-c4d4-4fd8-a03d-245fb60bf996
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3a8a8883bad9aa2abdc90d5db5702ef42f46c8a8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831770"
---
# <a name="atmadm"></a>atmadm

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Surveille les connexions et les adresses qui sont inscrits par l’atM appellent Manager sur un réseau transfert asynchrone (atM). Vous pouvez utiliser **atmadm** pour afficher des statistiques pour les appels entrants et sortants sur les adaptateurs atM. Utilisée sans paramètres, **atmadm** affiche des statistiques pour la surveillance de l’état des connexions atM active. 

## <a name="syntax"></a>Syntaxe

```
atmadm [/c][/a][/s]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|-------|--------|
|/c|Informations sur les appels affiche pour toutes les connexions actives à la carte réseau atM installée sur cet ordinateur.|
|/a|Affiche l’accès au service de réseau atM inscrits point adresse (NSAP) pour chaque carte installée sur cet ordinateur.|
|/s|Affiche les statistiques pour surveiller l’état des connexions atM active.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

- Le **atmadm /c** commande produit une sortie similaire à ce qui suit :

    ```
    Windows atM call Manager Statistics
    atM Connections on Interface : [009] Olicom atM PCI 155 Adapter
       Connection   VPI/VCI   remote address/
                              Media Parameters (rates in bytes/sec)
       In  PMP SVC    0/193   47000580FFE1000000F21A2E180020481A2E180B
                              Tx:UBR,Peak 0,Avg 0,MaxSdu 1516
                              Rx:UBR,Peak 16953936,Avg 16953936,MaxSdu 1516
       Out P-P SVC    0/192   47000580FFE1000000F21A2E180020481A2E180B
                              Tx:UBR,Peak 16953936,Avg 16953936,MaxSdu 1516
                              Rx:UBR,Peak 16953936,Avg 16953936,MaxSdu 1516
       In  PMP SVC    0/191   47000580FFE1000000F21A2E180020481A2E180B
                              Tx:UBR,Peak 0,Avg 0,MaxSdu 1516
                              Rx:UBR,Peak 16953936,Avg 16953936,MaxSdu 1516
       Out P-P SVC    0/190   47000580FFE1000000F21A2E180020481A2E180B
                              Tx:UBR,Peak 16953936,Avg 16953936,MaxSdu 1516
                              Rx:UBR,Peak 16953936,Avg 16953936,MaxSdu 1516
       In  P-P SVC    0/475   47000580FFE1000000F21A2E180000C110081501
                              Tx:UBR,Peak 16953984,Avg 16953984,MaxSdu 9188
                              Rx:UBR,Peak 16953936,Avg 16953936,MaxSdu 9188
       Out PMP SVC    0/194   47000580FFE1000000F21A2E180000C110081501 (0)
                              Tx:UBR,Peak 16953984,Avg 16953984,MaxSdu 9180
                              Rx:UBR,Peak 0,Avg 0,MaxSdu 0
       Out P-P SVC    0/474   4700918100000000613E5BFE010000C110081500
                              Tx:UBR,Peak 16953984,Avg 16953984,MaxSdu 9188
                              Rx:UBR,Peak 16953984,Avg 16953984,MaxSdu 9188
       In  PMP SVC    0/195   47000580FFE1000000F21A2E180000C110081500
                              Tx:UBR,Peak 0,Avg 0,MaxSdu 0
                              Rx:UBR,Peak 16953936,Avg 16953936,MaxSdu 9180
    ```
    
    Le tableau suivant contient des descriptions de chaque élément dans le **atmadm /c** exemple de sortie.
    
    |type de données|Capture d’écran|Description|
    |--------|---------|--------|
    |Informations de connexion|Entrée/Sortie|direction de l’appel.  **Dans** consiste à la carte réseau atM depuis un autre appareil.  **Out** provient de la carte réseau atM vers un autre appareil.|
    ||PMP|Appel de point-à-multipoint.|
    ||P-P|Appel de point à point.|
    ||SVC|La connexion est sur un circuit virtuel commuté.|
    ||PVC|La connexion est sur un circuit virtuel permanent.|
    |Informations VPI/VCI|VPI/VCI|Chemin d’accès virtuel et le canal virtuel de l’appel entrant ou sortant.|
    |adresse distante/paramètres de média|47000580FFE1000000F21A2E180000C110081500|Adresse NSAP de l’appel **(In)** ou appelé **(Out)** périphérique atM.|
    ||**Tx**|Le **Tx** paramètre inclut trois éléments suivants :<br /><br />-Par défaut ou spécifiée du type de vitesse de transmission (UBR, CBR, VBR ou ABR)<br />-Par défaut ou spécifiée la vitesse de ligne<br />-Taille d’unité (SDU) de données de service spécifié|
    ||**Rx**|Le **Rx** paramètre inclut trois éléments suivants :<br /><br />-Par défaut ou spécifiée du type de vitesse de transmission (UBR, CBR, VBR ou ABR)<br />-Par défaut ou spécifiée la vitesse de ligne<br />-Taille SDU spécifiée|
    
- Le **atmadm /a** commande produit une sortie similaire à ce qui suit :

    ```
    Windows atM call Manager Statistics
    atM addresses for Interface : [009] Olicom atM PCI 155 Adapter
    47000580FFE1000000F21A2E180000C110081500
    ```
    
- Le **atmadm /s** commande produit une sortie similaire à ce qui suit :

    ```
    Windows atM call Manager Statistics
    atM call Manager statistics for Interface : [009] Olicom atM PCI 155 Adapter
    Current active calls                        = 4
    Total successful Incoming calls             = 1332
    Total successful Outgoing calls             = 1297
    Unsuccessful Incoming calls                 = 1
    Unsuccessful Outgoing calls                 = 1
    calls Closed by remote                      = 1302
    calls Closed Locally                        = 1323
    Signaling and ILMI Packets Sent            = 33655
    Signaling and ILMI Packets Received        = 34989
    ```
    
    Le tableau suivant contient des descriptions de chaque élément dans le **atmadm /s** exemple de sortie.
    
    |appel de statistiques de gestionnaire|Description|
    |-------------|--------|
    |Appels actifs en cours|appels actuellement actifs sur la carte atM installée sur cet ordinateur.|
    |Nombre total d’appels entrants réussis|appels reçus avec succès à partir d’autres appareils sur ce réseau atM.|
    |Nombre total d’appels sortants réussis|appels terminées à d’autres périphériques atM sur ce réseau à partir de cet ordinateur.|
    |Appels entrants ayant échoué|Appels entrants qui n’a pas pu se connecter à cet ordinateur.|
    |Appels sortants ayant échoué|Appels sortants qui n’a pas pu se connecter à un autre appareil sur le réseau.|
    |appels fermé à distance|Appels fermés par un périphérique distant sur le réseau.|
    |appelle fermé localement|Appels fermés par cet ordinateur.|
    |La signalisation et les paquets ILMI envoyés|Nombre de paquets d’interface (ILMI) de gestion locale intégrée envoyés au commutateur à laquelle cet ordinateur tente de se connecter.|
    |La signalisation et les paquets ILMI reçus|Nombre de paquets ILMI reçus du commutateur atM.|
    
## <a name="BKMK_Examples"></a>Exemples

Pour afficher les informations d’appel pour toutes les connexions actives à la carte réseau atM installée sur cet ordinateur, tapez :

```
atmadm /c
```

Pour afficher l’adresse atM inscrits réseau service access point (NSAP) pour chaque carte installée sur cet ordinateur, tapez :

```
atmadm /a
```

Pour afficher les statistiques de surveillance de l’état des connexions atM active, tapez :

```
atmadm /s
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
