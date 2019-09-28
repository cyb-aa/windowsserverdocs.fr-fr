---
ms.assetid: ''
title: Configuration des systèmes pour une haute précision
description: La synchronisation de l’heure dans Windows 10 et Windows Server 2016 a été considérablement améliorée.  Dans des conditions raisonnables, les systèmes peuvent être configurés pour maintenir une précision de 1 ms (milliseconde) ou mieux (en ce qui concerne l’heure UTC).
author: shortpatti
ms.author: dacuo
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: b7cd256fdbbdbe7432e5b5d5b16254314132560f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405192"
---
# <a name="configuring-systems-for-high-accuracy"></a>Configuration des systèmes pour une haute précision
>S’applique à : Windows Server 2016 et Windows 10 version 1607 ou ultérieure

La synchronisation de l’heure dans Windows 10 et Windows Server 2016 a été considérablement améliorée.  Dans des conditions raisonnables, les systèmes peuvent être configurés pour maintenir une précision de 1 ms (milliseconde) ou mieux (en ce qui concerne l’heure UTC).

Les conseils suivants vous aideront à configurer vos systèmes pour obtenir une haute précision.  Cet article présente les conditions requises suivantes :

- Systèmes d'exploitation pris en charge
- Configuration du système 

> [!WARNING]
> **Objectifs de précision des systèmes d’exploitation antérieurs**<br>
>Windows Server 2012 R2 et versions antérieures ne peuvent pas atteindre les mêmes objectifs de précision élevée. Ces systèmes d’exploitation ne sont pas pris en charge pour une grande précision.
>
>Dans ces versions, le service de temps Windows respectait les conditions suivantes :
>
> - A fourni la précision de temps nécessaire pour répondre aux exigences d’authentification Kerberos version 5.
> - Offre un temps faible et précis pour les clients et les serveurs Windows joints à une forêt Active Directory commune.
>
>Les tolérances plus élevées sur 2012 R2 et les versions antérieures ne sont pas comprises dans les spécifications de conception du service de temps Windows.

## <a name="windows-10-and-windows-server-2016-default-configuration"></a>Configuration par défaut de Windows 10 et Windows Server 2016

Même si nous prenons en charge une précision allant jusqu’à 1 ms sur Windows 10 ou Windows Server 2016, la majorité des clients n’ont pas besoin de temps très précis.

Par conséquent, la **configuration par défaut** est conçue pour répondre aux mêmes exigences que les systèmes d’exploitation antérieurs qui sont les suivants :

- Fournissez la précision de temps nécessaire pour répondre aux exigences d’authentification Kerberos version 5.
- Fournissez un temps faiblement précis pour les clients et les serveurs Windows joints à une forêt Active Directory commune.

## <a name="how-to-configure-systems-for-high-accuracy"></a>Comment configurer des systèmes pour une haute précision

>[!IMPORTANT]
>**Remarque relative à la prise en charge de systèmes très précis**<br>
> L’exactitude du temps implique la distribution de bout en bout du temps précis entre la source de temps faisant autorité et l’appareil final.  Tout ce qui ajoute des assymetry dans les mesures sur ce chemin d’accès aura une incidence négative sur la précision obtenue sur vos appareils.
>
>Pour cette raison, nous avons documenté la [limite de support pour configurer le service de temps Windows pour les environnements à haute précision](support-boundary.md) qui détaillent les exigences environnementales qui doivent également être respectées pour atteindre des objectifs de haute précision.

### <a name="operating-system-requirements"></a>Configuration requise pour le système d’exploitation

Les configurations de précision élevée nécessitent Windows 10 ou Windows Server 2016.  Tous les appareils Windows de la topologie d’heure doivent répondre à cette exigence, y compris les serveurs de temps Windows de couche supérieure et, dans les scénarios virtualisés, les hôtes Hyper-V qui exécutent les ordinateurs virtuels sensibles au temps. Tous ces appareils doivent être au moins Windows 10 ou Windows Server 2016.

Dans l’illustration ci-dessous, les machines virtuelles nécessitant une haute précision exécutent Windows 10 ou Windows Server 2016.  De même, l’hôte Hyper-V sur lequel résident les ordinateurs virtuels et le serveur de temps Windows en amont doivent également exécuter Windows Server 2016.

![Topologie d’heure-1607](../media/Windows-Time-Service/Configuring-Systems-for-High-Accuracy/Topology2016.png)


>[!TIP] 
>**Détermination de la version de Windows**<br>
> Vous pouvez exécuter la commande `winver` à une invite de commandes pour vérifier que la version du système d’exploitation est 1607 (ou ultérieure) et que la version du système d’exploitation est 14393 (ou supérieure), comme indiqué ci-dessous :
>
> ![Winver-2016 1607](../media/Windows-Time-Service/Configuring-Systems-for-High-Accuracy/winver2016.png)

### <a name="system-configuration"></a>Configuration du système

