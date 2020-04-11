---
ms.assetid: ''
title: Configuration de systèmes de haute précision
description: La synchronisation de l’heure dans Windows 10 et Windows Server 2016 a été considérablement améliorée.  Dans des conditions de fonctionnement raisonnables, les systèmes peuvent être configurés pour maintenir une précision de 1 ms (milliseconde), ou une meilleure précision, par rapport à l’heure UTC.
author: dcuomo
ms.author: dacuo
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: 25472e4ba4837bd68c9b6914e22c2219c91d3ac0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861652"
---
# <a name="configuring-systems-for-high-accuracy"></a>Configuration de systèmes de haute précision
>S'applique à : Windows Server 2016 et Windows 10, version 1607 ou ultérieure

La synchronisation de l’heure dans Windows 10 et Windows Server 2016 a été considérablement améliorée.  Dans des conditions de fonctionnement raisonnables, les systèmes peuvent être configurés pour maintenir une précision de 1 ms (milliseconde), ou une meilleure précision, par rapport à l’heure UTC.

Les conseils suivants vous aident à configurer vos systèmes pour obtenir une haute précision.  Cet article présente les exigences suivantes :

- Systèmes d'exploitation pris en charge
- Configuration du système 

> [!WARNING]
> **Objectifs de précision des systèmes d’exploitation antérieurs**<br>
>Windows Server 2012 R2 et versions antérieures ne peuvent pas atteindre les mêmes objectifs de haute précision. Ces systèmes d’exploitation ne sont pas pris en charge pour la haute précision.
>
>Dans ces versions, le service de temps Windows respectait les exigences suivantes :
>
> - Il fournissait la précision de temps nécessaire pour répondre aux exigences d’authentification Kerberos version 5.
> - Il fournissait un temps moyennement précis pour les clients et les serveurs Windows joints à une forêt Active Directory commune.
>
>Les tolérances plus élevées sur 2012 R2 et les versions antérieures n’entrent pas dans les spécifications conceptuelles du service de temps Windows.

## <a name="windows-10-and-windows-server-2016-default-configuration"></a>Configuration par défaut sur Windows 10 et Windows Server 2016

Même si nous prenons en charge une précision allant jusqu’à 1 ms sur Windows 10 ou Windows Server 2016, la majorité des clients n’ont pas besoin d’un temps haute précision.

Ainsi, la **configuration par défaut** est destinée à répondre aux mêmes exigences que les systèmes d’exploitation antérieurs, à savoir :

- Fournir la précision nécessaire pour répondre aux exigences d’authentification Kerberos version 5.
- Fournir une heure moyennement précise pour les clients et les serveurs Windows joints à une forêt Active Directory commune.

## <a name="how-to-configure-systems-for-high-accuracy"></a>Comment configurer des systèmes de haute précision

>[!IMPORTANT]
>**Remarque relative à la prise en charge des systèmes de haute précision**<br>
> La précision de l’heure implique la distribution de bout en bout d’une heure précise depuis une source de temps de référence vers l’appareil final.  Tout ce qui ajoute une asymétrie dans les mesures sur ce chemin a une incidence négative sur la précision obtenue sur vos appareils.
>
>Pour cette raison, nous avons documenté les [Limites de prise en charge pour configurer le service de temps Windows pour les environnements de haute précision](support-boundary.md), qui soulignent les exigences environnementales à également respecter pour atteindre les objectifs de haute précision.

### <a name="operating-system-requirements"></a>Systèmes d'exploitation requis

Les configurations de haute précision nécessitent Windows 10 ou Windows Server 2016.  Tous les appareils Windows dans la topologie du temps doivent répondre à cette exigence, y compris les serveurs de temps Windows de couche supérieure et, dans les scénarios virtualisés, les hôtes Hyper-V qui exécutent les machines virtuelles sensibles au facteur temps. Tous ces appareils doivent exécuter au moins Windows 10 ou Windows Server 2016.

Dans l’illustration ci-dessous, les machines virtuelles nécessitant une haute précision exécutent Windows 10 ou Windows Server 2016.  De même, l’hôte Hyper-V sur lequel résident les machines virtuelles et le serveur de temps Windows en amont doivent également exécuter Windows Server 2016.

![Topologie du temps - 1607](../media/Windows-Time-Service/Configuring-Systems-for-High-Accuracy/Topology2016.png)


>[!TIP] 
>**Détermination de la version de Windows**<br>
> Vous pouvez exécuter la commande `winver` depuis une invite de commandes pour vérifier que la version du système d’exploitation est 1607 (ou supérieure) et que la build du système d’exploitation est 14393 (ou supérieure), comme indiqué ci-dessous :
>
> ![Winver - 2016 1607](../media/Windows-Time-Service/Configuring-Systems-for-High-Accuracy/winver2016.png)

### <a name="system-configuration"></a>Configuration du système

