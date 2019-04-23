---
ms.assetid: ''
title: Heure de Windows pour la traçabilité
description: Réglementations dans de nombreux secteurs requièrent des systèmes garantissant la traçabilité en temps UTC.  Cela signifie que vous pouvez attesté par décalage d’un système en ce qui concerne l’heure UTC.
author: shortpatti
ms.author: dacuo
manager: dougkim
ms.date: 10/17/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: e25217feba45516cd0e9a3aa2bf1a2581d2087f5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838040"
---
# <a name="windows-time-for-traceability"></a>Heure de Windows pour la traçabilité
>S’applique à : Windows Server 2016 version 1709 ou ultérieure et Windows 10 version 1703 ou ultérieure


Réglementations dans de nombreux secteurs requièrent des systèmes garantissant la traçabilité en temps UTC.  Cela signifie que vous pouvez attesté par décalage d’un système en ce qui concerne l’heure UTC.  Pour activer les scénarios de conformité aux réglementations, Windows 10 (version 1703 ou une version ultérieure) et Windows Server 2016 (version 1709 ou ultérieure) fournit des nouveaux journaux des événements pour fournir une image du point de vue du système d’exploitation pour former une compréhension des actions effectuées l’horloge système.  Ces journaux des événements sont générés en continu pour le service de temps de Windows et peut être examiné ou archivé pour une analyse ultérieure.

Ces nouveaux événements activer doit répondre aux questions suivantes :

* L’horloge système a été modifié
* La fréquence d’horloge a été modifiée
* La configuration de service de temps de Windows a été modifiée

## <a name="availability"></a>Disponibilité

Ces améliorations sont incluses dans Windows 10 version 1703 ou ultérieure et Windows Server 2016 version 1709 ou ultérieure.

## <a name="configuration"></a>Configuration

Aucune configuration est requise pour réaliser cette fonctionnalité.  Ces journaux des événements sont activés par défaut et se trouve dans l’événement Observateur sous le **Applications et Services Log\Microsoft\Windows\Time-Service\Operational** canal.


## <a name="list-of-event-logs"></a>Liste des journaux des événements

La section suivante décrit les événements enregistrés pour une utilisation dans les scénarios de traçabilité.

<!-- use tabs like the group policies -->
# <a name="257tab257"></a>[257](#tab/257)
Cet événement est consigné quand le Service de temps Windows (W32Time) est démarré et consigne des informations sur l’heure actuelle, nombre de cycles actuel, configuration du runtime, fournisseurs de temps, les taux actuel de l’horloge.

|||
|---|---|
|Description de l’événement |Démarrage du service |
|Détails |Se produit au démarrage de W32time |
|Données enregistrées |<ul><li>Heure actuelle au format UTC</li><li>Nombre de cycles actuel</li><li>Configuration de W32Time</li><li>Configuration du fournisseur de temps</li><li>Fréquence d’horloge</li></ul> |
|Mécanisme de limitation  |Aucun. Cet événement se déclenche chaque fois que le service démarre. |

**Exemple :**
```
W32time service has started at 2018-02-27T04:25:17.156Z (UTC), System Tick Count 3132937.
```

**Commande :**

Ces informations peuvent également être interrogées à l’aide des commandes suivantes

*Configuration W32Time et le fournisseur de temps*
```
w32tm.exe /query /configuration
```

*Fréquence d’horloge*
```
w32tm.exe /query /status /verbose
```


# <a name="258tab258"></a>[258](#tab/258)
Cet événement est consigné quand le Service de temps Windows (W32Time) est l’arrêt et consigne des informations sur le nombre actuel de temps et des graduations.

|||
|---|---|
|Description de l’événement |Arrêt du service |
|Détails |Se produit lors de l’arrêt de W32time |
|Données enregistrées |<ul><li>Heure actuelle au format UTC</li><li>Nombre de cycles actuel</li></ul> |
|Mécanisme de limitation  |Aucun. Cet événement se déclenche chaque fois que le service s’arrête. |

**Exemple de texte :**
`W32time service is stopping at 2018-03-01T05:42:13.944Z (UTC), System Tick Count 6370250.`

# <a name="259tab259"></a>[259](#tab/259)
Cet événement connecte périodiquement à sa liste actuelle des sources de temps et de sa source de temps choisi.  En outre, il enregistre le nombre de cycles actuel.  Cet événement ne déclenche pas à chaque modification d’une source de temps.  Autres événements répertoriés plus loin dans ce document fournissent cette fonctionnalité.

|||
|---|---|
|Description de l’événement |État périodique du fournisseur Client NTP |
|Détails |Liste des sources de temps (s) utilisé par le Client NTP |
|Données enregistrées |<ul><li>Sources de temps disponibles</li><li>Le serveur de temps de référence choisi au moment de la journalisation</li><li>Nombre de cycles actuel</li></ul>  |
|Mécanisme de limitation  |Connecté à toutes les 8 heures. |

**Exemple de texte :** État périodique de fournisseur Client NTP :

NTP Client reçoit des données de temps à partir des serveurs NTP suivantes :

server1.fabrikam.com, 0 x 8 (ntp.m|0x8|[::]:123 -> [IPAddress]:123)server2.fabrikam.com,0x8 (ntp.m|0x8|[::]:123 -> [adresse IP] : 123) ;  et le serveur de temps de référence choisi est Server1.fabrikam.com,0x8 (ntp.m|0x8|[::]:123 -> [adresse IP] : 123) (RefID:0x08d6648e63). Nombre de cycles système 13187937

