---
ms.assetid: ''
title: Temps Windows pour la traçabilité
description: Dans de nombreux secteurs, les réglementations exigent que les systèmes soient traçables au format UTC.  Cela signifie que le décalage d’un système peut être attesté par rapport au temps universel coordonné (UTC).
author: shortpatti
ms.author: dacuo
manager: dougkim
ms.date: 10/17/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: 307739042426088fa92c50e6ea4dc5d2a744f15a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405206"
---
# <a name="windows-time-for-traceability"></a>Temps Windows pour la traçabilité
>S'applique à : Windows Server 2016 version 1709 ou ultérieure et Windows 10 version 1703 ou ultérieure


Dans de nombreux secteurs, les réglementations exigent que les systèmes soient traçables au format UTC.  Cela signifie que le décalage d’un système peut être attesté par rapport au temps universel coordonné (UTC).  Pour autoriser les scénarios de conformité réglementaire, Windows 10 (version 1703 ou ultérieure) et Windows Server 2016 (version 1709 ou ultérieure) fournissent de nouveaux journaux des événements afin de proposer une image du point de vue du système d’exploitation permettant de comprendre les actions effectuées sur l’horloge système.  Ces journaux des événements sont générés en continu pour le service de temps Windows et peuvent être examinés ou archivés en vue de les analyser ultérieurement.

Ces nouveaux événements permettent de répondre aux questions suivantes :

* L’horloge système a-t-elle été modifiée ?
* La fréquence d’horloge a-t-elle été modifiée ?
* La configuration du service de temps Windows a-t-elle été modifiée ?

## <a name="availability"></a>Disponibilité

Ces améliorations sont incluses dans Windows 10 version 1703 ou ultérieure et Windows Server 2016 version 1709 ou ultérieure.

## <a name="configuration"></a>Configuration

Aucune configuration n’est nécessaire pour réaliser cette fonctionnalité.  Ces journaux des événements sont activés par défaut et se trouvent dans l’observateur d’événements sous le canal **Applications and Services Log\Microsoft\Windows\Time-Service\Operational**.


## <a name="list-of-event-logs"></a>Liste des journaux des événements

La section suivante présente les événements journalisés en vue d’une utilisation dans les scénarios de traçabilité.

# <a name="257tab257"></a>[257](#tab/257)
Cet événement est journalisé lors du démarrage du service de temps Windows (W32Time) et journalise les informations relatives à l’heure actuelle, au nombre de cycles actuel, à la configuration du runtime, aux fournisseurs de temps et à la fréquence d’horloge actuelle.

|||
|---|---|
|Description de l'événement |Début du service |
|Détails |Se produit au démarrage de W32time |
|Données journalisées |<ul><li>Heure actuelle UTC</li><li>Nombre de cycles actuel</li><li>Configuration de W32Time</li><li>Configuration du fournisseur de temps</li><li>Fréquence d’horloge</li></ul> |
|Mécanisme de limitation  |Aucune. Cet événement se déclenche chaque fois que le service démarre. |

**Exemple :**
```
W32time service has started at 2018-02-27T04:25:17.156Z (UTC), System Tick Count 3132937.
```

**Commande :**

Vous pouvez également interroger ces informations à l’aide des commandes suivantes :

*Configuration du fournisseur de temps et de W32Time*
```
w32tm.exe /query /configuration
```

*Fréquence d’horloge*
```
w32tm.exe /query /status /verbose
```


# <a name="258tab258"></a>[258](#tab/258)
Cet événement est journalisé quand le service de temps Windows (W32Time) est en cours d’arrêt et journalise des informations sur l’heure actuelle et le nombre de cycles.

|||
|---|---|
|Description de l'événement |Arrêt du service |
|Détails |Se produit à l’arrêt de W32time |
|Données journalisées |<ul><li>Heure actuelle UTC</li><li>Nombre de cycles actuel</li></ul> |
|Mécanisme de limitation  |Aucune. Cet événement se déclenche chaque fois que le service s’arrête. |

**Exemple de texte :** 
`W32time service is stopping at 2018-03-01T05:42:13.944Z (UTC), System Tick Count 6370250.`

# <a name="259tab259"></a>[259](#tab/259)
Cet événement journalise régulièrement la liste actuelle des sources de temps et la source de temps choisie.  De plus, il journalise le nombre de cycles actuel.  Cet événement ne se déclenche pas chaque fois qu’une source de temps change.  D’autres événements listés plus loin dans ce document fournissent cette fonctionnalité.

|||
|---|---|
|Description de l'événement |État périodique du fournisseur de clients NTP |
|Détails |Liste des sources de temps utilisées par le client NTP |
|Données journalisées |<ul><li>Sources de temps disponibles</li><li>Serveur de temps de référence choisi au moment de la journalisation</li><li>Nombre de cycles actuel</li></ul>  |
|Mécanisme de limitation  |Journalisé une fois toutes les 8 heures. |

**Exemple de texte :** État périodique du fournisseur de clients NTP :

Le client NTP reçoit les données de temps des serveurs NTP suivants :

server1.fabrikam.com,0x8 (ntp.m|0x8|[::]:123->[IPAddress]:123)server2.fabrikam.com,0x8 (ntp.m|0x8|[::]:123->[IPAddress]:123); et le serveur de temps de référence choisi est Server1.fabrikam.com,0x8 (ntp.m|0x8|[::]:123->[IPAddress]:123) (RefID:0x08d6648e63). Nombre de cycles système 13187937