Pour atteindre des objectifs de haute précision, vous devez configurer le système.  Il existe plusieurs façons d’effectuer cette configuration, notamment directement dans le registre ou par le biais de la stratégie de groupe.  Pour plus d’informations sur chacun de ces paramètres, consultez les Informations techniques de référence sur le service de temps Windows : [Outils du service de temps Windows](Windows-Time-Service-Tools-and-Settings.md#windows-time-service-tools).

#### <a name="windows-time-service-startup-type"></a>Type de démarrage du service de temps Windows

Le service de temps Windows (W32Time) doit s’exécuter en continu.  Pour ce faire, configurez le type de démarrage du service de temps Windows sur « Automatique ».

![Configuration automatique](../media/Windows-Time-Service/Configuring-Systems-for-High-Accuracy/AutomaticService.PNG)

#### <a name="cumulative-one-way-network-latency"></a>Latence réseau unidirectionnelle cumulée

L’incertitude des mesures et le « bruit » progressent à mesure qu’augmente la latence réseau.  Ainsi, il est impératif que la latence réseau se situe dans une limite raisonnable.  Les exigences spécifiques dépendent de la précision cible et sont présentées dans l’article [Limites de prise en charge pour configurer le service de temps Windows pour les environnements de haute précision](support-boundary.md).

Pour calculer la latence réseau unidirectionnelle cumulée, ajoutez les délais unidirectionnels individuels entre les paires de nœuds client-serveur NTP dans la topologie du temps, en commençant par la cible et en finissant à la source de temps de la couche 1 de haute précision.

Par exemple : Considérez une hiérarchie de synchronisation horaire comportant une source extrêmement précise, deux serveurs NTP intermédiaires A et B et la machine cible, dans cet ordre. Pour obtenir la latence réseau cumulée entre la cible et la source, mesurez les temps d’aller-retour (RTT) NTP individuels moyens entre :

- Le serveur cible et le serveur de temps B
- Le serveur de temps B et le serveur de temps A
- Le serveur de temps A et la source

Cette mesure peut être obtenue à l’aide de l’outil w32tm.exe intégré.  Pour ce faire :

1. Effectuez le calcul à partir de la cible et du serveur de temps B.
    
    `w32tm /stripchart /computer:TimeServerB /rdtsc /samples:450 > c:\temp\Target_TsB.csv`

2. Effectuez le calcul à partir du serveur de temps B par rapport au serveur de temps A (avec pointage vers ce dernier).
    
    `w32tm /stripchart /computer:TimeServerA /rdtsc /samples:450 > c:\temp\Target_TsA.csv`

3. Effectuez le calcul à partir du serveur de temps A par rapport à la source.
 
4. Ajoutez ensuite les temps d’aller-retour moyens mesurés à l’étape précédente et divisez le résultat par 2 pour obtenir le délai réseau cumulé entre la cible et la source.

#### <a name="registry-settings"></a>Paramètres du Registre

# <a name="minpollinterval"></a>[MinPollInterval](#tab/MinPollInterval)
Configure le plus petit intervalle en secondes log2 autorisé pour l’interrogation du système.

|  |  | 
|---------|---------|
|Emplacement de la clé     | HKLM:\SYSTEM\CurrentControlSet\Services\W32Time\Config        |
|Paramètre    | 6        |
|Résultat | L’intervalle d’interrogation minimal est maintenant de 64 secondes. |

La commande suivante indique au service de temps Windows de récupérer les paramètres mis à jour :

`w32tm /config /update`


# <a name="maxpollinterval"></a>[MaxPollInterval](#tab/MaxPollInterval)
Configure le plus grand intervalle en secondes log2 autorisé pour l’interrogation du système.

|  |  |  
|---------|---------|
|Emplacement de la clé     | HKLM:\SYSTEM\CurrentControlSet\Services\W32Time\Config        |
|Paramètre    | 6        |
|Résultat | L’intervalle d’interrogation maximal est maintenant de 64 secondes.  |

La commande suivante indique au service de temps Windows de récupérer les paramètres mis à jour :

`w32tm /config /update`

# <a name="updateinterval"></a>[UpdateInterval](#tab/UpdateInterval)
Nombre de cycles d’horloge entre les ajustements de correction de phase.

|  |  |  
|---------|---------|
|Emplacement de la clé     | HKLM:\SYSTEM\CurrentControlSet\Services\W32Time\Config       |
|Paramètre    | 100        |
|Résultat | Le nombre de cycles d’horloge entre les ajustements de correction de phase s’élève maintenant à 100. |

La commande suivante indique au service de temps Windows de récupérer les paramètres mis à jour :

`w32tm /config /update`

# <a name="specialpollinterval"></a>[SpecialPollInterval](#tab/SpecialPollInterval)
Configure l’intervalle d’interrogation en secondes quand l’indicateur SpecialInterval 0x1 est activé.

|  |  |  
|---------|---------|
|Emplacement de la clé     | HKLM:\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpClient        |
|Paramètre    | 64        |
|Résultat | L’intervalle d’interrogation est maintenant de 64 secondes. |

La commande suivante redémarre le service de temps Windows pour récupérer les paramètres mis à jour :

`net stop w32time && net start w32time`

# <a name="frequencycorrectrate"></a>[FrequencyCorrectRate](#tab/FrequencyCorrectRate)

|  |  |  
|---------|---------|
|Emplacement de la clé     | HKLM:\SYSTEM\CurrentControlSet\Services\W32Time\Config      |
|Paramètre    | 2        |


---
