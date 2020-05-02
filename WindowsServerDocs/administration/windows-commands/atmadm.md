---
title: atmadm
description: Rubrique de référence pour la commande atmadm qui surveille les connexions et les adresses enregistrées par le gestionnaire d’appels atM sur un réseau atM (Asynchronous Transfer Mode).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 37156c2e-c4d4-4fd8-a03d-245fb60bf996
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 32dad00e5a4d03c905f95c48e112f512a9dbc2e5
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718917"
---
# <a name="atmadm"></a>atmadm

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Analyse les connexions et les adresses qui sont enregistrées par le gestionnaire d’appels atM sur un réseau atM (Asynchronous Transfer Mode). Vous pouvez utiliser **atmadm** pour afficher des statistiques sur les appels entrants et sortants sur les adaptateurs atM. Utilisé sans paramètres, **atmadm** affiche des statistiques pour surveiller l’état des connexions atM actives.

## <a name="syntax"></a>Syntaxe

```
atmadm [/c][/a][/s]
```

#### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| ------- | -------- |
| /C | Affiche des informations sur les appels pour toutes les connexions en cours à la carte réseau atM installée sur cet ordinateur. |
| /a | Affiche l’adresse du point d’accès du service réseau atM (NSAP) inscrit pour chaque carte installée sur cet ordinateur. |
| /s | Affiche des statistiques pour surveiller l’état des connexions atM actives. |
| /? | Affiche l'aide à l'invite de commandes. |

### <a name="remarks"></a>Notes 

- La commande **atmadm/c** produit une sortie similaire à ce qui suit :

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

    Le tableau suivant contient les descriptions de chaque élément dans l’exemple de sortie **atmadm/c** .

    | Type de données | Affichage de l’écran | Description |
    | -------- | --------- | -------- |
    | Informations de connexion | Entrée/Sortie | Direction de l’appel. **Dans,** la carte réseau atM d’un autre appareil.  **Out** provient de la carte réseau atM vers un autre appareil. |
    | PMP | Appel point-à-multipoint. |
    | P-P | Appel point à point. |
    | SVC | La connexion est sur un circuit virtuel commuté. |
    | PVC | La connexion est sur un circuit virtuel permanent. |
    | Informations VPI/VCI | VPI/VCI | Chemin d’accès virtuel et canal virtuel de l’appel entrant ou sortant. |
    | Paramètres d’adresse/de média distants | 47000580FFE1000000F21A2E180000C110081500 | Adresse NSAP du périphérique atM appelant **(in)** ou appelé **(out)** . |
    | Émetteur | Le paramètre **TX** comprend les trois éléments suivants :<ul><li>Type de taux de bits par défaut ou spécifié (UBR, CBR, VBR ou ABR)</li><li>Vitesse de ligne par défaut ou spécifiée</li><li>Taille de l’unité de données de service (SDU) spécifiée.</li></ul> |
    | Rx | Le paramètre **RX** comprend les trois éléments suivants :<ul><li>Type de taux de bits par défaut ou spécifié (UBR, CBR, VBR ou ABR)</li><li>Vitesse de ligne par défaut ou spécifiée</li><li>Taille de SDU spécifiée.</li></ul> |

- La commande **atmadm/a** produit une sortie similaire à ce qui suit :

    ```
    Windows atM call Manager Statistics
    atM addresses for Interface : [009] Olicom atM PCI 155 Adapter
    47000580FFE1000000F21A2E180000C110081500
    ```

- La commande **atmadm/s** produit une sortie similaire à ce qui suit :

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

    Le tableau suivant contient les descriptions de chaque élément dans l’exemple de sortie **atmadm/s** .

    | Statistiques du gestionnaire d’appels | Description |
    | ------------- | -------- |
    | Appels actifs en cours | Appelle actuellement actif sur la carte atM installée sur cet ordinateur. |
    | Nombre total d’appels entrants réussis | Appels correctement reçus d’autres appareils sur ce réseau atM. |
    | Nombre total d’appels sortants réussis | Appels correctement effectués à d’autres appareils atM sur ce réseau à partir de cet ordinateur. |
    | Appels entrants ayant échoué | Appels entrants qui n’ont pas pu se connecter à cet ordinateur. |
    | Appels sortants ayant échoué | Appels sortants qui n’ont pas réussi à se connecter à un autre périphérique sur le réseau. |
    | Appels fermés par l’accès distant | Appels fermés par un périphérique distant sur le réseau. |
    | Appels fermés localement | Appels fermés par cet ordinateur. |
    | Paquets de signalisation et ILMI envoyés | Nombre de paquets de l’interface de gestion locale (ILMI) intégrés envoyés au commutateur auquel cet ordinateur tente de se connecter. |
    | Paquets de signalisation et ILMI reçus | Nombre de paquets ILMI reçus du commutateur atM. |

## <a name="examples"></a>Exemples

Pour afficher les informations d’appel de toutes les connexions en cours à la carte réseau atM installée sur cet ordinateur, tapez :

```
atmadm /c
```

Pour afficher l’adresse du point d’accès du service réseau atM (NSAP) inscrit pour chaque carte installée sur cet ordinateur, tapez :

```
atmadm /a
```

Pour afficher les statistiques de surveillance de l’état des connexions atM actives, tapez :

```
atmadm /s
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
