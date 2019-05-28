---
ms.assetid: ''
title: Configuration des systèmes de haute précision
description: Synchronisation date / heure dans Windows 10 et Windows Server 2016 a été considérablement améliorée.  Conditions raisonnable d’exploitation, les systèmes peuvent être configurés pour mettre à jour de 1 ms (millisecondes) ou mieux (par rapport à UTC).
author: shortpatti
ms.author: dacuo
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: 2a5a7a6bd6313f7a4eadd827e3d754c1e467c3bc
ms.sourcegitcommit: 2977c707a299929c6ab0d1e0adab2e1c644b8306
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63745424"
---
# <a name="configuring-systems-for-high-accuracy"></a>Configuration des systèmes de haute précision
>S’applique à : Windows Server 2016 et Windows 10 version 1607 ou ultérieure

Synchronisation date / heure dans Windows 10 et Windows Server 2016 a été considérablement améliorée.  Conditions raisonnable d’exploitation, les systèmes peuvent être configurés pour mettre à jour de 1 ms (millisecondes) ou mieux (par rapport à UTC).

Les conseils suivants vous aideront à configurer vos systèmes pour atteindre une haute précision.  Cet article aborde les conditions suivantes :

- Systèmes d'exploitation pris en charge
- Configuration du système 

> [!WARNING]
> **Objectifs de précision de systèmes d’exploitation antérieurs**<br>
>Windows Server 2012 R2 et ci-dessous peut respectent pas les mêmes objectifs de haute précision. Ces systèmes d’exploitation ne sont pas pris en charge pour une haute précision.
>
>Dans ces versions, le service de temps de Windows satisfait les exigences suivantes :
>
> - Fourni à la précision du temps nécessaire pour satisfaire les exigences d’authentification Kerberos version 5.
> - Fourni heure faiblement précise pour les clients Windows et les serveurs joints à une forêt Active Directory courantes.
>
>Les tolérances supérieure de 2012 R2 et versions inférieures sont en dehors de la spécification de conception du service de temps de Windows.

## <a name="windows-10-and-windows-server-2016-default-configuration"></a>Windows 10 et la Configuration de Windows Server 2016 par défaut

Bien que nous prenons en charge la précision jusqu'à 1 ms sur Windows 10 ou Windows Server 2016, la majorité des clients ne nécessitent pas de temps très précis.

Par conséquent, le **configuration par défaut** vise à répondre aux mêmes exigences que les systèmes d’exploitation antérieurs à :

- Fournir la précision du temps nécessaire pour satisfaire les exigences d’authentification Kerberos version 5.
- Fournit un temps faiblement précis pour les clients Windows et les serveurs joints à une forêt Active Directory courantes.

## <a name="how-to-configure-systems-for-high-accuracy"></a>Comment configurer des systèmes de haute précision

>[!IMPORTANT]
>**Remarque concernant la prise en charge des systèmes extrêmement précis**<br>
> Précision du temps implique la distribution de bout en bout de temps précis à partir de la source de temps faisant autorité pour le périphérique.  Tout ce qui ajoute assymetry dans les mesures le long de ce chemin d’accès aura un impact négatif sur précision affecte l’exactitude réalisable sur vos appareils.
>
>Pour cette raison, nous avons documenté le [limite de prise en charge pour configurer le service de temps de Windows pour les environnements de haute-précision](support-boundary.md) mise en relief les exigences de l’environnement qui doivent également être respectées pour atteindre les objectifs de haute précision.

### <a name="operating-system-requirements"></a>Configuration requise pour le système d’exploitation

Configurations de haute précision requièrent Windows 10 ou Windows Server 2016.  Tous les appareils Windows dans la topologie de temps doivent répondre à cette exigence, y compris les serveurs de temps Windows de niveau supérieur et dans virtualisée scénarios, les hôtes Hyper-V qui s’exécutent les machines virtuelles de temps. Tous ces appareils doivent être au moins Windows 10 ou Windows Server 2016.

Dans l’illustration ci-dessous, les machines virtuelles nécessitant une haute précision sont en cours d’exécution Windows 10 ou Windows Server 2016.  De même, l’hôte Hyper-V sur lequel résident les ordinateurs virtuels et le serveur de temps Windows en amont doivent également exécuter Windows Server 2016.

![Topologie de temps - 1607](../media/Windows-Time-Service/Configuring-Systems-for-High-Accuracy/Topology2016.png)


>[!TIP] 
>**Détermination de la Version de Windows**<br>
> Vous pouvez exécuter la commande `winver` à une invite de commandes pour vérifier le système d’exploitation version est 1607 (ou version ultérieure) et de système d’exploitation est 14393 (ou version ultérieure) comme indiqué ci-dessous :
>
> ![WINVER - 2016 1607](../media/Windows-Time-Service/Configuring-Systems-for-High-Accuracy/winver2016.png)

### <a name="system-configuration"></a>Configuration du système