Pour atteindre des objectifs de précision élevée, vous devez configurer le système.  Il existe plusieurs façons d’effectuer cette configuration, notamment directement dans le registre ou via la stratégie de groupe.  Pour plus d’informations sur chacun de ces paramètres, consultez la référence technique du service de temps Windows – [outils du service de temps Windows](Windows-Time-Service-Tools-and-Settings.md#windows-time-service-tools).

#### <a name="windows-time-service-startup-type"></a>Type de démarrage du service de temps Windows

Le service de temps Windows (W32Time) doit s’exécuter en continu.  Pour ce faire, configurez le type de démarrage du service de temps Windows sur « automatique ».

![Configuration automatique](../media/Windows-Time-Service/Configuring-Systems-for-High-Accuracy/AutomaticService.PNG)

#### <a name="cumulative-one-way-network-latency"></a>Latence réseau unidirectionnelle cumulative

L’incertitude de mesure et le « bruit » augmentent en cas d’augmentation de la latence du réseau.  Par conséquent, il est impératif qu’une latence du réseau se situe dans une limite raisonnable.  Les exigences spécifiques dépendent de la précision de votre cible et sont présentées dans la [limite de support pour configurer l’article service de temps Windows pour les environnements à haute précision](support-boundary.md) .

Pour calculer la latence réseau unidirectionnelle cumulée, ajoutez les retards unidirectionnels individuels entre les paires de nœuds client-serveur NTP dans la topologie d’heure, en commençant par la cible et en finissant à la source de temps de base 1 de la haute précision.

Exemple : Considérez une hiérarchie de synchronisation horaire avec une source très précise, deux serveurs NTP intermédiaires A et B et l’ordinateur cible dans cet ordre. Pour obtenir la latence réseau cumulée entre la cible et la source, mesurez la moyenne des temps de boucles NTP individuels (RTTs) entre :

- Le serveur cible et le serveur de temps B
- Serveur de temps B et le serveur de temps A
- Serveur de temps A et source

Cette mesure peut être obtenue à l’aide de l’outil w32tm. exe de la boîte de réception.  Pour ce faire :

1. Effectuez le calcul à partir du serveur cible et de l’heure B.
    
    `w32tm /stripchart /computer:TimeServerB /rdtsc /samples:450 > c:\temp\Target_TsB.csv`

2. Effectuez le calcul à partir du serveur de temps b par rapport au serveur de temps a.
    
    `w32tm /stripchart /computer:TimeServerA /rdtsc /samples:450 > c:\temp\Target_TsA.csv`

3. Effectuez le calcul à partir du serveur de temps a sur la source.
 
4. Ajoutez ensuite le nombre moyen de RoundTripDelay mesuré à l’étape précédente et divisez-le par 2 pour obtenir le délai réseau cumulé entre la cible et la source.

#### <a name="registry-settings"></a>Paramètres du Registre

# <a name="minpollintervaltabminpollinterval"></a>[MinPollInterval](#tab/MinPollInterval)
Configure le plus petit intervalle en log2 de secondes autorisé pour l’interrogation du système.

|  |  | 
|---------|---------|
|Emplacement de la clé     | HKLM : \ SYSTEM\CurrentControlSet\Services\W32Time\Config        |
|Paramètre    | 6\.        |
|Résultat | L’intervalle d’interrogation minimal est maintenant de 64 secondes. |

La commande suivante indique au temps Windows de récupérer les paramètres mis à jour :

`w32tm /config /update`


# <a name="maxpollintervaltabmaxpollinterval"></a>[MaxPollInterval](#tab/MaxPollInterval)
Configure le plus grand intervalle en log2 de secondes autorisé pour l’interrogation du système.

|  |  |  
|---------|---------|
|Emplacement de la clé     | HKLM : \ SYSTEM\CurrentControlSet\Services\W32Time\Config        |
|Paramètre    | 6\.        |
|Résultat | La fréquence d’interrogation maximale est maintenant de 64 secondes.  |

La commande suivante indique au temps Windows de récupérer les paramètres mis à jour :

`w32tm /config /update`

# <a name="updateintervaltabupdateinterval"></a>[UpdateInterval](#tab/UpdateInterval)
Nombre de battements d’horloge entre les ajustements de correction de phase.

|  |  |  
|---------|---------|
|Emplacement de la clé     | HKLM : \ SYSTEM\CurrentControlSet\Services\W32Time\Config       |
|Paramètre    | 100        |
|Résultat | Le nombre de battements d’horloge entre les ajustements de correction de phase est maintenant de 100 graduations. |

La commande suivante indique au temps Windows de récupérer les paramètres mis à jour :

`w32tm /config /update`

# <a name="specialpollintervaltabspecialpollinterval"></a>[SpecialPollInterval](#tab/SpecialPollInterval)
Configure l’intervalle d’interrogation en secondes lorsque l’indicateur SpecialInterval 0x1 est activé.

|  |  |  
|---------|---------|
|Emplacement de la clé     | HKLM : \ SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpClient        |
|Paramètre    | 64        |
|Résultat | L’intervalle d’interrogation est maintenant de 64 secondes. |

La commande suivante redémarre l’heure Windows pour récupérer les paramètres mis à jour :

`net stop w32time && net start w32time`

# <a name="frequencycorrectratetabfrequencycorrectrate"></a>[FrequencyCorrectRate](#tab/FrequencyCorrectRate)

|  |  |  
|---------|---------|
|Emplacement de la clé     | HKLM : \ SYSTEM\CurrentControlSet\Services\W32Time\Config      |
|Paramètre    | 2        |


---
