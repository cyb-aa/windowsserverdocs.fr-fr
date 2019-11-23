---
ms.assetid: ''
title: Temps Windows pour la traçabilité
description: Dans de nombreux secteurs, les réglementations requièrent que les systèmes soient traçables au format UTC.  Cela signifie que le décalage d’un système peut être sanctionné par rapport au temps universel coordonné (UTC).
author: shortpatti
ms.author: dacuo
manager: dougkim
ms.date: 10/17/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: 307739042426088fa92c50e6ea4dc5d2a744f15a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405206"
---
# <a name="windows-time-for-traceability"></a>Temps Windows pour la traçabilité
>S’applique à : Windows Server 2016 version 1709 ou ultérieure et Windows 10 version 1703 ou ultérieure


Dans de nombreux secteurs, les réglementations requièrent que les systèmes soient traçables au format UTC.  Cela signifie que le décalage d’un système peut être sanctionné par rapport au temps universel coordonné (UTC).  Pour activer les scénarios de conformité réglementaire, Windows 10 (version 1703 ou ultérieure) et Windows Server 2016 (version 1709 ou ultérieure) fournissent de nouveaux journaux des événements pour fournir une image du point de vue du système d’exploitation pour comprendre les actions effectuées sur horloge système.  Ces journaux des événements sont générés en continu pour le service de temps Windows et peuvent être examinés ou archivés en vue d’une analyse ultérieure.

Ces nouveaux événements permettent de répondre aux questions suivantes :

* L’horloge système a-elle été modifiée
* La fréquence d’horloge a été modifiée
* La configuration du service de temps Windows était-elle modifiée

## <a name="availability"></a>Disponibilité

Ces améliorations sont incluses dans Windows 10 version 1703 ou ultérieure, et Windows Server 2016 version 1709 ou ultérieure.

## <a name="configuration"></a>Configuration

Aucune configuration n’est requise pour réaliser cette fonctionnalité.  Ces journaux des événements sont activés par défaut et se trouvent dans l’observateur d’événements sous le canal **Log\Microsoft\Windows\Time-Service\Operational des applications et des services** .


## <a name="list-of-event-logs"></a>Liste des journaux des événements

La section suivante présente les événements consignés pour une utilisation dans les scénarios de traçabilité.

# <a name="257tab257"></a>[257](#tab/257)
Cet événement est consigné lors du démarrage du service de temps Windows (W32Time) et enregistre les informations relatives à l’heure actuelle, au nombre de cycles actuel, à la configuration du runtime, aux fournisseurs de temps et au taux d’horloge actuel.

|||
|---|---|
|Description de l’événement |Démarrage du service |
|Détails |Se produit au démarrage de w32time |
|Données enregistrées |<ul><li>Heure actuelle en heure UTC</li><li>Nombre de cycles actuel</li><li>Configuration de W32Time</li><li>Configuration du fournisseur de temps</li><li>Fréquence d’horloge</li></ul> |
|Mécanisme de limitation  |Aucun. Cet événement se déclenche chaque fois que le service démarre. |

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
Cet événement est consigné lorsque le service de temps Windows (W32Time) est en cours d’arrêt et journalise des informations sur l’heure actuelle et le nombre de cycles.

|||
|---|---|
|Description de l’événement |Arrêt du service |
|Détails |Se produit à l’arrêt de w32time |
|Données enregistrées |<ul><li>Heure actuelle en heure UTC</li><li>Nombre de cycles actuel</li></ul> |
|Mécanisme de limitation  |Aucun. Cet événement se déclenche chaque fois que le service s’arrête. |

**Texte d’exemple :** 
`W32time service is stopping at 2018-03-01T05:42:13.944Z (UTC), System Tick Count 6370250.`

# <a name="259tab259"></a>[259](#tab/259)
Cet événement journalise régulièrement la liste actuelle des sources de temps et la source de temps choisie.  En outre, il enregistre le nombre de cycles actuel.  Cet événement ne se déclenche pas chaque fois qu’une source de temps change.  D’autres événements listés plus loin dans ce document fournissent cette fonctionnalité.

|||
|---|---|
|Description de l’événement |État périodique du fournisseur de clients NTP |
|Détails |Liste des sources de temps utilisées par le client NTP |
|Données enregistrées |<ul><li>Sources de temps disponibles</li><li>Le serveur de temps de référence choisi au moment de la journalisation</li><li>Nombre de cycles actuel</li></ul>  |
|Mécanisme de limitation  |Consigné une fois toutes les 8 heures. |

**Exemple de texte :** État périodique du fournisseur de clients NTP :

Le client NTP reçoit les données de temps des serveurs NTP suivants :

Serveur1. fabrikam. com, 0x8 (NTP. m | 0x8 | [ ::] : 123-> [IPAddress] : 123) Server2. fabrikam. com, 0x8 (NTP. m | 0x8 | [ ::] : 123-> [IPAddress] : 123);  et le serveur de temps de référence choisi est server1. fabrikam. com, 0x8 (NTP. m | 0x8 | [ ::] : 123-> [IPAddress] : 123) (RefID : 0x08d6648e63). Nombre de cycles système 13187937

**Commande** Vous pouvez également interroger ces informations à l’aide des commandes suivantes :