Atteindre les objectifs de haute précision nécessite la configuration du système.  Il existe plusieurs façons d’effectuer cette configuration, y compris directement dans le Registre ou via une stratégie de groupe.  Vous trouverez plus d’informations pour chacun de ces paramètres dans Windows Time Service Technical Reference – [outils de Service de temps Windows](Windows-Time-Service-Tools-and-Settings.md#windows-time-service-tools).

#### <a name="windows-time-service-startup-type"></a>Service de temps de Windows Type de démarrage

Le service de temps de Windows (W32Time) doit s’exécuter en permanence.  Pour ce faire, configurez le type de démarrage du service de temps de Windows à démarrage « Automatique ».

![Configuration automatique](../media/Windows-Time-Service/Configuring-Systems-for-High-Accuracy/AutomaticService.PNG)

#### <a name="cumulative-one-way-network-latency"></a>Latence cumulative réseau unidirectionnel

Incertitude de mesure et de « bruit » peut faire apparaître en tant que les augmentations de latence réseau.  Par conséquent, il est impératif que la latence du réseau soit dans une limite raisonnable.  Les exigences spécifiques dépendent de votre analyse de précision de cible et sont décrites dans le [limite de prise en charge pour configurer le service de temps de Windows pour les environnements de haute-précision](support-boundary.md) article.

Pour calculer la latence du réseau unidirectionnel cumulé, ajoutez les retards individuels unidirectionnels entre les paires de nœuds de client-serveur NTP dans la topologie de temps, en commençant par la cible et se terminant à l’analyse de haute précision de niveau 1 source de temps.

Exemple : Envisagez une hiérarchie de synchronisation de temps avec une grande précision source, deux serveurs NTP intermédiaires A et B et l’ordinateur cible dans l’ordre. Pour obtenir la latence du réseau cumulé entre la cible et source, mesurer la moyenne des aller-retour NTP fois (RTTs) entre :

- Le serveur cible et l’heure B
- Le serveur de temps B et le serveur de temps A
- Le serveur de temps A et la Source

Cette mesure peut être obtenue à l’aide de l’outil w32tm.exe de boîte de réception.  Pour ce faire :
<!-- Use PowerShell to import the CSV then average the RTT Column -->

1. Effectuer le calcul à partir du serveur cible et l’heure de B.
    
    `w32tm /stripchart /computer:TimeServerB /rdtsc /samples:450 > c:\temp\Target_TsB.csv`

2. Effectuer le calcul du temps serveur b contre (pointé) serveur de temps un.
    
    `w32tm /stripchart /computer:TimeServerA /rdtsc /samples:450 > c:\temp\Target_TsA.csv`

3. Effectuer le calcul à partir du serveur de temps un par rapport à la source.
 
4. Ensuite, ajoutez que le RoundTripDelay moyenne mesurée à l’étape précédente et divisez par 2 pour obtenir le délai de réseau cumulé entre source et cible.

#### <a name="registry-settings"></a>Paramètres du Registre

# <a name="minpollintervaltabminpollinterval"></a>[MinPollInterval](#tab/MinPollInterval)
Configure le plus petit intervalle en secondes de log2 autorisé pour l’interrogation du système.

|  |  | 
|---------|---------|
|Emplacement de la clé     | HKLM:\SYSTEM\CurrentControlSet\Services\W32Time\Config        |
|Paramètre    | 6        |
|Résultat | L’intervalle d’interrogation minimale est désormais 64 secondes. |

La commande suivante signale les temps de Windows pour collecter les paramètres de mise à jour :

`w32tm /config /update`


# <a name="maxpollintervaltabmaxpollinterval"></a>[MaxPollInterval](#tab/MaxPollInterval)
Configure l’intervalle le plus grand en secondes de log2 autorisé pour l’interrogation du système.

|  |  |  
|---------|---------|
|Emplacement de la clé     | HKLM:\SYSTEM\CurrentControlSet\Services\W32Time\Config        |
|Paramètre    | 6        |
|Résultat | L’intervalle d’interrogation maximal est désormais 64 secondes.  |

La commande suivante signale les temps de Windows pour collecter les paramètres de mise à jour :

`w32tm /config /update`

# <a name="updateintervaltabupdateinterval"></a>[UpdateInterval](#tab/UpdateInterval)
Le nombre de battements d’horloge entre les ajustements de correction de phase.

|  |  |  
|---------|---------|
|Emplacement de la clé     | HKLM:\SYSTEM\CurrentControlSet\Services\W32Time\Config       |
|Paramètre    | 100        |
|Résultat | Le nombre de battements d’horloge entre les ajustements de correction de phase est désormais graduations de 100. |

La commande suivante signale les temps de Windows pour collecter les paramètres de mise à jour :

`w32tm /config /update`

# <a name="specialpollintervaltabspecialpollinterval"></a>[SpecialPollInterval](#tab/SpecialPollInterval)
Configure l’intervalle d’interrogation en secondes lorsque l’indicateur SpecialInterval 0 x 1 est activé.

|  |  |  
|---------|---------|
|Emplacement de la clé     | HKLM:\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpClient        |
|Paramètre    | 64        |
|Résultat | L’intervalle d’interrogation est désormais 64 secondes. |

La commande suivante redémarre les temps de Windows pour collecter les paramètres de mise à jour :

`net stop w32time && net start w32time`

# <a name="frequencycorrectratetabfrequencycorrectrate"></a>[FrequencyCorrectRate](#tab/FrequencyCorrectRate)

|  |  |  
|---------|---------|
|Emplacement de la clé     | HKLM:\SYSTEM\CurrentControlSet\Services\W32Time\Config      |
|Paramètre    | 2        |


---