**Commande** ces informations peuvent également être interrogées à l’aide des commandes suivantes

*Identifier les homologues*
`w32tm.exe /query /peers`

# <a name="260tab260"></a>[260](#tab/260)

|||
|---|---|
|Description de l’événement |État et la configuration de service de temps |
|Détails |W32Time enregistre périodiquement son état et la configuration. C’est l’équivalent de l’appel :<br><br>`w32tm /query /configuration /verbose`<br>OU<br>`w32tm /query /status /verbose` |
|Mécanisme de limitation  |Connecté à toutes les 8 heures. |

# <a name="261tab261"></a>[261](#tab/261)
Chaque instance s’ouvre quand l’heure système est modifié à l’aide des API de SetSystemTime.

|||
|---|---|
|Description de l’événement |Heure système est définie. |
|Mécanisme de limitation  |Aucun.<br><br>Cela doit se produire rarement sur des systèmes avec la synchronisation de temps raisonnable, et nous voulons ouvrir une session de chaque fois qu’il apparaît. Nous ignorons les paramètre TimeJumpAuditOffset lors de l’enregistrement de cet événement dans la mesure où ce paramètre a été conçu pour limiter les événements dans le journal des événements système de Windows. |

# <a name="262tab262"></a>[262](#tab/262)

|||
|---|---|
|Description de l’événement |Fréquence d’horloge système ajustée |
|Détails |Fréquence d’horloge système est constamment modifié par W32time lorsque l’horloge est étroitement synchronisé. Nous voulons capturer « raisonnablement significatifs » ajustements apportés à la fréquence d’horloge sans saturer le journal des événements. |
|Mécanisme de limitation  |Tous les ajustements ci-dessous TimeAdjustmentAuditThreshold d’horloge (min = 128 partie par million, par défaut = 800 partie par million) ne sont pas consignés.<br><br>Modification de 2 PPM de fréquence d’horloge avec une granularité en cours génère 120 modification µs/s dans la précision de l’horloge.<br><br>Sur un système synchronisé, la majorité des ajustements sont sous ce niveau. Si vous souhaitez plus fine de suivi, ce paramètre peut être ajusté vers le bas ou vous pouvez utiliser PerfCounters, ou vous pouvez faire les deux. |

# <a name="263tab263"></a>[263](#tab/263)

|||
|---|---|
|Description de l’événement |Modifier dans les paramètres de service de temps ou de la liste de fournisseurs de temps chargé. |
|Détails |Nouvelle lecture W32time paramètres peut entraîner certains paramètres critiques à être modifié en mémoire, ce qui peut affecter la précision globale de la synchronisation de temps.<br><br>W32Time enregistre chaque occurrence lors de la relecture de ses paramètres, ce qui donne l’impact potentiel sur la synchronisation. |
|Mécanisme de limitation  |Aucun.<br><br>Cet événement se produit uniquement lorsqu’un administrateur ou de mise à jour de la stratégie de groupe change les fournisseurs de temps, puis déclenche W32time. Nous souhaitons enregistrer chaque instance de modification des paramètres. |


# <a name="264tab264"></a>[264](#tab/264)

|||
|---|---|
|Description de l’événement |Modifier dans les sources de temps utilisés par le Client NTP |
|Détails |Client NTP enregistre un événement avec l’état actuel des serveurs de temps/homologues lorsqu’un serveur de temps/homologue change d’état (**en attente de synchronisation de->**, **-> synchronisation inaccessible**, ou d’autres transitions) |
|Mécanisme de limitation  |Fréquence maximale – uniquement une fois toutes les 5 minutes pour empêcher le journal des problèmes temporaires et l’implémentation de fournisseurs incorrecte. |

# <a name="265tab265"></a>[265](#tab/265)

|||
|---|---|
|Description de l’événement |Temps service source ou niveau nombre de modifications |
|Détails |Source de temps W32Time et nombre de couche sont des facteurs importants dans la traçabilité de temps et les modifications apportées à ces doivent être journalisées. Si W32time n’a aucune source de temps et que vous n’avez pas configuré en tant que source de temps fiable, puis il cessera de publicité comme un serveur de temps et à la conception répondre aux requêtes avec des paramètres non valides. Cet événement est essentiel pour suivre les modifications d’état dans une topologie NTP. |
|Mécanisme de limitation  |Aucun. |


# <a name="266tab266"></a>[266](#tab/266)

|||
|---|---|
|Description de l’événement |La resynchronisation de temps est demandée. |
|Détails |Cette opération est déclenchée :<ul><li>Lorsque les modifications de réseau se produisent</li><li>Système retourne standby/veille connectée</li><li>Lorsque nous synchronisés pendant une longue période</li><li>Administrateur émet la commande de resynchronisation</li></ul>Cette opération entraîne une perte immédiate de précision de synchronisation de temps précis, car elle force le client NTP effacer les filtres. |
|Mécanisme de limitation  |Fréquence max - toutes les 5 minutes.<br><br>Il est possible qu’une carte réseau défectueux (ou un script médiocre) permettre déclencher cette opération à plusieurs reprises et entraîner bien submergés de journaux. Par conséquent, la nécessité de limiter cet événement.<br><br>Notez que la synchronisation de l’heure précise prend beaucoup plus de 5 minutes pour atteindre et la limitation ne perd pas d’informations sur l’événement d’origine qui a entraîné une perte de précision du temps.  |

---