**Commande** Vous pouvez également interroger ces informations à l’aide des commandes suivantes :

*Identifier les pairs*
`w32tm.exe /query /peers`

# <a name="260tab260"></a>[260](#tab/260)

|||
|---|---|
|Description de l'événement |Configuration du service de temps et état |
|Détails |W32time journalise régulièrement sa configuration et son état. Il s’agit de l’équivalent de l’appel de :<br><br>`w32tm /query /configuration /verbose`<br>OU<br>`w32tm /query /status /verbose` |
|Mécanisme de limitation  |Journalisé une fois toutes les 8 heures. |

# <a name="261tab261"></a>[261](#tab/261)
Journalise chaque instance où l’heure système est modifiée à l’aide de l’API SetSystemTime.

|||
|---|---|
|Description de l'événement |L’heure système est définie |
|Mécanisme de limitation  |Aucune.<br><br>Cela devrait se produire rarement sur les systèmes avec une synchronisation horaire raisonnable, et nous voulons le journaliser à chaque fois. Nous ignorons le paramètre TimeJumpAuditOffset lors de la journalisation de cet événement, car ce paramètre était destiné à limiter les événements dans le journal des événements système de Windows. |

# <a name="262tab262"></a>[262](#tab/262)

|||
|---|---|
|Description de l'événement |Fréquence d’horloge système ajustée |
|Détails |La fréquence d’horloge système est constamment modifiée par W32time quand l’horloge n’est pas loin d’être synchronisée. Nous souhaitons capturer les ajustements « raisonnablement significatifs » apportés à la fréquence d’horloge sans saturer le journal des événements. |
|Mécanisme de limitation  |Tous les ajustements d’horloge inférieurs à TimeAdjustmentAuditThreshold (min = 128 parties par million, par défaut = 800 parties par million) ne sont pas journalisés.<br><br>Une modification de 2 PPM de la fréquence d’horloge avec la précision actuelle engendre un changement de 120 μsec/s de la précision de l’horloge.<br><br>Sur un système synchronisé, la majorité des ajustements sont inférieurs à ce niveau. Si vous souhaitez un suivi plus fin, vous pouvez abaisser ce paramètre et/ou utiliser PerfCounters. |

# <a name="263tab263"></a>[263](#tab/263)

|||
|---|---|
|Description de l'événement |Modification des paramètres du service de temps ou de la liste des fournisseurs de temps chargés. |
|Détails |La relecture des paramètres de W32time peut entraîner la modification en mémoire de certains paramètres critiques, ce qui peut affecter la précision globale de la synchronisation du temps.<br><br>W32time journalise chaque occurrence lors de la relecture de ses paramètres, ce qui indique l’impact potentiel sur la synchronisation de l’heure. |
|Mécanisme de limitation  |Aucune.<br><br>Cet événement se produit uniquement quand un administrateur ou une mise à jour de la stratégie de groupe change les fournisseurs de temps, puis déclenche W32time. Nous souhaitons journaliser chaque instance de modification des paramètres. |


# <a name="264tab264"></a>[264](#tab/264)

|||
|---|---|
|Description de l'événement |Modification des sources de temps utilisées par le client NTP |
|Détails |Le client NTP enregistre un événement avec l’état actuel des serveurs/pairs quand un serveur de temps/pair change d’état (**En attente > Synchronisation**, **Synchronisation > Inaccessible** ou autres transitions). |
|Mécanisme de limitation  |Fréquence maximale : une fois toutes les 5 minutes pour protéger le journal des problèmes temporaires et d’une implémentation de fournisseur incorrecte. |

# <a name="265tab265"></a>[265](#tab/265)

|||
|---|---|
|Description de l'événement |Modifications de la source du service de temps ou du nombre de couches |
|Détails |La source de temps W32time et le nombre de couches sont des facteurs importants de la traçabilité et toutes les modifications apportées à ceux-ci doivent être journalisées. Si W32time n’a pas de source de temps et que vous n’avez pas configuré de source de temps fiable, il cesse de se publier en tant que serveur de temps et répond donc aux demandes avec des paramètres non valides. Cet événement est essentiel pour suivre les changements d’état d’une topologie NTP. |
|Mécanisme de limitation  |Aucune. |


# <a name="266tab266"></a>[266](#tab/266)

|||
|---|---|
|Description de l'événement |Demande de resynchronisation de l’heure |
|Détails |Cette opération est déclenchée :<ul><li>En cas de modification du réseau</li><li>Quand le système revient du mode veille connectée/mise en veille prolongée</li><li>Quand aucune synchronisation n’a été effectuée depuis longtemps</li><li>Quand l’administrateur émet la commande de resynchronisation</li></ul>Cette opération entraîne la perte immédiate de la grande précision de la synchronisation du temps, car elle force le client NTP à effacer ses filtres. |
|Mécanisme de limitation  |Fréquence maximale : une fois toutes les 5 minutes.<br><br>Il est possible qu’une carte réseau incorrecte (ou un script médiocre) déclenche cette opération à plusieurs reprises et entraîne une saturation des journaux. C’est pourquoi il est nécessaire de limiter cet événement.<br><br>Notez que la synchronisation du temps précise prend beaucoup plus que 5 minutes et que la limitation ne perd pas d’informations sur l’événement à l’origine de la perte de précision.  |

---