*Identifier les homologues*
`w32tm.exe /query /peers`

# <a name="260tab260"></a>[260](#tab/260)

|||
|---|---|
|Description de l’événement |Configuration du service de temps et état |
|Détails |W32time journalise régulièrement sa configuration et son état. Il s’agit de l’équivalent de l’appel de :<br><br>`w32tm /query /configuration /verbose`<br>OU<br>`w32tm /query /status /verbose` |
|Mécanisme de limitation  |Consigné une fois toutes les 8 heures. |

# <a name="261tab261"></a>[261](#tab/261)
Cela journalise chaque instance lorsque l’heure système est modifiée à l’aide de l’API SetSystemTime.

|||
|---|---|
|Description de l’événement |L’heure système est définie |
|Mécanisme de limitation  |Aucun.<br><br>Cela devrait se produire rarement sur les systèmes avec une synchronisation horaire raisonnable, et nous voulons les enregistrer à chaque fois. Nous ignorons le paramètre TimeJumpAuditOffset lors de l’enregistrement de cet événement, car ce paramètre était destiné à limiter les événements dans le journal des événements système de Windows. |

# <a name="262tab262"></a>[262](#tab/262)

|||
|---|---|
|Description de l’événement |Fréquence d’horloge du système ajustée |
|Détails |La fréquence d’horloge du système est constamment modifiée par w32time lorsque l’horloge est synchronisée. Nous souhaitons capturer des ajustements « raisonnablement significatifs » apportés à la fréquence d’horloge sans exécuter le journal des événements. |
|Mécanisme de limitation  |Tous les ajustements d’horloge inférieurs à TimeAdjustmentAuditThreshold (min = 128 composant par million, par défaut = 800 pièce par million) ne sont pas journalisés.<br><br>2 PPM dans la fréquence d’horloge avec une granularité actuelle, 120 μsec/s changent en précision de l’horloge.<br><br>Sur un système synchronisé, la majorité des ajustements sont inférieurs à ce niveau. Si vous souhaitez un suivi plus fin, ce paramètre peut être ajusté ou vous pouvez utiliser PerfCounters, ou vous pouvez faire les deux. |

# <a name="263tab263"></a>[263](#tab/263)

|||
|---|---|
|Description de l’événement |Modifiez les paramètres du service de temps ou la liste des fournisseurs de temps chargés. |
|Détails |La relecture des paramètres de w32time peut entraîner la modification en mémoire de certains paramètres critiques, ce qui peut affecter la précision globale de la synchronisation de l’heure.<br><br>W32time journalise chaque occurrence lors de la relecture de ses paramètres, ce qui donne un impact potentiel sur la synchronisation de l’heure. |
|Mécanisme de limitation  |Aucun.<br><br>Cet événement se produit uniquement lorsqu’une mise à jour de l’administrateur ou de la stratégie de protection modifie les fournisseurs de temps, puis déclenche w32time. Nous souhaitons enregistrer chaque instance de modification des paramètres. |


# <a name="264tab264"></a>[264](#tab/264)

|||
|---|---|
|Description de l’événement |Modification de la ou des sources de temps utilisées par le client NTP |
|Détails |Le client NTP enregistre un événement avec l’état actuel des serveurs/homologues lorsqu’un serveur de temps/homologue change d’État (**en attente de synchronisation >** , de **synchronisation > inaccessible**ou d’autres transitions) |
|Mécanisme de limitation  |Fréquence maximale : une fois toutes les 5 minutes pour protéger le journal des problèmes temporaires et une implémentation de fournisseur incorrecte. |

# <a name="265tab265"></a>[265](#tab/265)

|||
|---|---|
|Description de l’événement |Modifications de la source du service de temps ou du nombre de couches |
|Détails |La source de temps w32time et le nombre de couches sont des facteurs importants dans la traçabilité et toutes les modifications apportées à celles-ci doivent être journalisées. Si w32time n’a pas de source de temps et que vous n’avez pas configuré comme source de temps fiable, il cessera de publier en tant que serveur de temps et, par conception, répondra aux demandes avec des paramètres non valides. Cet événement est essentiel pour suivre les changements d’État dans une topologie NTP. |
|Mécanisme de limitation  |Aucun. |


# <a name="266tab266"></a>[266](#tab/266)

|||
|---|---|
|Description de l’événement |Demande de resynchronisation de l’heure |
|Détails |Cette opération est déclenchée :<ul><li>En cas de modification du réseau</li><li>Le système revient du mode veille/veille connectée</li><li>Quand nous n’avions pas effectué de synchronisation depuis longtemps</li><li>L’administrateur émet la commande Resync</li></ul>Cette opération entraîne une perte immédiate de la précision de la synchronisation du temps, car elle force le client NTP à effacer ses filtres. |
|Mécanisme de limitation  |Fréquence maximale : une fois toutes les 5 minutes.<br><br>Il est possible qu’une carte réseau incorrecte (ou un script médiocre) déclenche cette opération à plusieurs reprises et que les journaux soient submergés. C’est pourquoi il est nécessaire de limiter cet événement.<br><br>Notez que la synchronisation de l’heure précise prend plus de 5 minutes et que la limitation ne perd pas d’informations sur l’événement d’origine qui a entraîné une perte de précision.  |

---
